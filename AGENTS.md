# Codex + Conductor Rules (Project)

This file defines how Codex should operate in this repository.

## Conductor First

- At startup, load the conductor status prompt from `.codex\\skills\\conductor-status\\SKILL.md`.
- Treat `conductor/` as the source of truth for scope, workflow, and commit rules.
- If Conductor rules conflict with other instructions, Conductor wins.
- Conductor artifacts are local-only always. Conductor artifacts must never be staged, committed or pushed.

## Debate Escalation

- If a coding problem cant be solved after 50 failed approaches write a prompt to `debate.md` with:
  - **Problem**: Clear statement of what's blocking progress
  - **Context**: Relevant background and state
  - **What I tried**: Specific attempts made (with evidence)
  - **Evidence/Logs**: Key errors, outputs, or observations
  - **Hypotheses**: Ranked theories about root cause
  - **Questions**: Specific asks for external agents
  - **Constraints**: Known limitations or requirements.

## Execution Discipline

- **Work doggedly**: Your goal is maximum autonomy. If the overall goal is clear (via Conductor) and progress is possible, continue until genuinely blocked. When stopping, be prepared to justify why.

- **Work smart**:
  - Before debugging, form hypotheses about root causes
  - Add strategic and extensive logging to validate assumptions (for example loguru)
  - Use binary search debugging when appropriate
  - Read error messages completely before acting

- **Check your work**:
  - After writing code, identify a verification method and execute it
  - After starting processes, wait 30s then check logs/status
  - Before claiming completion, verify against acceptance criteria
  - Run tests if they exist

- **Be cautious with terminal commands**:
  - Before each command, assess: will this exit cleanly or run indefinitely?
  - Never start long-running servers unless explicitly requested
  - For required background processes: use separate terminal + document PID + set timeout
  - Always check logs within 30s of starting any process,make sure you read the logs(or tail them)

- **Incremental progress**:
  - Commit logical chunks frequently (following Conductor rules,but not Conductor itself)
  - Document decisions in code comments or commit messages
  - Update relevant documentation as you go (Like the main root README.md and subsequent referenced READMES in root_repo/0.READMEs

## PowerShell Usage

- **Always user powershell 5.1**

## Safety

- **Never touch** `.env`, secrets, infrastructure config, or production data without explicit instruction.
- **Read-only by default** for: database connections, external APIs, file deletion operations.
- **Confirm before**: dropping tables, deleting branches, force-pushing, or modifying CI/CD configs.
- If a command could cause data loss, describe impact and wait for approval.

## Communication

- **Status updates**: Provide brief progress summaries every 5-10 minutes for long tasks.
- **Completion reports**: When done, summarize what was accomplished, what was learned, and any follow-up items.
- **Uncertainty**: If confidence in a solution is <70%, say so and explain why.

## Meta

- This file can be updated. If you discover patterns that should be codified, propose additions via debate.md.

