# Chapter 6: Documentation Writing Standards

## Why Document Quality Matters

The core of OpenSpec is **spec documents**. Document quality directly determines the quality of the AI's implementation.

```
Vague specs → AI guesses → Implementation drift → Repeated revisions
Clear specs → AI understands → Precise implementation → Done right the first time
```

---

## proposal.md Writing Guide

### Must Include

| Field | Description | Example |
|-------|-------------|---------|
| Background | Why we're doing this | "No user authentication currently; security risk" |
| Goals | What we want to achieve | "Implement secure user login" |
| Scope | What's in / what's out | "Includes phone login, excludes email login" |
| Success Criteria | How we know it's done | "Users can log in and access protected pages" |

### Good vs Bad

**❌ Bad proposal**:
```markdown
# Proposal: Improve Login

Need to improve the login feature to make it better.
```

**✅ Good proposal**:
```markdown
# Proposal: Add Phone Number Verification Code Login

## Background
The current system uses username + password login. Users report
frequently forgetting passwords, resulting in 200+ "forgot password"
requests per week, accounting for 30% of customer service workload.

## Goals
- Add phone number + verification code login
- Reduce password-related support requests

## Scope
**Included**:
- Phone number + SMS verification code login
- Link to existing accounts (matched by phone number)

**Not included**:
- Replacing existing username + password login (keep it)
- WeChat login (next phase)

## Success Criteria
- Users can log in with phone number + verification code
- Login success rate > 99%
- Verification code delivery latency < 5 seconds
```

---

## specs/ Writing Guide

### Requirements Must Be Testable

Every requirement should be verifiable:

**❌ Not testable**:
```markdown
- System should be fast
- UI should look nice
- User experience should be good
```

**✅ Testable**:
```markdown
- API response time < 200ms (P99)
- Login page displays correctly at 375px width
- Verification code input field auto-focuses
```

### Scenarios Must Be Complete

Don't just write the happy path -- include error flows too:

```markdown
## Scenarios

### Normal Scenario
1. User enters correct verification code
2. Login succeeds, redirects to homepage

### Error Scenario 1: Wrong Verification Code
1. User enters incorrect verification code
2. System displays "Incorrect code, 2 attempts remaining"

### Error Scenario 2: Expired Verification Code
1. User enters an expired verification code
2. System displays "Code has expired, please request a new one"

### Error Scenario 3: Network Error
1. Network disconnects while sending verification code
2. System displays "Network error, please check your connection"
3. Button returns to clickable state

### Edge Case: Frequent Requests
1. User clicks "Get verification code" twice within 1 minute
2. Second click shows "Please try again in XX seconds"
```

### Use Specific Numbers

**❌ Vague**:
```markdown
- Verification code has a certain validity period
- Supports multiple retries
- Password must be sufficiently complex
```

**✅ Specific**:
```markdown
- Verification code is valid for 5 minutes
- Maximum 3 retries, then locked for 60 seconds
- Password must be at least 8 characters with uppercase, lowercase, and numbers
```

---

## design.md Writing Guide

### API Design Must Be Complete

Every endpoint should include:

```markdown
### POST /api/auth/login

**Description**: User login

**Request Headers**:
```
Content-Type: application/json
```

**Request Body**:
```json
{
  "phone": "13800138000",    // Phone number, required
  "code": "123456"           // Verification code, required
}
```

**Success Response** (200):
```json
{
  "success": true,
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIs...",
    "expires_in": 604800,
    "user": {
      "id": 1,
      "phone": "138****8000"
    }
  }
}
```

**Error Response** (400):
```json
{
  "success": false,
  "error": {
    "code": "INVALID_CODE",
    "message": "Incorrect verification code"
  }
}
```

**Error Codes**:
| Error Code | Description |
|------------|-------------|
| INVALID_CODE | Incorrect verification code |
| CODE_EXPIRED | Verification code has expired |
| TOO_MANY_ATTEMPTS | Too many attempts |
```

### Data Models Must Be Clear

```markdown
## Data Model

### User
```typescript
interface User {
  id: number;
  phone: string;           // Phone number, unique
  wechat_openid?: string;  // WeChat OpenID, optional
  created_at: Date;
  last_login_at: Date;
}
```

### VerificationCode
```typescript
interface VerificationCode {
  id: number;
  phone: string;
  code: string;            // 6-digit number
  expires_at: Date;        // Expires after 5 minutes
  used: boolean;           // Whether it's been used
  created_at: Date;
}
```
```

---

## tasks.md Writing Guide

### Tasks Should Be Atomic

Each task should be the smallest **independently completable** unit:

**❌ Too large**:
```markdown
- [ ] 1.1 Implement login feature
```

**✅ Atomic**:
```markdown
- [ ] 1.1 Create users database table
- [ ] 1.2 Create verification_codes database table
- [ ] 1.3 Implement POST /api/auth/send-code endpoint
- [ ] 1.4 Implement POST /api/auth/login endpoint
- [ ] 1.5 Add rate limiting for send-code endpoint
- [ ] 1.6 Write unit tests for send-code endpoint
- [ ] 1.7 Write unit tests for login endpoint
```

### Tasks Should Be Ordered

Sort by dependency:

```markdown
## 1. Infrastructure (do first)
- [ ] 1.1 Create database tables
- [ ] 1.2 Configure Redis connection

## 2. Backend API (depends on 1)
- [ ] 2.1 Implement send verification code endpoint
- [ ] 2.2 Implement login endpoint

## 3. Frontend Pages (depends on 2)
- [ ] 3.1 Create login page
- [ ] 3.2 Integrate APIs

## 4. Testing (last)
- [ ] 4.1 Unit tests
- [ ] 4.2 Integration tests
```

### Include Testing Tasks

```markdown
## 4. Testing
- [ ] 4.1 Unit tests
  - [ ] 4.1.1 Test verification code generation logic
  - [ ] 4.1.2 Test Token generation and validation
  - [ ] 4.1.3 Test rate limiting logic

- [ ] 4.2 Integration tests
  - [ ] 4.2.1 Test complete login flow
  - [ ] 4.2.2 Test error scenarios

- [ ] 4.3 Manual test checklist
  - [ ] 4.3.1 Test on iOS Safari
  - [ ] 4.3.2 Test on Android Chrome
  - [ ] 4.3.3 Test network disconnection scenario
```

---

## Common Mistakes

### 1. Specs Are Too Vague

```markdown
# ❌
Users can log into the system

# ✅
Users can log into the system using phone number + 6-digit verification code,
code is valid for 5 minutes, locked for 60 seconds after 3 failed attempts
```

### 2. Missing Error Scenarios

```markdown
# ❌ Only the happy path
User enters verification code, clicks login, redirects to homepage

# ✅ Includes error scenarios
Normal: User enters correct code, login succeeds, redirects to homepage
Error 1: Wrong code, show remaining attempts
Error 2: Expired code, prompt to request a new one
Error 3: Network error, prompt to check connection
```

### 3. Unclear Technical Approach

```markdown
# ❌
Store the Token somehow

# ✅
Token is stored in localStorage with key "auth_token",
JWT format, valid for 7 days, automatically cleared on expiry
with redirect to login page
```

---

## Next

→ [Chapter 7: Hands-On Examples](07-examples-en.md)
