# Cursor Setup Guide

Complete setup instructions for using Postman API Lifecycle Management tools with Cursor IDE.

---

## Overview

Cursor integration uses **Composer Rules** (`.cursorrules`) that are automatically detected when you open the project. Reference Postman tools in natural language, and Cursor will execute the appropriate Python scripts.

---

## Prerequisites

- ✅ **Cursor IDE** installed - Get it from [cursor.com](https://cursor.com)
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

### Step 4: Open Project in Cursor

```bash
cursor .
```

Or open Cursor and select "Open Folder" → Choose `postman-slash-command/`

**That's it!** 🎉 Cursor automatically detects the `.cursorrules` file in the repository root.

---

## Usage

### Basic Workflow

1. Open Cursor Composer (Cmd/Ctrl + I)
2. Reference Postman tools in your requests
3. Cursor executes the appropriate scripts automatically

---

### Example Commands

#### Discovery

**In Composer:**
```
Use the Postman tools to list all my collections
```

**What Cursor does:**
```bash
python scripts/list_collections.py --all
```

---

**In Composer:**
```
Show me all environments in my workspace using Postman tools
```

**What Cursor does:**
```bash
python scripts/list_collections.py --environments
```

---

#### Collection Management

**In Composer:**
```
Use Postman tools to create a new collection called "Payment API Tests"
```

**What Cursor does:**
```bash
python scripts/manage_collections.py --create --name="Payment API Tests"
```

---

**In Composer:**
```
Get details of collection abc123 using Postman tools
```

**What Cursor does:**
```bash
python scripts/manage_collections.py --get abc123
```

---

#### Testing

**In Composer:**
```
Run tests for my Payment API collection using Postman tools
```

**What Cursor does:**
```bash
python scripts/run_collection.py --collection="Payment API"
```

---

**In Composer:**
```
Execute the User Authentication collection with Production environment
```

**What Cursor does:**
```bash
python scripts/run_collection.py --collection="User Authentication" --environment="Production"
```

---

#### Code Generation

**In Composer:**
```
Generate Python code for all requests in my Payment API collection
```

**What Cursor does:**
```bash
python scripts/generate_code.py --collection <collection-id> --language python
```

---

**In Composer:**
```
Generate code in all languages for collection abc123
```

**What Cursor does:**
```bash
python scripts/generate_code.py --collection abc123 --all
```

---

#### Security Auditing

**In Composer:**
```
Audit the security of my User API collection
```

**What Cursor does:**
```bash
python scripts/audit_security.py --collection <collection-id>
```

---

**In Composer:**
```
Run security audits on all my collections
```

**What Cursor does:**
```bash
python scripts/audit_security.py --all-collections
```

---

#### Mock Servers

**In Composer:**
```
Create a mock server for my Payment API collection
```

**What Cursor does:**
```bash
python scripts/manage_mocks.py --create --name="Payment Mock" --collection <collection-id>
```

---

**In Composer:**
```
List all mock servers
```

**What Cursor does:**
```bash
python scripts/manage_mocks.py --list
```

---

#### OpenAPI/Spec Management

**In Composer:**
```
Create a new spec from my openapi.json file named "User API"
```

**What Cursor does:**
```bash
python scripts/manage_spec.py create --name="User API" --file=openapi.json
```

---

**In Composer:**
```
Validate the OpenAPI schema for API abc123
```

**What Cursor does:**
```bash
python scripts/validate_schema.py --api abc123
```

---

**In Composer:**
```
Detect breaking changes between these two collections
```

**What Cursor does:**
```bash
python scripts/detect_breaking_changes.py --collection <col-id-1> <col-id-2>
```

---

## How Cursor Integration Works

### The `.cursorrules` File

This file lives in the repository root and contains:

1. **Tool Documentation** - All available Postman tools
2. **Usage Examples** - How to call each tool
3. **Environment Variables** - Configuration requirements
4. **Architecture Overview** - Project structure

### Auto-Detection

When you open the project in Cursor:

1. Cursor scans for `.cursorrules` file
2. Loads tool documentation into context
3. Understands available commands
4. Maps natural language → script execution

### Natural Language Processing

Cursor understands variations:

- "Use Postman tools to list collections"
- "Show my collections with Postman"
- "List all collections using the Postman tools"

All map to the same script execution.

---

## Configuration

### Environment Variables

Your `.env` file supports:

```env
# Required
POSTMAN_API_KEY=PMAK-your-key-here

# Optional
POSTMAN_WORKSPACE_ID=your-default-workspace-id
POSTMAN_BASE_URL=https://api.postman.com
POSTMAN_TIMEOUT=30
POSTMAN_MAX_RETRIES=3
```

### Workspace Selection

By default, scripts use your default workspace. To specify:

1. Get workspace ID from Postman
2. Add to `.env`: `POSTMAN_WORKSPACE_ID=your-workspace-id`

Or list workspaces in Cursor:
```
List all my workspaces using Postman tools
```

---

## Features

### ✅ What You Get

- 🎯 **IDE Integration** - Tools available in your coding environment
- 🤖 **Smart Context** - Cursor understands your project
- 📝 **Code Generation** - Generate and save client code
- 🔄 **Automatic Execution** - Scripts run seamlessly
- 📊 **Complete Coverage** - All 8 API lifecycle phases
- 🛠️ **18 Powerful Tools** - Full Postman API management

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

## Advanced Usage

### Inline Tool Usage

While coding, you can use Cursor Composer inline:

```python
# In your Python file, select this comment and use Composer:
# "Use Postman tools to generate Python code for my Payment API"

# Cursor will generate the client code right here
```

---

### Chaining Operations

Ask Cursor to perform multiple steps:

```
Use Postman tools to:
1. Create a collection called "User API v2"
2. Create a development environment
3. Create a mock server
4. Run security audit
```

Cursor executes all operations in sequence.

---

### Code Integration

Generate and integrate code directly:

```
Generate a Python client for my Payment API collection and save it to clients/payment.py
```

Cursor will:
1. Run code generation script
2. Create the directory if needed
3. Save the generated code
4. Format it properly

---

### Documentation Generation

Ask Cursor to document APIs:

```
Use Postman tools to analyze my User API and generate markdown documentation
```

---

## Troubleshooting

### Cursor Doesn't Recognize Tools

**Issue**: Cursor doesn't understand Postman tool references

**Solutions**:
1. Ensure `.cursorrules` exists in repository root
2. Close and reopen the project in Cursor
3. Verify file isn't gitignored
4. Check Cursor settings → ensure rules are enabled

---

### API Key Not Found

**Issue**: "API key not found" error

**Solutions**:
1. Verify `.env` exists in repository root
2. Check `POSTMAN_API_KEY` is set correctly
3. API key should start with `PMAK-`
4. No extra spaces or quotes around the key

---

### Permission Denied

**Issue**: "Permission denied" when running scripts

**Solution**:
```bash
chmod +x scripts/*.py
```

---

### Scripts Not Executing

**Issue**: Cursor shows output but scripts don't run

**Solutions**:
1. Verify Python is in PATH: `which python3`
2. Ensure dependencies installed: `pip install -r requirements.txt`
3. Check terminal working directory
4. Try running script manually to debug

---

## Cursor-Specific Tips

### 1. Use Composer for Tool Operations

The Composer panel (Cmd/Ctrl + I) is best for Postman operations:
- Clear intent communication
- Better context handling
- Automatic script execution

---

### 2. Reference in Chat

You can also use regular Chat:
```
@workspace Use Postman tools to list collections
```

The `@workspace` tag gives Cursor full project context.

---

### 3. Combine with Code Generation

Great for creating API clients:
```
Generate a TypeScript client for my Payment API using the Postman collection
```

Cursor will:
1. Use Postman tools to fetch collection
2. Generate TypeScript code
3. Add proper types and interfaces

---

### 4. Multi-File Operations

Cursor can work across files:
```
Use Postman tools to generate Python clients for all my collections and save them in the clients/ directory with proper imports
```

---

### 5. Quick Actions

Set up custom keyboard shortcuts for common operations:
- List collections
- Run tests
- Generate code
- Security audit

---

## Best Practices

### 1. Be Explicit with Tool Names

Good:
```
Use Postman tools to list collections
```

Better than:
```
Show my collections
```

Explicitly mentioning "Postman tools" helps Cursor understand intent.

---

### 2. Provide Context

When referencing resources:
```
Use Postman tools to audit the security of my "User Authentication" collection in the Production workspace
```

More context = better results.

---

### 3. Verify Before Deletion

Always confirm before destructive operations:
```
Use Postman tools to show me collection abc123 details before I delete it
```

---

### 4. Save Generated Code

When generating code, specify save location:
```
Generate Python code for Payment API and save to clients/payment_api.py
```

---

## Integration Patterns

### Pattern 1: Development Workflow

```
1. Use Postman tools to list my collections
2. Generate Python code for the Payment API collection
3. Save to src/clients/payment.py
4. Create unit tests for the client
```

---

### Pattern 2: Testing Workflow

```
1. Use Postman tools to run Payment API tests with Staging environment
2. If tests fail, show me the detailed error report
3. Generate a summary of failures
```

---

### Pattern 3: Security Workflow

```
1. Use Postman tools to audit all collections
2. Generate a security report
3. Create GitHub issues for critical findings
```

---

### Pattern 4: Documentation Workflow

```
1. Use Postman tools to fetch my User API collection
2. Generate markdown documentation with examples
3. Save to docs/api/user-api.md
```

---

## Next Steps

### Learn More
- 📖 [Complete Tool Reference](../tools.json) - All available tools
- 📊 [API Lifecycle Coverage](../API_LIFECYCLE_COVERAGE.md) - Full capabilities
- 🔧 [Direct CLI Usage](../README.md#direct-script-execution) - Run scripts manually

### Explore Other Agents
- [Claude Code Setup Guide](SETUP_CLAUDE_CODE.md) - Use with Claude Code
- [Gemini Setup Guide](SETUP_GEMINI.md) - Use with Google Gemini
- [Multi-Agent Setup](MULTI_AGENT_SETUP.md) - All integration options

### Advanced Topics
- Custom Cursor rules
- Keyboard shortcuts
- Team workflows
- CI/CD integration

---

## Support

- **Issues**: Report at GitHub Issues
- **Cursor Docs**: [cursor.com/docs](https://cursor.com/docs)
- **Postman Docs**: [Postman API Documentation](https://www.postman.com/postman/workspace/postman-public-workspace/documentation/12959542-c8142d51-e97c-46b6-bd77-52bb66712c9a)

---

**Enjoy using Postman with Cursor!** 🚀
