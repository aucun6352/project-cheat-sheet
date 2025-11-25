# Phase Details - Complete Guide

This document contains detailed instructions for each phase of the skill creation process.

## Phase Overview

| Phase | Purpose | Duration | Mode |
|-------|---------|----------|------|
| 0 | Conversation Analysis | 1-2 min | Conditional |
| 1 | Type & Basic Info | 3-5 questions | Interactive |
| 2 | Content Collection | 5-7 questions | Interactive |
| 3 | File Generation | Automated | Processing |
| 4 | Validation | Automated + Review | Verification |

---

## Phase 0: Conversation Analysis (Conditional)

### Purpose
Analyze current session conversation to automatically generate skill recommendations, reducing manual input needed.

### When to Execute
**Execute Phase 0 if:**
- Conversation message count ≥ 5
- Technical content or code examples present
- Substantive technical discussion occurred

**Skip Phase 0 if:**
- Conversation message count < 5
- Only greetings or simple questions
- No technical content to analyze

### Conversation Sufficiency Criteria

```
✅ Sufficient (Execute Phase 0):
- ≥ 5 meaningful messages
- Technical discussions present
- Code examples or patterns discussed
- Clear domain/topic identified

❌ Insufficient (Skip to Phase 1):
- < 5 messages
- Only simple Q&A
- No technical depth
- Just getting started
```

### Analysis Process

**Step 1: Content Extraction**
Analyze conversation history and extract:
- **Main topics**: What technical domains discussed?
- **Keywords**: What terms used repeatedly?
- **Code examples**: What code was written or discussed?
- **Patterns**: What patterns or approaches emerged?
- **Concepts**: What principles or ideas covered?
- **Best practices**: What recommendations made?
- **Pitfalls**: What problems or issues mentioned?

**Step 2: Metadata Generation**
Based on extracted content, automatically generate:

1. **Skill Names** (2-3 suggestions):
   - Format: kebab-case
   - Style: Gerund form (processing-pdfs, building-apis)
   - Example: "managing-docker-containers", "optimizing-sql-queries"

2. **Description**:
   - Extract key trigger phrases from conversation
   - Format: Third person with specific scenarios
   - Example: "This skill should be used when the user asks to 'optimize database queries', 'improve SQL performance'..."

3. **Usage Scenarios**:
   - Identify when this skill would be helpful
   - Extract from conversation context
   - Format: Multi-line list

4. **Core Concepts**:
   - Summarize key principles discussed
   - Organize main ideas
   - Structure for skill body

5. **Best Practices**:
   - Collect recommendations from conversation
   - Extract "do this" advice

6. **Pitfalls**:
   - Collect warnings from conversation
   - Extract "avoid this" cautions

7. **Examples**:
   - Gather code examples from conversation
   - Note which patterns demonstrated

**Step 3: Present Analysis Results**

```markdown
📊 Conversation analysis complete!

🔍 Discovered content:
- Main topics: {list_of_topics}
- Code examples: {count} items discovered
- Discussed patterns: {pattern_list}
- Core concepts: {concept_count} items
- Best practices: {practice_count} identified
- Potential pitfalls: {pitfall_count} noted

💡 Suggested skill names:
1. {name-suggestion-1}
2. {name-suggestion-2}
3. {name-suggestion-3}

✅ Now proceeding with skill creation using these recommendations.
```

### Insufficient Content Guidance

When conversation is insufficient:

```markdown
📊 Conversation content analysis result

ℹ️ Current session conversation content is insufficient for automatic recommendations.

💡 Proceeding in manual input mode.
   - Please manually enter all information
   - Or have a conversation about the skill topic first and try again

🔄 Switching to Phase 1 with manual input mode.
```

### Transition to Phase 1

- **If analysis successful**: Set mode to "recommendation mode" for Phase 1-2
- **If insufficient**: Set mode to "manual input mode" for Phase 1-2

---

## Phase 1: Type & Basic Information

### Purpose
Collect fundamental skill metadata: type, name, location, description, usage scenarios.

### Question Pattern by Mode

**Recommendation Mode** (after successful Phase 0):
- Present extracted content
- Offer "Use as-is" / "Modify" / "Manual entry" options
- Reduce friction with smart defaults

**Manual Input Mode** (Phase 0 skipped):
- Provide format guidance
- Show examples
- Request user input

