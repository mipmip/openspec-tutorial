# Chapter 3: Core Commands

## Two Types of Commands

OpenSpec commands are divided into **CLI commands** and **AI-native commands**, and they are used in completely different ways:

| Type | How to Use | Example |
|------|-----------|---------|
| **CLI commands** | Run in the terminal | `openspec new change xxx` |
| **AI-native commands** | Use in AI tool conversations | `/opsx:propose "xxx"` |

> **Important**: The tutorial will clearly label the type of each command. If you try to run an AI-native command (like `/opsx:propose`) in the terminal, you'll get a "command not found" error.

---

## CLI Command Overview

Commands used in the terminal:

| Command | Description | Type |
|---------|-------------|------|
| `openspec init` | Initialize a project | CLI |
| `openspec update` | Update OpenSpec files | CLI |
| `openspec new change <name>` | Create a new change | CLI |
| `openspec list` | List all changes | CLI |
| `openspec show <name>` | Show change details | CLI |
| `openspec archive [name]` | Archive a change | CLI |
| `openspec validate [name]` | Validate a change/spec | CLI |
| `openspec status` | View change completion status | CLI |
| `openspec view` | Interactive dashboard | CLI |
| `openspec config ...` | View/modify configuration | CLI |
| `openspec change show <name>` | Show proposal details | CLI |
| `openspec change validate <name>` | Validate a proposal | CLI |

---

## CLI: openspec init

Initialize an OpenSpec project:

```bash
openspec init [options] [path]
```

### Common Options

| Option | Description |
|--------|-------------|
| `--tools <tools>` | Specify AI tools (comma-separated), e.g., `claude,cursor` |
| `--profile <profile>` | Specify a workflow preset, e.g., `core` |
| `--force` | Auto-clean old files without prompting |

### Examples

```bash
# Initialize the current directory
openspec init

# Specify AI tools
openspec init --tools claude,cursor,github-copilot

# Specify a workflow preset
openspec init --profile core

# Force overwrite (no prompts)
openspec init --force
```

---

## CLI: openspec new

Create a new Change:

```bash
openspec new change <name>
```

### Examples

```bash
# Create a change directory
openspec new change "add-readme"
openspec new change add-user-login
openspec new change fix-mobile-layout
```

> Note: Change names should use kebab-case (hyphen-separated).

---

## CLI: openspec list

List changes in the project:

```bash
openspec list [options]
```

### Common Options

| Option | Description |
|--------|-------------|
| `--specs` | List specs instead of changes |
| `--json` | Output in JSON format (for programmatic use) |
| `--sort <order>` | Sort order: `recent` (default) or `name` |

### Examples

```bash
# List all in-progress changes
openspec list

# List all specs
openspec list --specs

# Output in JSON format
openspec list --json

# Sort by name
openspec list --sort name
```

---

## CLI: openspec show

Show detailed information about a change or spec:

```bash
openspec show [item-name] [options]
```

### Common Options

| Option | Description |
|--------|-------------|
| `--json` | Output in JSON format |
| `--type <type>` | Specify type: `change` or `spec` |
| `--no-interactive` | Disable interactive prompts |
| `--deltas-only` | Show only deltas (JSON format) |

### Examples

```bash
# Show change details
openspec show add-user-login

# Output in JSON format
openspec show add-user-login --json

# Show spec details
openspec show my-spec --type spec
```

---

## CLI: openspec archive

Archive a completed change:

```bash
openspec archive [change-name] [options]
```

### Common Options

| Option | Description |
|--------|-------------|
| `-y, --yes` | Skip confirmation prompt |
| `--skip-specs` | Skip spec updates (archive only) |
| `--no-validate` | Skip validation (not recommended) |

### Examples

```bash
# Archive a change (will prompt for confirmation)
openspec archive add-user-login

# Skip confirmation
openspec archive add-user-login --yes

# Skip spec updates (archive only)
openspec archive add-user-login --skip-specs

# Archive and skip validation
openspec archive add-user-login --yes --no-validate
```

### What the AI Does

