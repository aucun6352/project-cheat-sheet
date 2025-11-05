# Create Command

This command interactively creates a new Slash command that can be used in Claude Code.

## Purpose

Creates a slash command file with a standardized structure through interactive user interaction. Enables rapid creation of consistent, high-quality commands without manually building complex file structures.

## Extended Thinking

You act as a Slash command creation expert with the following behavior:

**📋 Progress Checklist**:
- [ ] Phase 0: Conversation analysis and workflow extraction (conditional)
- [ ] Phase 1: Collect basic information (name, description)
- [ ] Phase 2: Define workflow and tools (Triggers, Flow, Tools, Boundaries)
- [ ] Phase 3: Document examples (Usage, Examples)
- [ ] Phase 4: Create and validate file (generate, validate, guide)

**🔄 Progress Reporting**:
At the start of each Phase, report progress as follows:
```
✅ Phase X complete → 🔄 Phase Y starting: [Phase description]

📊 Progress: X/5 Phases complete
```

**Core Actions**:
1. **Conversation Analysis**: Analyze conversation history in the current session to extract workflows, automation processes, and execution steps
2. **Intelligent Recommendations**: Automatically suggest command names, triggers, execution steps, and tools based on analysis results
3. **Interactive Information Gathering**: Use AskUserQuestion tool to present recommendations and allow users to select/modify them
4. **Input Validation**: Verify user input is valid and re-ask if necessary
5. **Structured Generation**: Maintain consistent structure using standard Slash command templates
6. **Quality Assurance**: Validate generated files contain correct format and required sections
7. **Friendly Guidance**: Clearly guide usage after creation is complete

## Execution Steps

### Phase 0: Conversation Analysis and Workflow Extraction (Conditional)

**⚠️ Important**: If there is insufficient conversation content in the current session, skip this step and proceed directly to **Phase 1**.

**Criteria for Sufficient Conversation Content**:
- Fewer than 5 conversation messages → Skip Phase 0
- No technical content or workflow → Skip Phase 0
- Only greetings or simple questions → Skip Phase 0

**Only perform the following if conversation content is sufficient:**

1. **Conversation History Analysis**
   - Identify workflows and automation processes discussed in the current session
   - Extract execution steps and procedures
   - Collect tools and commands mentioned in conversation
   - Identify tasks and conditions requiring automation

2. **Automatic Command Metadata Generation**
   - **Recommended Command Names (2-3)**:
     - Suggest kebab-case format names based on workflow
     - Examples: "deploy-production", "run-tests", "code-review"

   - **Auto-Generated Description**:
     - Summarize workflow content in 1-2 sentences
     - Clearly express command purpose and execution results

   - **Extracted Trigger Conditions**:
     - Collect conditions like "when ~", "if ~" from conversation
     - Identify automatic execution scenarios

   - **Organized Execution Steps** (Behavioral Flow):
     - Structure step-by-step work sequence discussed in conversation
     - Clarify purpose and actions of each step

   - **Tool Usage Identification** (Tool Coordination):
     - Collect Bash commands, file operations, etc. mentioned in conversation
     - Identify required tools

   - **Boundary Condition Identification** (Boundaries):
     - Collect "must ~", "must not ~" from conversation
     - Clarify Will/Will Not conditions

3. **Analysis Results Summary**
   ```
   📊 Conversation analysis complete!

   🔍 Discovered workflows:
   - Main tasks: {identified_workflow}
   - Execution steps: {number_of_steps} steps
   - Tools used: {tool_list}
   - Trigger conditions: {number_of_conditions} conditions

   💡 Now creating command based on recommendations.
   ```

**Guidance for Insufficient Conversation Content**:
   ```
   📊 Conversation Content Analysis Results

   ℹ️ Current session has insufficient conversation content for automatic recommendations.

   💡 Proceeding in manual input mode.
      - Please enter all information manually
      - Or discuss the command topic first and try again
   ```

### Phase 1: Collect Basic Information

**If Phase 0 was skipped**: Collect direct input without recommendations.
**If Phase 0 was completed**: Present recommended content and allow user to select/modify.

**Question 1 - Select Command Type**

Choose type matching command complexity:

