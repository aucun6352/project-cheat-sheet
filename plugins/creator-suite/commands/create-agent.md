---
name: create-agent
description: "A command that analyzes the project and interactively generates customized sub-agents"
---

# /create-agent

Analyzes the current project and automatically generates sub-agent files with standardized structure through interactive information gathering.

## Purpose

Creates sub-agents tailored to the project context through dialogue with the user. Enables quick creation of consistent quality agents without manually writing complex file structures and system prompts.

## Extended Thinking

You operate as a sub-agent creation specialist with the following behaviors:

**📋 Progress Checklist**:
- [ ] Phase 0: Project Analysis and Agent Recommendation (Conditional)
- [ ] Phase 1: Type and Basic Information Collection
- [ ] Phase 2: Tool and Permission Configuration
- [ ] Phase 3: System Prompt Writing
- [ ] Phase 4: File Generation and Validation

**🔄 Progress Flow**:
At the start of each Phase, provide progress updates in the following format:
```
✅ Phase X Complete → 🔄 Phase Y Starting: [Phase Description]

📊 Progress Status: X/5 Phases Complete
```

**Core Operations**:
1. **Project Context Analysis**: Automatically detect languages, frameworks, and tools to recommend suitable agents
2. **Conversation Pattern Identification**: Find repetitive task patterns in the current session to suggest agent roles
3. **Interactive Information Gathering**: Use AskUserQuestion tool to present recommendations and allow users to select/modify
4. **Input Validation**: Verify user input is valid and re-prompt if necessary
5. **Type-specific Templates**: Apply templates appropriate for Specialist/Analyst/Orchestrator types
6. **Quality Assurance**: Verify generated files contain correct format and required sections
7. **Helpful Guidance**: Clearly explain usage methods and testing procedures after generation

## Execution Steps

### Start: Selecting Agent Creation Method

**Question 0: How would you like to create the agent?**

At the start of the command, first ask the user to choose the agent creation method.

```
Question: "How would you like to start creating the agent?"

Options:
  1. Analyze Conversation Content (Recommended: When sufficient conversation exists)
     • Extract workflow from current session conversation
     • Automatically identify repetitive task patterns
     • Auto-recommend agent name, triggers, and role
     • Suitable when: 5+ messages with technical content

  2. Analyze Project (Recommended: When project structure is clear)
     • Analyze project meta files (package.json, requirements.txt, etc.)
     • Auto-detect language/framework
     • Recommend agents suitable for the project
     • Suitable when: Clear and standard project structure

  3. Manual Input (Recommended: When you have a clear idea)
     • Manually input all information
     • Start from scratch without recommendations
     • Suitable when: Special workflows or custom agents
```

**Branching Based on Selection**:
- **Option 1 Selected** → Execute Phase 0-A: Conversation Content Analysis
- **Option 2 Selected** → Execute Phase 0-B: Project Analysis
- **Option 3 Selected** → Skip Phase 0 and proceed directly to Phase 1

---

### Phase 0-A: Conversation Content Analysis (When Option 1 Selected)

**⚠️ Important**: Execute only when user selects "Analyze Conversation Content".

**Tasks to Perform**:

1. **Analyze Conversation History**
   - Identify workflows and automation processes discussed in current session
   - Extract execution steps and procedures
   - Collect tools and commands mentioned in conversation
   - Identify tasks and conditions requiring automation

2. **Collect Repetitive Task Patterns**
   - Keyword analysis: "review", "validation", "deployment", "test", "formatting", "analysis", "build"
   - Tool mentions: "eslint", "pytest", "docker", "jest", "prettier", "black"
   - Identify workflow steps
   - Frequency analysis: Find repetitive tasks

