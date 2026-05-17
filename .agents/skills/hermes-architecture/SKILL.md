---
name: hermes-architecture
description: Hermes Agent core architecture, component relationships, and design patterns
triggers:
  - "how is hermes-agent structured"
  - "explain hermes architecture"
  - "AIAgent class architecture"
  - "gateway runner architecture"
  - "plugin system design"
  - "tool registry design"
---

# Hermes Agent Architecture

## Structure

```
hermes-agent/
├── run_agent.py          # AIAgent class — core conversation loop (~12k LOC)
├── model_tools.py        # Tool orchestration, discover_builtin_tools(), handle_function_call()
├── toolsets.py           # Toolset definitions, _HERMES_CORE_TOOLS list
├── cli.py                # HermesCLI class — interactive CLI orchestrator (~11k LOC)
├── hermes_state.py       # SessionDB — SQLite session store (FTS5 search)
├── hermes_constants.py   # get_hermes_home(), display_hermes_home() — profile-aware paths
├── hermes_logging.py     # setup_logging() — agent.log / errors.log / gateway.log
├── batch_runner.py       # Parallel batch processing
├── agent/                # Agent internals (provider adapters, memory, caching, compression)
├── hermes_cli/           # CLI subcommands, setup wizard, plugins loader, skin engine
├── tools/                # Tool implementations — auto-discovered via tools/registry.py
│   └── environments/     # Terminal backends (local, docker, ssh, modal, daytona, singularity)
├── gateway/              # Messaging gateway — run.py + session.py + platforms/
│   ├── platforms/        # Adapter per platform (telegram, discord, slack, whatsapp, etc.)
│   └── builtin_hooks/    # Extension point for gateway hooks
├── plugins/              # Plugin system (memory, context_engine, model-providers, etc.)
├── skills/               # Built-in skills bundled with the repo
├── optional-skills/      # Heavier/niche skills shipped but NOT active by default
├── ui-tui/               # Ink (React) terminal UI — `hermes --tui`
├── tui_gateway/          # Python JSON-RPC backend for the TUI
├── acp_adapter/          # ACP server (VS Code / Zed / JetBrains integration)
└── cron/                 # Scheduler — jobs.py, scheduler.py
```

**User config:** `~/.hermes/config.yaml` (settings), `~/.hermes/.env` (API keys only)
**Logs:** `~/.hermes/logs/` — `agent.log` (INFO+), `errors.log` (WARNING+), `gateway.log`

### File Dependency Chain

```
tools/registry.py  (no deps — imported by all tool files)
       ↑
tools/*.py  (each calls registry.register() at import time)
       ↑
model_tools.py  (imports tools/registry + triggers tool discovery)
       ↑
run_agent.py, cli.py, batch_runner.py, environments/
```

---

## Core Classes

### AIAgent (run_agent.py)

The main agent class handling conversation loops and tool execution.

```python
class AIAgent:
    def __init__(self,
        base_url: str = None,
        api_key: str = None,
        provider: str = None,
        api_mode: str = None,              # "chat_completions" | "codex_responses"
        model: str = "",
        max_iterations: int = 90,
        enabled_toolsets: list = None,
        disabled_toolsets: list = None,
        quiet_mode: bool = False,
        save_trajectories: bool = False,
        platform: str = None,
        session_id: str = None,
        skip_context_files: bool = False,
        skip_memory: bool = False,
        credential_pool=None,
        # ... ~60 params total
    ): ...

    def chat(self, message: str) -> str:
        """Simple interface — returns final response string."""

    def run_conversation(self, user_message: str, system_message: str = None,
                         conversation_history: list = None, task_id: str = None) -> dict:
        """Full interface — returns dict with final_response + messages."""
```

**Agent Loop** (inside `run_conversation()`):

```python
while (api_call_count < self.max_iterations and self.iteration_budget.remaining > 0) \
        or self._budget_grace_call:
    if self._interrupt_requested: break
    response = client.chat.completions.create(model=model, messages=messages, tools=tool_schemas)
    if response.tool_calls:
        for tool_call in response.tool_calls:
            result = handle_function_call(tool_call.name, tool_call.args, task_id)
            messages.append(tool_result_message(result))
        api_call_count += 1
    else:
        return response.content
```

Messages follow OpenAI format: `{"role": "system/user/assistant/tool", ...}`

---

### HermesCLI (cli.py)

Interactive CLI orchestrator with Rich for UI and prompt_toolkit for input.

- **KawaiiSpinner** (`agent/display.py`) — animated faces during API calls
- **Skin engine** (`hermes_cli/skin_engine.py`) — data-driven CLI theming
- `process_command()` dispatches on canonical command name via `resolve_command()`

### GatewayRunner (gateway/run.py)

Main gateway controller managing platform adapters and message routing.

```python
class GatewayRunner:
    def __init__(self, config: Optional[GatewayConfig] = None):
        self.adapters: Dict[Platform, BasePlatformAdapter] = {}
        self.session_store = SessionStore(...)
        self.delivery_router = DeliveryRouter(self.config)
        self._running_agents: Dict[str, Any] = {}  # session_key -> AIAgent
        self._queued_events: Dict[str, List[MessageEvent]] = {}
```