```
Question: "Please select the type of command to create"

Options:
  1. Simple Task (50-150 words)
     - Single task, simple automation
     - Examples: file formatting, code cleanup, single command execution

  2. Workflow Pipeline (150-400 words)
     - Multi-step workflow, sequential execution
     - Examples: build → test → deploy, CI/CD pipeline

  3. Complex Orchestration (400-800 words)
     - Complex conditional branching, multiple tool coordination
     - Examples: multi-environment deployment, complex release process
```

**Guidance After Type Selection**:
```
✅ Creating with {selected_type} type.
Recommended word count: {range}
Complexity: {low/medium/high}
```

---

**Question 2 - Select Command Name**

**A. If Phase 0 completed (Recommendation mode)**:
```
Question: "Based on conversation analysis, we recommend the following command names. Please select or enter your own."
Options:
  - Option 1: {recommended_name_1} (Workflow: {related_task_1})
  - Option 2: {recommended_name_2} (Workflow: {related_task_2})
  - Option 3: {recommended_name_3} (Workflow: {related_task_3})
  - Option 4: Enter manually
```

**B. If Phase 0 skipped (Manual input mode)**:
```
Question: "Please enter the command name to create"
- Format: kebab-case (lowercase, hyphens only)
- Examples: deploy-production, run-tests, code-review
- Validation: Re-ask if format is incorrect
```

---

**Question 3 - Select File Location**

```
Question: "Where would you like to create this command?"

Options:
  1. **Project** (.claude/commands/) - Recommended
     - Available only in current project
     - Project-specific command
     - Default choice

  2. **Global** (~/.claude/commands/)
     - Available in all projects
     - Reusable across projects
     - Good for general-purpose commands

  3. **Current Directory** (./{command-name}/)
     - Created in current working directory
     - Useful for plugin development or distribution

Default: Project (.claude/commands/)

💡 Location Guide:
  - Most commands should be project-specific (Option 1)
  - Create global commands only if they're truly reusable
  - Use current directory for plugin development
```

**Store the selected location path**:
- Option 1 → `{location_path} = ".claude/commands"`
- Option 2 → `{location_path} = "~/.claude/commands"`
- Option 3 → `{location_path} = "./{command-name}"`

---

**Question 4 - Command Description**

**A. If Phase 0 completed (Recommendation mode)**:
```
Question: "Here is the command description generated from conversation content. What would you like to do?"

Present auto-generated description: "{analyzed_description}"

Options:
  - Use as is
  - Modify

If "Modify" is selected:
  - Provide current description as default
  - Allow text input for user editing
```

**B. If Phase 0 skipped (Manual input mode)**:
```
Question: "Please enter a brief description of the command (1-2 sentences)"
- Format: Concise and clear description
- Example: "Workflow for building project and deploying to production environment"
```

### Phase 2: Define Workflow and Tools

**In this Phase, collect all of Triggers, Behavioral Flow, Tool Coordination, and Boundaries.**
**Proceed in recommendation mode or manual input mode depending on Phase 0 completion.**

**Question 5 - Triggers (Trigger Conditions)**

**A. If Phase 0 completed (Recommendation mode)**:
```
Question: "Here are the trigger conditions extracted from conversation. Would you like to review and add more?"

Extracted trigger list:
{auto_extracted_trigger_list}

Options:
  - Use as is
  - Add triggers
  - Modify all

If additional input needed:
  - Keep existing triggers and add new items
  - Multi-line input allowed
```

**B. If Phase 0 skipped (Manual input mode)**:
```
Question: "Please enter conditions (triggers) for automatic command execution"
- Format: List format
- Examples:
  - When deployment is ready
  - When merged to production branch
  - When release tag is created
```

**Question 6 - Behavioral Flow (Execution Steps)**

**A. If Phase 0 completed (Recommendation mode)**:
```
Question: "Here are the execution steps identified from conversation. Would you like to review and modify?"

Extracted execution steps:
{auto_organized_execution_steps}

Options:
  - Use as is
  - Add steps
  - Modify all

When adding/modifying:
  - Provide existing steps in editable format
  - Guide step numbering and description format
```

**B. If Phase 0 skipped (Manual input mode)**:
```
Question: "Please enter the execution steps for the command"
- Format:
  1. **Step name**: Step description
  2. **Step name**: Step description
- Examples:
  1. **Validation**: Pre-deployment condition check (tests passed, branch verified)
  2. **Build**: Build and optimize project
  3. **Test**: Validate build artifacts
  4. **Deploy**: Deploy to production server
```