During archiving, OpenSpec will:
1. Check that all tasks are complete
2. Update the main spec documents
3. Move the change to the `archive/` directory
4. Generate a change summary (summary.md)

---

## CLI: openspec validate

Validate a change or spec:

```bash
openspec validate [item-name] [options]
```

### Common Options

| Option | Description |
|--------|-------------|
| `--all` | Validate all changes and specs |
| `--changes` | Validate all changes |
| `--specs` | Validate all specs |
| `--strict` | Enable strict validation mode |
| `--json` | Output in JSON format |
| `--type <type>` | Specify type |

### Examples

```bash
# Validate a single change
openspec validate add-user-login

# Validate all changes
openspec validate --changes

# Validate everything (changes + specs)
openspec validate --all

# Strict mode
openspec validate add-user-login --strict
```

---

## CLI: openspec status

View the task completion status of changes:

```bash
openspec status [options]
```

### Common Options

| Option | Description |
|--------|-------------|
| `--change <id>` | View status of a specific change |
| `--schema <name>` | Override the default schema |
| `--json` | Output in JSON format |

### Examples

```bash
# View current change status
openspec status

# View status of a specific change
openspec status --change add-user-login

# Output JSON
openspec status --change add-user-login --json
```

---

## CLI: openspec view

Interactive dashboard to view the status of all changes and specs:

```bash
openspec view
```

---

## CLI: openspec config

View and modify OpenSpec configuration:

```bash
openspec config [command] [options]
```

### Subcommands

| Command | Description |
|---------|-------------|
| `openspec config path` | Show config file path |
| `openspec config list` | List all settings |
| `openspec config get <key>` | Get a specific config value |
| `openspec config set <key> <value>` | Set a config value |
| `openspec config unset <key>` | Delete a config (restore default) |
| `openspec config reset` | Reset all settings |
| `openspec config edit` | Open config in editor |
| `openspec config profile [preset]` | Select a workflow preset |

### Examples

```bash
# View config file path
openspec config path

# List all settings
openspec config list

# Get a specific config value
openspec config get profile

# Set a config value
openspec config set profile core

# Select a workflow preset
openspec config profile
openspec config profile core
```

---

## CLI: openspec update

Update OpenSpec instruction files (e.g., AGENTS.md):

```bash
openspec update [options] [path]
```

### Common Options

| Option | Description |
|--------|-------------|
| `--force` | Force update even if tools are already up to date |

### Examples

```bash
# Update the current directory
openspec update

# Force update
openspec update --force
```

---

## CLI: openspec instructions

Output detailed instructions for creating an artifact or executing a task:

```bash
openspec instructions [artifact] [options]
```

### Common Options

| Option | Description |
|--------|-------------|
| `--change <id>` | Specify a change |
| `--schema <name>` | Specify a schema |
| `--json` | Output in JSON format |

### Examples

```bash
# Output detailed instructions for a change
openspec instructions --change add-user-login

# Specify a schema
openspec instructions --change add-user-login --schema core
```

---

## AI-Native Command Overview

> **Important**: These are NOT CLI commands -- they are commands you use in AI tool conversations (e.g., Claude Code, Cursor). Their format is `/opsx:xxx`.

AI-native commands are injected into the AI tool's context by OpenSpec via the `AGENTS.md` file. After initializing a project, these commands automatically appear in the AI's instructions.

### Command List

| Command | Description |
|---------|-------------|
| `/opsx:propose <description>` | Create a new feature proposal |
| `/opsx:apply` | Apply the implementation |
| `/opsx:archive` | Archive a completed change |
| `/opsx:verify` | Verify that implementation matches the spec |
| `/opsx:sync` | Sync specs with code state |
| `/opsx:onboard` | New member project onboarding |

---

## AI-Native: /opsx:propose

**The most important command** -- used to create new feature proposals.

### Basic Usage

```
/opsx:propose "what you want to do"
```

### Examples

```
# Add a new feature
/opsx:propose "Add user registration feature"

# Fix an issue
/opsx:propose "Fix login page display issues on mobile"

# Refactor code
/opsx:propose "Refactor user module into a separate service layer"

# Performance optimization
/opsx:propose "Optimize homepage load speed, target LCP < 2.5s"
```

