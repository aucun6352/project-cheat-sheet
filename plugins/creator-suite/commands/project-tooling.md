---
name: project-tooling
description: "A workflow that analyzes the project to identify required roles, tech stack, and key features, then generates customized agents and skills prioritizing non-development and project-specific content"
---

# /project-tooling

An intelligent workflow that automatically generates customized development tools by deeply analyzing project characteristics.

This command analyzes project metadata, tech stack, and business logic to prioritize recommending sub-agents and skills that are **project-specific**. The goal is to create tools that contain unique requirements and business rules for the project, rather than general development knowledge easily found on the web.

For example, for an E-commerce project, it will prioritize project-specific tools like "Order Validation Logic Specialist" or "Payment Integration Specialist" over generic "TypeScript Code Reviewer".

## Triggers

Use this command in the following situations:

- When customized development tools are needed for a new project
- When you want to generate project-specific agents and skills all at once
- When you want to toolify project business logic or domain knowledge
- When building a project knowledge base for team collaboration
- Explicit invocation: Execute `/project-tooling`

## Usage

```bash
/project-tooling
```

The current version runs without flags or options. Executing it starts an interactive interface where you can select from recommended lists.

## Behavioral Flow

### Phase 1: In-depth Project Analysis

Understand the overall structure and characteristics of the project.

**Steps:**
1. **Search Meta Files**: Use Glob to search for language-specific meta files like package.json, requirements.txt, go.mod, pom.xml, Gemfile
2. **Read Meta Information**: Use Read to parse discovered meta files and analyze dependencies, scripts, and configuration
3. **Detect Tech Stack**: Identify language, framework, and major libraries
4. **Classify Project Type**: Determine business domain such as E-commerce, SaaS, CMS, API
5. **Evaluate Uniqueness**: Identify special patterns, rules, and requirements unique to this project

**Conditions:**
- If multiple meta files found: Treat as multi-language project, analyze all tech stacks
- If no meta files found: Infer language based on file extensions
- If business keywords found (payment, order, inventory, etc.): Increase priority of domain-specific tools

**Error Handling:**
- Meta file parsing failure: Ignore error and continue analyzing next file
- Project type unclear: Request user to select project type via AskUserQuestion
  - Options: E-commerce, SaaS, CMS, API Service, Financial, Healthcare, Education, Media/Content, DevOps/Infrastructure, Other (manual input)

### Phase 2: Sub-agent Recommendation and Generation

Recommend and generate project-specific sub-agents based on analysis results.

**Steps:**
1. **Generate Agent Candidates**: Recommend 3-5 agents based on project analysis results
2. **Determine Priority**:
   - **Highest**: Project-specific business logic (e.g., e-commerce-order-validator)
   - **Medium**: Project domain-specific (e.g., payment-integration-specialist)
   - **Lowest**: General development tools (e.g., typescript-reviewer)
3. **User Selection Interface**: Present recommendation list via AskUserQuestion (sorted by priority)
4. **Agent Creation**: Invoke `/create-agent` via SlashCommand for selected agents
5. **Confirm Results**: Display generated agent file paths and quality scores

**Validation:**
- Verify recommendation list has at least 3 items
- Verify at least 1 project-specific agent is included
- Limit general agents to 50% or less of total

**Error Handling:**
- If `/create-agent` fails: Display error message and continue with next item
- If user makes no selection: Skip to Phase 3

### Phase 3: Skill Recommendation and Generation

Document project knowledge and business rules as skills.

**Steps:**
1. **Generate Skill Candidates**: Recommend 3-5 skills based on project analysis results
2. **Determine Priority**:
   - **Highest**: Project business rules and policies (e.g., e-commerce-business-rules)
   - **Medium**: Domain-specific patterns (e.g., payment-security-patterns)
   - **Lowest**: General development best practices (e.g., react-best-practices)
