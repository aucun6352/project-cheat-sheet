# Create Skill Command

This command interactively creates a new Skill that can be used in Claude Code.

## Purpose

This command interactively creates standardized skill documentation through conversational interaction with the user. It enables rapid creation of consistently high-quality skills without manually building complex file structures.

**💡 Note:** If you want to evaluate an existing SKILL.md, use the `/evaluate-skill` command.

## Extended Thinking

You operate as a skill creation expert with the following behavior:

**📋 Progress Checklist**:
- [ ] Phase 0: Conversation analysis and recommendation generation (conditional)
- [ ] Phase 1: Skill type selection + basic information collection (type, name, description, usage scenarios, License)
- [ ] Phase 2: Content writing (core concepts, patterns/examples, best practices, pitfalls, formatting)
- [ ] Phase 3: File generation (applying Progressive Disclosure + type-specific templates)
- [ ] Phase 4: Validation and completion (validation against @shared/skill/validation-criteria.md + user guidance)

**🔄 Progress Flow**:
At the start of each Phase, guide progress as follows:
```
✅ Phase X complete → 🔄 Starting Phase Y: [Phase description]

📊 Overall progress: X/4 Phases complete (YY%)
```

**Progress display within Phase:**
In Phases 1 and 2, also display question progress:
```
🔄 Phase 2: Content writing
📊 Phase progress: 3/5 questions complete (60%)
📊 Overall progress: 2/4 Phases complete (50%)
```

**Core Actions**:
1. **Conversation analysis and recommendations**: Analyze session conversation → automatically suggest skill metadata
2. **Type and information collection**: Select skill type → collect information through optimized questions
3. **Template-based generation**: Apply type-specific template + Progressive Disclosure
4. **Quality validation and guidance**: Validate against @shared/skill/validation-criteria.md and guide usage

## Execution Steps

### Phase 0: Conversation Analysis and Recommendation Generation (Conditional)

**⚠️ Important**: If the current session's conversation content is insufficient, skip this step and move directly to **Phase 1**.

**Conversation content sufficiency criteria**:
- If conversation message count is less than 5 → Skip Phase 0
- If there is no technical content or code examples → Skip Phase 0
- If there are only simple greetings or questions → Skip Phase 0

**Only perform the following if conversation content is sufficient:**

1. **Conversation history analysis and automatic generation**
   - Identify and extract main topics/keywords/code examples
   - Suggest 2-3 skill names (kebab-case, Gerund form)
   - Automatically generate skill description, usage scenarios, core concepts
   - Collect best practices and pitfalls

2. **Analysis result summary**
   ```
   📊 Conversation analysis complete!

   🔍 Discovered content:
   - Main topics: {identified_topics}
   - Code examples: {discovered_example_count} items
   - Discussed patterns: {pattern_list}
   - Core concepts: {concept_count} items

   💡 Now generating skill based on recommendations.
   ```

**Guidance when conversation content is insufficient**:
   ```
   📊 Conversation content analysis result

   ℹ️ Current session conversation content is insufficient for automatic recommendations.

   💡 Proceeding in manual input mode.
      - Please manually enter all information
      - Or have a conversation about the skill topic first and try again
   ```

### Phase 1: Skill Type Selection + Basic Information Collection

**Step 1: Skill type selection**

**Purpose**: Provide optimized templates and guides suited to skill characteristics

**4 Skill Types**:

1. **Quick Workflow** (200-400 words) - Single-purpose sequential process
   - Suitable for: Simple tools, quick automation

2. **Comprehensive Guide** (600-1,500 words) - Multi-functional + examples
   - Suitable for: Standard development workflows

3. **Technical Reference** (1,500-5,000+ words) - In-depth API documentation
   - Suitable for: Complex toolkits, libraries

4. **Philosophy-Driven** (1,000-3,000 words) - Conceptual framework + implementation
   - Suitable for: Creative processes, judgment-based tasks