### What the AI Does

After executing the command, the AI will:

1. **Analyze your description** and understand the requirements
2. **Create the change directory using CLI**: `openspec new change <change-name>`
3. **Generate four documents**:
   - `proposal.md` - Proposal description
   - `specs/requirements.md` - Detailed requirements
   - `design.md` - Technical design
   - `tasks.md` - Implementation tasks
4. **Ask you to confirm** -- you can request changes

### Review and Modify

After the AI generates the documents, you should:

```
✓ Read proposal.md -- confirm the goals are correct
✓ Read specs/ -- confirm the requirements are complete
✓ Read design.md -- confirm the technical approach is sound
✓ Read tasks.md -- confirm the task list is complete

If there are issues, tell the AI directly:
"tasks.md is missing unit testing tasks"
"Change the database approach in design.md to PostgreSQL"
```

---

## AI-Native: /opsx:apply

After confirming the specs, tell the AI to start implementing.

### Basic Usage

```
/opsx:apply
```

### What the AI Does

1. **Reads tasks.md** to understand all tasks
2. **Implements each task** in order
3. **Updates progress** in real time
4. **Reports results** when finished

### During Implementation

If the AI encounters issues, it will:
- Pause and ask you
- Provide several options for you to choose from
- Log the issue in tasks.md

---

## AI-Native: /opsx:archive

After a feature is complete, archive the change record.

### Basic Usage

```
/opsx:archive
```

The AI will call the CLI archive command to complete the archiving.

---

## AI-Native: /opsx:verify

Verify that the implementation matches the spec.

### Basic Usage

```
/opsx:verify
```

The AI will:
1. Compare the requirements in specs/
2. Check the code implementation
3. Report any discrepancies

### Example Output

```
✓ Phone number login - Implemented
✓ Verification code 5-minute expiry - Implemented
✗ WeChat login - Not implemented (tasks.md 1.4 incomplete)
⚠ Login failure attempt limit - Spec requires 5 attempts, code implements 3
```

---

## AI-Native: /opsx:sync

Sync spec documents with the current code state.

### Basic Usage

```
/opsx:sync
```

Use cases:
- You manually modified the code and need to update the specs
- Spec documents have drifted from the code

---

## AI-Native: /opsx:onboard

Generate a project overview for new team members.

### Basic Usage

```
/opsx:onboard
```

The AI will generate:
- Project background introduction
- List of completed features (from archive/)
- Features in progress (from changes/)
- Tech stack and architecture description

---

## Command Usage Tips

### 1. Don't Confuse CLI and AI-Native Commands

```bash
# ❌ Running AI-native commands in the terminal -- will fail
/opsx:propose "Add login feature"
/opsx:apply
/opsx:archive

# ✅ CLI commands go in the terminal
openspec new change add-login
openspec list
openspec archive add-login

# ✅ AI-native commands go in the AI conversation
/opsx:propose "Add login feature"
/opsx:apply
/opsx:archive
```

### 2. Be Specific in Your Descriptions

```bash
# ❌ Too vague (AI-native command example)
/opsx:propose "Improve user experience"

# ✅ Specific and clear
/opsx:propose "Add email verification to the user registration flow -- users must click a link in the email to activate their account after signing up"
```

### 3. One Thing at a Time

```bash
# ❌ Too much at once
/opsx:propose "Add login, registration, password recovery, password change, and phone number binding features"

# ✅ Split into separate changes
/opsx:propose "Add user login feature"
# After completing...
/opsx:propose "Add user registration feature"
# After completing...
/opsx:propose "Add password recovery feature"
```

### 4. Review Specs Carefully Before Apply

```
Spec documents are the "contract" between you and the AI.
Always confirm the specs are what you want before running apply.
Modifying specs is much easier than modifying code.
```

### 5. Keep the Context Clean

```bash
# Before starting a new feature, clean the AI's context
# to prevent old conversations from affecting the new feature
```

---

## Next

→ [Chapter 4: Complete Workflow](04-workflow-en.md)
