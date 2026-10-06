# Claude Code Tuts

## Getting Started

- `claude` — Starts Claude Code.
- `claude -r` — Resumes a previous conversation/session.
- `/resume` — Switches to or resumes another session.

### Session Best Practices

1. **One session = one task/feature**
   - Keep each session focused on a single task or feature.

2. **Name your session immediately**
   - Use `/rename` to give the session a meaningful name.
   - Example:
     ```text
     /rename intro-session
     ```

3. **Commit frequently**
   - Make Git commits regularly within a session so you can easily revert changes if something goes wrong.

4. **Export a session before a big refactor**
   - Save/export the session before making major architectural or code changes.

5. `/btw` — Ask a side question without adding it to the main conversation context.
   - Useful when you want to ask something temporarily without affecting the ongoing task.

6. `/logout` — Log out of Claude Code.

---

# Models

Claude Code provides different models for different types of tasks.

### 1. Opus

- Most powerful model.
- More expensive.
- Best for complex programming and difficult reasoning tasks.
- Useful for:
  - Architecture planning
  - Complex debugging
  - System design
  - Writing specifications
  - Making important technical decisions

### 2. Sonnet

- Default/balanced model for many users.
- Good balance between:
  - Speed
  - Quality
  - Token cost
- Suitable for everyday coding and implementation tasks.

### 3. Haiku

- Fastest and cheapest option.
- Best for simple and repetitive tasks where deep reasoning is not required.

### Common Model Workflow

A common workflow is:

**Opus → Planning → Sonnet → Implementation**

For example:

1. Use **Opus** during the planning phase:
   - Think through the architecture.
   - Write specifications.
   - Analyze requirements.
   - Make technical decisions.

2. Switch to **Sonnet** during implementation:
   - The architecture and decisions are already defined.
   - Claude mainly needs to implement the planned solution reliably.

> **Note:** Model names, availability, pricing, and default models can change over time.

---

# Useful Commands

| Command | Purpose |
|---|---|
| `/model` | Change the current model |
| `/usage` | View token/usage information |
| `/stats` | View usage statistics |
| `/insights` | Generate an HTML report based on usage and provide suggestions |
| `/exit` | Exit Claude Code |
| `/config` | Manage configuration/settings |
| `/permissions` | Manage tool permissions |
| `/context` | Display context-window usage |
| `/compact` | Manually compact the conversation context |
| `/clear` | Clear the current session/context |

---

# Context Window

A **context window** is the amount of information, measured in tokens, that the model can access at one time while generating a response.

Think of it as the model's **working memory**.

Context can include:

- Your messages
- Claude's previous responses
- Codebase information
- PRDs/specifications
- Jira/GitHub issues
- Slack messages
- Tool results
- `CLAUDE.md`
- Skills
- MCP information
- System instructions

### Context Window Size

Claude Code supports large context windows, but the exact size can vary by model/version and configuration.

> **Important:** Don't rely on a fixed `200k` token limit without checking the current model/documentation.

---

# What Happens When Context Gets Full?

As the context window becomes heavily used, several things can happen:

### Stage 1 — Quality Degradation

As more information accumulates, the model may have more difficulty maintaining focus on the most relevant information.

### Stage 2 — Auto Compaction

Claude Code can automatically compact older conversation context when the context becomes sufficiently full.

### Stage 3 — Repeated Compaction

Repeated compaction can make it harder to preserve every detail from the original conversation.

### Stage 4 — Context Limit

Eventually, the session can reach the model's context limit and require you to clear, compact, or start a new session.

---

# `/context`

Use:

```text
/context
```

to view information about the current context usage.

It can help you understand how much of the available context is being consumed by things such as:

- System prompt
- Tool schemas
- `CLAUDE.md`
- Conversation history
- Tool results
- Skills
- MCP
- Other context
- Available/free context

---

# Why Does Context Window Matter?

The context window is important because it affects:

### 1. Cost

More context means more tokens are processed, which can affect usage/cost depending on the model and plan.

### 2. Response Quality

As the context becomes very large, the model may have difficulty identifying the most relevant information.

### 3. Workflow

Large, long-running sessions can become harder to manage.

Therefore, structuring your Claude Code workflow properly is important.

---

# Subagents

Subagents can be used for **isolated or exploratory tasks**.

A subagent gets its own context separate from the main session.

For example:

```text
Main Session
     │
     ├── Subagent 1 → Explore authentication code
     │
     ├── Subagent 2 → Investigate database structure
     │
     └── Subagent 3 → Analyze testing requirements
```

The subagent works independently and returns relevant findings to the main session rather than filling the main context with its entire working history.

This makes subagents useful for:

- Research
- Exploration
- Codebase investigation
- Independent debugging
- Parallel analysis

---

# Auto Compaction

Claude Code can automatically compact the conversation when the context becomes heavily utilized.

You can also manually compact the context using:

```text
/compact
```

### Good Practice

Use `/compact` **proactively**, rather than waiting until the context is almost completely full.

For example:

```text
Long feature development
        ↓
Context grows
        ↓
/compact
        ↓
Continue implementation
```

---

# `/clear`

Use:

```text
/clear
```

to clear the current conversation context and start fresh.

Use it when:

- The current task is finished.
- The conversation has become too large.
- You want to start a completely different task.
- Old context is no longer useful.

---

# Recommended Claude Code Workflow

A good workflow is:

### 1. One Session Per Feature

Keep each session focused.

```text
Session 1 → Authentication
Session 2 → Payment integration
Session 3 → Admin dashboard
Session 4 → AI/RAG feature
```

This prevents unrelated conversations from accumulating in the same context.

### 2. Name Sessions Immediately

Use:

```text
/rename authentication-feature
```

Meaningful names make sessions easier to identify and resume later.

### 3. Plan Before Implementing

For complex tasks:

```text
Requirements
    ↓
Architecture
    ↓
Implementation Plan
    ↓
Coding
    ↓
Testing
    ↓
Commit
```

Use a powerful reasoning model for complex planning when appropriate, then use a faster/balanced model for implementation.

### 4. Commit Frequently

Don't wait until the entire feature is finished.

```text
git add .
git commit -m "Add authentication API"

git add .
git commit -m "Add login UI"

git add .
git commit -m "Add authentication middleware"
```

Frequent commits give you safe rollback points.

### 5. Compact Proactively

Don't wait until the context is almost full.

Use:

```text
/compact
```

when the conversation has accumulated a lot of irrelevant or old context.

### 6. Use Subagents for Isolated Work

Delegate independent investigations or exploratory tasks to subagents so the main session remains focused.

### 7. Keep Irrelevant Files Out of Context

Use `.claudeignore` where supported to prevent unnecessary files from being included in Claude's working context.

For example, exclude things such as:

```text
node_modules/
.next/
dist/
build/
.env
*.log
```

This keeps the context cleaner and helps Claude focus on the actual project.

---

# Quick Cheat Sheet

```text
claude              → Start Claude Code
claude -r           → Resume a previous session
/resume             → Resume/switch session
/rename             → Rename current session
/btw                → Ask a side question
/model              → Change model
/usage              → Check usage
/stats              → View statistics
/insights            → Generate usage insights
/context            → View context usage
/compact             → Compact context
/clear               → Clear current session
/config              → Configuration
/permissions         → Manage permissions
/logout              → Log out
/exit                → Exit Claude Code
```

## Core Principles

> **One session → One feature → Plan → Implement → Test → Commit → Compact when needed**

The main goal is to keep the **session focused, the context clean, and the codebase recoverable**.