### Step 1: Skill Type Selection

**Purpose**: Determine optimal template and structure for skill.

**Question Format**:
```
Which type of skill are you creating?

1. Quick Workflow (200-400 words)
   - Single-purpose sequential process
   - Simple tools, quick automation
   - Example: File converters, validation tools

2. Comprehensive Guide (600-1,500 words) [Default]
   - Multi-functional with examples
   - Standard development workflows
   - Example: API integration, testing strategies

3. Technical Reference (1,500-5,000+ words)
   - In-depth API documentation
   - Complex toolkits, libraries
   - Example: Database toolkits, cloud SDKs

4. Philosophy-Driven (1,000-3,000 words)
   - Conceptual framework + implementation
   - Creative processes, judgment tasks
   - Example: Design thinking, architecture approaches

Default: Comprehensive Guide
```

**Use AskUserQuestion tool** with:
- header: "Type"
- multiSelect: false
- 4 options as above

**Store**: Selected type for template application in Phase 3

**Reference**: See `skill-types.md` for detailed type descriptions

### Step 2: Skill Name

**Purpose**: Create properly formatted, descriptive skill identifier.

**Format Requirements**:
- kebab-case (lowercase with hyphens)
- Max 64 characters
- Descriptive and clear
- Gerund form recommended (processing-X, building-Y, managing-Z)

**Recommendation Mode**:
```
Based on conversation analysis, here are suggested skill names:

1. {extracted-name-1}
2. {extracted-name-2}
3. {extracted-name-3}
4. Enter custom name

Which would you like to use?
```

**Manual Input Mode**:
```
Enter skill name (kebab-case format):

Examples:
- processing-pdf-documents
- building-rest-apis
- managing-docker-containers
- optimizing-database-queries

Your skill name:
```

**Validation**:
- Check kebab-case format: `^[a-z0-9]+(-[a-z0-9]+)*$`
- Check length ≤ 64 characters
- If invalid: Re-ask with error message

**Common Mistakes**:
- ❌ camelCase: processDocuments
- ❌ snake_case: process_documents
- ❌ UPPERCASE: PROCESS-DOCUMENTS
- ❌ Spaces: process documents
- ✅ Correct: processing-documents

### Step 3: File Location

**Purpose**: Determine where to create the skill.

**Question Format**:
```
Where would you like to create this skill?

1. Project (.claude/skills/) [Recommended]
   - Available only in current project
   - Project-specific skill
   - Path: .claude/skills/{skill-name}/

2. Global (~/.claude/skills/)
   - Available in all projects
   - Reusable across projects
   - Path: ~/.claude/skills/{skill-name}/

3. Current Directory (./{skill-name}/)
   - Created in current working directory
   - Useful for plugin development or distribution
   - Path: ./{skill-name}/

Default: Project

💡 Location Guide:
  - Most skills should be project-specific (Option 1)
  - Create global skills only if truly reusable
  - Use current directory for plugin development
```

**Use AskUserQuestion tool** with:
- header: "Location"
- multiSelect: false
- 3 options as above

**Store Location Path**:
- Option 1 → `{location_path} = ".claude/skills"`
- Option 2 → `{location_path} = "~/.claude/skills"`
- Option 3 → `{location_path} = "."`

This path is used in Phase 3 for file creation.

### Step 4: Skill Description

**Purpose**: Create third-person description with specific trigger phrases for skill discovery.

**Format Requirements**:
- Third person: "This skill should be used when..."
- Include specific trigger phrases: "create X", "build Y", "manage Z"
- Max 1024 characters
- 1-2 sentences
- Specify "what + when"

**Recommendation Mode**:
```
Based on conversation analysis, here's the generated description:

"{auto_generated_description}"

Options:
1. Use as-is
2. Modify (you'll be able to edit)
3. Write from scratch

Choose option:
```

**Manual Input Mode**:
```
Enter skill description (third-person format):

Format: "This skill should be used when the user asks to \"trigger phrase 1\", \"trigger phrase 2\", \"trigger phrase 3\". Additional context about purpose."

Good example:
"This skill should be used when the user asks to \"create a hook\", \"add a PreToolUse hook\", \"validate tool use\", or mentions hook events. Provides comprehensive hooks API guidance."

Bad examples:
- "Use this skill when working with hooks." (Not third person)
- "Provides hook guidance." (No trigger phrases)
- "Load when user needs help." (Vague, no specifics)

Your description:
```

