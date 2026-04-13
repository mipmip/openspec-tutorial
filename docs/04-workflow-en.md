# Chapter 4: Complete Workflow Walkthrough

This chapter demonstrates the actual OpenSpec workflow through a complete example.

## Two Ways to Use OpenSpec

OpenSpec can be used in two ways:

| Method | Description | Example |
|--------|-------------|---------|
| **CLI method** | Use CLI commands directly in the terminal | `openspec new change xxx` |
| **AI-native method** | Use commands in AI tool conversations | `/opsx:propose "xxx"` |

The two methods can be combined:
- Use CLI to create and manage changes
- Use AI-native commands to let the AI auto-generate documents and implement code

---

## Example: Adding Dark Mode

Let's say we want to add a dark mode feature to a React project.

---

## Step 1: Create a Change

### CLI Method (Direct Creation)

```bash
openspec new change add-dark-mode
```

### AI-Native Method (Let the AI Help)

Tell your AI tool:

```
/opsx:propose "Add dark mode so users can switch between light and dark themes"
```

The AI will automatically:
1. Run `openspec new change add-dark-mode` to create the directory
2. Generate spec documents
3. Present them for your confirmation

Regardless of which method you use, the directory structure will be:

```
openspec/changes/add-dark-mode/
├── proposal.md
├── specs/
│   └── requirements.md
├── design.md
└── tasks.md
```

### proposal.md Example

```markdown
# Proposal: Add Dark Mode

## Background
The app currently only has a light theme, resulting in a poor experience in low-light environments.

## Goals
- Support light/dark theme switching
- Automatically follow system theme preferences
- Persist user preferences

## Success Criteria
- Users can toggle themes via a switch
- All components display correctly after switching
- User selection persists after page refresh
```

### specs/requirements.md Example

```markdown
# Requirements Specification

## Functional Requirements
1. Add a theme toggle switch (positioned on the right side of the navbar)
2. Support three modes:
   - Light
   - Dark
   - System (follow system setting)
3. Save user selection to localStorage
4. Default to System

## Visual Requirements
- Dark mode background color: #1a1a2e
- Dark mode text color: #eaeaea
- All components must support both themes

## Scenarios
### Scenario 1: First Visit
User visits for the first time; detect system theme and apply automatically

### Scenario 2: Manual Switch
User clicks the toggle button; theme changes immediately

### Scenario 3: Return Visit
User returns; restore their previously selected theme
```

### design.md Example

```markdown
# Technical Design

## Approach
Use React Context + CSS Variables

## File Structure
```
src/
├── components/
│   └── ThemeToggle.tsx      # Theme toggle button
├── contexts/
│   └── ThemeContext.tsx     # Theme context
├── hooks/
│   └── useTheme.ts          # Theme Hook
└── styles/
    └── theme.css            # Theme variables
```

## Implementation Details
1. ThemeContext manages the current theme state
2. CSS Variables define color tokens
3. localStorage persists user selection
4. matchMedia listens for system theme changes

## Color Variables
```css
:root {
  --bg-primary: #ffffff;
  --text-primary: #1a1a2e;
}

[data-theme="dark"] {
  --bg-primary: #1a1a2e;
  --text-primary: #eaeaea;
}
```
```

### tasks.md Example

```markdown
# Implementation Tasks

## 1. Create Theme System
- [ ] 1.1 Create ThemeContext.tsx
- [ ] 1.2 Create useTheme hook
- [ ] 1.3 Create theme.css variables file

## 2. Create Toggle Component
- [ ] 2.1 Create ThemeToggle component
- [ ] 2.2 Add sun/moon icons
- [ ] 2.3 Implement toggle logic

## 3. Integrate into the App
- [ ] 3.1 Wrap ThemeProvider in App.tsx
- [ ] 3.2 Add ThemeToggle to the navbar
- [ ] 3.3 Update existing components to use CSS variables

## 4. Persistence and System Adaptation
- [ ] 4.1 Implement localStorage storage
- [ ] 4.2 Implement system theme listener
- [ ] 4.3 Handle default theme on first visit
```

---

## Step 2: Review the Specs

**This is the most critical step!**

Carefully read the AI-generated documents:

```bash
# View the proposal
openspec show add-dark-mode
cat openspec/changes/add-dark-mode/proposal.md

# View the requirements
cat openspec/changes/add-dark-mode/specs/requirements.md

# View the design
cat openspec/changes/add-dark-mode/design.md

# View the tasks
cat openspec/changes/add-dark-mode/tasks.md
```

### If Everything Looks Good

Tell the AI:
```
The spec documents look good, let's start implementing
```

### If Changes Are Needed

Point out the issues directly:
```
In design.md, let's use Tailwind's dark: prefix instead of CSS Variables
tasks.md needs unit testing tasks added
```

The AI will update the documents until you're satisfied.

---

## Step 3: Start Implementing

### AI-Native Method (Recommended)

In your AI tool:

```
/opsx:apply
```

### Implementation Process

The AI will execute tasks in the order listed in tasks.md:

