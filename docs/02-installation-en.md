# Chapter 2: Installation & Setup

## Requirements

- **Node.js**: 20.19.0 or higher
- **npm**: 10.x or higher

Check your Node.js version:
```bash
node --version
```

If your version is too old, upgrade Node.js first:
- Official download: https://nodejs.org/
- Or use nvm/fnm for version management

---

## Install OpenSpec

### Global Installation (Recommended)

```bash
npm install -g @fission-ai/openspec@latest
```

### Using Other Package Managers

```bash
# Using pnpm
pnpm add -g @fission-ai/openspec@latest

# Using yarn
yarn global add @fission-ai/openspec@latest

# Using bun
bun add -g @fission-ai/openspec@latest
```

### Verify Installation

```bash
openspec -V
```

You should see output like: `1.2.0`

---

## Initialize a Project

### New Project

```bash
# Create a project directory
mkdir my-project
cd my-project

# Initialize OpenSpec
openspec init
```

Example output:
```
✓ Created openspec/ directory
✓ Created openspec/changes/ directory
✓ Created openspec/archive/ directory
✓ Created AGENTS.md
✓ Initialized OpenSpec in my-project/
```

### Existing Project

```bash
# Navigate to your existing project directory
cd existing-project

# Initialize (won't overwrite existing files)
openspec init
```

OpenSpec is **non-invasive** -- it won't modify your existing code.

---

## Project Structure

After initialization, your project will have:

```
my-project/
├── openspec/                 # OpenSpec directory
│   ├── changes/             # Changes in progress
│   │   └── (empty)
│   └── archive/             # Completed changes
│       └── (empty)
├── AGENTS.md                # AI assistant configuration
└── (your existing project files)
```

### AGENTS.md File

This is a configuration file for AI assistants, containing OpenSpec's AI-native command definitions.

---

## Configuring the Workflow

### View Current Configuration

```bash
openspec config list
```

### Choose a Workflow

OpenSpec supports selecting workflow modes via `--profile`:

```bash
# View available workflow presets
openspec config profile

# Apply a preset (e.g., core)
openspec init --profile core
openspec init --profile <preset-name>
```

### View All Available Settings

```bash
openspec config list
openspec config get <key>     # View a specific config value
openspec config set <key> <value>  # Set a config value
openspec config path          # View config file path
```

---

## Configuring AI Tools

### Specify AI Tools During Initialization

```bash
openspec init --tools claude,cursor,github-copilot
```

Supported AI tools (comma-separated):
`amazon-q`, `antigravity`, `auggie`, `claude`, `cline`, `codex`, `codebuddy`, `continue`, `costrict`, `crush`, `cursor`, `factory`, `gemini`, `github-copilot`, `iflow`, `kilocode`, `kiro`, `opencode`, `pi`, `qoder`, `qwen`, `roocode`, `trae`, `windsurf`

### View Change Status

```bash
# View the status of all changes in the current project
openspec status

# View the completion status of a specific change
openspec status --change <change-name>
```

---

## Your First Command

After installation, try your first command:

```bash
# In your project directory, create a new change
openspec new change "add-readme"
```

Or use an AI-native command in your AI tool:

```
/opsx:propose "Add a README file"
```

The AI will create:
```
openspec/changes/add-readme/
├── proposal.md
├── specs/
├── design.md
└── tasks.md
```

Review the generated files to understand OpenSpec's document structure.

---

## Common Issues

### Q: Installation fails with a permissions error

```bash
# macOS/Linux
sudo npm install -g @fission-ai/openspec@latest

# Or use npx (no installation needed)
npx @fission-ai/openspec@latest
```

### Q: Command not found

```bash
# Check the npm global install path
npm config get prefix

# Make sure the path is in your PATH
export PATH="$PATH:$(npm config get prefix)/bin"
```

### Q: How to update OpenSpec

```bash
npm install -g @fission-ai/openspec@latest
openspec update
```

---

## Next

→ [Chapter 3: Core Commands](03-commands-en.md)