**Question 7 - Boundaries (Boundary Definition)**

**A. If Phase 0 completed (Recommendation mode)**:
```
Question: "Here are the boundary conditions identified from conversation. Would you like to review and add more?"

Will (Tasks to perform):
{auto_collected_will_list}

Will Not (Tasks not to perform):
{auto_collected_will_not_list}

Options:
  - Use as is
  - Add items
  - Modify

When adding:
  - Keep existing items
  - Enter new items in list format
```

**B. If Phase 0 skipped (Manual input mode)**:
```
Question: "Please enter tasks the command Will perform"
- Format: List format
- Examples:
  - Build the project
  - Run tests and verify results
  - Deploy to production server

Question: "Please enter tasks the command Will Not perform"
- Format: List format
- Examples:
  - Will not deploy if tests fail
  - Will not auto-deploy without manual approval
  - Will not perform rollback operations
```

**Question 8 - Tool Coordination (Tool Usage)**

**A. If Phase 0 completed (Recommendation mode)**:
```
Question: "Here are the tools identified from conversation. Please review."

Extracted tool list:
{auto_identified_tool_list}

Options:
  - Include all
  - Include selectively
  - Add tools

When adding tools:
  - Enter tool name and usage purpose
```

**B. If Phase 0 skipped (Manual input mode)**:
```
Question: "Please select tools to use in the command"
- Format: tool_name: usage purpose
- Examples:
  - Bash: Execute build scripts and deployment commands
  - Read: Read configuration files and verify environment variables
  - Write: Write deployment logs
```

---

### Phase 3: Document Examples

**In this Phase, collect Usage and Examples.**

**Question 9 - Usage (How to Use)**

**A. If Phase 0 completed (Recommendation mode)**:
```
Question: "Based on conversation, we've generated usage instructions. Please review."

Auto-generated usage:
{generated_usage_example}

Options:
  - Use as is
  - Modify
```

**B. If Phase 0 skipped (Manual input mode)**:
```
Question: "Please enter usage instructions for the command"
- Format:
  /{command-name} [arguments] [--flags]
- Example:
  /deploy-production --env=prod --confirm
```

**Question 10 - Examples (Usage Examples)**

**A. If Phase 0 completed (Recommendation mode)**:
```
Question: "Here are usage examples found in conversation. Please review."

Number of examples found: {number_of_examples}

For each example:
  - Example title: {auto_generated_title}
  - Usage: {extracted_usage}
  - Description: {extracted_description}

Options:
  - Include all
  - Include selectively
  - Add examples
  - Exclude examples
```

**B. If Phase 0 skipped (Manual input mode)**:
```
Question: "Would you like to add usage examples?"

Options:
  - Yes (write examples)
  - No (include default examples only)

If "Yes" is selected:
  Request example title, usage, and description
```

---

### Phase 4: Create and Validate File

**In this Phase, perform file creation, validation, and user guidance.**

**1. File Creation**

Create the following file structure based on collected information:

```bash
mkdir -p {location_path}
Write {location_path}/{command-name}.md
```

Where `{location_path}` is the path selected in Question 3:
- Project: `.claude/commands`
- Global: `~/.claude/commands` (expand to full path)
- Current Directory: `./{command-name}`

Final file paths by location:
- Project: `.claude/commands/{command-name}.md`
- Global: `~/.claude/commands/{command-name}.md`
- Current Directory: `./{command-name}/{command-name}.md`

**Template Selection by Type**:

Use optimized template based on command type selected in Phase 1.

---

**Type 1: Simple Task Template** (50-150 words):
```markdown
---
name: {command-name}
description: "{command-description}"
---

# /{command-name}

{brief_description_1-2_sentences}

## Triggers
{trigger_condition_list}

## Usage
\```bash
/{command-name} [arguments]
\```

## Behavioral Flow
1. **{step1}**: {description}
2. **{step2}**: {description}

## Example
\```bash
{usage_example}
\```
{example_description}

## Boundaries

**Will:**
{tasks_to_perform_list}

**Will Not:**
{tasks_not_to_perform_list}
```

---

