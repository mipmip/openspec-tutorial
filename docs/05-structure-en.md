# Chapter 5: Project Structure Deep Dive

## openspec/ Directory Structure

OpenSpec creates an `openspec/` directory in the project root. All spec-related files live here.

```
my-project/
├── openspec/                      # OpenSpec root directory
│   ├── changes/                  # Changes in progress
│   │   └── add-dark-mode/       # A single change
│   │       ├── proposal.md      # Proposal document
│   │       ├── specs/           # Requirements spec
│   │       │   └── requirements.md
│   │       ├── design.md        # Technical design
│   │       └── tasks.md         # Implementation tasks
│   │
│   └── archive/                  # Archived changes
│       └── 2025-03-16-add-dark-mode/
│           ├── proposal.md
│           ├── specs/
│           ├── design.md
│           ├── tasks.md
│           └── summary.md       # Implementation summary
│
├── AGENTS.md                     # AI assistant configuration
└── (your project files)
```

---

## changes/ Directory

Stores changes that are **currently in progress**.

### Naming Convention

Change directory names use **kebab-case** (hyphen-separated):

```
add-dark-mode              ✅
add_dark_mode              ❌
AddDarkMode                ❌
add dark mode              ❌
```

### Good Naming Examples

```
add-user-login             # Add user login
fix-mobile-layout          # Fix mobile layout
refactor-auth-module       # Refactor auth module
optimize-homepage-speed    # Optimize homepage speed
update-dependencies        # Update dependencies
```

### Directory Structure

Each change directory contains 4 documents:

```
changes/add-feature/
├── proposal.md             # Proposal: why we're doing this
├── specs/                  # Specs: what to do
│   └── requirements.md    # Requirements document
├── design.md               # Design: how to do it
└── tasks.md                # Tasks: specific steps
```

---

## archive/ Directory

Stores **completed** changes.

### Naming Convention

Archive directory names include a **date prefix**:

```
2025-03-16-add-dark-mode
2025-03-15-fix-login-bug
2025-03-10-refactor-api
```

### Why Archive

1. **Keep changes/ clean** - Only see work currently in progress
2. **Preserve history** - Review past changes at any time
3. **Generate changelogs** - Automatically generate release notes
4. **Team knowledge retention** - New members can understand project evolution

---

## Document Details

### 1. proposal.md - Proposal

**Purpose**: Explain why this change is being made

**Template**:
```markdown
# Proposal: [Change Title]

## Background
The current problem or opportunity

## Goals
Specific objectives to achieve

## Scope
- What's included
- What's not included

## Success Criteria
How to determine it's done
```

**Example**:
```markdown
# Proposal: Add User Login Feature

## Background
The current system has no user authentication; anyone can access sensitive data.

## Goals
- Implement secure user authentication
- Support phone number and WeChat login
- Maintain login state for 7 days

## Scope
**Included**:
- Phone number + verification code login
- WeChat OAuth login
- JWT Token mechanism

**Not included**:
- Email login (future iteration)
- Multi-factor authentication (MFA)

## Success Criteria
- Users can successfully log in
- Unauthenticated users cannot access protected pages
- Expired tokens automatically redirect to the login page
```

---

### 2. specs/ - Requirements Specification

**Purpose**: Describe what to do in detail

**Template**:
```markdown
# Requirements Specification

## Functional Requirements
1. Requirement 1
2. Requirement 2

## Non-Functional Requirements
- Performance requirements
- Security requirements

## Scenarios
### Scenario 1: Normal Flow
1. Step 1
2. Step 2

### Scenario 2: Error Flow
...

## UI Mockups (optional)
- Screenshots or links
```

**Example**:
```markdown
# Requirements Specification

## Functional Requirements
1. Users can log in with phone number + verification code
2. Verification code is valid for 5 minutes
3. After 3 failed attempts, user must wait 60 seconds
4. Successful login returns a JWT Token
5. Token is valid for 7 days

## Non-Functional Requirements
- API response time < 200ms
- Support 1,000 concurrent users
- Verification code send rate limit (1 per minute)

## Scenarios

### Scenario 1: Normal Login
1. User enters phone number
2. Clicks "Get verification code"
3. Receives SMS with verification code
4. Enters verification code
5. Clicks "Login"
6. Login succeeds, redirects to homepage

### Scenario 2: Expired Verification Code
1. User enters an expired verification code
2. Clicks "Login"
3. System displays "Verification code has expired, please request a new one"
4. User requests a new code

### Scenario 3: Frequent Requests
1. User clicks "Get verification code" multiple times within 1 minute
2. System displays "Please try again later"
```

---

### 3. design.md - Technical Design

**Purpose**: Describe the technical approach

**Template**:
```markdown
# Technical Design

## Architecture
- Tech stack
- Overall architecture diagram

## API Design
- Endpoint list
- Request/response formats

## Data Model
- Database table structures

## Key Implementation Details
- Core logic
- Algorithm descriptions
```

