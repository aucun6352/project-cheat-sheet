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

**Key Features:**
- **Comprehensive Recommendations**: Generates up to 10-15 agent/skill recommendations based on project complexity
- **Smart Duplicate Detection**: Automatically detects and marks already-created items with ✅
- **Iterative Usage**: Run multiple times to see remaining recommendations (previously created items are shown but non-selectable)
- **Simple Priority System**: Categorizes recommendations as Highest, Medium, or Lowest priority
- **Project-Specific Focus**: Prioritizes unique project patterns over general development knowledge

**Typical Workflow:**
1. First run: Analyze project and create initial set of agents/skills
2. Subsequent runs: View remaining recommendations with previously created items marked ✅
3. Select additional tools as project evolves and new patterns emerge

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

### Phase 2: Project-Specific Pattern Analysis

Deeply analyze patterns unique to this project that are difficult to find on the internet.

**Analysis Targets:**

1. **Business Concepts & Rules**:
   - Domain models and business entities (e.g., financial transaction models, medical record structures)
   - Business validation rules and constraints (e.g., order validation logic, pricing calculation rules)
   - Business workflow patterns (e.g., approval processes, state transitions)
   - Industry-specific regulations and compliance requirements

2. **Unique Conventions & Patterns**:
   - Non-standard architectural patterns (e.g., custom layered architecture, unique module structure)
   - Project-specific error handling strategies (e.g., custom error codes, domain-specific exceptions)
   - Naming conventions and code organization patterns unique to this project
   - Custom authentication/authorization mechanisms

3. **Special Technology Usage**:
   - Uncommon library combinations or integrations
   - Custom framework extensions or modifications
   - Unique API integration patterns with external services
   - Special performance optimization techniques specific to project needs

**Analysis Process:**

1. **Search for Business Logic**: Use Grep to find business-related keywords (order, payment, transaction, validation, policy, rule, etc.)
2. **Analyze Directory Structure**: Identify non-standard patterns in project organization
3. **Review Configuration Files**: Check for custom configurations, environment-specific settings
4. **Calculate Uniqueness Score**:
   ```
   Uniqueness Score (0-100) =
     + Business domain specificity: 0-40 points
     + Custom pattern usage: 0-30 points
     + Special tech integration: 0-20 points
     + Convention uniqueness: 0-10 points
   ```

**Output:**

- **Uniqueness Score**: Overall project specificity score (0-100)
- **Top Distinctive Aspects**: Top 3 most unique characteristics of this project
- **Specific Patterns**: Patterns not found in general tutorials or documentation
- **Recommendation Focus**: Areas to prioritize for agent/skill generation

**Conditions:**

- If uniqueness score ≥70: Focus heavily on project-specific agents/skills (80%+ of recommendations)
- If uniqueness score 40-69: Balance project-specific and domain-specific recommendations (60%+ project-specific)
- If uniqueness score <40: Include more general development tools while still prioritizing project patterns (40%+ project-specific)

**Error Handling:**

- If pattern analysis fails: Use default balanced recommendation strategy
- If no distinctive patterns found: Ask user to describe unique aspects via AskUserQuestion

### Phase 3: Sub-agent Recommendation and Generation

Recommend and generate project-specific sub-agents based on analysis results.

**Steps:**
1. **Generate Agent Candidates**:
   - Recommend 3-15 agents based on project uniqueness and complexity
   - **Check for existing agents**: Scan `.claude/agents/` directory to identify already created agents

2. **Determine Priority**:
   - **Highest**: Project-specific business logic and unique patterns (e.g., e-commerce-order-validator)
   - **Medium**: Domain-specific patterns (e.g., payment-integration-specialist)
   - **Lowest**: General development tools (e.g., typescript-reviewer)

3. **User Selection Interface**:
   - Present full recommendation list via AskUserQuestion (sorted by priority)
   - Format for each option:
     - **Active**: `⭐ [agent-name] ([Priority])`
     - **Already created**: `✅ [agent-name] - Already created ([Priority])`
   - Already created agents are shown but marked as non-selectable in description
   - Display total count and breakdown by category (Project-specific: X, Domain: Y, General: Z)

4. **Agent Creation**: Invoke `/create-agent` via SlashCommand for selected agents (skip items marked as "Already created")
5. **Confirm Results**: Display generated agent file paths and quality scores