3. **User Selection Interface**: Present recommendation list via AskUserQuestion (sorted by priority)
4. **Skill Creation**: Invoke `/create-skill` via SlashCommand for selected skills
5. **Confirm Results**: Display generated skill file paths

**Validation:**
- Verify recommendation list has at least 3 items
- Verify at least 1 project-specific skill is included
- Limit general skills to 50% or less of total

**Error Handling:**
- If `/create-skill` fails: Display error message and continue with next item
- If user makes no selection: End workflow

## Tool Coordination

### Primary Tools

- **Glob**: Project meta file pattern search
  - Purpose: Find language-specific meta files like `package.json`, `requirements.txt`, `*.csproj`
  - Usage timing: At Phase 1 start

- **Read**: Read meta file contents
  - Purpose: Parse discovered meta file contents to extract dependencies, scripts, project information
  - Usage timing: After finding files with Glob

- **AskUserQuestion**: User selection interface
  - Purpose: User selects items to generate from recommended agent/skill lists
  - Usage timing: Once each in Phase 2 and Phase 3

- **SlashCommand**: Invoke sub-commands
  - Purpose: Execute `/create-agent` and `/create-skill`
  - Usage timing: After user selects items

### Secondary Tools

- **Grep**: Search specific patterns in project (optional)
  - Purpose: Identify domain by searching business keywords (payment, order, inventory, etc.)
  - Usage timing: During Phase 1 project type classification

## Key Patterns

### Priority Scoring System

Assign scores to each agent/skill candidate based on project analysis results:

```
Score Calculation:
- Project-specific business logic: +100 points
- Domain-specific (payment, order, etc.): +50 points
- Project tech stack related: +30 points
- General development tools: +10 points

Example:
"e-commerce-order-validator" (specific logic) = 100 points
"payment-integration-specialist" (domain-specific) = 50 points
"typescript-reviewer" (general) = 10 points

→ Sort by score and present to user
```

### Non-development Content Detection

The following elements are prioritized as "non-development/project-specific" content:

- **Business Rules**: Order validation logic, pricing calculation rules, discount policies
- **Domain Knowledge**: Industry-specific regulations and standards in finance, healthcare, logistics
- **Company Policies**: Data retention policies, security requirements, access control rules
- **Workflows**: Approval processes, notification rules, state transition logic
- **Integration Specifications**: External API integration methods, legacy system communication protocols

Conversely, the following have lower priority as "general development content":
- Code formatting, linting rules
- Framework best practices
- General design patterns
- General testing strategies

## Examples

### Example 1: E-commerce Project

```bash
/project-tooling
```

**Project Analysis Results:**
```
📊 Project Analysis Complete!

🔍 Detected Environment:
- Language: TypeScript, Python
- Framework: React (frontend), FastAPI (backend)
- Major Libraries: stripe, sendgrid, redis
- Project Type: E-commerce Platform

🎯 Business Keywords Found: payment, order, cart, checkout, inventory
```

**Phase 2: Sub-agent Recommendations**
```
💡 Recommended Sub-agents (Project-specific priority):

1. ⭐ order-validation-specialist (Project-specific, 100 points)
   - Role: Order validation logic specialist (inventory, price, discount rules)
   - Type: Analyst

2. ⭐ payment-integration-coordinator (Domain-specific, 50 points)
   - Role: Stripe payment integration and error handling specialist
   - Type: Orchestrator

3. inventory-sync-manager (Domain-specific, 50 points)
   - Role: Inventory synchronization and notification management
   - Type: Specialist

4. typescript-code-reviewer (General, 10 points)
   - Role: TypeScript code quality verification
   - Type: Analyst

Which agents would you like to create? (Multiple selection allowed)
```

**User Selection:** Selected 1, 2

**Phase 3: Skill Recommendations**
```
💡 Recommended Skills (Project knowledge priority):

1. ⭐ e-commerce-business-rules (Project-specific, 100 points)
   - Content: Order rules, discount policies, return regulations, inventory management rules

2. ⭐ payment-security-patterns (Domain-specific, 50 points)
   - Content: PCI-DSS compliance, payment data encryption, fraud detection patterns

3. react-state-management (General, 10 points)
   - Content: React state management best practices

Which skills would you like to create? (Multiple selection allowed)
```