```
Starting dark mode implementation...

✓ 1.1 Create ThemeContext.tsx
  Created theme context with theme, setTheme, toggleTheme

✓ 1.2 Create useTheme hook
  Wrapped theme state getter and toggle methods

✓ 1.3 Create theme.css variables file
  Defined color variables for light and dark modes

✓ 2.1 Create ThemeToggle component
  Created toggle button with sun/moon icons

✓ 2.2 Add sun/moon icons
  Using lucide-react icon library

✓ 2.3 Implement toggle logic
  Toggle switches theme and saves to localStorage

✓ 3.1 Wrap ThemeProvider in App.tsx
  Added theme provider at app root

✓ 3.2 Add ThemeToggle to navbar
  Added toggle button to right side of Header component

✓ 3.3 Update existing components to use CSS variables
  Updated Button, Card, Input, and other components

✓ 4.1 Implement localStorage storage
  Theme selection persisted to localStorage

✓ 4.2 Implement system theme listener
  Using matchMedia to monitor system theme changes

✓ 4.3 Handle default theme on first visit
  Detects system preference on first visit

All tasks complete!
```

---

## Step 4: Verify the Feature

### CLI Verification

```bash
# Verify the change matches the spec
openspec validate add-dark-mode

# View task completion status
openspec status --change add-dark-mode
```

### AI-Native Verification

In your AI tool:

```
/opsx:verify
```

The AI will compare specs/ against the code and report compliance:

```
✓ Phone number login - Implemented
✓ Verification code 5-minute expiry - Implemented
✗ WeChat login - Not implemented (tasks.md 1.4 incomplete)
⚠ Login failure attempt limit - Spec requires 5 attempts, code implements 3
```

### Manual Testing

```bash
# Start the app
npm run dev

# Test the following scenarios:
# 1. Click the toggle button -- does the theme switch correctly?
# 2. Refresh the page -- does the theme persist?
# 3. Change the system theme -- does the app follow?
```

---

## Step 5: Archive the Change

### CLI Method

```bash
# Archive the change (will prompt for confirmation)
openspec archive add-dark-mode

# Skip confirmation
openspec archive add-dark-mode --yes
```

### AI-Native Method

In your AI tool:

```
/opsx:archive
```

The AI will call the CLI archive command to complete the archiving.

### Archive Result

The change is moved to:
```
openspec/archive/YYYY-MM-DD-add-dark-mode/
```

Directory structure:
```
openspec/archive/YYYY-MM-DD-add-dark-mode/
├── proposal.md
├── specs/
├── design.md
├── tasks.md
└── summary.md          # New: implementation summary
```

### summary.md Example

```markdown
# Implementation Summary

## Completion Date
2025-03-16

## What Was Implemented
- ✅ ThemeContext and useTheme hook
- ✅ ThemeToggle component (sun/moon icons)
- ✅ CSS variable theme system
- ✅ localStorage persistence
- ✅ Automatic system theme adaptation

## New Files
- src/contexts/ThemeContext.tsx
- src/hooks/useTheme.ts
- src/components/ThemeToggle.tsx
- src/styles/theme.css

## Modified Files
- src/App.tsx
- src/components/Header.tsx
- src/components/Button.tsx
- src/components/Card.tsx
- src/components/Input.tsx

## Notes
Implementation went smoothly; no spec adjustments needed
```

---

## Complete Flowchart

```
Start
  │
  ▼
┌─────────────────────────────────────┐
│  Create a change                     │
│  CLI: openspec new change xxx       │
│  AI: /opsx:propose "xxx"           │
└─────────────────────────────────────┘
  │
  ▼
┌─────────────────────────────────────┐
│  AI generates spec documents         │
│  proposal.md / specs/               │
│  design.md / tasks.md              │
└─────────────────────────────────────┘
  │
  ▼
  You review the spec documents
  │
  ├─ Not satisfied ──→ Tell the AI to revise ──→ Back to review
  │
  └─ Satisfied ─────→ /opsx:apply
                  │
                  ▼
              AI implements the code
                  │
                  ▼
              Manual testing & verification
                  │
              ├─ Issues found ──→ Tell the AI to fix ──→ Continue testing
              │
              └─ All good ──→ /opsx:archive
                              │
                              ▼
                          Archiving complete
                              │
                              ▼
                          Start the next feature
```

---

## Quick Reference

### CLI Commands (Terminal)

```bash
# Initialize
openspec init
openspec init --tools claude,cursor

# Create
openspec new change <name>

# View
openspec list
openspec show <name>
openspec view

# Validate
openspec validate [name]
openspec validate --all

# Status
openspec status
openspec status --change <name>

# Archive
openspec archive <name>
openspec archive <name> --yes

# Configuration
openspec config list
openspec config profile
openspec update
```

### AI-Native Commands (AI Conversations)

```
/opsx:propose "describe requirements"    # Create a proposal
/opsx:apply                              # Start implementing
/opsx:verify                             # Verify implementation
/opsx:sync                               # Sync state
/opsx:archive                            # Archive
/opsx:onboard                            # New member onboarding
```

---

## Next

→ [Chapter 5: Project Structure Deep Dive](05-structure-en.md)