### BasePlatformAdapter (gateway/platforms/base.py)

Abstract base for platform adapters. Subclasses implement:
- Connecting and authenticating
- Receiving messages
- Sending messages/responses
- Handling media

```python
class BasePlatformAdapter(ABC):
    def __init__(self, config: PlatformConfig, platform: Platform):
        self.config = config
        self.platform = platform
        self._message_handler: Optional[MessageHandler] = None
        self._active_sessions: Dict[str, asyncio.Event] = {}
        self._pending_messages: Dict[str, MessageEvent] = {}
```

Platforms include: Telegram, Discord, Slack, WhatsApp, Signal, Matrix, Mattermost, SMS, DingTalk, WeCom, Weixin, Feishu, QQBot, BlueBubbles, Yuanbao, Webhook, APIServer, HomeAssistant, and more.

---

## Tools

### Tool Registry (tools/registry.py)

Central registry for all hermes-agent tools. Each tool file calls `registry.register()` at module level.

```python
from tools.registry import registry

def check_requirements() -> bool:
    return bool(os.getenv("EXAMPLE_API_KEY"))

def example_tool(param: str, task_id: str = None) -> str:
    return json.dumps({"success": True, "data": "..."})

registry.register(
    name="example_tool",
    toolset="example",
    schema={"name": "example_tool", "description": "...", "parameters": {...}},
    handler=lambda args, **kw: example_tool(param=args.get("param", ""), task_id=kw.get("task_id")),
    check_fn=check_requirements,
    requires_env=["EXAMPLE_API_KEY"],
)
```

Auto-discovery: any `tools/*.py` file with a top-level `registry.register()` call is imported automatically.

### Tool Model (model_tools.py)

Thin orchestration layer over the tool registry.

```python
from model_tools import (
    get_tool_definitions,    # -> list of tool schemas
    handle_function_call,    # execute tool by name
    get_all_tool_names,      # -> list
    get_toolset_for_tool,    # -> str
    get_available_toolsets,  # -> dict
    check_tool_availability, # -> tuple
)
```

### Toolsets (toolsets.py)

Group tools into named bundles. `_HERMES_CORE_TOOLS` is the default bundle for all platforms.

```python
_HERMES_CORE_TOOLS = [
    "web_search", "web_extract",
    "terminal", "process",
    "read_file", "write_file", "patch", "search_files",
    "vision_analyze", "image_generate",
    "skills_list", "skill_view", "skill_manage",
    "browser_navigate", "browser_snapshot", "browser_click",
    "text_to_speech",
    "todo", "memory",
    "session_search", "clarify",
    "execute_code", "delegate_task",
    "cronjob",
    "send_message",
    # ... more
]
```

### Tool Environment Backends (tools/environments/)

Pluggable terminal execution backends:

| Backend | Description |
|---------|-------------|
| `local.py` | Local subprocess execution |
| `docker.py` | Docker container isolation |
| `ssh.py` | Remote SSH execution |
| `modal.py` | Modal.com cloud execution |
| `daytona.py` | Daytona sandbox execution |
| `singularity.py` | Singularity container execution |

---

## Plugins

### Plugin Architecture (hermes_cli/plugins.py)

Discovers, loads, and manages plugins from four sources:

1. **Bundled plugins** — `<repo>/plugins/<name>/`
2. **User plugins** — `~/.hermes/plugins/<name>/`
3. **Project plugins** — `./.hermes/plugins/<name>/` (opt-in via `HERMES_ENABLE_PROJECT_PLUGINS`)
4. **Pip plugins** — packages exposing `hermes_agent.plugins` entry-point group

Each directory plugin must contain a `plugin.yaml` manifest **and** an `__init__.py` with a `register(ctx)` function.

```python
# plugin.yaml
name: my-plugin
version: "1.0"
description: My plugin description
provides_tools:
  - my_tool
requires_env:
  - MY_API_KEY

# __init__.py
def register(ctx: PluginContext):
    ctx.register_tool(name="my_tool", schema={...}, handler=my_handler)
    ctx.invoke_hook("on_agent_start", agent=agent)
```

### Plugin Categories

```
plugins/
├── memory/           # Memory-provider plugins (honcho, mem0, supermemory, ...)
├── context_engine/  # Context-engine plugins
├── model-providers/ # Inference backend plugins (openrouter, anthropic, gmi, ...)
├── kanban/          # Multi-agent board dispatcher + worker
├── hermes-achievements/  # Gamified achievement tracking
├── observability/   # Metrics / traces / logs
├── image_gen/       # Image-generation providers (openai, stability, ...)
├── browser/         # Browser providers (browserbase, firecrawl, browser_use)
├── web/             # Web search providers (tavily, exa, searxng, brave_free, ...)
├── video_gen/       # Video generation (xai, fal)
├── platforms/       # Additional platform adapters (teams, simplex, line, irc, google_chat)
└── <others>/        # disk-cleanup, example-dashboard, google_meet, spotify, ...
```