**Validation:**
- Verify recommendation list has at least 3 items
- Verify at least 1 project-specific agent is included
- Limit general agents to 50% or less of total

**Error Handling:**
- If `/create-agent` fails: Display error message and continue with next item
- If user makes no selection: Skip to Phase 4

### Phase 4: Skill Recommendation and Generation

Document project knowledge and business rules as skills.

**Steps:**
1. **Generate Skill Candidates**:
   - Recommend 3-15 skills based on project uniqueness and complexity
   - **Check for existing skills**: Scan `.claude/skills/` directory to identify already created skills

2. **Determine Priority**:
   - **Highest**: Project business rules and unique policies (e.g., e-commerce-business-rules)
   - **Medium**: Domain-specific patterns (e.g., payment-security-patterns)
   - **Lowest**: General development best practices (e.g., clean-code-principles)

3. **User Selection Interface**:
   - Present full recommendation list via AskUserQuestion (sorted by priority)
   - Format for each option:
     - **Active**: `⭐ [skill-name] ([Priority])`
     - **Already created**: `✅ [skill-name] - Already created ([Priority])`
   - Already created skills are shown but marked as non-selectable in description
   - Display total count and breakdown by category (Project-specific: X, Domain: Y, General: Z)

4. **Skill Creation**: Invoke `/create-skill` via SlashCommand for selected skills (skip items marked as "Already created")
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

### Priority System

Recommendations are categorized into three priority levels based on project analysis:

**Priority Levels:**

- **Highest**: Project-specific business logic and unique patterns
  - Contains requirements and rules unique to this project
  - Not easily found in general tutorials or documentation
  - Examples: Custom order validation workflows, proprietary business rules, project-specific state machines

- **Medium**: Domain-specific patterns and industry knowledge
  - Applicable to the project's business domain (e-commerce, finance, healthcare, etc.)
  - Requires domain expertise but follows known patterns
  - Examples: Payment integration patterns, regulatory compliance frameworks, industry-standard workflows

- **Lowest**: General development tools and best practices
  - Universally applicable development knowledge
  - Widely available in documentation and tutorials
  - Examples: Code quality tools, framework best practices, general design patterns

**Presentation:**
1. Sort all candidates by priority (Highest → Medium → Lowest)
2. Within same priority, sort by relevance to project
3. Show 3-15 top candidates
4. Mark already-created items with ✅
5. Display priority level with each option

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
📊 Phase 1: Project Analysis Complete!

🔍 Detected Environment:
- Language: TypeScript, Python
- Framework: React (frontend), FastAPI (backend)
- Major Libraries: stripe, sendgrid, redis
- Project Type: E-commerce Platform

🎯 Business Keywords Found: payment, order, cart, checkout, inventory

📊 Phase 2: Project Specificity Analysis Complete!

🎯 Uniqueness Score: 75/100 (High uniqueness - Project-specific focus)

Top 3 Distinctive Aspects:
1. Custom order validation workflow with multi-step approval process
2. Proprietary discount calculation engine with complex business rules
3. Real-time inventory sync across multiple warehouses

💡 Recommendation Focus: 80% project-specific, 15% domain-specific, 5% general
```

**Phase 3: Sub-agent Recommendations**
```
💡 Recommended Sub-agents (12 candidates, sorted by priority):

📊 Total: 12 agents | Project-specific: 8 | Domain: 3 | General: 1

[Highest Priority - Project-specific]
1. ⭐ order-validation-specialist
   - Role: Multi-step order validation with approval workflow specialist
   - Type: Analyst

2. ⭐ discount-calculation-engine
   - Role: Complex discount rules and promotional pricing specialist
   - Type: Specialist

3. ⭐ inventory-sync-coordinator
   - Role: Real-time multi-warehouse inventory synchronization
   - Type: Orchestrator

4. ⭐ cart-abandonment-handler
   - Role: Custom cart recovery and notification logic
   - Type: Specialist

5. ⭐ shipping-rate-calculator
   - Role: Dynamic shipping rate calculation with carrier integration
   - Type: Analyst

6. product-recommendation-engine
   - Role: Personalized product recommendation logic
   - Type: Specialist

7. customer-segmentation-analyst
   - Role: Customer behavior analysis and segmentation
   - Type: Analyst

8. email-campaign-coordinator
   - Role: Automated marketing email workflow management
   - Type: Orchestrator

