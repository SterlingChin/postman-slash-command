# Claude Code Setup Guide

Complete setup instructions for using Postman API Lifecycle Management tools with Claude Code.

---

## Overview

Claude Code integration uses a **slash command** (`/postman`) that provides a natural language interface to all Postman tools. Ask Claude to perform operations, and it will automatically execute the appropriate Python scripts.

---

## Prerequisites

- ✅ **Claude Code** installed - Get it from [claude.ai/claude-code](https://claude.ai/claude-code)
- ✅ **Python 3.7+** - Pre-installed on macOS/Linux
- ✅ **Postman API Key** - Generate from [your account settings](https://go.postman.co/settings/me/api-keys)

---

## Installation

### Step 1: Clone the Repository

```bash
cd ~/your-projects-directory
git clone <repository-url> postman-slash-command
cd postman-slash-command
```

Or download and extract the ZIP file.

---

### Step 2: Install Python Dependencies

```bash
pip install -r requirements.txt
```

**Note**: `python-dotenv` is optional - scripts have a fallback `.env` parser.

---

### Step 3: Configure Your API Key

```bash
cp .env.example .env
```

Edit `.env` and add your Postman API key:

```env
POSTMAN_API_KEY=PMAK-your-actual-api-key-here
```

**Get your API key**: https://go.postman.co/settings/me/api-keys

---

### Step 4: Install the Slash Command

Choose one of these options:

#### Option 1: Global Access (Recommended)

Install for all your projects:

```bash
cp -r .claude ~/
```

This copies the slash command to your home directory where Claude Code can find it globally.

#### Option 2: Project-Specific Access

Install for a specific project only:

```bash
cp -r .claude /path/to/your/project/
```

Claude Code will only load this command when working in that project.

#### Option 3: Manual Installation

If you prefer manual control:

```bash
mkdir -p ~/.claude/commands
cp .claude/commands/postman.md ~/.claude/commands/
```

---

### Step 5: Verify Installation

1. Open Claude Code
2. Start typing `/postman`
3. You should see the command autocomplete

If you see the command, you're ready to go! 🎉

---

## Usage

### Basic Workflow

1. Type `/postman` in Claude Code
2. Ask Claude to perform Postman operations
3. Claude executes the appropriate scripts automatically

---

### Example Commands

#### Discovery
```
/postman
List all my collections
```

```
/postman
Show me all environments in my workspace
```

```
/postman
What monitors do I have running?
```

#### Collection Management
```
/postman
Create a new collection called "Payment API Tests"
```

```
/postman
Show me the details of collection abc123
```

```
/postman
Duplicate my "User API" collection and name it "User API v2"
```

#### Testing
```
/postman
Run tests for the "Payment API" collection
```

```
/postman
Run the "User Authentication" collection with the "Production" environment
```

#### Code Generation
```
/postman
Generate Python code for all requests in my "Payment API" collection
```

```
/postman
Generate curl commands for collection abc123
```

#### Security
```
/postman
Audit the security of my "User API" collection
```

```
/postman
Run a security audit on all my collections
```

#### Mock Servers
```
/postman
Create a mock server for my "Payment API" collection
```

```
/postman
List all my mock servers
```

#### OpenAPI/Spec Management
```
/postman
Create a new spec from my openapi.json file
```

```
/postman
Validate the schema for API abc123
```

```
/postman
Detect breaking changes between collection v1 and v2
```

---

## Advanced Usage

### Natural Language Flexibility

Claude understands variations:

- "List collections" = "Show me my collections" = "What collections do I have?"
- "Create collection" = "Make a new collection" = "Add a collection"
- "Run tests" = "Execute tests" = "Test my collection"

Just ask naturally!

---

### Chaining Operations

You can ask Claude to perform multiple steps:

```
/postman
Create a new collection called "User API", then create a development environment for it, and finally create a mock server
```

Claude will execute all three operations in sequence.

---

### Context Awareness

Claude remembers context within a conversation:

```
/postman
List my collections

# Claude shows collections, including "Payment API" (ID: abc123)

Create a mock server for the Payment API

# Claude knows to use ID abc123 from the previous response
```

---

## Configuration

### Environment Variables

Your `.env` file supports these variables:

```env
# Required
POSTMAN_API_KEY=PMAK-your-key-here

# Optional
POSTMAN_WORKSPACE_ID=your-default-workspace-id
POSTMAN_BASE_URL=https://api.postman.com
POSTMAN_TIMEOUT=30
POSTMAN_MAX_RETRIES=3
```

### Workspace Configuration

By default, scripts use your default workspace. To specify a workspace:

1. Get workspace ID from Postman UI
2. Add to `.env`: `POSTMAN_WORKSPACE_ID=your-workspace-id`

Or ask Claude to list workspaces:
```
/postman
List all my workspaces
```

---

## Features

### ✅ What You Get

- 🗣️ **Natural Language Interface** - No need to remember commands
- 🤖 **Intelligent Context** - Claude understands your intent
- 🔄 **Automatic Retries** - Built-in handling for rate limits
- 📊 **Rich Output** - Formatted results and summaries
- 🎯 **Complete Coverage** - All 8 API lifecycle phases
- 🛠️ **18 Powerful Tools** - Collections, environments, monitors, mocks, security, and more

### 🎨 Lifecycle Coverage

- **Define** - List and discover resources
- **Design** - Validate schemas, detect breaking changes
- **Develop** - Generate code, manage environments
- **Test** - Run collections with environments
- **Secure** - Security auditing and scoring
- **Deploy** - Mock server management
- **Observe** - Monitor analysis and health checks
- **Distribute** - Documentation publishing

---

## Troubleshooting

### Command Not Found

**Issue**: Typing `/postman` doesn't show autocomplete

**Solutions**:
1. Verify `.claude/` directory is in home directory: `ls ~/.claude/commands/`
2. Restart Claude Code
3. Re-run installation: `cp -r .claude ~/`

---

### API Key Not Found

**Issue**: "API key not found" error

**Solutions**:
1. Verify `.env` exists in repository root
2. Check `POSTMAN_API_KEY` is set correctly
3. API key should start with `PMAK-`
4. Ensure no extra spaces or quotes around the key

---

### Permission Denied

**Issue**: "Permission denied" when running scripts

**Solution**:
```bash
chmod +x scripts/*.py
```

---

### Rate Limiting

**Issue**: Getting rate limit errors

**Solutions**:
- Scripts automatically retry with exponential backoff
- Wait a few seconds between large operations
- Check your Postman plan's rate limits

---

## Tips & Best Practices

### 1. Use Descriptive Names

When creating resources, use clear names:
```
Create a collection called "User Management API v1.0"
```

Better than:
```
Create a collection called "test"
```

---

### 2. Reference by Name or ID

Claude can work with both:
- By name: "Run tests for the Payment API collection"
- By ID: "Run tests for collection abc123"

Names are more readable, IDs are more precise.

---

### 3. Ask for Explanations

Claude can explain what it's doing:
```
/postman
What would happen if I delete this collection?
```

---

### 4. Combine with Code

Ask Claude to generate code and save it:
```
/postman
Generate Python code for my Payment API and save it to payment_client.py
```

---

### 5. Use for Documentation

Ask Claude to document your APIs:
```
/postman
Analyze my User API collection and create documentation
```

---

## Next Steps

### Learn More
- 📖 [Complete Tool Reference](../tools.json) - All available tools
- 📊 [API Lifecycle Coverage](../API_LIFECYCLE_COVERAGE.md) - Full capabilities
- 🔧 [Direct CLI Usage](../README.md#direct-script-execution) - Run scripts manually

### Explore Other Agents
- [Cursor Setup Guide](SETUP_CURSOR.md) - Use with Cursor IDE
- [Gemini Setup Guide](SETUP_GEMINI.md) - Use with Google Gemini
- [Multi-Agent Setup](MULTI_AGENT_SETUP.md) - All integration options

### Advanced Topics
- Create custom workflows
- Integrate with CI/CD
- Build automation scripts
- Extend with new tools

---

## Support

- **Issues**: Report at GitHub Issues
- **Documentation**: [Postman API Docs](https://www.postman.com/postman/workspace/postman-public-workspace/documentation/12959542-c8142d51-e97c-46b6-bd77-52bb66712c9a)
- **Community**: [Postman Community](https://community.postman.com)

---

**Enjoy using Postman with Claude Code!** 🚀