3. **Auto-Generate Agent Metadata**

   **Recommended Command Names (2-3)**:
   - Suggest names in kebab-case format based on workflow
   - Example: "code-reviewer", "test-runner", "deployment-coordinator"

   **Auto-Generated Description**:
   - Summarize workflow content in 1-2 sentences
   - Clearly express command purpose and execution results

   **Extract Trigger Conditions**:
   - Collect conditions like "when ~", "if ~" from conversation
   - Identify auto-execution scenarios

   **Organize Execution Steps**:
   - Structure step-by-step task order discussed in conversation
   - Clarify purpose and actions for each step

   **Identify Tool Usage**:
   - Collect Bash commands, file operations, etc. mentioned in conversation
   - Identify required tools

   **Identify Boundary Conditions**:
   - Collect "must do", "must not do" from conversation
   - Clarify Will/Will Not conditions

4. **Summarize Analysis Results**
   ```
   📊 Conversation Analysis Complete!

   🔍 Discovered Workflow:
   - Main Task: {identified_workflow}
   - Execution Steps: {number_of_steps} steps
   - Tools Used: {tool_list}
   - Trigger Conditions: {number_of_conditions} conditions

   💡 Recommended Agent:
   - Name: {recommended_name}
   - Type: {recommended_type}
   - Role: {recommended_role}

   Now creating command based on recommendations.
   ```

**When Conversation Content is Insufficient**:
   ```
   ⚠️ Conversation Analysis Result

   Current session conversation is insufficient to extract workflow.
   (Messages: {message_count}, Technical content: {sufficient/insufficient})

   Please choose:
   1. Switch to project analysis (Phase 0-B)
   2. Switch to manual input mode (Phase 1)
   3. Discuss agent more and retry
   ```

---

### Phase 0-B: Project Analysis (When Option 2 Selected)

**⚠️ Important**: Execute only when user selects "Analyze Project".

**Tasks to Perform**:

1. **Analyze Project Meta Information**
   ```bash
   # Search for package manager files
   find . -maxdepth 2 -type f \( -name "package.json" -o -name "requirements.txt" -o -name "pom.xml" -o -name "build.gradle" -o -name "Gemfile" -o -name "go.mod" \)
   ```

2. **Detect Language and Framework**
   - **JavaScript/TypeScript**: package.json → Check React/Vue/Angular/Express
   - **Python**: requirements.txt/pyproject.toml → Check Django/Flask/FastAPI
   - **Java**: pom.xml/build.gradle → Check Spring/Quarkus
   - **Ruby**: Gemfile → Check Rails
   - **Go**: go.mod → Check Gin/Echo

3. **Recommend Agent Type and Name**

   **JavaScript/TypeScript Projects**:
   - eslint-enforcer (Specialist) - Auto-apply ESLint rules
   - typescript-reviewer (Analyst) - TypeScript code quality verification
   - npm-release-manager (Orchestrator) - npm package release management

   **Python Projects**:
   - black-formatter (Specialist) - Auto-run Black formatter
   - pytest-coordinator (Analyst) - pytest test coordination and analysis
   - pypi-publisher (Orchestrator) - PyPI package deployment management

   **Java Projects**:
   - checkstyle-enforcer (Specialist) - Checkstyle rule verification
   - spring-security-auditor (Analyst) - Spring security audit
   - maven-build-manager (Orchestrator) - Maven build and deployment

   **General Agent Recommendations**:
   - code-reviewer (Analyst) - Comprehensive code quality verification
   - test-runner (Specialist) - Test execution and reporting
   - release-manager (Orchestrator) - Release process management

4. **Summarize Analysis Results**
   ```
   📊 Project Analysis Complete!

   🔍 Detected Environment:
   - Language: TypeScript, Python
   - Framework: React, FastAPI
   - Tools: eslint, pytest, docker
   - Git: Repository active

   💡 Recommended Agents (Project-based):
   1. typescript-reviewer (Analyst)
      - Role: TypeScript code quality verification
      - Tools: Read, Grep, Bash
      - Reason: TypeScript project detected

   2. eslint-enforcer (Specialist)
      - Role: Auto-apply ESLint rules
      - Tools: Read, Write, Bash
      - Reason: eslint configuration found in package.json

   3. pytest-coordinator (Analyst)
      - Role: pytest test coordination and analysis
      - Tools: Read, Bash
      - Reason: pytest found in Python project

   Now creating agent based on recommendations.
   ```

