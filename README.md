# Postman API Lifecycle Management Tools

A universal toolkit for managing your complete Postman API lifecycle. Works with **Claude Code**, **Cursor**, **Gemini**, and other AI coding agents, or as standalone CLI tools.

## 🌐 Multi-Agent Compatibility

✅ **Claude Code** - Slash command with natural language interface
✅ **Cursor** - Auto-detected composer rules
✅ **Gemini** - JSON manifest integration
✅ **Direct CLI** - Universal command-line access
🚧 **MCP Server** - Planned for universal agent compatibility

📖 **[Multi-Agent Setup Guide](docs/MULTI_AGENT_SETUP.md)** - Choose your integration method

## Features

- 📋 **List & Search** - Browse all your Postman collections, environments, monitors, and APIs
- 🔨 **Collection Management** - Create, update, delete, and duplicate collections
- 🌍 **Environment Management** - Manage environment variables across workspaces
- 📊 **Monitor Analysis** - Check API health and analyze monitor runs
- 🧪 **Test Execution** - Run collection tests with environments
- 🔄 **Retry Logic** - Built-in retry handling for rate limits and network issues

## Prerequisites

- **Python 3.7+** - Pre-installed on most systems
- **Postman API Key** - Generate from your [Postman account settings](https://go.postman.co/settings/me/api-keys)
- **AI Agent** (Optional) - Claude Code, Cursor, Gemini, or any other coding assistant
  - See [Multi-Agent Setup Guide](docs/MULTI_AGENT_SETUP.md) for agent-specific installation

**Note**: All tools work as standalone CLI scripts without any AI agent.

## Installation

### 1. Clone or Download

```bash
cd ~/your-projects-directory
git clone <repository-url> postman-slash-command
cd postman-slash-command
```

Or download and extract the ZIP file to your desired location.

### 2. Install Dependencies (Optional)

```bash
pip install -r requirements.txt
```

Note: `python-dotenv` is optional - the scripts have a fallback parser for `.env` files.

### 3. Configure Your API Key

```bash
cp .env.example .env
```

Edit `.env` and add your Postman API key:

```env
POSTMAN_API_KEY=PMAK-your-actual-api-key-here
```

### 4. Install the Slash Command

Copy the `.claude` directory to your project:

```bash
# Option 1: For global access (recommended)
cp -r .claude ~/

# Option 2: For project-specific access
cp -r .claude /path/to/your/project/
```

Alternatively, you can manually copy the slash command file:

```bash
mkdir -p ~/.claude/commands
cp .claude/commands/postman.md ~/.claude/commands/
```

### 5. Verify Installation

Open Claude Code and type `/postman` - you should see the command autocomplete.

## Usage

### In Claude Code

Once installed, use the `/postman` command to access Postman functionality:

```
/postman
```

Then ask Claude to perform operations like:

- "List all my collections"
- "Create a new collection called 'Payment API Tests'"
- "Show me the details of collection abc123"
- "Run tests for the 'User Authentication' collection"
- "How are my monitors doing?"
- "Create a new environment for staging"

### Direct Script Execution

You can also run scripts directly from the command line:

```bash
# List all resources
python scripts/list_collections.py --all

# Create a collection
python scripts/manage_collections.py --create --name="My API"

# Run collection tests
python scripts/run_collection.py --collection="My Collection"

# Analyze monitors
python scripts/manage_monitors.py --analyze <monitor-id>
```

## Available Scripts

### Core Scripts

| Script | Purpose |
|--------|---------|
| `list_collections.py` | List collections, environments, monitors, and APIs |
| `manage_collections.py` | Full CRUD operations for collections |
| `manage_environments.py` | Full CRUD operations for environments |
| `manage_monitors.py` | Monitor management and analysis |
| `run_collection.py` | Execute collection test runs |

### API Workflow Scripts (Generic)

| Script | Purpose |
|--------|---------|
| `manage_api.py` | Create and manage APIs with OpenAPI specs (legacy API Builder) |
| `manage_spec.py` | Create and manage specifications in Spec Hub (recommended) |
| `manage_collection_workflow.py` | Collection-based workflows with import/export |
| `validate_schema.py` | Validate OpenAPI specifications for correctness |
| `detect_breaking_changes.py` | Detect breaking changes between API versions |
| `generate_code.py` | Generate code snippets in multiple languages |
| `manage_mocks.py` | Create and manage mock servers for API simulation |
| `audit_security.py` | Security auditing for APIs, collections, and specs |
| `publish_docs.py` | Publish documentation and generate changelogs |

Run any script with `--help` to see all available options.

## Configuration

### Environment Variables

Create a `.env` file in the `postman-slash-command` directory:

```env
# Required
POSTMAN_API_KEY=PMAK-your-key-here

# Optional
POSTMAN_BASE_URL=https://api.postman.com
POSTMAN_TIMEOUT=30
POSTMAN_MAX_RETRIES=3
```

### Workspace Configuration

By default, scripts use your default workspace. To specify a workspace:

```bash
python scripts/list_collections.py --workspace="My Workspace"
```

Or set it in your `.env`:

```env
POSTMAN_WORKSPACE_ID=your-workspace-id
```

## Examples

### Example 1: Create and Populate a Collection

```bash
# Create collection
python scripts/manage_collections.py --create --name="E-commerce API"

# Get the collection ID from output, then add requests via Postman UI
# or import from OpenAPI spec
```

### Example 2: Monitor Health Check

```bash
# List all monitors
python scripts/manage_monitors.py --list

# Analyze specific monitor
python scripts/manage_monitors.py --analyze abc123 --limit 10
```

### Example 3: Environment Setup

```bash
# Create development environment
python scripts/manage_environments.py --create --name="Development"

# Duplicate for staging
python scripts/manage_environments.py --duplicate <env-id> --name="Staging"
```

### Example 4: Create API from OpenAPI Spec (Spec Hub - Recommended)

```bash
# Create specification in Spec Hub
python scripts/manage_spec.py create --name="Payment API" --file=openapi.json

# Generate collection from spec
python scripts/manage_spec.py generate-collection --spec-id=<spec-id> --collection-name="Payment API Collection"

# List all specs
python scripts/manage_spec.py list
```

### Example 5: Import Collection from OpenAPI

```bash
# Import OpenAPI spec as a collection
python scripts/manage_collection_workflow.py import --name="User API" --spec-file=openapi.json

# Compare two collection versions
python scripts/manage_collection_workflow.py compare --id1=<old-id> --id2=<new-id>
```

### Example 6: Create API with Versions (Legacy API Builder)

```bash
# Create API with schema
python scripts/manage_api.py --name="Order API" --spec-file=openapi.json

# List all APIs
python scripts/manage_api.py --list

# Get API details
python scripts/manage_api.py --get <api-id>
```

### Example 7: Mock Server Management

```bash
# List all mock servers
python scripts/manage_mocks.py --list

# Create a mock server from collection
python scripts/manage_mocks.py --create --name="Payment Mock" --collection=<collection-id>

# Create private mock with delay
python scripts/manage_mocks.py --create --name="Test Mock" --collection=<collection-id> --private --delay=1000

# Get mock details
python scripts/manage_mocks.py --get <mock-id>

# Delete mock server
python scripts/manage_mocks.py --delete <mock-id>
```

### Example 8: Security Auditing

```bash
# Audit a specific API
python scripts/audit_security.py --api <api-id>

# Audit a collection
python scripts/audit_security.py --collection <collection-id>

# Audit a Spec Hub specification
python scripts/audit_security.py --spec <spec-id>

# Audit all collections in workspace
python scripts/audit_security.py --all-collections

# Audit all APIs in workspace
python scripts/audit_security.py --all-apis
```

### Example 9: Documentation Publishing

```bash
# Publish collection documentation
python scripts/publish_docs.py --collection <collection-id> --publish

# Check documentation status
python scripts/publish_docs.py --collection <collection-id> --status

# Generate changelog between versions
python scripts/publish_docs.py --compare --old <old-collection-id> --new <new-collection-id>

# Generate API version changelog
python scripts/publish_docs.py --changelog --api <api-id>
```

### Example 10: Schema Validation

```bash
# Validate API schema
python scripts/validate_schema.py --api <api-id>

# Validate Spec Hub specification
python scripts/validate_schema.py --spec <spec-id>

# Validate local OpenAPI file
python scripts/validate_schema.py --file openapi.json
```

### Example 11: Breaking Change Detection

```bash
# Detect breaking changes between API versions
python scripts/detect_breaking_changes.py --api <api-id> --old-version <v1-id> --new-version <v2-id>

# Compare two specs
python scripts/detect_breaking_changes.py --spec <spec-id-1> <spec-id-2>

# Compare two collections
python scripts/detect_breaking_changes.py --collection <col-id-1> <col-id-2>

# Compare local files
python scripts/detect_breaking_changes.py --file old.json new.json
```

### Example 12: Code Generation

```bash
# Generate Python code for all requests
python scripts/generate_code.py --collection <collection-id> --language python

# Generate code in all languages
python scripts/generate_code.py --collection <collection-id> --all

# Generate curl commands only
python scripts/generate_code.py --collection <collection-id> --language curl

# Available languages: curl, python, javascript, nodejs, go
```

## Troubleshooting

### "API key not found"
- Ensure `.env` exists in the `postman-slash-command` directory
- Verify `POSTMAN_API_KEY` is set correctly
- API key should start with `PMAK-`

### "curl: command not found"
- Install curl: `brew install curl` (macOS) or `apt-get install curl` (Linux)

### "Permission denied"
- Make scripts executable: `chmod +x scripts/*.py`

### Rate Limiting
- Scripts automatically retry with exponential backoff
- Reduce concurrent requests if you hit rate limits frequently

## Architecture

```
postman-slash-command/
├── .claude/
│   └── commands/
│       └── postman.md                    # Slash command definition
├── scripts/
│   ├── postman_client.py                 # Core API client with curl
│   ├── config.py                         # Configuration management
│   ├── list_collections.py               # Discovery operations
│   ├── manage_collections.py             # Collection CRUD
│   ├── manage_environments.py            # Environment CRUD
│   ├── manage_monitors.py                # Monitor management
│   ├── manage_mocks.py                   # Mock server management
│   ├── run_collection.py                 # Test execution
│   ├── manage_api.py                     # Generic API management (legacy)
│   ├── manage_spec.py                    # Generic Spec Hub management
│   ├── manage_collection_workflow.py     # Generic collection workflows
│   ├── validate_schema.py                # OpenAPI schema validation
│   ├── detect_breaking_changes.py        # Breaking change detection
│   ├── generate_code.py                  # Code snippet generation
│   ├── audit_security.py                 # Security auditing
│   ├── publish_docs.py                   # Documentation publishing
│   └── validate_setup.py                 # Setup validation
├── utils/
│   ├── exceptions.py                     # Error handling
│   ├── formatters.py                     # Output formatting
│   └── retry_handler.py                  # Retry logic
├── .env.example                          # Configuration template
├── .gitignore                            # Git ignore rules
├── requirements.txt                      # Python dependencies
└── README.md                             # This file
```

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

[Your License Here]

## Support

- **Issues**: Report bugs or request features via GitHub Issues
- **Documentation**: Full API reference at [Postman API Docs](https://www.postman.com/postman/workspace/postman-public-workspace/documentation/12959542-c8142d51-e97c-46b6-bd77-52bb66712c9a)
- **Community**: Join the Postman Community at [community.postman.com](https://community.postman.com)

## Acknowledgments

Built for [Claude Code](https://claude.ai/claude-code) by Anthropic.
Powered by the [Postman API](https://www.postman.com/postman/workspace/postman-public-workspace/api).
