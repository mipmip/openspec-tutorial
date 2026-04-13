# Chapter 10: Frequently Asked Questions

## Installation Issues

### Q: Permission error when installing OpenSpec

**A**: Use admin privileges or npx

```bash
# macOS/Linux
sudo npm install -g @fission-ai/openspec@latest

# Windows (admin PowerShell)
npm install -g @fission-ai/openspec@latest

# Or use npx (no installation needed)
npx @fission-ai/openspec@latest
```

### Q: Command not found after installation

**A**: Check the npm global path

```bash
# Check npm global install path
npm config get prefix

# Make sure the path is in your PATH
# macOS/Linux
export PATH="$PATH:$(npm config get prefix)/bin"

# Windows
# Add the path to your system PATH environment variable
```

### Q: Node.js version is too old

**A**: Upgrade Node.js to 20.19.0+

```bash
# Using nvm
nvm install 20
nvm use 20

# Or using fnm
fnm install 20
fnm use 20

# Or download from the official website
# https://nodejs.org/
```

---

## Usage Issues

### Q: /opsx:propose generates specs that don't match expectations

**A**: Provide a more detailed description

```bash
# ❌ Too vague
/opsx:propose "Improve login"

# ✅ More detailed
/opsx:propose "Add phone number verification code login,
  support sending 6-digit verification codes,
  codes valid for 5 minutes,
  locked for 60 seconds after 3 failed attempts"
```

### Q: AI drifts from the spec during implementation

**A**: Correct it promptly and update the spec

```
"Hold on -- design.md says to use JWT,
 not Sessions. Please implement according to the spec."

or

"I've updated design.md to use the Session approach instead.
 Please implement according to the new spec."
```

### Q: A problem is found in the spec during implementation

**A**: This is normal -- specs can be modified

```
"While implementing 1.3, I found a problem with the approach
 in design.md because [reason]. It needs to be changed to [new approach]."

The AI will update the spec documents and continue implementing.
```

### Q: /opsx:apply stopped halfway through

**A**: Continue the conversation and tell the AI to resume

```
Continue executing the incomplete tasks in tasks.md
```

The AI will read tasks.md, find the incomplete tasks (checked tasks are done), and continue from where it left off.

### Q: How to see which changes are currently in progress

**A**: Check the changes/ directory

```bash
ls openspec/changes/
```

### Q: How to see completed features

**A**: Check the archive/ directory

```bash
ls openspec/archive/
```

---

## Spec Document Issues

### Q: How detailed should proposal.md be?

**A**: Detailed enough for team members to understand

```markdown
# Good proposal

## Background
The system currently has no user authentication; anyone can access sensitive data.

## Goals
Implement phone number + verification code login

## Scope
Included: phone login
Not included: email login, WeChat login
```

No need to write technical details -- that's what design.md is for.

### Q: How many scenarios should specs/ include?

**A**: Cover the normal flow and major error flows

```markdown
## Scenarios

### Normal Scenario
User enters correct verification code, login succeeds

### Error Scenarios
1. Wrong verification code
2. Expired verification code
3. Network error

### Edge Cases
1. Frequent verification code requests
2. Invalid phone number format
```

### Q: Must the technical approach in design.md be followed strictly?

**A**: No, it can be adjusted

- Before implementation: design.md can be modified
- During implementation: adjust the approach if issues are found
- After implementation: record deviations in summary.md

### Q: How granular should tasks in tasks.md be?

**A**: The smallest independently completable unit

```markdown
# ❌ Too large
- [ ] Implement login feature

# ✅ Just right
- [ ] 1.1 Create users database table
- [ ] 1.2 Implement send verification code endpoint
- [ ] 1.3 Implement verification code login endpoint
```

---

## Team Collaboration Issues

### Q: Multiple people modifying the same change

**A**: Avoid this situation

- Each change should be owned by one person
- Use Git branches for management
- Tag assignees in tasks.md

### Q: How to share specs with team members

**A**: Through Git

```bash
git add openspec/changes/add-feature/
git commit -m "Add spec for user login feature"
git push
```

Team members will see the latest specs after pulling.

### Q: How do new members get up to speed quickly?

**A**: Use /opsx:onboard

```bash
/opsx:onboard
```

The AI will generate a project overview document.

---

## Performance Issues

### Q: AI implementation is very slow

**A**: Possible causes

1. **Context too large** - Clear the AI context, start a new conversation
2. **Too many tasks** - Split into smaller changes
3. **Model choice** - Use a more capable model (Opus 4.5, GPT 5.2)

### Q: How to speed up development

**A**: Let the AI batch-execute all tasks

In your AI tool:

```
/opsx:apply
```

The AI will automatically execute all tasks in tasks.md order. Best suited for cases where specs are clear and tasks are straightforward.

---

## Other Questions

### Q: Which AI tools does OpenSpec support?

**A**: 20+ tools

- Claude Code
- Codex (OpenAI)
- Pi
- Kiro
- Cursor
- GitHub Copilot
- ...

### Q: How to update OpenSpec

**A**:

```bash
npm install -g @fission-ai/openspec@latest
openspec update
```

### Q: How to uninstall OpenSpec

**A**:

```bash
npm uninstall -g @fission-ai/openspec
```

### Q: Is OpenSpec free?

**A**: Yes, it's MIT open source licensed

---

## Getting Help

### Official Resources

- GitHub: https://github.com/Fission-AI/OpenSpec
- Documentation: https://openspec.dev/
- Discord: https://discord.gg/YctCnvvshC

### Reporting Issues

Report on GitHub Issues:
https://github.com/Fission-AI/OpenSpec/issues

---

**Congratulations! You've completed the full OpenSpec tutorial.**

Now you can start using OpenSpec in your own projects!

```bash
# Start your first change
/opsx:propose "Add a new feature"
```