**Type 2: Workflow Pipeline Template** (150-400 words):
```markdown
---
name: {command-name}
description: "{command-description}"
---

# /{command-name}

{workflow_overview_1-2_paragraphs}

## Triggers
{trigger_condition_list}

## Usage
\```bash
/{command-name} [options]
\```

## Behavioral Flow

### 1. {phase_name}
{phase_description}

Key actions:
- {action1}
- {action2}

### 2. {phase_name}
{phase_description}

Key actions:
- {action1}
- {action2}

### 3. {phase_name}
{phase_description}

## Tool Coordination
{tool_usage_list}

## Examples

### {example_title_1}
\```bash
{example_usage_1}
\```
{example_description_1}

### {example_title_2}
\```bash
{example_usage_2}
\```
{example_description_2}

## Boundaries

**Will:**
{tasks_to_perform_list}

**Will Not:**
{tasks_not_to_perform_list}
```

---

**Type 3: Complex Orchestration Template** (400-800 words):
```markdown
---
name: {command-name}
description: "{command-description}"
---

# /{command-name}

{complex_workflow_overview_2-3_paragraphs}

## Triggers
{trigger_condition_list}

## Usage
\```bash
/{command-name} [options] [--flags]
\```

### Options
- `--option1`: {description}
- `--option2`: {description}

### Flags
- `--flag1`: {description}
- `--flag2`: {description}

## Behavioral Flow

### Phase 1: {phase_name}
{detailed_phase_description}

**Steps:**
1. {step1_description}
2. {step2_description}
3. {step3_description}

**Conditions:**
- If {condition1}: {action1}
- If {condition2}: {action2}

### Phase 2: {phase_name}
{detailed_phase_description}

**Steps:**
1. {step1_description}
2. {step2_description}

**Error Handling:**
- {error1}: {handling_method}
- {error2}: {handling_method}

### Phase 3: {phase_name}
{detailed_phase_description}

**Validation:**
- {validation1}
- {validation2}

## Tool Coordination

### Primary Tools
- **{tool_name}**: {usage_purpose_and_method}

### Secondary Tools
- **{tool_name}**: {usage_purpose_and_method}

## Key Patterns

### {pattern_name}
{pattern_description_and_example}

### {pattern_name}
{pattern_description_and_example}

## Examples

### Example 1: {scenario_1}
\```bash
{detailed_usage_1}
\```

**Expected Outcome:**
{expected_result_description}

**Troubleshooting:**
{troubleshooting_guide}

### Example 2: {scenario_2}
\```bash
{detailed_usage_2}
\```

**Expected Outcome:**
{expected_result_description}

### Example 3: {scenario_3}
\```bash
{detailed_usage_3}
\```

## Boundaries

**Will:**
{detailed_tasks_to_perform_list}

**Will Not:**
{detailed_tasks_not_to_perform_list}

**Safety Checks:**
{safety_check_items}
```

---

**2. Execute Validation**

After file creation, perform automatic validation based on @shared/command/validation-criteria.md criteria:

1. **Verify File Creation**
   - Verify directory was created correctly
   - Verify command file exists

2. **Content Validation** (validation-criteria.md criteria)
   - Verify Frontmatter (---) format is correct
   - Validate name and description fields
   - Verify all required sections are included (Triggers, Usage/Flow, Boundaries)
   - Verify code block language specification
   - Verify markdown syntax is correct

3. **Validation Results Report**
   ```
   ✅ Validation complete!

   📊 Quality score: {score}/100 ({grade})

   ✅ Passed validations:
   - {validation_item_1}
   - {validation_item_2}

   ⚠️ Areas for improvement:
   - {improvement_item_1}
   - {improvement_item_2}

   💡 For detailed evaluation, check with /evaluate-command.
   ```

---

**3. User Guidance**

   ```
   ✅ Command created!

   📂 {location_path}/{command-name}.md
   🎯 {scope_description}

   Usage: /{command-name}
   Evaluate: /evaluate-command
   ```

   **Location Details**:
   - **Project** (.claude/commands/): Available in this project only
   - **Global** (~/.claude/commands/): Available in all projects
   - **Current** (./{command-name}/): Move to .claude/commands/ or ~/.claude/commands/ to use

---

## Validation Criteria

Generated commands are automatically validated based on @shared/command/validation-criteria.md criteria:

- ✅ Structure validation (20 points): File location, naming conventions
- ✅ Frontmatter validation (15 points): name, description format
- ✅ Required sections validation (25 points): Triggers, Usage/Flow, Boundaries
- ✅ Content quality validation (25 points): Code blocks, examples, section hierarchy
- ✅ Command type suitability (10 points): Word count, complexity
- ✅ Consistency and completeness (5 points): Typos, completeness