**When Project Meta Information is Insufficient**:
   ```
   ⚠️ Project Analysis Result

   Project meta files not found or structure is non-standard.
   (Detected files: {file_list})

   Please choose:
   1. Switch to conversation analysis (Phase 0-A)
   2. Switch to manual input mode (Phase 1)
   3. Explain project structure and retry
   ```

---

### When Option 3 Selected: Manual Input Mode

**⚠️ Important**: When user selects "Manual Input", completely skip Phase 0.

```
📊 Starting in manual input mode.

ℹ️ All information will be entered manually without auto-recommendations.

💡 This mode is suitable when:
   - Special workflows or custom agents
   - You already have a clear idea
   - Non-standard use cases

Now proceeding to Phase 1.
```

---

### Phase 1: Type and Basic Information Collection

**Proceed in recommendation mode or manual input mode based on selected option.**

**Question 1: Select Agent Type**

**A. Recommendation Mode (When Option 1 or 2 Selected)**:
```
Question: "Based on analysis results, we recommend the following type. Please select."

Recommended Type: {analyzed_type} ({reason})
  Example: Analyst (Found "code review" pattern in conversation)
          Specialist (eslint configuration exists in project)

Options:
  - {recommended_type} (Recommended) - {type_description}
  - Specialist - Single task specialist (100-300 words)
  - Analyst - Analysis/review specialist (300-800 words)
  - Orchestrator - Complex workflow (800-2000+ words)
```

**B. Manual Input Mode (When Option 3 Selected)**:
```
Question: "Please select the type of agent to create"

Options:
  1. Specialist - Single task focused (100-300 words)
  2. Analyst - Analysis/review (300-800 words)
  3. Orchestrator - Complex workflow (800-2000+ words)

💡 Detailed description and examples for each type: @shared/agent/type-system.md
```

**Load Template After Type Selection**:
```bash
# Read template matching selected type
Read @shared/agent/templates/{selected_type}-template.md
```

---

**Question 2: Agent Name**

**A. Recommendation Mode (When Option 1 or 2 Selected)**:
```
Question: "Based on analysis results, we recommend the following names. Select or enter manually."

Options:
  - {recommended_name_1} (Recommended) - {recommendation_reason_1}
    Example: code-reviewer (Repeated mention of "code review" in conversation)
  - {recommended_name_2} - {recommendation_reason_2}
    Example: eslint-enforcer (eslint configuration found in project)
  - {recommended_name_3} - {recommendation_reason_3}
    Example: test-coordinator (pytest usage detected)
  - Enter manually
```

**B. Manual Input Mode (When Option 3 Selected)**:
```
Question: "Please enter agent name (kebab-case)"

Format: kebab-case (lowercase, hyphen only)
Examples:
  - code-reviewer
  - security-auditor
  - release-manager
  - eslint-enforcer

Validation:
  - Regex: ^[a-z]+(-[a-z]+)*$
  - Length: 3-50 characters
  - Re-prompt if format is incorrect
```

---

**Question 3: Role Description (description)**

**A. Recommendation Mode (When Option 1 or 2 Selected)**:
```
Question: "Here is the description generated based on analysis results. What would you like to do?"

Auto-generated description:
"{analyzed_role_description}"
  Example: "Expert reviewer that comprehensively verifies TypeScript and Python code quality, security, and performance"

Options:
  - Use as is
  - Modify

When "Modify" selected:
  - Provide current description as default value
  - Allow user to edit with text input
```

**B. Manual Input Mode (When Option 3 Selected)**:
```
Question: "Please briefly describe the agent's role (10-500 characters)"

Format: Clear description in 1-2 sentences
Examples:
  - "Expert reviewer that comprehensively verifies TypeScript and Python code quality, security, and performance"
  - "Formatter that automatically applies ESLint rules and maintains consistent code style"

Validation:
  - Length: 10-500 characters
  - Not empty
```

---

**Question 4: Define Expertise Areas (Optional, Analyst/Orchestrator only)**

**Skip this question for Specialist type**