**Execution**: Provide type selection via AskUserQuestion (default: Comprehensive Guide)

**Steps 2-5: Basic information collection**

**Question pattern** (common):
- **Recommendation mode** (Phase 0 completed): Present extracted content → choose "Use as-is/Modify/Add"
- **Manual input mode** (Phase 0 skipped): Format guidance → Provide example → Request input

**Information collected**:

2. **Skill name**: kebab-case, within 64 characters, Gerund form recommended (e.g., processing-pdfs)
   - Recommendation mode: Present 2-3 names + direct input option
   - Validation: Re-ask on format errors

3. **File location**: Select where to create the skill
   ```
   Question: "Where would you like to create this skill?"

   Options:
     1. **Project** (.claude/skills/) - Recommended
        - Available only in current project
        - Project-specific skill
        - Default choice

     2. **Global** (~/.claude/skills/)
        - Available in all projects
        - Reusable across projects
        - Good for general-purpose skills

     3. **Current Directory** (./{skill-name}/)
        - Created in current working directory
        - Useful for plugin development or distribution

   Default: Project (.claude/skills/)

   💡 Location Guide:
     - Most skills should be project-specific (Option 1)
     - Create global skills only if they're truly reusable
     - Use current directory for plugin development
   ```

   **Store the selected location path**:
   - Option 1 → `{location_path} = ".claude/skills"`
   - Option 2 → `{location_path} = "~/.claude/skills"`
   - Option 3 → `{location_path} = "."`

4. **Skill description**: Within 1024 characters, 1-2 sentences, 3rd person, specify "what+when"
   - Recommendation mode: Present auto-generated description + modification option
   - Validation: Required field

5. **Usage scenarios**: Multi-line list format
   - Recommendation mode: Present extracted scenarios + add/modify options
   - Examples: "When designing a new API", "During code review", etc.

6. **License** (optional): Whether to include license field in Frontmatter
   - Recommended: "Complete terms in LICENSE.txt" (standard format)
   - Can choose not to include

### Phase 2: Content Writing

**Question pattern**: Same as Phase 1 (recommendation mode/manual input mode)

⚠️ **Writing precautions**: Refer to Phase 4 quality validation criteria

**Information collected**:

7. **Core concepts**: Free-form text, multiple paragraphs allowed
   - Include main concepts, principles, theories, etc.
   - Recommendation mode: Present auto-organized content + edit option

8. **Patterns/Examples** (optional):
   - Code blocks: Pattern name + language + code
   - Recommendation mode: Present discovered examples + select/add/exclude options
   - Manual input mode: Choose whether to include → Enter examples
   - **Example quality level selection**:
     - Basic: Syntax demonstration only (minimal)
     - Standard: Working code with actual values (recommended)
     - Production: Error handling + edge cases (advanced)

9. **Best practices**: List format
   - Examples: "Use clear naming", "Consistent error handling"
   - Recommendation mode: Present collected practices + add option

10. **Pitfalls**: List format
   - Examples: "Avoid over-abstraction", "Consider performance"
   - Recommendation mode: Present identified pitfalls + add option

11. **Formatting style** (optional):
    - **Emoji markers**: For section separation (🚀, 📋, ⚠️, etc.)
    - **Visual markers**: Best practice indicators (✅/❌)
    - **Table usage**: For quick reference
    - Default: Minimal use (only when necessary)

### Phase 3: File Generation

Based on collected information, generate the following file structure at the selected location (`{location_path}`):

**Progressive Disclosure criteria** (based on word count):

**Level 1** (< 400 words):
```bash
mkdir -p {location_path}/{skill-name}
Write {location_path}/{skill-name}/SKILL.md
```
```
{location_path}/{skill-name}/
└── SKILL.md          # All content included
```