### Plugin Lifecycle Hooks

Plugins may register callbacks for hooks in `VALID_HOOKS`:
- `on_agent_start`, `on_agent_stop`
- `on_before_tool_call`, `on_after_tool_call`
- `on_message_received`, `on_message_sent`
- `on_session_start`, `on_session_end`

---

## Platforms

### Gateway Architecture (gateway/run.py)

The gateway is a long-running daemon that bridges messaging platforms to the AIAgent.

```
hermes gateway
├── GatewayRunner
│   ├── adapters: Dict[Platform, BasePlatformAdapter]
│   ├── session_store: SessionStore
│   ├── delivery_router: DeliveryRouter
│   └── _running_agents: Dict[session_key, AIAgent]
│
└── Platform Adapters (one per platform)
    ├── TelegramAdapter
    ├── DiscordAdapter
    ├── SlackAdapter
    ├── WhatsAppAdapter
    └── ...
```

### Session Management (hermes_state.py)

SQLite-based session storage with FTS5 full-text search.

```python
class SessionDB:
    def __init__(self, db_path: Path = DEFAULT_DB_PATH):
        # WAL mode for concurrent readers + one writer
        # FTS5 virtual table for message search
        # Session compression via parent_session_id chains
```

Schema: `sessions`, `messages` tables with full-text search on message content.

### Adding a New Platform

1. Create `gateway/platforms/<name>.py` inheriting from `BasePlatformAdapter`
2. Register in `gateway/platform_registry.py`
3. Add config handling in `gateway/config.py`
4. Document in `ADDING_A_PLATFORM.md`

---

## MCP (Model Context Protocol)

### MCP Tool Support (tools/mcp_tool.py)

Connects to external MCP servers via stdio, HTTP/StreamableHTTP, or SSE transport.

```yaml
# config.yaml
mcp_servers:
  filesystem:
    command: "npx"
    args: ["-y", "@modelcontextprotocol/server-filesystem", "/tmp"]
    timeout: 120
  github:
    command: "npx"
    args: ["-y", "@modelcontextprotocol/server-github"]
    env:
      GITHUB_PERSONAL_ACCESS_TOKEN: "ghp_..."
  remote_api:
    url: "https://my-mcp-server.example.com/mcp"
    headers:
      Authorization: "Bearer sk-..."
```

Features:
- Stdio, HTTP/StreamableHTTP, and SSE transports
- Automatic reconnection with exponential backoff (up to 5 retries)
- Environment variable filtering for stdio subprocesses
- Per-server timeouts for tool calls and connections
- Thread-safe architecture with dedicated background event loop
- Sampling support: MCP servers can request LLM completions

---

## Session Management

### HermesState / SessionDB (hermes_state.py)

- **WAL mode** for concurrent readers + one writer (gateway multi-platform)
- **FTS5** virtual table for fast text search across all session messages
- **Compression-triggered session splitting** via `parent_session_id` chains
- Session source tagging (`'cli'`, `'telegram'`, `'discord'`, etc.) for filtering

### Session Key Format

Session keys encode platform, chat, and thread IDs:
```python
session_key = f"{platform}:{chat_id}:{thread_id}"
```

### Message Flow

```
User Message → Platform Adapter → GatewayRunner
    → AIAgent.run_conversation()
    → Tool Execution (handle_function_call)
    → Response → Platform Adapter → User
```

### AIAgent Caching

Gateway caches AIAgent instances per session to preserve prompt caching:
```python
# Without caching: new AIAgent per message = rebuilding system prompt every turn
# With caching: preserved prefix cache, ~10x cost reduction on Anthropic
```
Max cache size: 128 agents, evict after 1 hour idle.

---

## Key Design Patterns

### 1. Self-Registering Tools
Tools declare themselves via `registry.register()` at module import time. No manual registration lists.

### 2. Profile-Aware Paths
Use `get_hermes_home()` for state files, `display_hermes_home()` for user-facing paths:
```python
from hermes_constants import get_hermes_home, display_hermes_home
# ~/.hermes/ or ~/.hermes-profile-name/
```

### 3. Lazy SDK Import
Heavy SDKs (OpenAI ~240ms) imported lazily to speed up CLI startup:
```python
# Instead of: from openai import OpenAI
from agent.process_bootstrap import OpenAI  # Thin proxy, imports on first use
```

### 4. Persistent Event Loops
Tool execution uses persistent event loops to avoid "Event loop is closed" errors:
```python
_tool_loop = None  # CLI thread
_worker_thread_local.loop  # Per-worker-thread loops
```

### 5. Slash Command Registry
Central `COMMAND_REGISTRY` in `hermes_cli/commands.py` drives:
- CLI dispatch
- Gateway hook emission
- Telegram BotCommand menu
- Slack subcommand routing
- Autocomplete
- Help text

### 6. Plugin Override Order
Later sources override earlier: project > user > bundled