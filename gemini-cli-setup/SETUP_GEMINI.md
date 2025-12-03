# Google Gemini Setup Guide

Complete setup instructions for using Postman API Lifecycle Management tools with Google Gemini Code Assist.

---

## Overview

Gemini integration uses the **`tools.json` manifest** as a reference guide. You provide the manifest in your conversation context, and Gemini understands all available Postman tools and how to use them.

---

## Prerequisites

- ✅ **Google Gemini** access - Available at [gemini.google.com](https://gemini.google.com) or via IDE extensions
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

### Step 4: Copy the Tools Manifest

Open `tools.json` in your editor and copy its contents. You'll provide this to Gemini in your first message.

**Location**: `/Users/sterling.chin@postman.com/work/postman-slash-command/tools.json`

---

## Usage

### Basic Workflow

1. **First Message**: Provide tools.json context
2. **Subsequent Messages**: Reference Postman operations
3. **Gemini**: Executes appropriate Python scripts

---

### Method 1: Full Context (Recommended)

**Your first message to Gemini:**

```
I have Postman API lifecycle management tools. Here's the complete manifest:

[Paste entire contents of tools.json]

I want to use these tools to manage my Postman workspace. The scripts are located in the `scripts/` directory of my project.

Please help me list all my collections.
```

**What Gemini does:**
1. Reads and understands the tools.json manifest
2. Identifies the correct script: `list_collections.py`
3. Executes: `python scripts/list_collections.py --all`
4. Shows you the results

---

### Method 2: Quick Reference

**Your first message to Gemini:**

```
I have Postman management tools. Here's a quick reference:

- list_collections.py - List all resources
- manage_collections.py - Collection CRUD
- run_collection.py - Run tests
- generate_code.py - Generate code snippets
- audit_security.py - Security auditing
- manage_mocks.py - Mock servers
- and 12 more tools

Full manifest available at: tools.json

Help me list all my Postman collections.
```

**What Gemini does:**
1. Uses your quick reference
2. Asks for more details if needed
3. Executes the appropriate script

---

## Example Conversations

### Example 1: Discovery

**You:**
```
I have Postman tools (see tools.json manifest I provided earlier).

List all my collections.
```

**Gemini:**
```python
python scripts/list_collections.py --all
```

---

### Example 2: Create Collection

**You:**
```
Create a new Postman collection called "Payment API v2"
```

**Gemini:**
```python
python scripts/manage_collections.py --create --name="Payment API v2"
```

---

### Example 3: Run Tests

**You:**
```
Run tests for my "User Authentication" collection using the "Production" environment
```

**Gemini:**
```python
python scripts/run_collection.py --collection="User Authentication" --environment="Production"
```

---

### Example 4: Code Generation

**You:**
```
Generate Python code for all requests in my Payment API collection
```

**Gemini:**
```python
# First, Gemini helps you find the collection ID
python scripts/list_collections.py --all

# Then generates code
python scripts/generate_code.py --collection <collection-id> --language python
```

---

### Example 5: Security Audit

**You:**
```
Run a security audit on all my collections
```

**Gemini:**
```python
python scripts/audit_security.py --all-collections
```

---

### Example 6: Mock Server

**You:**
```
Create a mock server for my Payment API collection with a 500ms delay
```

**Gemini:**
```python
# First, get collection ID
python scripts/list_collections.py --all

# Create mock with delay
python scripts/manage_mocks.py --create --name="Payment Mock" --collection=<id> --delay=500
```

---

### Example 7: Breaking Changes

**You:**
```
Detect breaking changes between my old and new API versions
```

**Gemini:**
```python
python scripts/detect_breaking_changes.py --api <api-id> --old-version <v1> --new-version <v2>
```

---

## Advanced Usage

### Persistent Context

To avoid re-pasting tools.json in every conversation:

**Option 1: Use Gemini Code Assist in IDE**

If you're using Gemini integrated in VS Code or other IDEs:
1. Keep tools.json open in a tab
2. Gemini can reference it automatically
3. Just say: "Reference the tools.json file"

**Option 2: Create a Custom Instruction**

In Gemini settings, add custom instruction:
```
When I mention "Postman tools", reference the manifest at:
/Users/sterling.chin@postman.com/work/postman-slash-command/tools.json

Available tools include: list_collections, manage_collections, run_collection,
generate_code, audit_security, manage_mocks, and 12 more.
```

---

### Chaining Operations

Ask Gemini to perform multiple operations:

**You:**
```
Using my Postman tools:
1. List all collections
2. Find the "Payment API" collection
3. Generate Python code for it
4. Save to clients/payment.py
```

**Gemini:**
```bash
# List collections
python scripts/list_collections.py --all

# Generate code (using collection ID from step 1)
python scripts/generate_code.py --collection <id> --language python

# Gemini then saves output to clients/payment.py
```

---

### Exploratory Workflows

**You:**
```
I want to understand my Postman workspace security posture. What can I do with the tools?
```

**Gemini:**
```
Based on the tools manifest, you can:

1. Run comprehensive security audits:
   python scripts/audit_security.py --all-collections

2. Validate OpenAPI schemas:
   python scripts/validate_schema.py --api <api-id>

3. Check for breaking changes:
   python scripts/detect_breaking_changes.py --collection <old> <new>

Would you like me to run any of these?
```

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

### Tool Discovery

Gemini uses tools.json to understand:

1. **Available Scripts** - All 18 tools
2. **Script Purpose** - What each tool does
3. **Parameters** - Required and optional arguments
4. **Examples** - How to use each tool
5. **Lifecycle Mapping** - Which phase each tool belongs to

---

## Features

### ✅ What You Get

- 📖 **Full Documentation** - Complete tool manifest
- 🤖 **Intelligent Execution** - Gemini maps intent to scripts
- 🔄 **Flexible Integration** - Works in web or IDE
- 📊 **Complete Coverage** - All 8 API lifecycle phases
- 🛠️ **18 Powerful Tools** - Full Postman API management
- 💬 **Natural Language** - Describe what you want

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

### Gemini Doesn't Find Scripts

**Issue**: Gemini says it can't find the scripts

**Solutions**:
1. Provide full file paths in your request
2. Ensure you're in the correct directory: `cd /Users/sterling.chin@postman.com/work/postman-slash-command`
3. Verify scripts directory exists: `ls scripts/`

---

### API Key Not Found

**Issue**: "API key not found" error

**Solutions**:
1. Verify `.env` exists in repository root
2. Check `POSTMAN_API_KEY` is set correctly
3. API key should start with `PMAK-`
4. No extra spaces or quotes

---

### Context Lost

**Issue**: Gemini forgets tools.json context

**Solutions**:
1. Re-paste tools.json in conversation
2. Use IDE integration with file open
3. Add to custom instructions
4. Reference specific tool by name: "Use the list_collections.py script"

---

### Permission Denied

**Issue**: "Permission denied" when running scripts

**Solution**:
```bash
chmod +x scripts/*.py
```

---

## Gemini-Specific Tips

### 1. Be Explicit with File Paths

Gemini works best with full paths:

Good:
```
Run python scripts/list_collections.py --all
```

Less reliable:
```
List my collections
```

---

### 2. Reference the Manifest

When context seems lost:
```
According to the tools.json manifest I shared, which tool should I use to...?
```

---

### 3. Ask for Explanations

Gemini can explain tools before running:
```
What does the audit_security.py script do? Show me the relevant section from tools.json
```

---

### 4. Use Multi-Step Instructions

Break complex tasks into steps:
```
Step 1: List my collections
Step 2: For each collection, run a security audit
Step 3: Summarize the results
```

---

### 5. Validate Commands

Ask Gemini to show commands before execution:
```
Show me the exact command you'll run, then wait for my confirmation before executing
```

---

## Best Practices

### 1. Start Each Session with Context

First message should always include tools reference:
```
I'm working with Postman API management tools. Reference: tools.json (previously shared).

Now, help me [your task]
```

---

### 2. Keep tools.json Accessible

- Keep file open in editor
- Bookmark the location
- Save a snippet with quick reference
- Create custom instruction in Gemini

---

### 3. Verify Before Destructive Operations

Always confirm:
```
Show me what will be deleted before running the delete command
```

---

### 4. Use Explicit Tool Names

When possible, reference tools directly:
```
Use the manage_mocks.py script to create a mock server
```

More reliable than:
```
Create a mock server
```

---

## Integration Patterns

### Pattern 1: Discovery → Action

```
1. List all my collections (list_collections.py)
2. Show details of "Payment API" collection
3. Generate Python code for it
4. Save to file
```

---

### Pattern 2: Validation → Deployment

```
1. Validate my OpenAPI spec (validate_schema.py)
2. If valid, create API (manage_api.py)
3. Generate collection (manage_spec.py)
4. Create mock server (manage_mocks.py)
```

---

### Pattern 3: Audit → Report

```
1. Run security audit on all collections (audit_security.py)
2. Summarize critical findings
3. Create action items
4. Save report to markdown
```

---

### Pattern 4: Test → Monitor

```
1. Run collection tests (run_collection.py)
2. If tests pass, create monitor (manage_monitors.py)
3. Analyze initial run
4. Report results
```

---

## Tools Quick Reference

For easy copy-paste into Gemini conversations:

```
Postman Tools Quick Reference:

Discovery:
- list_collections.py --all
- list_workspaces.py

Collections:
- manage_collections.py [--list|--get|--create|--update|--delete|--duplicate]
- manage_collection_workflow.py [create|import|duplicate|compare]

Environments:
- manage_environments.py [--list|--get|--create|--update|--delete|--duplicate]

Design:
- manage_api.py [--list|--get|--name|--spec-file]
- manage_spec.py [create|list|get|generate-collection]
- validate_schema.py [--api|--spec|--file]
- detect_breaking_changes.py [--api|--spec|--collection|--file]

Development:
- generate_code.py --collection <id> [--language|--all]

Testing:
- run_collection.py --collection <name-or-id> [--environment]

Security:
- audit_security.py [--api|--collection|--spec|--all-collections|--all-apis]

Deployment:
- manage_mocks.py [--list|--get|--create|--update|--delete]

Monitoring:
- manage_monitors.py [--list|--get|--create|--update|--delete|--analyze]

Documentation:
- publish_docs.py [--publish|--status|--compare|--changelog]

Full details: tools.json
```

---

## Next Steps

### Learn More
- 📖 [Complete Tool Reference](../tools.json) - All available tools (recommended to keep open)
- 📊 [API Lifecycle Coverage](../API_LIFECYCLE_COVERAGE.md) - Full capabilities
- 🔧 [Direct CLI Usage](../README.md#direct-script-execution) - Run scripts manually

### Explore Other Agents
- [Claude Code Setup Guide](SETUP_CLAUDE_CODE.md) - Use with Claude Code (better natural language)
- [Cursor Setup Guide](SETUP_CURSOR.md) - Use with Cursor IDE (auto-detected)
- [Multi-Agent Setup](MULTI_AGENT_SETUP.md) - All integration options

### Improve Your Setup
- Add custom instructions in Gemini
- Create code snippets for common operations
- Use IDE integration for better context
- Explore Gemini Code Assist features

---

## Support

- **Issues**: Report at GitHub Issues
- **Gemini Help**: [support.google.com/gemini](https://support.google.com/gemini)
- **Postman Docs**: [Postman API Documentation](https://www.postman.com/postman/workspace/postman-public-workspace/documentation/12959542-c8142d51-e97c-46b6-bd77-52bb66712c9a)

---

## Limitations

### Context Windows

Gemini has context limits. For long conversations:
- Re-paste tools.json periodically
- Use IDE integration when possible
- Reference tools.json location explicitly

### Manual Execution

Unlike Claude Code or Cursor, Gemini may:
- Show you the command instead of running it
- Require you to confirm execution
- Need explicit permission for file operations

This is actually a **safety feature** - you maintain control over what runs.

---

**Enjoy using Postman with Gemini!** 🚀
