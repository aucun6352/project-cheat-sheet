# Creator Suite

Integrated content creation plugin - Create and manage Agents, Commands, and Skills from a single plugin.

## 📖 Overview

Creator Suite is an integrated solution that allows you to create three core content types that extend the Claude Code ecosystem from a single plugin:

- **🤖 Agent**: Sub-agents specialized for specific tasks
- **⚡ Command**: Slash commands that automate workflows
- **📚 Skill**: Skill guides that document knowledge and patterns

### Why Creator Suite?

**"If Claude Code automates most tasks, what if Claude could automatically create Agents, Commands, and Skills too?"**

This idea led to the birth of Creator Suite.

Claude Code automates many tasks such as writing code, debugging, and refactoring. However, these automation tools themselves (Agents, Commands, Skills) still had to be created manually.

Creator Suite is a **tool that creates tools**:

- 💬 **Create through conversation**: Generate Agents/Commands/Skills through dialogue without complex syntax
- 🤖 **Automatic analysis**: Analyze project and conversation patterns to recommend optimal tools
- ⚡ **Immediate use**: Instantly available in Claude Code upon creation
- 🎯 **Quality assurance**: Ensure high-quality tools through validation

**Meta-automation**: Automatically creates tools that help Claude automate better.

---

## 🚀 Quick Start

### 1. Agent Creation

Create sub-agents specialized for specific tasks.

```bash
/create-agent
```

**Creation location**: `.claude/agents/{name}.md`

**Types**:
- **Specialist**: Simple, clear tasks (1-3 tools)
- **Analyst**: Analysis and validation tasks (3-6 tools)
- **Orchestrator**: Complex multi-step coordination (6+ tools)

**Example**:
```bash
/create-agent

# Interactive prompts:
Name: api-integration-specialist
Description: REST API integration and error handling specialist
Type: Specialist
Tools: Bash, Read, Write
Model: sonnet
```

### 2. Command Creation

Automate repetitive workflows with Slash commands.

```bash
/create-command
```

**Creation location**: `.claude/commands/{name}.md` or `plugins/*/commands/{name}.md`

**Types**:
- **Simple Task Command**: Single task (50-150 words)
- **Workflow Pipeline Command**: Sequential execution (150-400 words)
- **Complex Orchestration Command**: Complex conditions/iterations (400+ words)

**Example**:
```bash
/create-command

# Interactive prompts:
Name: code-review
Description: Code review workflow that sequentially runs type check, tests, and lint
Type: Workflow Pipeline Command
```

### 3. Skill Creation

Document team knowledge and patterns.

```bash
/create-skill
```

**Creation location**: `{skill-name}/SKILL.md`

**Types**:
- **Quick Workflow**: Simple tips/tricks (50-200 words)
- **Comprehensive Guide**: Step-by-step guide (200-600 words)
- **Deep Dive**: In-depth analysis (600-1500 words)
- **Pattern Library**: Pattern collection (1500+ words)

**Example**:
```bash
/create-skill

# Interactive prompts:
Name: react-hooks-patterns
Description: React Hooks usage patterns and best practices
Type: Comprehensive Guide
```

### 4. Automatic Project Analysis and Generation

Analyze your project to automatically recommend and create customized Agents and Skills.

```bash
/project-tooling
```

**How it works**:
1. **Project Analysis**: Automatically detect meta files, tech stack, and business domain
2. **Customized Recommendations**: Recommend project-specific Agents/Skills by priority
3. **User Selection**: Select items to generate from recommendation list
4. **Automatic Generation**: Automatically invoke `/create-agent` and `/create-skill`

**Example Scenario**:
```bash
# Run in E-commerce project
/project-tooling

# Auto-detected:
# - Languages: TypeScript, Python
# - Frameworks: React, FastAPI
# - Business keywords: payment, order, cart

# Recommendations:
# ⭐ order-validation-specialist (project-specific)
# ⭐ payment-integration-coordinator (domain-specific)
# inventory-sync-manager (domain-specific)
# typescript-code-reviewer (general)
```

---

## 📚 Core Concepts

### Phase System

All generation commands follow a consistent 5-phase system:

```
Phase 0: Conversation Analysis (conditional)
  ↓
Phase 1: Basic Information Collection
  ↓
Phase 2: Detailed Content Creation
  ↓
Phase 3: Examples and Additional Information
  ↓
Phase 4: File Creation and Validation
```

### Conversation-Based Auto-Extraction

In Phase 0, analyze current session conversation history to:
- Automatically extract workflow patterns
- Auto-generate metadata (name, description, type)
- Support quick generation in recommendation mode

### 100-Point Quality Validation

Automatically validate generated documents in Phase 4:

| Area | Points |
|------|--------|
| Structure Validation | 20 points |
| Frontmatter Validation | 15-20 points |
| Required Sections Validation | 20-25 points |
| Content Quality Validation | 25 points |
| Type Suitability | 10 points |
| Completeness | 5-10 points |

**Grading System**:
- A (90-100 points): Excellent
- B (80-89 points): Good
- C (70-79 points): Average
- D (60-69 points): Poor
- F (<60 points): Fail

---

## 🎯 Usage Scenarios

### Scenario 1: Automating Repetitive Tasks

```bash
# Problem: Repeatedly doing authentication → call → validation for API testing

# Solution 1: Create Agent
/create-agent
Name: api-test-specialist
Type: Specialist

# Solution 2: Create Command
/create-command
Name: test-api
Type: Simple Task Command
```

### Scenario 2: Documenting Team Knowledge

```bash
# Problem: Want to share project-specific patterns with team members

# Solution: Create Skill
/create-skill
Name: project-architecture-patterns
Type: Comprehensive Guide

# Usage: Reference @project-architecture-patterns/SKILL.md in conversations
```

### Scenario 3: Setting Up New Project

```bash
# Problem: Need customized tools for new project

# Solution: Automatic project analysis
/project-tooling

# Result: Automatically recommend and generate 3 project-specific Agents, 2 Skills
```

---

## 📖 Advanced Usage

### Conversation-Based Generation

Automatically generate based on current conversation content:

```
User: "When doing code review, always type check first, then run tests, then lint"
```

(Continued in original document structure...)
