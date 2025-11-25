---
name: create-skill
description: "This skill should be used when the user asks to \"create a skill\", \"add a skill\", \"write a skill\", \"make a new skill\", \"generate skill documentation\", or \"build a skill\". Interactively creates standardized skill documentation through conversational interaction, supporting project-specific, global, and plugin skills with conversation analysis and progressive disclosure."
---

# Create Skill Command

This command interactively creates standardized skill documentation through conversational interaction with the user.

## Purpose

Create high-quality Claude Code skills through guided conversation. Supports:
- **Conversation Analysis**: Auto-generate skill metadata from session content
- **Interactive Collection**: Step-by-step information gathering
- **Progressive Disclosure**: Automatic file structure optimization
- **Multiple Scopes**: Project-specific, global, or plugin skills

**💡 Evaluation**: To evaluate existing skills, use `/evaluate-skill`.

---

## Core Workflow Overview

The skill creation process follows 4 phases (with optional Phase 0):

```
Phase 0: Conversation Analysis (conditional)
    ↓
Phase 1: Type & Basic Info Collection (5 questions)
    ↓
Phase 2: Content Collection (6 questions)
    ↓
Phase 3: File Generation (automated)
    ↓
Phase 4: Validation & Completion
```

---

## Extended Thinking

**Role**: Skill creation expert providing guided, interactive skill development.

**📋 Phase Checklist**:
- [ ] Phase 0: Conversation analysis (conditional)
- [ ] Phase 1: Type & basic info (type, name, location, description, scenarios)
- [ ] Phase 2: Content collection (concepts, examples, practices, pitfalls, formatting, tools)
- [ ] Phase 3: File generation (Progressive Disclosure + templates)
- [ ] Phase 4: Validation & completion

**🔄 Progress Tracking**:
```
✅ Phase X complete → 🔄 Starting Phase Y: [description]
📊 Overall progress: X/4 Phases complete (YY%)
```

**Within phases** (Phase 1-2):
```
📊 Phase progress: X/Y questions complete
```

**Core Actions**:
1. Analyze conversation → generate recommendations
2. Collect information → type-specific questions
3. Generate files → Progressive Disclosure + templates
4. Validate → quality check + usage guidance

## Execution Steps

**📖 Detailed Instructions**: For complete phase details, see `@creator-suite/references/create-skill/phase-details.md`

### Phase 0: Conversation Analysis (Conditional)

**Skip if**:
- Conversation < 5 messages
- No technical content
- Only simple greetings

**Execute if sufficient conversation**:
1. Extract main topics, keywords, code examples, patterns
2. Generate 2-3 skill name suggestions (kebab-case, gerund form)
3. Auto-generate description, scenarios, concepts, best practices, pitfalls
4. Present analysis results:
   ```
   📊 Conversation analysis complete!
   🔍 Discovered: topics, examples, patterns, concepts
   💡 Proceeding with recommendations
   ```

**If insufficient**:
```
ℹ️ Insufficient conversation content
💡 Proceeding in manual input mode
```

**Result**: Set mode to "recommendation" or "manual" for Phase 1-2.

📖 **See**: `phase-details.md` → Phase 0 for complete conversation analysis procedures.

### Phase 1: Type & Basic Information

**Collect 5 pieces of information via AskUserQuestion**:

1. **Skill Type** (use AskUserQuestion):
   - Quick Workflow (200-400 words): Simple tools
   - Comprehensive Guide (600-1,500 words): Standard workflows [Default]
   - Technical Reference (1,500-5,000+ words): Complex toolkits
   - Philosophy-Driven (1,000-3,000 words): Conceptual frameworks

2. **Skill Name**:
   - Format: kebab-case, ≤64 chars
   - Style: Gerund form (processing-pdfs, managing-containers)
   - Validate: `^[a-z0-9]+(-[a-z0-9]+)*$`

3. **File Location** (use AskUserQuestion):
   - **Project** (.claude/skills/) [Recommended, Default]
   - **Global** (~/.claude/skills/)
   - **Current Directory** (./{skill-name}/)

   Store path: `.claude/skills` or `~/.claude/skills` or `.`

4. **Description**:
   - Format: Third person, ≤1024 chars
   - Include: Specific trigger phrases ("create X", "build Y")
   - Example: "This skill should be used when the user asks to \"create hook\", \"add PreToolUse hook\"..."

5. **Usage Scenarios**:
   - Format: Multi-line list
   - Examples: "When designing new API", "During code review"

**Question Mode**:
- **Recommendation mode**: Present extracted content → Modify/Use/Custom
- **Manual mode**: Guidance → Example → Request input

📖 **See**:
- `phase-details.md` → Phase 1 for detailed question flows
- `skill-types.md` for complete type descriptions

### Phase 2: Content Collection

**Collect 6 pieces of content information**:

6. **Core Concepts**:
   - Format: Free-form text, multiple paragraphs
   - Include: Main concepts, principles, theories
   - Recommendation: Present auto-organized content

7. **Patterns/Examples** (optional):
   - Ask: Include examples? Yes/No
   - If yes, quality level:
     - Basic: Syntax only
     - Standard: Working code (recommended)
     - Production: Error handling + edge cases
   - Format: Pattern name + language + code block

