---
description: Manage Postman collections, environments, monitors, and APIs
---

# Postman Command

Execute Postman API operations to manage your API lifecycle. This command provides access to Postman collections, environments, monitors, and more through Python scripts.

## Prerequisites

Before using this command, ensure:
1. A `.env` file exists in the `postman-slash-command/` directory with `POSTMAN_API_KEY` set
2. Python 3 is available
3. `curl` is installed (usually pre-installed on macOS and Linux)
4. Optional: `python-dotenv` for easier .env loading (has fallback if not available)

If the user hasn't set up their environment yet, guide them through:
```bash
cd postman-slash-command
cp .env.example .env
# Edit .env and add POSTMAN_API_KEY=PMAK-your-key-here
```

## Available Operations

### Discovery & Listing

**List all resources:**
```bash
python postman-slash-command/scripts/list_collections.py --all
```

**List collections only:**
```bash
python postman-slash-command/scripts/list_collections.py
```

**List environments:**
```bash
python postman-slash-command/scripts/list_collections.py --environments
```

**List monitors:**
```bash
python postman-slash-command/scripts/list_collections.py --monitors
```

**List APIs:**
```bash
python postman-slash-command/scripts/list_collections.py --apis
```

### Collection Management

**List collections:**
```bash
python postman-slash-command/scripts/manage_collections.py --list
```

**Get collection details:**
```bash
python postman-slash-command/scripts/manage_collections.py --get <collection-id>
```

**Create new collection:**
```bash
python postman-slash-command/scripts/manage_collections.py --create --name="My Collection"
```

**Update collection:**
```bash
python postman-slash-command/scripts/manage_collections.py --update <collection-id> --name="New Name"
```

**Delete collection:**
```bash
python postman-slash-command/scripts/manage_collections.py --delete <collection-id>
```

**Duplicate collection:**
```bash
python postman-slash-command/scripts/manage_collections.py --duplicate <collection-id> --name="Copy"
```

### Environment Management

**List environments:**
```bash
python postman-slash-command/scripts/manage_environments.py --list
```

**Create environment:**
```bash
python postman-slash-command/scripts/manage_environments.py --create --name="Development"
```

**Get environment details:**
```bash
python postman-slash-command/scripts/manage_environments.py --get <environment-id>
```

**Update environment:**
```bash
python postman-slash-command/scripts/manage_environments.py --update <environment-id> --name="New Name"
```

**Delete environment:**
```bash
python postman-slash-command/scripts/manage_environments.py --delete <environment-id>
```

**Duplicate environment:**
```bash
python postman-slash-command/scripts/manage_environments.py --duplicate <environment-id> --name="Copy"
```

### Monitor Management

**List monitors:**
```bash
python postman-slash-command/scripts/manage_monitors.py --list
```

**Get monitor details:**
```bash
python postman-slash-command/scripts/manage_monitors.py --get <monitor-id>
```

**Analyze monitor runs:**
```bash
python postman-slash-command/scripts/manage_monitors.py --analyze <monitor-id> --limit 10
```

### Test Execution

**Run collection tests:**
```bash
python postman-slash-command/scripts/run_collection.py --collection="Collection Name"
```

**Run with environment:**
```bash
python postman-slash-command/scripts/run_collection.py --collection="Collection Name" --environment="Environment Name"
```

## How to Respond

When the user asks about Postman operations:

1. **Understand the intent** - What do they want to do?
   - List resources? → Use list_collections.py
   - Manage collections? → Use manage_collections.py
   - Manage environments? → Use manage_environments.py
   - Check monitors? → Use manage_monitors.py
   - Run tests? → Use run_collection.py

2. **Execute the appropriate script** - Run the Python script with correct arguments

3. **Parse and present results** - Format the output in a user-friendly way

4. **Suggest next steps** - Based on what they accomplished, suggest related actions

## Examples

### Example 1: User wants to see their collections

**User:** "Show me my Postman collections"

**You should:**
1. Run: `python postman-slash-command/scripts/list_collections.py`
2. Parse the output
3. Present in a clear format
4. Ask if they want to do anything with the collections

### Example 2: User wants to create a new collection

**User:** "Create a new collection called 'Payment API Tests'"

**You should:**
1. Run: `python postman-slash-command/scripts/manage_collections.py --create --name="Payment API Tests"`
2. Confirm success
3. Show the collection ID
4. Ask if they want to add requests to it

### Example 3: User wants to check monitor status

**User:** "How is my API monitoring doing?"

**You should:**
1. First list monitors: `python postman-slash-command/scripts/manage_monitors.py --list`
2. For each monitor, analyze runs: `python postman-slash-command/scripts/manage_monitors.py --analyze <monitor-id> --limit 5`
3. Summarize the health (success rates, response times, recent failures)
4. Alert if any monitors are failing

## Error Handling

If a script fails:
1. Check if `.env` file exists and has `POSTMAN_API_KEY`
2. Verify the API key is valid (starts with `PMAK-`)
3. Check if the resource ID/name is correct
4. Provide helpful error messages to the user

## Notes

- All scripts automatically load configuration from `.env` file
- Scripts use retry logic for rate limits
- Collection/environment IDs are UUIDs (e.g., `12345678-1234-1234-1234-123456789012`)
- Monitor IDs are shorter alphanumeric strings
- Use `--help` flag on any script to see all available options