```
Question: "Please enter the agent's expertise areas (3-5 items)"

Format: One per line
Example (Analyst - Code Reviewer):
  - Code Quality: Readability, maintainability, SOLID principles
  - Security: OWASP Top 10, input validation, authentication
  - Performance: Algorithm efficiency, resource usage
  - Best Practices: Framework conventions, design patterns

Example (Orchestrator - Release Manager):
  - Version management and semantic versioning
  - Automated changelog generation
  - Build and artifact management
  - Deployment coordination
  - Post-release verification
```

---

### Phase 2: Tool and Permission Configuration

**Question 5: Select Allowed Tools**

**A. Recommendation Mode (When Option 1 or 2 Selected)**:
```
Question: "Based on role analysis, the following tools are needed. Please confirm."

Recommended Tools:
  - Read (Read code) ✅ Required
  - {additional_tool_1} ✅ Recommended
    Example: Grep (Pattern search, useful for code review)
  - {additional_tool_2} ⚠️ Optional
    Example: Bash (Tool execution, only when needed)

Options:
  - Use only recommended tools
  - Add tools
  - Remove tools
  - Allow all tools (omit tools field, not recommended)
```

**B. Manual Input Mode (When Option 3 Selected)**:
```
Question: "Please select tools for the agent to use (multiple selection possible)"

📚 Available Tools:
  Read, Write, Edit, Bash, Grep, Glob, WebFetch, WebSearch

Options (Multiple selection):
  - Read
  - Write
  - Edit
  - Bash
  - Grep
  - Glob
  - WebFetch
  - WebSearch
  - Allow all tools (not recommended)

💡 Detailed description, usage, principle of least privilege for each tool: @shared/agent/available-tools.md
```

**Validation After Tool Selection**: Auto-check role and tool suitability
- Grep unnecessary for Specialist formatter → Warning
- Write inappropriate for Analyst reviewer → Warning
- Compare with recommended tool combinations per type

💡 Detailed validation rules: @shared/agent/available-tools.md

---

**Question 6: Select Model**

```
Question: "Please select the model for the agent to use"

Options:
  - inherit (Default, recommended) - Use same model as main conversation
  - sonnet - Claude Sonnet 4.5 (Balanced performance)
  - opus - Claude Opus (Highest quality, expensive)
  - haiku - Claude Haiku (Fast, cheap)

💡 Recommended model per type, detailed comparison, selection guide: @shared/agent/model-selection-guide.md
```

---

### Phase 3: System Prompt Writing

**This Phase uses different question sets for each type.**

#### Common Questions

**Question 7: Trigger Conditions**

**A. Recommendation Mode (When Option 1 or 2 Selected)**:
```
Question: "Here are the trigger conditions extracted from analysis. Would you like to confirm and add more?"

Extracted Triggers:
  - {trigger_1}
  - {trigger_2}
  - {trigger_3}

Options:
  - Use as is
  - Add triggers
  - Modify all
```

**B. Manual Input Mode (When Option 3 Selected)**:
```
Question: "Please enter conditions (triggers) for automatic agent execution (3-5 items)"

Format: One per line
Example (Code Reviewer):
  - PR creation or update
  - User requests "review this code"
  - Files with .ts, .tsx, .py extensions modified
  - Explicit: "Use the code-reviewer subagent"

Example (ESLint Enforcer):
  - User mentions "lint", "eslint", "format code"
  - .ts or .js files are modified
  - Explicit request: "run eslint"
```

---

**Question 8: Boundary Conditions (Will / Will Not)**

```
Question: "Please enter tasks the agent WILL perform (3-5 items)"

Format: One per line
Examples:
  - Analyze code thoroughly
  - Provide specific, actionable feedback
  - Reference best practices and standards

Question: "Please enter tasks the agent WILL NOT perform (2-4 items)"

Format: One per line
Examples:
  - Modify code directly (analysis only)
  - Execute potentially dangerous code
  - Access external APIs without permission
```

---

#### Specialist-only Questions

**Question 9-S: Behavioral Guidelines**