**Validation**:
- Required field (cannot be empty)
- Length ≤ 1024 characters
- Should include specific trigger phrases
- Should use third person

**Quality Check**:
- ✅ Third person format
- ✅ Specific trigger phrases (3+ recommended)
- ✅ Clear use case scenarios
- ✅ Within character limit

### Step 5: Usage Scenarios

**Purpose**: Define when this skill should be triggered.

**Format**: Multi-line list of scenarios

**Recommendation Mode**:
```
Based on conversation, here are identified usage scenarios:

{scenario_1}
{scenario_2}
{scenario_3}

Options:
1. Use these scenarios
2. Add more scenarios
3. Modify existing
4. Write from scratch

Choose option:
```

**Manual Input Mode**:
```
Enter usage scenarios (one per line):

Examples:
- When designing a new API endpoint
- During code review for API changes
- When documenting API specifications
- During API performance optimization

Your scenarios (enter each on a new line, or provide as list):
```

**Format Guidelines**:
- Start with "When..." or "During..." for context
- Be specific about the situation
- Include 3-5 scenarios minimum
- Cover different use cases

---

## Phase 2: Content Writing

### Purpose
Collect detailed content: core concepts, patterns, examples, best practices, pitfalls, formatting preferences, tool restrictions.

### Question Pattern
Same as Phase 1: Recommendation mode or Manual input mode based on Phase 0 results.

### Step 6: Core Concepts

**Purpose**: Capture main concepts, principles, and theories for skill body.

**Format**: Free-form text, multiple paragraphs allowed

**Recommendation Mode**:
```
Based on conversation, here are the core concepts extracted:

{auto_organized_concepts}

Options:
1. Use as-is
2. Edit/expand
3. Write from scratch

Choose option:
```

**Manual Input Mode**:
```
Enter core concepts and principles for this skill:

Include:
- Main concepts that need to be understood
- Key principles or theories
- Foundational knowledge
- Important context

You can use multiple paragraphs. Be as detailed as needed.

Your core concepts:
```

**Content Guidelines**:
- Explain fundamental principles
- Provide necessary context
- Include theoretical background if relevant
- Balance depth with clarity
- Multiple paragraphs are fine

### Step 7: Patterns/Examples (Optional)

**Purpose**: Collect code examples and patterns to include in skill.

**Question Flow**:
1. Ask if examples should be included
2. If yes, collect examples with quality level
3. If no, skip to Step 8

**Recommendation Mode**:
```
Found {count} code examples in conversation:

{example_1_preview}
{example_2_preview}
{example_3_preview}

Options:
1. Include selected examples (choose which)
2. Include all examples
3. Add more examples
4. Skip examples

Choose option:
```

**Manual Input Mode**:
```
Do you want to include code examples or patterns?

1. Yes - I'll provide examples
2. No - Skip examples

Choose option:
```

**If including examples**, ask for quality level:

```
What quality level should examples have?

1. Basic: Syntax demonstration only (minimal)
   - Shows basic syntax
   - Minimal context
   - Good for reference

2. Standard: Working code with actual values (recommended)
   - Complete, runnable code
   - Realistic values
   - Common use cases

3. Production: Error handling + edge cases (advanced)
   - Full error handling
   - Edge case coverage
   - Production-ready code

Choose level:
```

**Example Collection Format**:
For each example:
- Pattern name/title
- Programming language
- Code block
- Optional explanation

```
Example {n}:

Pattern name: {name}
Language: {language}
Code:
```{language}
{code_block}
```
Explanation (optional): {explanation}
```

### Step 8: Best Practices

**Purpose**: Collect recommended practices and guidelines.

**Format**: List format

**Recommendation Mode**:
```
Based on conversation, these best practices were identified:

- {practice_1}
- {practice_2}
- {practice_3}

Options:
1. Use these practices
2. Add more practices
3. Edit existing
4. Write from scratch

Choose option:
```

**Manual Input Mode**:
```
Enter best practices (one per line or as list):

Examples:
- Use clear, descriptive naming
- Implement consistent error handling
- Write comprehensive tests
- Document public APIs
- Follow framework conventions

Your best practices:
```