**Example**:
```markdown
# Technical Design

## Architecture
- Frontend: React + TypeScript
- Backend: Node.js + Express
- Database: PostgreSQL
- Cache: Redis (for storing verification codes)

## API Design

### POST /api/auth/send-code
Send verification code

**Request**:
```json
{
  "phone": "13800138000"
}
```

**Response**:
```json
{
  "success": true,
  "message": "Verification code sent"
}
```

### POST /api/auth/login
Verification code login

**Request**:
```json
{
  "phone": "13800138000",
  "code": "123456"
}
```

**Response**:
```json
{
  "success": true,
  "token": "eyJhbGciOiJIUzI1NiIs...",
  "expires_in": 604800
}
```

## Data Model

### users table
```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  phone VARCHAR(20) UNIQUE NOT NULL,
  wechat_openid VARCHAR(50),
  created_at TIMESTAMP DEFAULT NOW(),
  last_login_at TIMESTAMP
);
```

### verification_codes table
```sql
CREATE TABLE verification_codes (
  id SERIAL PRIMARY KEY,
  phone VARCHAR(20) NOT NULL,
  code VARCHAR(6) NOT NULL,
  expires_at TIMESTAMP NOT NULL,
  created_at TIMESTAMP DEFAULT NOW()
);
```

## Key Implementation Details

### Verification Code Generation
- 6-digit random number
- Stored in Redis with TTL of 300 seconds

### Token Generation
- JWT format
- Payload includes user_id, phone
- Signed using HS256
```

---

### 4. tasks.md - Implementation Tasks

**Purpose**: Concrete step-by-step implementation checklist

**Template**:
```markdown
# Implementation Tasks

## 1. Backend API
- [ ] 1.1 Create database tables
- [ ] 1.2 Implement send verification code endpoint
- [ ] 1.3 Implement login endpoint

## 2. Frontend Pages
- [ ] 2.1 Create login page
- [ ] 2.2 Implement verification code countdown
```

**Example**:
```markdown
# Implementation Tasks

## 1. Backend API
- [ ] 1.1 Create database tables
  - [ ] 1.1.1 Create users table
  - [ ] 1.1.2 Create verification_codes table
  - [ ] 1.1.3 Add indexes

- [ ] 1.2 Implement send verification code endpoint
  - [ ] 1.2.1 Create POST /api/auth/send-code
  - [ ] 1.2.2 Integrate SMS provider (Alibaba Cloud)
  - [ ] 1.2.3 Implement rate limiting

- [ ] 1.3 Implement login endpoint
  - [ ] 1.3.1 Create POST /api/auth/login
  - [ ] 1.3.2 Verify the verification code
  - [ ] 1.3.3 Generate JWT Token

- [ ] 1.4 Write unit tests
  - [ ] 1.4.1 Test send verification code
  - [ ] 1.4.2 Test login flow

## 2. Frontend Pages
- [ ] 2.1 Create login page
  - [ ] 2.1.1 Create LoginPage component
  - [ ] 2.1.2 Add /login route

- [ ] 2.2 Implement phone number input
  - [ ] 2.2.1 Add phone number input field
  - [ ] 2.2.2 Add format validation

- [ ] 2.3 Implement verification code feature
  - [ ] 2.3.1 Add verification code input field
  - [ ] 2.3.2 Implement countdown button
  - [ ] 2.3.3 Call send verification code API

- [ ] 2.4 Implement login logic
  - [ ] 2.4.1 Call login API
  - [ ] 2.4.2 Store Token
  - [ ] 2.4.3 Redirect after successful login
```

---

### 5. summary.md - Implementation Summary (Generated During Archiving)

**Purpose**: Record what was actually implemented

**Template**:
```markdown
# Implementation Summary

## Completion Date
2025-03-16

## What Was Implemented
- ✅ Feature 1
- ✅ Feature 2
- ⚠️ Feature 3 (partially implemented)

## New Files
- File 1
- File 2

## Modified Files
- File 3
- File 4

## Notes
Important notes from the implementation process
```

---

## AGENTS.md

The `AGENTS.md` file in the project root is a configuration file for AI assistants.

**Example**:
```markdown
# OpenSpec Agent Configuration

## Project Information
- Name: My Project
- Tech Stack: React + TypeScript + Node.js
- Database: PostgreSQL

## Commands
- `/opsx:propose <description>` - Create a new change proposal
- `/opsx:apply` - Apply the implementation
- `/opsx:archive` - Archive a change

## Standards
- Code style: ESLint + Prettier
- Testing: All features must have unit tests
- Documentation: API changes require README updates

## Important Notes
- Don't modify database schema without a migration
- Use environment variables for sensitive information
```

---

## Next

→ [Chapter 6: Documentation Writing Standards](06-documentation-en.md)