8. **Best Practices**:
   - Format: List
   - Examples: "Use clear naming", "Consistent error handling"
   - Recommendation: Present collected practices

9. **Common Pitfalls**:
   - Format: List
   - Examples: "Avoid over-abstraction", "Consider performance"
   - Recommendation: Present identified pitfalls

10. **Formatting Style** (optional):
    - Minimal (default): Clean, simple
    - Enhanced: Emoji markers (🚀, 📋, ⚠️), Visual indicators (✅/❌)
    - Default: Minimal

11. **Allowed Tools** (optional):
    - **Default**: No restriction (all tools available)
    - **Restrict when**: Specific workflows needed, prevent side effects
    - **Tools**: Read, Write, Edit, Grep, Glob, Bash, WebSearch, WebFetch, Task, SlashCommand
    - **Recommendation mode**: Analyze tool usage → Suggest restriction
    - **Manual mode**: Ask if restriction needed → Multi-select tools

📖 **See**: `phase-details.md` → Phase 2 for detailed content collection procedures

### Phase 3: File Generation

**Automated file creation based on collected information.**

**Process**:
1. Calculate total word count
2. Determine Progressive Disclosure level (1-4)
3. Create directory structure with `mkdir -p`
4. Apply type-specific template
5. Generate files with Write tool

| Level | Word Count | Files | Structure |
|-------|------------|-------|-----------|
| 1 | < 400 | SKILL.md only | All content in one file |
| 2 | 400-1,500 | + examples/ | Separate example files |
| 3 | 1,500-3,500 | + REFERENCE.md | Detailed API docs separated |
| 4 | > 3,500 | + EXAMPLES.md + FORMS.md | Full documentation suite |

**Frontmatter** (all skills):
```yaml
---
name: {skill-name}
description: "{skill-description}"
allowed-tools: {tools-list}  # Only if restricted
---
```

**Required body sections**:
- `# {Skill Title}` (H1)
- `## When to Use This Skill`
- `## Core Concepts`

**Recommended sections** (type-specific):
- Quick Start / Overview / Philosophy (entry point)
- Patterns / Examples
- Best Practices
- Common Pitfalls
- Additional Resources (with references for Level 3+)

📖 **See**:
- `progressive-disclosure.md` for complete level specifications
- `templates.md` for all type-specific templates

### Phase 4: Validation & Completion

**Validation** (against `@shared/skill/validation-criteria.md`):

1. **Structure**: File existence, Progressive Disclosure level
2. **Frontmatter**: name, description format, allowed-tools
3. **Required sections**: When to Use, Core Concepts
4. **Content quality**: Code blocks, hierarchy, links

**Validation Report**:
```markdown
📊 Generation complete - Validation result

## Basic quality check
✅ File structure: {status}
✅ Frontmatter: {status}
✅ Required sections: {status}
⚠️ Improvable items: {if_any}

💡 For detailed evaluation: /evaluate-skill
```

**User Guidance**:
```
✅ Skill created!

📂 {location_path}/{skill-name}/SKILL.md
🎯 {scope_description}

Usage: @{skill-name}/SKILL.md
Evaluate: /evaluate-skill
```

**Scope**:
- Project: Available in this project only
- Global: Available in all projects
- Current: Move to .claude/skills/ or ~/.claude/skills/ to activate

## Error Handling

**Common Errors**:
- **Invalid skill name**: Re-ask with kebab-case format guidance
- **Skill exists**: Offer rename / overwrite / cancel
- **Missing required info**: Re-ask for missing fields

---

## Implementation Notes

**Critical Requirements**:
1. **Progress tracking**: Show phase transitions and overall progress
2. **Conversation analysis priority**: Phase 0 before Phase 1 when possible
3. **Question modes**: Recommendation (with extracted content) vs Manual (with examples)
4. **Tool usage**:
   - **AskUserQuestion**: All information collection
   - **Write**: All file creation
   - **Bash**: Directory creation (`mkdir -p`)
5. **Validation**: Always validate after generation
6. **User feedback**: Clear progress at each step

**Phase 0 Behavior**:
- Always attempt conversation analysis first
- Present discovered content to user
- Fall back to manual mode gracefully if insufficient

---

## Additional Resources

**Detailed Documentation**:
- **[@creator-suite/references/create-skill/phase-details.md](../references/create-skill/phase-details.md)** - Complete phase-by-phase procedures
- **[@creator-suite/references/create-skill/skill-types.md](../references/create-skill/skill-types.md)** - Detailed type descriptions and selection guide
- **[@creator-suite/references/create-skill/templates.md](../references/create-skill/templates.md)** - All template structures and examples
- **[@creator-suite/references/create-skill/progressive-disclosure.md](../references/create-skill/progressive-disclosure.md)** - Complete Progressive Disclosure specifications

**Validation**:
- **[@shared/skill/validation-criteria.md](../shared/skill/validation-criteria.md)** - Quality standards and validation procedures

---

**Ready to create skill. Starting with conversation analysis, then proceeding through guided phases.**