**Content Guidelines**:
- Start with imperative verbs (Use, Implement, Write)
- Be specific and actionable
- Include rationale if helpful
- 3-7 practices recommended

### Step 9: Common Pitfalls

**Purpose**: Collect warnings and anti-patterns to avoid.

**Format**: List format

**Recommendation Mode**:
```
Based on conversation, these pitfalls were identified:

- {pitfall_1}
- {pitfall_2}
- {pitfall_3}

Options:
1. Use these pitfalls
2. Add more
3. Edit existing
4. Write from scratch

Choose option:
```

**Manual Input Mode**:
```
Enter common pitfalls to avoid (one per line or as list):

Examples:
- Avoid over-abstraction early
- Don't ignore error handling
- Consider performance implications
- Test edge cases thoroughly

Your pitfalls:
```

**Content Guidelines**:
- Start with "Avoid..." or "Don't..."
- Explain why it's a pitfall
- Provide alternative approach if possible
- 3-5 pitfalls recommended

### Step 10: Formatting Style (Optional)

**Purpose**: Determine visual formatting preferences for skill content.

**Question Format**:
```
Choose formatting style preferences:

1. Minimal (recommended)
   - Clean, simple formatting
   - Emoji only when genuinely helpful
   - Focus on content clarity

2. Enhanced
   - Section emoji markers (🚀, 📋, ⚠️)
   - Visual indicators (✅/❌)
   - Structured tables

3. Custom
   - Specify your preferences

Default: Minimal

Choose option:
```

**Formatting Options**:

**Emoji Markers**:
- 🚀 Quick Start / Getting Started
- 📋 Core Concepts / Documentation
- ⚠️ Common Pitfalls / Warnings
- ✅ Best Practices / Do This
- 💡 Tips / Insights

**Visual Markers**:
- ✅ Do this (best practices)
- ❌ Don't do this (pitfalls)

**Tables**:
- Quick reference tables
- Comparison tables
- API reference tables

**Default**: Minimal use - only when it genuinely improves clarity

### Step 11: Allowed Tools (Optional)

**Purpose**: Restrict which tools Claude can use when this skill is active.

**Why Restrict Tools?**
- Create focused, predictable workflows
- Prevent unwanted side effects
- Ensure specific tool usage patterns
- Control execution environment

**When to Restrict?**
- Skills requiring specific tool workflows
- Skills that should NOT modify files
- Skills that should NOT execute commands
- Skills with strict tool requirements

**Question Flow**:

**Recommendation Mode**:
```
Analyzing conversation for tool usage patterns...

Detected tool usage:
- Read: {count} times
- Grep: {count} times
- Bash: {count} times

Recommendation:
{if_clear_pattern}
  Suggest restricting to: {tool_list}
{else}
  Suggest no restriction (allow all tools)
{endif}

Options:
1. Use recommendation
2. Specify custom tool list
3. No restriction (all tools available)

Choose option:
```

**Manual Input Mode**:
```
Do you want to restrict tool access for this skill?

⚠️ Most skills should NOT restrict tools (choose "No restriction").

Restrict tools when:
- Skill requires specific tool workflow
- Preventing unwanted side effects is critical
- Specific tools must be used in specific ways

Options:
1. Yes - Restrict to specific tools
2. No - Allow all tools (recommended)

Choose option:
```

**If restricting, present tool selection**:
```
Select which tools to allow (multi-select):

Common tools:
□ Read - Read files
□ Write - Create files
□ Edit - Modify files
□ Grep - Search content
□ Glob - Find files
□ Bash - Execute commands
□ WebSearch - Web search
□ WebFetch - Fetch web content
□ Task - Launch agents
□ SlashCommand - Execute commands

Select tools to allow:
```

**Use AskUserQuestion tool** with:
- header: "Tools"
- multiSelect: true
- Options for each common tool

**Result Format**:
- If restricted: Store comma-separated list: `"Read, Grep, Glob, Bash"`
- If not restricted: Store null or omit from frontmatter

---

## Phase 3: File Generation

### Purpose
Generate skill files based on collected information and Progressive Disclosure principles.

### Process Flow

**Step 1: Calculate Total Word Count**

Sum all content:
- Core concepts word count
- Examples word count (code + explanations)
- Best practices word count
- Pitfalls word count
- Additional sections word count

