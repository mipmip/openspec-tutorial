# Chapter 1: OpenSpec Basic Concepts

## Pain Points of AI-Assisted Programming

In 2024-2025, AI coding assistants (Claude, GPT, Cursor, etc.) fundamentally changed software development. But new problems came along with them:

### 1. Misunderstood Requirements

```
You: Build me a user login feature
AI: Sure, let me implement that... (writes 200 lines of code)
You: No, I wanted phone number login, not email
AI: OK, let me fix that... (changes another 100 lines)
You: Also, it needs to support WeChat login
AI: OK... (keeps changing)
You: Forget it, let's start over
```

**The problem**: The AI is *guessing* your requirements instead of *understanding* them.

### 2. Lost Context

- AI conversations have length limits; long projects lose early context
- Switch to a new chat window and the AI forgets previous requirements
- Team members can't share the AI's understanding

### 3. Unpredictable Results

- Same prompt, different results at different times
- No unified standards, inconsistent code quality
- Hard to code review -- you don't know why the AI wrote it that way

### 4. Difficult Team Collaboration

- Everyone has different AI conversation histories
- No unified requirements documentation
- New members don't know the project background or decision history

---

## What Problems Does OpenSpec Solve?

### Core Solution: **Spec-Driven Development**

Write spec documents before writing code. Let the AI implement based on **clear specifications** instead of guessing.

```
Traditional: Requirements → AI guesses → Code → Revise → Revise → Code
OpenSpec:    Requirements → Spec docs → AI understands → Code → Done
```

### Specific Problems Solved

| Pain Point | OpenSpec Solution |
|------------|-------------------|
| Misunderstood requirements | Spec documents clarify requirements; AI stops guessing |
| Lost context | Documents persist and can be referenced anytime |
| Unpredictable results | Implementation based on specs yields predictable outcomes |
| Difficult team collaboration | Shared specs, unified understanding |
| Hard code reviews | Specs serve as the review baseline |
| No project knowledge retention | Every change has a complete record |

### Real-World Impact

**Without OpenSpec**:
- Developing a feature requires an average of 3-5 rounds of conversation corrections
- Team members have inconsistent understanding of requirements
- 3 months later, nobody remembers why it was designed that way

**With OpenSpec**:
- Align once, implement precisely
- Everyone works from the same specs
- Complete change history, traceable at any time

---

## What Is OpenSpec?

OpenSpec is a **Spec-Driven Development (SDD) framework** designed specifically for AI coding assistants.

### Workflow

OpenSpec can be used in two ways: **CLI commands** (terminal) and **AI-native commands** (AI tool conversations).

```
Step 1: Create a change
CLI:    openspec new change add-login
AI:     /opsx:propose "Add user login feature"
   ↓
2. AI creates spec documents
   - proposal.md  (why we're doing it)
   - specs/       (what to do)
   - design.md    (how to do it)
   - tasks.md     (specific steps)
   ↓
3. You review and confirm the specs
   ↓
4. /opsx:apply
   AI implements code based on the specs
   ↓
5. /opsx:archive
   Archive the change record
```

### Core Values

1. **Alignment before implementation** - Confirm requirements first, then write code
2. **Documents as contracts** - Specs are contracts between humans and AI
3. **Traceable history** - Every change has a complete record
4. **Foundation for teamwork** - Shared specs, unified understanding

---

## Core Concepts

### 1. Change

Every feature or modification is a **Change** with its own directory:

```
openspec/changes/
└── add-user-login/          ← A Change
    ├── proposal.md
    ├── specs/
    ├── design.md
    └── tasks.md
```

### 2. Proposal

Describes **why** this feature should be built:

```markdown
# Proposal: User Login Feature

## Background
The current system has no user authentication; everyone can access all data.

## Goals
- Implement secure user authentication
- Support multiple login methods

## Scope
- Phone number + verification code login
- WeChat OAuth login
- Not included: email login (future iteration)
```

### 3. Specs

Describes **what to do**, with detailed requirements and scenarios:

```markdown
# Requirements Specification

## Functional Requirements
- Users can log in with phone number + verification code
- Verification code is valid for 5 minutes
- Returns a JWT Token upon successful login

## Scenarios
### Scenario 1: Normal Login
1. User enters phone number
2. Clicks "Get verification code"
3. Enters verification code
4. Clicks "Login"
5. Redirects to homepage

### Scenario 2: Expired Verification Code
1. User enters an expired verification code
2. System displays "Verification code has expired, please request a new one"
```

### 4. Design

Describes **how to do it** -- the technical approach:

```markdown
# Technical Design

## Architecture
- Frontend: React + Ant Design
- Backend: Node.js + Express
- Database: MySQL

## API Design
POST /api/auth/send-code    # Send verification code
POST /api/auth/login        # Verification code login
POST /api/auth/wechat       # WeChat login

## Data Model
User {
  id, phone, wechat_openid,
  created_at, last_login_at
}
```

### 5. Tasks

Describes the **specific steps**; the AI implements accordingly:

```markdown
# Implementation Tasks

## 1. Backend API
- [ ] 1.1 Create User database table
- [ ] 1.2 Implement send verification code endpoint
- [ ] 1.3 Implement verification code login endpoint
- [ ] 1.4 Integrate WeChat OAuth

## 2. Frontend Pages
- [ ] 2.1 Create login page
- [ ] 2.2 Implement phone number input component
- [ ] 2.3 Implement verification code countdown
- [ ] 2.4 Implement WeChat login button
```

---

## OpenSpec vs Traditional Development

| Aspect | Traditional Development | OpenSpec |
|--------|------------------------|---------|
| Requirements alignment | Verbal descriptions, easy to misunderstand | Spec documents, clear and explicit |
| AI collaboration | Repeated revisions, wasted time | Align once, implement precisely |
| Project records | Scattered across chat logs | Structured documents, accessible anytime |
| Team collaboration | Different interpretations | Shared specs, unified understanding |
| Iteration management | No idea what changed | Complete record for every change |

---

## OpenSpec vs Alternatives

| Tool | Strengths | Weaknesses |
|------|-----------|------------|
| **OpenSpec** | Lightweight, flexible, multi-tool support | - |
| **GitHub Spec-Kit** | Comprehensive | Heavyweight, Python dependency, rigid stage gates |
| **Kiro (AWS)** | Feature-rich | IDE lock-in, only supports Claude |
| **No specs** | Simple | AI improvises freely, unpredictable results |

---

## Core Philosophy

```
→ fluid not rigid          Flexible, not rigid
  Modify any document at any time; no strict stage gates

→ iterative not waterfall  Iterative, not waterfall
  Supports continuous iteration; no need to write all docs at once

→ easy not complex         Simple, not complex
  Minimize ceremony, focus on value

→ built for brownfield     Works with existing projects
  Not just for new projects; existing projects benefit equally

→ scalable                 Scalable
  From personal projects to enterprise teams
```

---

## Next

→ [Chapter 2: Installation & Setup](02-installation-en.md)
