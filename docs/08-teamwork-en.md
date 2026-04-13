# Chapter 8: Team Collaboration

## How OpenSpec Supports Team Collaboration

### The Problem: Team Collaboration with Traditional AI Coding

- Everyone has different AI conversation histories, leading to inconsistent understanding
- No unified requirements documentation
- No visibility into what teammates are working on
- No context during code reviews

### The Solution: OpenSpec's Team Mode

```
Unified spec documents
    ↓
Everyone works from the same understanding
    ↓
Consistent AI implementations
    ↓
Code reviews have a clear baseline
```

---

## Team Workflow

### 1. Requirements Review

```bash
# Product/BA creates a proposal
/opsx:propose "Add user comment feature"
```

**Team review**:
- Product manager reviews proposal.md
- Tech lead reviews design.md
- QA adds test scenarios to specs/

### 2. Task Assignment

```markdown
# tasks.md

## 1. Backend API (@backend-dev)
- [ ] 1.1 Create Comment data model
- [ ] 1.2 Implement comment CRUD endpoints

## 2. Frontend Pages (@frontend-dev)
- [ ] 2.1 Create comment list component
- [ ] 2.2 Create comment form component

## 3. Testing (@qa)
- [ ] 3.1 Write API test cases
- [ ] 3.2 Write UI automation tests
```

### 3. Parallel Development

Each developer works on their own branch, based on the same specs:

```bash
# Backend developer
git checkout -b feature/comments-backend
/opsx:apply
# AI will execute backend-related tasks from tasks.md

# Frontend developer
git checkout -b feature/comments-frontend
/opsx:apply
# AI will execute frontend-related tasks from tasks.md
```

> Note: To execute only specific tasks, tell the AI: "Please only implement tasks 1.1 and 1.2 from tasks.md"

### 4. Code Review

Reviews have complete spec documents as reference:

```markdown
## PR Description

**Change**: Add user comment feature
**OpenSpec**: openspec/changes/add-comments/

**What was implemented**:
- ✅ Created Comment data model
- ✅ Implemented comment CRUD endpoints
- ✅ Created comment list component

**Deviations from spec**:
- design.md planned WebSocket for real-time updates; changed to polling
  (sufficient performance, simpler implementation)
```

---

## Team Configuration

### Shared AGENTS.md

Create a team-shared `AGENTS.md` in the project root:

```markdown
# OpenSpec Team Configuration

## Team Info
- Team: XX Development Team
- Project: XX Product

## Tech Stack
- Frontend: Vue 3 + TypeScript + Vite
- Backend: Go + Gin
- Database: MySQL
- Cache: Redis

## Standards
### Code Standards
- Use ESLint + Prettier
- All code must pass CI checks
- Unit test coverage > 80%

### Git Standards
- Branch naming: feature/xxx, bugfix/xxx
- Commits use Conventional Commits
- PRs must reference an OpenSpec change

### OpenSpec Standards
- Every feature must have complete spec documents
- Technical designs must be reviewed by the tech lead
- Complex features require a design review meeting

## Common Commands
- `/opsx:propose` - Create a new feature proposal
- `/opsx:apply` - Implement the feature
- `/opsx:onboard` - New member onboarding

## Important Notes
- Don't modify database schema without a migration
- Use environment variables for sensitive information
- API changes require documentation updates
```

---

## Code Review Checklist

### What to Check During Review

```markdown
## PR Review Checklist

### Spec Alignment
- [ ] Does the implementation match the technical approach in design.md?
- [ ] Are all requirements in specs/ covered?
- [ ] Are all error scenarios handled?

### Code Quality
- [ ] Does the code follow team standards?
- [ ] Are there sufficient unit tests?
- [ ] Is there any sensitive information leakage?

### Documentation
- [ ] Does the README need updating?
- [ ] Is the API documentation updated?
- [ ] Does the change need to be archived?
```

---

## Using /opsx:onboard

Quickly onboard new team members:

```bash
/opsx:onboard
```

The AI will generate:

```markdown
# Project Overview

## About the Project
XX Product is a...

## Completed Features
1. User authentication system (2025-03-01)
2. Task management (2025-03-05)
3. Comment feature (2025-03-10)

## Features In Progress
1. File upload (@Zhang San)
2. Notifications (@Li Si)

## Tech Stack
- Frontend: Vue 3 + TypeScript
- Backend: Go + Gin
- Database: MySQL

## Quick Start
1. git clone xxx
2. npm install
3. npm run dev

## Key Documents
- API documentation: /docs/api.md
- Architecture documentation: /docs/architecture.md
```

---

## Next

→ [Chapter 9: Advanced Tips](09-advanced-en.md)