**Level 2** (400-1,500 words):
```bash
mkdir -p {location_path}/{skill-name}/examples
Write {location_path}/{skill-name}/SKILL.md
```
```
{location_path}/{skill-name}/
├── SKILL.md          # Overview + core guide
└── examples/         # Detailed examples directory
```

**Level 3** (1,500-3,500 words):
```bash
mkdir -p {location_path}/{skill-name}/examples
Write {location_path}/{skill-name}/SKILL.md
Write {location_path}/{skill-name}/REFERENCE.md
```
```
{location_path}/{skill-name}/
├── SKILL.md          # Overview + When to Use + simple examples
├── REFERENCE.md      # Detailed API documentation
└── examples/         # Examples and templates
```

**Level 4** (> 3,500 words):
```bash
mkdir -p {location_path}/{skill-name}
Write {location_path}/{skill-name}/SKILL.md
Write {location_path}/{skill-name}/EXAMPLES.md
Write {location_path}/{skill-name}/REFERENCE.md
Write {location_path}/{skill-name}/FORMS.md
```
```
{location_path}/{skill-name}/
├── SKILL.md          # Overview (with reference links)
├── EXAMPLES.md       # Detailed examples
├── REFERENCE.md      # API/technical documentation
└── FORMS.md          # Templates/checklists
```

**Location-specific paths**:
- Project: `.claude/skills/{skill-name}/`
- Global: `~/.claude/skills/{skill-name}/`
- Current Directory: `./{skill-name}/`

**Common section components**:

**Frontmatter** (required):
```yaml
---
name: {skill-name}           # kebab-case, within 64 characters
description: {skill-description}     # Within 1024 characters, "what+when"
license: Complete terms in LICENSE.txt  # Optional, used by 85% of skills
---
```

**Required body sections**:
- `# {Skill Title}` (H1, only one)
- `## When to Use This Skill` - Usage scenarios, triggers
- `## Core Concepts` - Core concepts, principles

**Recommended body sections** (choose based on skill type):
- `## Quick Start` / `## Overview` / `## Philosophy` - Starting point (choose by type)
- `## Patterns` / `## Examples` - Patterns and examples
- `## Best Practices` - Best practices
- `## Common Pitfalls` - Pitfalls, anti-patterns
- `## Additional Resources` - Additional resources, reference links

**Type-specific template structures**:

**1. Quick Workflow type** (200-400 words):
```markdown
---
name: quick-tool-name
description: Brief description of what and when
---

# Quick Tool Name

## Quick Start
[3-5 immediately usable steps]

## When to Use This Skill
- Trigger 1
- Trigger 2

## Detailed Workflow
[Step-by-step detailed guide]

## Common Pitfalls
- List of pitfalls
```

**2. Comprehensive Guide type** (600-1,500 words):
```markdown
---
name: comprehensive-skill-name
description: Detailed description of purpose and context
license: Complete terms in LICENSE.txt
---

# Comprehensive Skill Name

## Overview
[1-2 paragraph introduction]

## When to Use This Skill
- Use case 1
- Use case 2
- Use case 3

## Core Concepts
[Core concept explanation]

## How to Use
[Step-by-step workflow or patterns]

## Patterns / Examples
[Code examples]

## Best Practices
- Best practice 1
- Best practice 2

## Common Pitfalls
- Pitfalls

## Additional Resources
- Related resource links
```

**3. Technical Reference type** (1,500-5,000+ words):
```markdown
---
name: technical-toolkit-name
description: Comprehensive toolkit for specific technical domain
license: Complete terms in LICENSE.txt
---

# Technical Toolkit Name

## Overview
[Tech stack and purpose]

## When to Use This Skill
- Complex scenario 1
- Complex scenario 2

## Quick Reference
| Task | Tool/Method | Example |
|------|------------|---------|
| ... | ... | ... |

## Core Concepts
[Theoretical background]

## Detailed API Reference
[Detailed API documentation]

## Examples
### Example 1: [Use Case]
[Complete code example]

### Example 2: [Use Case]
[Complete code example]

## Best Practices
- Practice 1
- Practice 2

## Additional Resources
- [@{skill-name}/REFERENCE.md](./REFERENCE.md)
- [@{skill-name}/EXAMPLES.md](./EXAMPLES.md)
```