[Medium Priority - Domain-specific]
9. ⭐ payment-integration-coordinator
   - Role: Stripe payment integration and error handling specialist
   - Type: Orchestrator

10. fraud-detection-specialist
    - Role: Payment fraud pattern detection and prevention
    - Type: Analyst

11. tax-calculation-specialist
    - Role: Multi-jurisdiction tax calculation logic
    - Type: Specialist

[Lowest Priority - General]
12. react-performance-optimizer
    - Role: Frontend performance optimization specialist
    - Type: Specialist

Which agents would you like to create? (Multiple selection allowed)
```

**User Selection:** Selected 1, 2, 3, 4, 5

**Phase 4: Skill Recommendations**
```
💡 Recommended Skills (10 candidates, sorted by priority):

📊 Total: 10 skills | Project-specific: 7 | Domain: 2 | General: 1

[Highest Priority - Project-specific]
1. ⭐ e-commerce-business-rules
   - Content: Order validation rules, discount policies, return regulations, inventory rules

2. ⭐ multi-step-approval-workflow
   - Content: Custom approval process logic and state transitions

3. ⭐ warehouse-sync-protocols
   - Content: Real-time inventory synchronization patterns and conflict resolution

4. ⭐ discount-calculation-formulas
   - Content: Promotional pricing algorithms, coupon validation logic

5. ⭐ cart-recovery-strategies
   - Content: Abandoned cart detection and recovery notification patterns

6. shipping-carrier-integration
   - Content: Multi-carrier API integration and rate comparison logic

7. customer-lifecycle-management
   - Content: Customer journey stages and lifecycle marketing rules

[Medium Priority - Domain-specific]
8. ⭐ payment-security-patterns
   - Content: PCI-DSS compliance, payment data encryption, fraud detection

9. tax-jurisdiction-rules
   - Content: Tax calculation patterns for different jurisdictions

[Lowest Priority - General]
10. react-state-patterns
    - Content: React state management best practices

Which skills would you like to create? (Multiple selection allowed)
```

**User Selection:** Selected 1, 2, 3, 4, 5, 6

**Expected Outcome:**
```
✅ Project Tooling Complete!

📂 Generated Sub-agents (5 of 12 selected):
- .claude/agents/order-validation-specialist.md (Quality: 95/100)
- .claude/agents/discount-calculation-engine.md (Quality: 93/100)
- .claude/agents/inventory-sync-coordinator.md (Quality: 94/100)
- .claude/agents/payment-integration-coordinator.md (Quality: 92/100)
- .claude/agents/cart-abandonment-handler.md (Quality: 90/100)

📂 Generated Skills (6 of 10 selected):
- .claude/skills/e-commerce-business-rules/SKILL.md
- .claude/skills/multi-step-approval-workflow/SKILL.md
- .claude/skills/warehouse-sync-protocols/SKILL.md
- .claude/skills/payment-security-patterns/SKILL.md
- .claude/skills/discount-calculation-formulas/SKILL.md
- .claude/skills/cart-recovery-strategies/SKILL.md

💡 Next Steps:
1. Agents are automatically activated and execute during related tasks
2. Reference skills in conversation using @{skill-name}/SKILL.md format
3. Run /project-tooling again to see remaining recommendations (marked with ✅ for already created)
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
📊 Phase 1: Project Analysis Complete!

🔍 Detected Environment:
- Language: Go
- Framework: Gin
- Major Libraries: jwt-go, gorm, kafka-go
- Project Type: Financial API Service

🎯 Business Keywords Found: transaction, account, balance, audit

📊 Phase 2: Project Specificity Analysis Complete!

🎯 Uniqueness Score: 82/100 (Very high uniqueness - Heavy project-specific focus)

Top 3 Distinctive Aspects:
1. Complex audit trail system with immutable transaction logs
2. Multi-currency transaction processing with real-time exchange rates
3. Custom financial regulation compliance framework (specific to region)

💡 Recommendation Focus: 85% project-specific, 10% domain-specific, 5% general
```

**Phase 3: Sub-agent Recommendations**
```
💡 Recommended Sub-agents (10 candidates, sorted by priority):

📊 Total: 10 agents | Project-specific: 7 | Domain: 2 | General: 1

[Highest Priority - Project-specific]
1. ⭐ transaction-audit-specialist
   - Role: Immutable audit trail and transaction log management
   - Type: Analyst

