# Chapter 9: Advanced Tips

## 1. Context Management

### Keep the Context Clean

OpenSpec recommends cleaning the AI's context before starting a new feature:

```
# Before starting a new feature
1. Complete and archive the previous feature
2. Clear the AI conversation context (start a new conversation)
3. Have the AI read AGENTS.md and the current changes/
4. Run /opsx:propose
```

**Why this matters**:
- Information from old conversations can interfere with the new feature
- Clean context = more accurate implementation
- Reduces AI "hallucinations"

### Provide Enough Context

```bash
# Provide background in your proposal
/opsx:propose "Add file upload feature,
  Background: users need to upload avatars and attachments,
  Technical constraint: use Alibaba Cloud OSS,
  File size limits: images 5MB, documents 20MB"
```

---

## 2. Iterative Specs

### Don't Try to Write All Specs at Once

```bash
# First pass: basic functionality
/opsx:propose "Add file upload feature (basic version)"
# After implementing...

# Second pass: enhanced functionality
/opsx:propose "Add progress bar and resumable uploads to file upload"
# After implementing...

# Third pass: optimization
/opsx:propose "Optimize file upload performance with concurrent uploads"
```

**Benefits**:
- Each change is small in scope, lower risk
- You can adjust direction based on real results
- Easier to code review

---

## 3. Spec Reuse

### Create Spec Templates

For common features, create templates:

```
openspec/
└── templates/
    ├── crud-feature.md      # CRUD feature template
    ├── auth-feature.md      # Auth feature template
    └── notification.md      # Notification feature template
```

**crud-feature.md template**:
```markdown
# Proposal: [Feature Name] CRUD

## Background
[Describe background]

## Goals
Implement complete CRUD operations for [Entity Name]

## Scope
**Included**:
- Create [Entity Name]
- View [Entity Name] list
- View [Entity Name] details
- Update [Entity Name]
- Delete [Entity Name]

## Data Structure
[Entity Name] contains:
- id: unique identifier
- [Field 1]: [description]
- [Field 2]: [description]
- created_at: creation timestamp
- updated_at: update timestamp
```

---

## 4. CI/CD Integration

### Validate Specs in CI

```yaml
# .github/workflows/openspec-check.yml
name: OpenSpec Check

on: [pull_request]

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Check OpenSpec structure
        run: |
          # Check that changes in changes/ have complete documentation
          for dir in openspec/changes/*/; do
            if [ -d "$dir" ]; then
              name=$(basename "$dir")

              # Check required files
              for file in proposal.md design.md tasks.md; do
                if [ ! -f "$dir$file" ]; then
                  echo "❌ $name is missing $file"
                  exit 1
                fi
              done

              echo "✅ $name documentation is complete"
            fi
          done
```

---

## 5. Parallel Changes

### Working on Multiple Changes Simultaneously

```
openspec/changes/
├── add-file-upload/      # Owned by Zhang San
├── add-notifications/    # Owned by Li Si
└── fix-performance/      # Owned by Wang Wu
```

**Important notes**:
- Ensure changes don't conflict with each other
- Tag assignees in tasks.md
- Sync progress regularly

---

## 6. Handling Spec Changes

### When Specs Need to Change During Implementation

Sometimes you'll discover during implementation that the specs need adjusting:

```bash
# Tell the AI
"While implementing 1.3, I discovered the approach in design.md
 doesn't work because PostgreSQL doesn't support this operation.
 We need to use a different approach."

# The AI will:
# 1. Update design.md
# 2. Possibly update tasks.md
# 3. Continue implementation
```

**Principles**:
- Specs are living documents -- they can be modified
- Record the reason for any spec changes
- Major changes require re-review

---

## 7. Batch Task Execution

OpenSpec supports having the AI execute tasks in batch without confirming each one individually.

### Basic Usage

In your AI tool:

```
/opsx:apply
```

The AI will execute all tasks in tasks.md order, updating status as each completes.

### Controlling Execution Scope

If you only need the AI to execute certain tasks, just say so:

```
# Execute specific tasks only
"Please only implement tasks 1.1 and 1.2 from tasks.md"

# Execute only a specific phase
"Please only implement the Phase 2 tasks from tasks.md"
```

### When to Use Batch Execution

- Specs are very clear
- Tasks are relatively simple
- You trust the AI's implementation ability

### When to Execute Step by Step

- Specs aren't detailed enough
- Tasks involve complex logic
- Decisions need to be made during implementation

---

## 8. Spec-Driven Code Review

### Reference Specs in PR Descriptions

```markdown
## PR Description

**Feature**: Add file upload
**Specs**: openspec/changes/add-file-upload/

### What Was Implemented
Completed the following tasks from tasks.md:
- ✅ 1.1 Configure Alibaba Cloud OSS
- ✅ 1.2 Implement file upload endpoint
- ✅ 1.3 Implement frontend upload component
- ✅ 1.4 Add file size limits

### Deviations from Spec
- design.md planned to support WebP format,
  but the current Alibaba Cloud OSS version doesn't support
  automatic conversion. Updated design.md to document this limitation.

### Testing
- Unit tests: 85% coverage
- Manual testing: Tested in Chrome/Safari/Firefox
```

---

## Next

→ [Chapter 10: FAQ](10-faq-en.md)