**4. Philosophy-Driven type** (1,000-3,000 words):
```markdown
---
name: creative-approach-name
description: Conceptual framework for creative/judgment tasks
license: Complete terms in LICENSE.txt
---

# Creative Approach Name

## Philosophy / Approach
[Conceptual framework, mindset]

## When to Use This Skill
- Creative scenario 1
- Judgment-based scenario 2

## Core Concepts
[Core principles and theories]

## Implementation Workflow
1. [Concept application step]
2. [Execution step]
3. [Validation step]

## Patterns / Examples
[Concept demonstration examples]

## Best Practices
- Creative approach guide

## Common Pitfalls
- Traps to avoid

## Additional Resources
- Related philosophy/theory links
```

**Note**:
- Level 3-4 skills include reference links in the Additional Resources section of the above templates
- When generating Progressive Disclosure files, utilize `examples/`, `REFERENCE.md`, `EXAMPLES.md`, `FORMS.md`

### Phase 4: Validation and Completion

**Validation execution**:

Follow the validation procedures in @shared/skill/validation-criteria.md.

**1. Perform basic validation**:
   - Structure validation (file existence, Progressive Disclosure appropriateness)
   - Frontmatter validation (name, description format)
   - Required section validation (When to Use, Core Concepts, Best Practices)
   - Content quality validation (code blocks, section hierarchy, links)

**2. Generate simple summary report**:

```markdown
📊 Generation complete - Simple validation result

## Basic quality check
✅ File structure: Appropriate
✅ Frontmatter: Correct format
✅ Required sections: All included
⚠️ Improvable items: {If any, briefly list 1-2}

💡 For more detailed evaluation, use the `/evaluate-skill` command.
```

**User guidance**:

   ```
   ✅ Skill created!

   📂 {location_path}/{skill-name}/SKILL.md
   🎯 {scope_description}

   Usage: @{skill-name}/SKILL.md
   Evaluate: /evaluate-skill
   ```

   **Location Details**:
   - **Project** (.claude/skills/): Available in this project only
   - **Global** (~/.claude/skills/): Available in all projects
   - **Current** (./{skill-name}/): Move to .claude/skills/ or ~/.claude/skills/ to use

## Error Handling

### Generation mode errors
- **Invalid skill name**: Request re-entry in kebab-case format
- **Skill already exists**: Provide options to use different name / overwrite / cancel
- **Missing required information**: Request re-entry of missing fields

## Precautions

1. **Progress tracking**:
   - At the start of each Phase, guide progress in the format "✅ Phase X complete → 🔄 Starting Phase Y: [Phase description]"
   - Clearly indicate current position in overall process: "📊 Progress: X/4 Phases complete"

2. **Prioritize conversation analysis** (when executing Phase 0):
   - Always analyze current session conversation content to generate recommendations before asking questions
   - If analysis results are missing or insufficient, inform user and switch to manual input mode

3. **Question approach by mode**:
   - **Recommendation mode**: Present extracted content as options and include "Manual input" option
   - **Manual input mode**: Request direct user input with examples

4. **Use AskUserQuestion**: All information collection proceeds step-by-step through the AskUserQuestion tool

5. **Use Write tool**: Use Write tool when creating SKILL.md file

6. **Use Bash tool**: Use `mkdir -p` command when creating directories

7. **Validation required**: Always validate content after file creation

8. **Friendly feedback**: Guide user on progress at each step

9. **Share analysis results**: When Phase 0 completes, summarize and present discovered content to user

---

**Now starting skill creation. First, we'll analyze the current session's conversation, generate recommendations, then complete the skill through dialogue with the user.**