2. ⭐ multi-currency-processor
   - Role: Real-time currency conversion and exchange rate handling
   - Type: Specialist

3. ⭐ account-balance-reconciler
   - Role: Account balance verification and reconciliation logic
   - Type: Specialist

4. ⭐ fraud-prevention-engine
   - Role: Transaction pattern analysis and fraud detection
   - Type: Analyst

5. kafka-event-coordinator
   - Role: Event-driven transaction processing coordination
   - Type: Orchestrator

6. double-entry-validator
   - Role: Double-entry bookkeeping validation and verification
   - Type: Specialist

7. transaction-rollback-handler
   - Role: Complex transaction rollback and compensation logic
   - Type: Orchestrator

[Medium Priority - Domain-specific]
8. ⭐ financial-compliance-checker
   - Role: Regional financial regulation compliance validation
   - Type: Analyst

9. regulatory-reporting-generator
   - Role: Financial regulatory report generation
   - Type: Specialist

[Lowest Priority - General]
10. go-concurrency-optimizer
    - Role: Go concurrency patterns for high-throughput processing
    - Type: Specialist
```

**Phase 4: Skill Recommendations**
```
💡 Recommended Skills (9 candidates, sorted by priority):

📊 Total: 9 skills | Project-specific: 6 | Domain: 2 | General: 1

[Highest Priority - Project-specific]
1. ⭐ financial-transaction-patterns
   - Content: Transaction processing workflows, state machines, error handling

2. ⭐ audit-trail-requirements
   - Content: Immutable logging patterns, audit compliance, data retention

3. ⭐ multi-currency-handling
   - Content: Currency conversion logic, exchange rate management, precision handling

4. ⭐ fraud-detection-rules
   - Content: Suspicious pattern detection, risk scoring, alert workflows

5. kafka-event-patterns
   - Content: Event sourcing, message schemas, consumer patterns

6. transaction-compensation-logic
   - Content: Saga patterns, rollback strategies, data consistency

[Medium Priority - Domain-specific]
7. ⭐ double-entry-bookkeeping
   - Content: Accounting principles, ledger management, balance verification

8. regulatory-compliance-framework
   - Content: Financial regulations, reporting requirements, compliance checks

[Lowest Priority - General]
9. go-performance-patterns
   - Content: Concurrency patterns, memory optimization, benchmarking
```

**Expected Outcome:**
```
Tools reflecting the project's financial domain and regional compliance requirements are generated, with heavy emphasis on project-specific audit trail and transaction processing logic. The high uniqueness score (82/100) ensures that 85% of recommendations focus on patterns unique to this financial system rather than general Go development or common financial patterns.
```

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
- **Perform deep project-specific pattern analysis** (Phase 2) to identify unique characteristics
- Calculate project uniqueness score (0-100) based on business concepts, conventions, and tech usage
- **Generate up to 10-15 agent/skill recommendations** scaled to project complexity and uniqueness
- Prioritize tools related to project-specific business logic and unique patterns
- Treat non-development/domain-specific content with higher priority than general development knowledge
- **Scan existing `.claude/agents/` and `.claude/skills/` directories** to identify already created items
- **Mark already created items with ✅** and display them as non-selectable options
- Categorize recommendations into three priority levels (Highest, Medium, Lowest)
- Provide clear priority information in selection interface to user
- Delegate actual file generation using `/create-agent` and `/create-skill` commands
- Display quality scores and file paths of generated tools
- Support iterative usage where users can run `/project-tooling` multiple times to see remaining options

**Will Not:**
- Not automatically generate agent or skill files without user confirmation
- Not recommend general development tools with higher priority than project-specific tools
- Not access directories or files outside the project
- Not modify or change meta file contents (read-only)
- Not force continuation when agent/skill generation fails (display error and continue)
- Not regenerate already created agents/skills (marked with ✅ and non-selectable)
- Not limit recommendations to arbitrary small numbers (scales with project complexity)

**Safety Checks:**
- Search files only within project root directory
- Safely ignore meta file parsing errors and continue with next file
- Request user selection instead of auto-estimating when project type is unclear
- Display warning if recommendation list has fewer than 3 items
- Display "Only general tools recommended" warning if 0 project-specific items
- Verify validity before invoking `/create-agent` and `/create-skill`
- Skip already-created items when processing user selections
- Do not force continuation if user makes no selection; terminate instead