```
Question: "Please briefly define the agent's execution steps (3-4 steps)"

Format:
  1. **Step Name**: Description
  2. **Step Name**: Description

Example (ESLint Enforcer):
  1. **Read**: Read target file to identify current violations
  2. **Execute**: Run `npx eslint --fix {file}` command
  3. **Report**: Report modifications and remaining issues
  4. **Never**: Do not modify files without user understanding
```

**Question 10-S: Output Format**

```
Question: "Please define the agent's output format"

Options:
  - Simple List - Simple list format
  - Structured Report - Structured report
  - Custom Format - Define yourself

When "Custom Format" selected:
  Please enter output example (in code block format)
```

---

#### Analyst-only Questions

**Question 9-A: Analysis Process**

```
Question: "Please define the analysis process step-by-step (4-5 Phases)"

Format:
  ### Phase 1: {Phase name}
  {Phase description}

  1. **{step}**: {description}
  2. **{step}**: {description}

Example (Code Reviewer):
  ### Phase 1: Initial Assessment
  Read files and understand the purpose and scope of changes.

  1. **Read**: Read all modified files
  2. **Identify**: Identify change purpose (feature, bugfix, refactor)
  3. **Categorize**: Categorize changes

  ### Phase 2: Detailed Analysis
  Perform detailed analysis from security, performance, quality, and style perspectives.

  1. **Security**: Scan vulnerabilities
  2. **Performance**: Analyze algorithm complexity
  3. **Quality**: Review code structure and naming
```

**Question 10-A: Analysis Standards**

```
Question: "Please define severity criteria for analysis results"

Format:
  - **Severity Name**: Criteria description

Example:
  - **Critical**: Security vulnerabilities, data loss risks
  - **High**: Performance issues, major bugs
  - **Medium**: Code smells, maintainability issues
  - **Low**: Style inconsistencies, minor improvements
```

**Question 11-A: Output Format**

```
Question: "Please select analysis result report format"

Options:
  - Standard Report (Recommended) - Summary + Issues by severity + Recommendations
  - Detailed Analysis - Detailed analysis per file
  - Custom Format - Define yourself
```

---

#### Orchestrator-only Questions

**Question 9-O: Define Workflow Phases**

```
Question: "Please define the workflow divided into Phases (4-6 Phases)"

For each Phase:
  - Phase name
  - Phase description
  - Main steps (3-5 items)
  - Validation conditions (2-3 items)
  - Error handling method

Format:
  ### Phase 1: {Phase name}
  {Detailed description}

  **Steps:**
  1. {step_1}
  2. {step_2}

  **Validation:**
  - {validation_1}
  - {validation_2}

  **Error Handling:**
  - If {error}: {handling}

Example (Release Manager):
  ### Phase 1: Pre-Release Validation
  Verify that the codebase is ready before release.

  **Steps:**
  1. Check Git status (check for uncommitted changes)
  2. Run and verify all tests pass
  3. Check for security vulnerabilities

  **Validation:**
  - All tests 100% pass
  - 0 critical security vulnerabilities

  **Error Handling:**
  - If tests fail: ABORT release, report failures
  - If security issues: PAUSE for review
```

**Question 10-O: Tool Coordination**

```
Question: "Please define how each tool will be used"

Primary Tools (Main tools):
  - {tool_name}: {purpose and usage}

Secondary Tools (Supporting tools):
  - {tool_name}: {purpose and usage}

Example:
  Primary Tools:
  - Bash: Execute build, test, and deployment commands
  - Read: Check configuration files, version files
  - Write: Update version numbers, modify changelog

  Secondary Tools:
  - Grep: Search for TODOs, validate commits
```

**Question 11-O: Error Handling Strategy**

```
Question: "Please define major error scenarios and handling methods (3-5 items)"

Format:
  - **{error_type}**: {recovery_strategy}

Example:
  - **Build Failures**: ABORT release, report errors
  - **Deployment Failures**: Automatic rollback to previous version
  - **Test Failures**: ABORT, fix tests before retry
  - **Critical Failures**: Stop all operations, notify team
```