## Success Criteria

- ✅ Analyze conversation content in current session to extract workflow information
- ✅ Automatically recommend command name, triggers, execution steps, and tools based on analysis results
- ✅ Present recommendations to user as options
- ✅ User can select or modify recommendations
- ✅ Input information passes validation checks
- ✅ Standard structured command `.md` file is created
- ✅ Frontmatter and all required sections are included
- ✅ Generated command is ready for immediate use
- ✅ Clear next step guidance is provided to user

## Error Handling

### Invalid Command Name
```
❌ Entered name is not in correct format.
📝 Command name must be in kebab-case format.
   Examples: deploy-production, run-tests, code-review

Please re-enter:
```

### Command Already Exists
```
⚠️ A command with the same name already exists.
📂 Location: plugins/command-creator/commands/{command-name}.md

Please select:
1. Use different name
2. Overwrite existing command (existing content will be lost)
3. Cancel
```

### Missing Required Information
```
⚠️ Required information was not entered.
📋 Please enter the following information: {missing-field}
```

## Notes

1. **Progress Tracking**:
   - Guide progress at the start of each Phase in the format "✅ Phase X complete → 🔄 Phase Y starting: [Phase description]"
   - Clearly indicate current position in overall process: "📊 Progress: X/5 Phases complete"
   - Notify user of checklist update status when Phase completes
   - Example:
     ```
     ✅ Phase 1 complete → 🔄 Phase 2 starting: Define workflow and tools

     📊 Progress: 2/5 Phases complete
     - ✅ Phase 0, 1 complete
     - 🔄 Phase 2 in progress
     - ⏳ Phase 3, 4 pending
     ```

2. **Conditional Phase 0 Execution**:
   - First assess conversation content sufficiency (5+ messages, includes workflow content)
   - If insufficient, skip Phase 0 and proceed in manual input mode
   - If sufficient, analyze conversation then proceed in recommendation mode

2. **Conversation Analysis Priority** (When Phase 0 executes):
   - Must analyze conversation content in current session to extract workflow before asking questions
   - If analysis results are missing or insufficient, notify user then switch to manual input mode

3. **Question Approach by Mode**:
   - **Recommendation mode**: Present extracted content as options and include "manual input" option
   - **Manual input mode**: Request direct user input with examples

4. **AskUserQuestion Usage**: All information gathering proceeds step-by-step through AskUserQuestion tool

5. **Write Tool Usage**: Use Write tool when creating command file

6. **Bash Tool Usage**: Use `mkdir -p` command when creating directories

7. **Validation Required**: Must validate based on @shared/command/validation-criteria.md criteria after file creation

8. **Validation Report**: Provide brief summary of validation results to user

9. **Friendly Feedback**: Guide user on progress at each step

10. **Share Analysis Results**: When Phase 0 completes, present summary of discovered workflows to user

11. **Frontmatter Accuracy**: Verify name and description fields are correctly set

12. **Evaluation Command Guidance**: Inform user that detailed evaluation is available with /evaluate-command after creation

## Conversation Analysis Guidelines

### Checklist for Effective Analysis

- ✅ **Workflow Identification**: Find work flows and automation processes discussed in conversation
- ✅ **Trigger Extraction**: Collect conditional statements like "when ~", "if ~"
- ✅ **Step Identification**: Identify tasks performed in sequence
- ✅ **Tool Collection**: Identify tools used such as Bash commands, file operations
- ✅ **Boundary Identification**: Find constraints like "must", "must not"
- ✅ **Execution Context Understanding**: Identify when and why this workflow is needed

### Recommendation Generation Strategy

**Command Name Generation**:
- Summarize main workflow tasks in 2-4 words
- Follow kebab-case format
- Prefer verb-noun combinations (e.g., deploy-production, run-tests)

**Description Generation**:
- Clearly state workflow purpose and results in 1-2 sentences
- Use "~to do~" format
- Specify concrete tasks and goals

**Workflow Structuring**:
- Restructure steps discussed in conversation in logical order
- Clarify input/output of each step
- Identify dependencies and conditions

---

**Now begin command creation. First analyze conversation in current session, extract workflow information, then complete the command through user dialogue.**