**Total = Core + Examples + Practices + Pitfalls + Other**

**Step 2: Determine Progressive Disclosure Level**

Based on total word count:
- **< 400 words** → Level 1 (SKILL.md only)
- **400-1,500 words** → Level 2 (SKILL.md + examples/)
- **1,500-3,500 words** → Level 3 (+ REFERENCE.md)
- **> 3,500 words** → Level 4 (+ EXAMPLES.md + FORMS.md)

See `progressive-disclosure.md` for complete level specifications.

**Step 3: Create Directory Structure**

**Level 1**:
```bash
mkdir -p {location_path}/{skill-name}
```

**Level 2**:
```bash
mkdir -p {location_path}/{skill-name}/examples
```

**Level 3**:
```bash
mkdir -p {location_path}/{skill-name}/examples
```

**Level 4**:
```bash
mkdir -p {location_path}/{skill-name}/examples/{basic,advanced}
```

**Step 4: Apply Type-Specific Template**

Based on selected type (Phase 1, Step 1), apply template from `templates.md`:
- Quick Workflow template
- Comprehensive Guide template
- Technical Reference template
- Philosophy-Driven template

**Step 5: Populate Content**

**Frontmatter**:
```yaml
---
name: {skill-name}
description: "{skill-description}"
{if allowed-tools specified}
allowed-tools: {tool-list}
{endif}
---
```

**Body Sections** (based on type):
- Title (H1)
- Overview/Quick Start/Philosophy (based on type)
- When to Use This Skill
- Core Concepts
- Patterns/Examples (if provided)
- Best Practices
- Common Pitfalls
- Additional Resources (with references to supporting files if Level 3+)

**Step 6: Create Supporting Files**

**Level 2**: Create example files in `examples/`

**Level 3**:
- Create REFERENCE.md with detailed content
- Create example files

**Level 4**:
- Create REFERENCE.md
- Create EXAMPLES.md
- Create FORMS.md
- Create organized examples/

**Step 7: Apply Formatting Style**

Based on Step 10 selection:
- Minimal: Clean formatting, minimal emoji
- Enhanced: Section markers, visual indicators
- Custom: User-specified formatting

### File Writing

Use **Write tool** for all file creation:
- `Write {location_path}/{skill-name}/SKILL.md`
- `Write {location_path}/{skill-name}/REFERENCE.md` (Level 3+)
- `Write {location_path}/{skill-name}/EXAMPLES.md` (Level 4)
- `Write {location_path}/{skill-name}/FORMS.md` (Level 4)
- `Write {location_path}/{skill-name}/examples/{filename}` (Level 2+)

---

## Phase 4: Validation & Completion

### Purpose
Validate generated skill against quality standards and guide user on usage.

### Validation Process

Follow validation procedures in `@shared/skill/validation-criteria.md`.

**Step 1: Structure Validation**
- ✅ File existence check
- ✅ Progressive Disclosure appropriateness
- ✅ Directory structure correct

**Step 2: Frontmatter Validation**
- ✅ `name` field present and valid format
- ✅ `description` field present, properly quoted, within limit
- ✅ `allowed-tools` format correct (if present)

**Step 3: Required Sections**
- ✅ Title (H1) present
- ✅ "When to Use This Skill" section present
- ✅ "Core Concepts" section present
- ✅ "Best Practices" section present (recommended)

**Step 4: Content Quality**
- ✅ Code blocks have language specified
- ✅ Section hierarchy correct (H1 → H2 → H3)
- ✅ Links are valid (if present)
- ✅ Cross-references correct (Level 3+)

### Validation Report

Generate simple summary:

```markdown
📊 Generation complete - Validation result

## Basic quality check
✅ File structure: {status}
✅ Frontmatter: {status}
✅ Required sections: {status}
{if any issues}
⚠️ Improvable items:
- {issue_1}
- {issue_2}
{endif}

💡 For detailed evaluation, use the `/evaluate-skill` command.
```

### User Guidance

```markdown
✅ Skill created successfully!

📂 Location: {full_path}
🎯 Scope: {scope_description}

**How to use:**
@{skill-name}/SKILL.md

**Next steps:**
- Test the skill by using it in conversations
- Run `/evaluate-skill` for detailed quality analysis
- Iterate based on usage patterns

{if location is current directory}
⚠️ Note: Skill created in current directory. To use:
- Move to .claude/skills/ for project scope
- Move to ~/.claude/skills/ for global scope
{endif}
```