**User Selection:** Selected 1, 2

**Expected Outcome:**
```
✅ Project Tooling Complete!

📂 Generated Sub-agents:
- .claude/agents/order-validation-specialist.md (Quality: 95/100)
- .claude/agents/payment-integration-coordinator.md (Quality: 92/100)

📂 Generated Skills:
- e-commerce-business-rules/SKILL.md
- payment-security-patterns/SKILL.md

💡 Next Steps:
1. Agents are automatically activated and execute during related tasks
2. Reference skills in conversation using @{skill-name}/SKILL.md format
```

**Troubleshooting:**
- **Issue**: "Cannot detect project type"
  - **Solution**: Verify meta files like package.json exist in project root
- **Issue**: "Recommendations are too general"
  - **Solution**: Verify business logic is clearly expressed in project code

### Example 2: Financial API Service

```bash
/project-tooling
```

**Project Analysis Results:**
```
📊 Project Analysis Complete!

🔍 Detected Environment:
- Language: Go
- Framework: Gin
- Major Libraries: jwt-go, gorm, kafka-go
- Project Type: Financial API Service

🎯 Business Keywords Found: transaction, account, balance, audit
```

**Recommended Agents:**
1. ⭐ transaction-audit-specialist (Project-specific, 100 points)
2. ⭐ financial-compliance-checker (Domain-specific, 50 points)
3. go-api-performance-analyzer (General, 10 points)

**Recommended Skills:**
1. ⭐ financial-transaction-patterns (Project-specific, 100 points)
2. ⭐ audit-trail-requirements (Domain-specific, 50 points)
3. go-concurrency-patterns (General, 10 points)

**Expected Outcome:**
Tools reflecting the project's financial domain characteristics are generated, with tools specialized in financial regulation and audit trails prioritized over general Go development tools.

### Example 3: Multi-language Project

```bash
/project-tooling
```

**Project Analysis Results:**
```
📊 Project Analysis Complete!

🔍 Detected Environment:
- Language: TypeScript, Python, Rust
- Framework: Next.js (frontend), Django (backend), Tauri (desktop)
- Project Type: Desktop Application with Web Dashboard

🎯 Multi-language Project Detected: Recommending language-specific tools
```

**Recommended Agents:**
1. ⭐ cross-platform-sync-coordinator (Project-specific, 100 points)
2. tauri-integration-specialist (Domain-specific, 50 points)
3. typescript-code-reviewer (General, 10 points)

**Expected Outcome:**
Tools specialized in cross-platform synchronization and integration are prioritized, reflecting the multi-language project characteristics.

## Boundaries

**Will:**
- Analyze project meta files (package.json, requirements.txt, etc.) to detect tech stack
- Classify project type by analyzing business keywords and domain patterns
- Provide type selection interface to user when project type is unclear
- Prioritize tools related to project-specific business logic
- Treat non-development/domain-specific content with higher priority than general development knowledge
- Provide clear priority information with selection interface to user
- Delegate actual file generation using `/create-agent` and `/create-skill` commands
- Display quality scores and file paths of generated tools

**Will Not:**
- Not automatically generate agent or skill files without user confirmation
- Not recommend general development tools with higher priority than project-specific tools
- Not access directories or files outside the project
- Not modify or change meta file contents (read-only)
- Not force continuation when agent/skill generation fails (display error and continue)

**Safety Checks:**
- Search files only within project root directory
- Safely ignore meta file parsing errors and continue with next file
- Request user selection instead of auto-estimating when project type is unclear
- Display warning if recommendation list has fewer than 3 items
- Display "Only general tools recommended" warning if 0 project-specific items
- Verify validity before invoking `/create-agent` and `/create-skill`
- Do not force continuation if user makes no selection; terminate instead