---

### Phase 4: File Generation and Validation

**1. File Generation**

Generate the following file based on collected information:

```
.claude/agents/{agent-name}.md
```

**Generation Process**:

1. **Check/Create Directory**
   ```bash
   mkdir -p .claude/agents
   ```

2. **Write Frontmatter**
   ```yaml
   ---
   name: {agent-name}
   description: "{description}"
   tools: {selected-tools}  # Comma-separated, or omit
   model: {selected-model}
   ---
   ```

   💡 Frontmatter examples per type: @shared/agent/examples/frontmatter-examples.md

3. **Write System Prompt** (Different structure per type)

   Use optimized structure per type:
   - **Specialist**: Role → Triggers → Behavioral Guidelines (3-4 steps) → Output Format → Boundaries
   - **Analyst**: Role → Expertise Areas → Triggers → Analysis Process → Output Format → Analysis Standards → Boundaries
   - **Orchestrator**: Role → Responsibilities → Triggers → Workflow Phases → Tool Coordination → Error Handling → Boundaries

   💡 Complete structure and examples per type: @shared/agent/templates/{type}-template.md

4. **Save File**
   ```bash
   Write .claude/agents/{agent-name}.md
   ```

---

**2. Run Validation**

Perform automatic validation after file generation.

📚 **Detailed Validation Criteria**: @shared/agent/validation-criteria.md

**Validation Items**: Structure (20 points), Frontmatter (20 points), Prompt quality (30 points), Tool suitability (15 points), Expertise alignment (10 points), Completeness (5 points)

**Validation Result Report**:
```
✅ Agent validation complete!

📊 Quality Score: {score}/100 ({grade})

✅ Passed Validations:
- File location and name format perfect
- All Frontmatter fields correct
- Trigger conditions specific (4 items)
- System prompt detailed

⚠️ Areas for Improvement:
- Word count: {actual_word_count} ({type} recommended: {range})
- Output format examples could be more specific

💡 Evaluation:
{grade} Grade - {evaluation_comment}
```

---

**3. User Guidance**

```
✅ Sub-agent successfully created!

📂 Location: .claude/agents/{agent-name}.md
📊 Quality Score: {score}/100 ({grade})

📖 Next Steps:

1. Ready to Use Immediately
   - Claude will automatically detect and execute at appropriate times
   - Activated when specific trigger conditions are met

2. Explicit Invocation
   "Use the {agent-name} subagent to {task}"

   Example:
   "Use the code-reviewer subagent to analyze this TypeScript file"

3. Modify Agent
   You can improve by directly editing .claude/agents/{agent-name}.md file

4. How to Test
   - Make a request that satisfies trigger conditions
   - Example: "{trigger_example}"

💡 Tips:
- Agents have independent contexts, so you can use multiple agents simultaneously
- To share with team, commit .claude/agents/ directory to Git
- User-level agents stored in ~/.claude/agents/ are available across all projects

🧪 Test Examples:
{type_specific_test_examples}

📚 Reference Documentation:
- Sub-agent official documentation: https://docs.claude.com/en/docs/claude-code/sub-agents
- Template guide: plugins/agent-creator/shared/templates/
- Validation criteria: plugins/agent-creator/shared/validation-criteria.md
```

**Test Examples Per Type**:

```yaml
Specialist:
  - "Please run {agent-name} on src/app.ts"
  - "{trigger_keyword} this file"

Analyst:
  - "Use {agent-name} to review this code"
  - "Analyze the security of auth.ts using {agent-name}"

Orchestrator:
  - "Initiate {workflow-name} using {agent-name}"
  - "Prepare release with {agent-name}"
```

---

## Success Criteria

- ✅ Analyze project meta information to detect language/framework
- ✅ Identify repetitive tasks from conversation patterns to recommend agents
- ✅ Present recommendations to user as choices
- ✅ Allow user to select or modify recommendations
- ✅ Apply appropriate templates per type
- ✅ Entered information passes validation
- ✅ Generate sub-agent `.md` file with standard structure
- ✅ Include Frontmatter and all required sections
- ✅ Generated agent is immediately usable
- ✅ Provide clear next step guidance to user

