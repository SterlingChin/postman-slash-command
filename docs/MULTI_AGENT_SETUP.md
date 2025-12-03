# Multi-Agent Setup Guide

This repository is designed to work across multiple AI coding agents. Choose your setup method based on your preferred agent.

## 🎯 Claude Code (Slash Command)

**Status**: ✅ **Production Ready**

### Installation

```bash
# Option 1: Global access (recommended)
cp -r .claude ~/

# Option 2: Project-specific
cp -r .claude /path/to/your/project/
```

### Usage

Type `/postman` in Claude Code, then ask:
- "List all my collections"
- "Create a new environment for staging"
- "Run tests for the Payment API collection"
- "Generate Python code for my User API collection"

### Features
- Natural language interface
- Automatic script execution
- Context-aware suggestions
- Full access to all 18 scripts

---

## 🎨 Cursor (Composer Rules)

**Status**: ✅ **Production Ready**

### Installation

The `.cursorrules` file is already included in the root directory.

1. Open your project in Cursor
2. Cursor will automatically detect and load `.cursorrules`
3. Reference Postman tools in your prompts

### Usage

Ask Cursor directly:
- "Use the Postman tools to list my collections"
- "Create a mock server for my Payment API collection"
- "Audit the security of my User API"
- "Generate curl commands for all my requests"

Cursor will automatically use the appropriate Python scripts.

### Features
- Inline tool documentation
- Auto-completion for tool names
- Direct script execution
- Full Python script access

---

## 💎 Google Gemini Code Assist

**Status**: ✅ **Compatible**

### Installation

1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

2. Configure API key:
   ```bash
   cp .env.example .env
   # Edit .env and add your POSTMAN_API_KEY
   ```

3. Reference `tools.json` in your prompts

### Usage

Provide `tools.json` to Gemini in your initial prompt:
```
I have Postman API management tools. Here's the manifest: [paste tools.json]

Please help me [your task here]
```

Or reference scripts directly:
- "Run python scripts/list_collections.py --all"
- "Use manage_mocks.py to create a mock server"

### Features
- Command-line tool access
- JSON manifest for tool discovery
- Direct Python execution
- Full documentation in tools.json

---

## 🌐 Universal MCP Server (Future)

**Status**: 🚧 **Planned**

### What is MCP?

MCP (Model Context Protocol) is Anthropic's universal standard for AI agent tool integration. Converting these tools to an MCP server will enable:

- **Universal Compatibility**: Works with any MCP-compatible agent
- **Native Integration**: Tools appear as native capabilities
- **Type Safety**: Strongly-typed tool parameters
- **Automatic Discovery**: Agents auto-discover available tools

### Conversion Roadmap

1. **Create MCP Server Wrapper** (`server.py`)
   - Expose each script as an MCP tool
   - Map script arguments to tool parameters
   - Handle authentication and configuration

2. **Define Tool Schemas** (`mcp_manifest.json`)
   - Convert tools.json to MCP format
   - Define input/output schemas
   - Add tool metadata

3. **Add Server Configuration** (`mcp_config.json`)
   - Server connection details
   - Authentication method
   - Resource endpoints

4. **Package for Distribution**
   - Create installable package
   - Add to MCP server registry
   - Provide installation instructions

### Expected Structure

```
postman-slash-command/
├── mcp/
│   ├── server.py              # MCP server implementation
│   ├── manifest.json          # MCP tool manifest
│   ├── config.json            # Server configuration
│   └── README.md              # MCP-specific docs
├── scripts/                   # Existing scripts (unchanged)
└── tools.json                 # Universal manifest
```

### Benefits

Once converted, any MCP-compatible agent can use:

```python
# Agent can call tools directly
await mcp.call_tool("list_collections", {"all": true})
await mcp.call_tool("create_mock", {
    "name": "Payment Mock",
    "collection_id": "abc123"
})
```

### Timeline

MCP server implementation is planned for a future release. Current CLI and slash command methods provide full functionality.

---

## 🔧 Direct CLI Usage

**Status**: ✅ **Universal**

Works with **any** agent or no agent at all.

### Installation

```bash
pip install -r requirements.txt
cp .env.example .env
# Edit .env and add your POSTMAN_API_KEY
```

### Usage

Run scripts directly:

```bash
# List resources
python scripts/list_collections.py --all

# Create collection
python scripts/manage_collections.py --create --name="My API"

# Run tests
python scripts/run_collection.py --collection="Payment API"

# Generate code
python scripts/generate_code.py --collection=<id> --language python

# Security audit
python scripts/audit_security.py --all-collections
```

### Features
- No agent dependency
- Full script control
- Shell scripting integration
- CI/CD compatible

---

## 📊 Comparison Matrix

| Agent | Setup Time | Integration | Natural Language | Maintenance |
|-------|-----------|-------------|------------------|-------------|
| Claude Code | 1 min | Slash Command | ✅ Best | Automatic |
| Cursor | 0 min | Auto-detect | ✅ Excellent | Automatic |
| Gemini | 2 min | Manual | ⚠️ Limited | Manual |
| MCP (Future) | TBD | Native | ✅ Best | Automatic |
| Direct CLI | 2 min | Manual | ❌ None | Manual |

---

## 🎯 Recommendations

**For Claude Code users**: Use the slash command (already set up)

**For Cursor users**: Use .cursorrules (already included)

**For Gemini users**: Reference tools.json in your prompts

**For other agents**: Use direct CLI until MCP server is available

**For CI/CD**: Use direct CLI for maximum control

---

## 🔗 Related Files

- `tools.json` - Universal tool manifest
- `.claude/commands/postman.md` - Claude Code slash command
- `.cursorrules` - Cursor integration rules
- `README.md` - Main documentation

---

## 💡 Contributing

Want to add support for another agent? Please:

1. Create an integration file (e.g., `.gemini_rules`, `.windsurf_config`)
2. Document the setup in this guide
3. Test with real workflows
4. Submit a pull request

We welcome integrations for:
- GitHub Copilot Workspace
- Replit Agent
- Windsurf
- Cody
- Other AI coding assistants