**Scope Descriptions**:
- Project: "Available in this project only"
- Global: "Available in all projects"
- Current: "Created in current directory - move to .claude/skills/ or ~/.claude/skills/ to activate"

### Completion Checklist

Before marking Phase 4 complete:
- ✅ All files generated successfully
- ✅ Validation performed
- ✅ No critical errors
- ✅ User guidance provided
- ✅ Next steps communicated

---

## Error Handling

### Phase 0 Errors
- **Insufficient conversation**: Skip to Phase 1 with manual mode
- **Analysis failure**: Fall back to manual mode

### Phase 1 Errors
- **Invalid skill name**: Re-ask with format explanation
- **Skill already exists**: Offer options (rename, overwrite, cancel)
- **Invalid location**: Use default (.claude/skills/)

### Phase 2 Errors
- **Missing required info**: Mark as required, re-ask
- **Invalid format**: Provide format guidance, re-ask
- **Content too long**: Warn about Progressive Disclosure level

### Phase 3 Errors
- **Directory creation failure**: Report error, suggest manual creation
- **File write failure**: Report error, provide content for manual creation
- **Template application error**: Fall back to basic template

### Phase 4 Errors
- **Validation failure**: Report issues, offer to fix
- **Critical errors**: Must fix before completion
- **Warnings**: Report but allow completion

---

## Progress Tracking

### At Start of Each Phase

```markdown
✅ Phase {N-1} complete → 🔄 Starting Phase {N}: [Phase description]

📊 Overall progress: {N-1}/4 Phases complete ({percentage}%)
```

### Within Phase (Phase 1-2)

```markdown
🔄 Phase {N}: [Phase name]
📊 Phase progress: {current}/{total} questions complete ({percentage}%)
📊 Overall progress: {N-1}/4 Phases complete ({percentage}%)
```

### Phase 3 (Processing)

```markdown
🔄 Phase 3: File Generation

⏳ Analyzing content... ({word_count} words)
⏳ Determining Progressive Disclosure level... (Level {level})
⏳ Creating directory structure...
⏳ Generating SKILL.md...
{if Level 3+}
⏳ Generating REFERENCE.md...
{endif}
✅ Files generated successfully

📊 Overall progress: 3/4 Phases complete (75%)
```

### Phase 4 (Validation)

```markdown
🔄 Phase 4: Validation and Completion

⏳ Validating structure...
⏳ Checking frontmatter...
⏳ Verifying required sections...
⏳ Analyzing content quality...
✅ Validation complete

📊 Overall progress: 4/4 Phases complete (100%)
```

---

## Quality Standards

### Content Quality
- Imperative/infinitive form throughout body
- Third-person description in frontmatter
- Specific trigger phrases (3+ recommended)
- Clear, actionable guidance
- Complete examples (if included)

### Structure Quality
- Proper Progressive Disclosure applied
- Files organized logically
- Cross-references are clear
- No content duplication
- Supporting files have clear purpose

### Technical Quality
- Valid YAML frontmatter
- Correct markdown syntax
- Proper code block formatting
- Valid links and references
- Executable examples (if code provided)

---

## Best Practices

### Interactive Approach
- Ask one question at a time (or small groups)
- Provide clear examples
- Offer smart defaults
- Explain rationale when helpful

### Content Collection
- Prefer recommendation mode when possible
- Always offer "Manual entry" option
- Validate input before proceeding
- Provide immediate feedback

### File Generation
- Apply appropriate Progressive Disclosure level
- Use type-specific templates
- Create only needed directories
- Reference supporting files clearly

### Validation
- Perform all checks
- Report issues clearly
- Distinguish critical vs. warnings
- Offer to fix issues

### User Experience
- Show progress clearly
- Provide next steps
- Make success obvious
- Guide on skill usage

---

## Summary

Each phase builds on the previous:
- **Phase 0**: Smart defaults from conversation
- **Phase 1**: Foundation (type, name, location, description)
- **Phase 2**: Content (concepts, examples, practices)
- **Phase 3**: Generation (files, structure, templates)
- **Phase 4**: Validation (quality, guidance, completion)

Result: High-quality skill following all best practices, ready for immediate use.