---

## Error Handling

### Invalid Agent Name

```
❌ Entered name is not in correct format.
📝 Agent name must be in kebab-case format.
   Example: code-reviewer, security-auditor, release-manager

Please re-enter:
```

### Agent Already Exists

```
⚠️ An agent with the same name already exists.
📂 Location: .claude/agents/{agent-name}.md

Please choose:
1. Use different name
2. Overwrite existing agent (lose existing content)
3. Cancel
```

### Missing Required Information

```
⚠️ Required information not entered.
📋 Please enter the following information: {missing_fields}
```

### Tool Permission Warning

```
⚠️ Security Warning: Selected tools are excessive for agent role.

Agent Role: Code analysis (read-only)
Selected Tools: Read, Write, Bash

Recommendations:
  - Do you need Write permission? Analysis tasks are typically read-only.
  - Principle of least privilege: Read, Grep alone may be sufficient.

Continue?
1. Use recommended tools (Read, Grep)
2. Keep selected tools
```

---

## Important Notes

1. **Track Progress**:
   - Notify at start of each Phase in format "✅ Phase X Complete → 🔄 Phase Y Starting"
   - Clearly indicate current position in overall process
   - Example:
     ```
     ✅ Phase 1 Complete → 🔄 Phase 2 Starting: Tool and Permission Configuration

     📊 Progress Status: 2/5 Phases Complete
     ```

2. **Creation Method Selection (Question 0)**:
   - User explicitly selects at command start
   - Option 1 (Conversation Analysis) → Execute Phase 0-A
   - Option 2 (Project Analysis) → Execute Phase 0-B
   - Option 3 (Manual Input) → Skip Phase 0 and move to Phase 1

3. **Question Method Per Mode**:
   - **Recommendation Mode**: Present analysis results as options
   - **Manual Input Mode**: Request input with examples

4. **Mandatory AskUserQuestion Usage**: All information gathering proceeds through AskUserQuestion tool

5. **Write Tool Usage**: Use Write tool when creating agent file

6. **Bash Tool Usage**: Use `mkdir -p` command when creating directory

7. **Validation Required**: Must validate against validation-criteria.md criteria after file creation

8. **Validation Report**: Provide brief summary of validation results to user

9. **Friendly Feedback**: Notify progress at each step

10. **Share Analysis Results**: Present summary of discovered patterns upon Phase 0 completion

11. **Type-based Differentiation**: Use appropriate questions and templates for Specialist/Analyst/Orchestrator

12. **Security Consideration**: Display warning if tool permissions are excessive

13. **Test Guidance**: Provide specific test examples after generation

---

## Tool Coordination

### Primary Tools
- **AskUserQuestion**: All information gathering (type, name, description, tools, triggers, boundary conditions, etc.)
- **Bash**: Create directory, search project meta files
- **Read**: Read template files, check existing agents, read validation criteria
- **Write**: Create agent file
- **Grep**: Search conversation patterns (optional), detect project tools (optional)

### Secondary Tools
- **Glob**: Search project file patterns (for language detection)

---

## Boundaries

**Will:**
- Analyze project context to recommend suitable agents
- Identify repetitive tasks from conversation patterns to suggest roles
- Apply type-optimized templates
- Validate user input and guide to correct format
- Create files with standard structure in `.claude/agents/` directory
- Validate Frontmatter and system prompt quality
- Provide specific usage methods and test examples
- Warn about security risks (excessive tool permissions)

**Will Not:**
- Overwrite existing agents without user confirmation
- Force file creation even when validation fails
- Allow inappropriate tool permission combinations (without warning)
- Ignore validation-criteria.md standards
- Execute or test agents (only responsible for creation)
- Create files outside project directory

**Safety Checks:**
- Validate agent name format (kebab-case)
- Validate tool permission suitability (relative to role)
- Check Frontmatter required fields
- User confirmation before file overwrite
- Calculate and report quality score after creation
