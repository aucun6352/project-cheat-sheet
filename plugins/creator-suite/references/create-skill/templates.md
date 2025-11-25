# Skill Templates

This document contains all template structures for creating skills.

## Frontmatter Template

All skills require YAML frontmatter:

```yaml
---
name: {skill-name}                  # Required: kebab-case, max 64 characters
description: "{skill-description}"  # Required: max 1024 characters, "what+when"
allowed-tools: {tools-list}         # Optional: comma-separated tool names
---
```

### Frontmatter Notes
- `description` must be wrapped in quotes for proper YAML parsing
- `allowed-tools` only included if restricting tool access
- Example: `allowed-tools: Read, Grep, Glob, Bash`

---

## Required Body Sections

Every skill must include:

```markdown
# {Skill Title}                     # H1 heading, only one

## When to Use This Skill            # Required
- Usage scenario 1
- Usage scenario 2
- Usage scenario 3

## Core Concepts                     # Required
[Core principles and concepts]
```

---

## Recommended Body Sections

Choose based on skill type:

```markdown
## Quick Start / Overview / Philosophy
[Entry point - choose based on type]

## Patterns / Examples
[Code examples and patterns]

## Best Practices
- Practice 1
- Practice 2

## Common Pitfalls
- Pitfall 1
- Pitfall 2

## Additional Resources
- Resource links
- Reference file links
```

---

## Type 1: Quick Workflow Template (200-400 words)

```markdown
---
name: quick-tool-name
description: "This skill should be used when the user asks to \"action 1\", \"action 2\", \"action 3\". Brief description of tool purpose and when to use."
---

# Quick Tool Name

## Quick Start

Follow these steps to [accomplish goal]:

1. **Step 1**: [Action with brief explanation]
2. **Step 2**: [Action with brief explanation]
3. **Step 3**: [Action with brief explanation]
4. **Step 4**: [Action with brief explanation]
5. **Step 5**: [Action with brief explanation]

## When to Use This Skill

Use this skill when:
- Scenario 1 occurs
- Scenario 2 is needed
- Scenario 3 applies

## Detailed Workflow

### Preparation
[Preparation steps if needed]

### Execution
[Detailed step-by-step process]

### Verification
[How to verify success]

## Common Pitfalls

- **Pitfall 1**: [Description and how to avoid]
- **Pitfall 2**: [Description and how to avoid]
- **Pitfall 3**: [Description and how to avoid]

## Tips

- Tip 1 for efficiency
- Tip 2 for accuracy
- Tip 3 for best results
```

---

## Type 2: Comprehensive Guide Template (600-1,500 words)

```markdown
---
name: comprehensive-skill-name
description: "This skill should be used when the user asks to \"task 1\", \"task 2\", \"task 3\", mentions \"keyword 1\", \"keyword 2\". Detailed description of purpose and context."
---

# Comprehensive Skill Name

## Overview

[1-2 paragraph introduction explaining:
- What this skill does
- Why it's useful
- What problems it solves
- High-level approach]

## When to Use This Skill

This skill is appropriate for:
- **Use case 1**: [Scenario description]
- **Use case 2**: [Scenario description]
- **Use case 3**: [Scenario description]
- **Use case 4**: [Scenario description]

## Core Concepts

### Concept 1: [Name]
[Explanation of first core concept]

### Concept 2: [Name]
[Explanation of second core concept]

### Concept 3: [Name]
[Explanation of third core concept]

## How to Use

### Step 1: [Phase Name]
[Detailed explanation of first step]

### Step 2: [Phase Name]
[Detailed explanation of second step]

### Step 3: [Phase Name]
[Detailed explanation of third step]

## Patterns / Examples

### Pattern 1: [Use Case Name]

```language
# Code example demonstrating pattern 1
[working code example]
```

**Explanation**: [What this pattern does and when to use it]

### Pattern 2: [Use Case Name]

```language
# Code example demonstrating pattern 2
[working code example]
```

**Explanation**: [What this pattern does and when to use it]

## Best Practices

- **Practice 1**: [Description and rationale]
- **Practice 2**: [Description and rationale]
- **Practice 3**: [Description and rationale]
- **Practice 4**: [Description and rationale]

## Common Pitfalls

- **Pitfall 1**: [What to avoid and why]
  - Solution: [How to avoid this pitfall]

- **Pitfall 2**: [What to avoid and why]
  - Solution: [How to avoid this pitfall]

- **Pitfall 3**: [What to avoid and why]
  - Solution: [How to avoid this pitfall]

## Additional Resources

- [External resource 1]
- [External resource 2]
- Related skills: @skill-name/SKILL.md
```

---

## Type 3: Technical Reference Template (1,500-5,000+ words)

```markdown
---
name: technical-toolkit-name
description: "This skill should be used when the user asks to \"technical task 1\", \"API operation 2\", \"complex operation 3\", mentions \"technical keyword\". Comprehensive toolkit for specific technical domain."
---

# Technical Toolkit Name

## Overview

[2-3 paragraph introduction covering:
- Technology stack
- Primary purpose
- Key capabilities
- When to use this toolkit]

## When to Use This Skill

This toolkit is designed for:
- **Complex scenario 1**: [Detailed description]
- **Complex scenario 2**: [Detailed description]
- **Complex scenario 3**: [Detailed description]
- **Complex scenario 4**: [Detailed description]

## Quick Reference

| Task | Tool/Method | Example | Notes |
|------|-------------|---------|-------|
| Common task 1 | `method1()` | `example1` | Brief note |
| Common task 2 | `method2()` | `example2` | Brief note |
| Common task 3 | `method3()` | `example3` | Brief note |
| Common task 4 | `method4()` | `example4` | Brief note |

## Core Concepts

### Concept 1: [Fundamental Principle]
[Deep explanation of first concept with theory]

### Concept 2: [Key Mechanism]
[Deep explanation of second concept with theory]

### Concept 3: [Critical Understanding]
[Deep explanation of third concept with theory]

## API Overview

### Component 1: [Name]
- **Purpose**: [What it does]
- **Key methods**: method1, method2, method3
- **Common uses**: [Primary use cases]

### Component 2: [Name]
- **Purpose**: [What it does]
- **Key methods**: method1, method2, method3
- **Common uses**: [Primary use cases]

## Detailed API Reference

See [@{skill-name}/REFERENCE.md](./REFERENCE.md) for complete API documentation.

## Examples

### Example 1: [Common Use Case]

**Scenario**: [What this example demonstrates]

```language
# Complete working example
[substantial code example with comments]
```

**Explanation**: [Step-by-step breakdown of the example]

### Example 2: [Advanced Use Case]

**Scenario**: [What this example demonstrates]

```language
# Complete working example
[substantial code example with comments]
```

**Explanation**: [Step-by-step breakdown of the example]

### Example 3: [Edge Case]

**Scenario**: [What this example demonstrates]

```language
# Complete working example
[substantial code example with comments]
```

**Explanation**: [Step-by-step breakdown of the example]

## Best Practices

### Performance
- Practice 1 with rationale
- Practice 2 with rationale

### Security
- Practice 1 with rationale
- Practice 2 with rationale

### Maintainability
- Practice 1 with rationale
- Practice 2 with rationale

## Common Pitfalls

### Category 1: [Type of Issues]
- **Pitfall 1**: [Description]
  - **Solution**: [How to fix]

- **Pitfall 2**: [Description]
  - **Solution**: [How to fix]

### Category 2: [Type of Issues]
- **Pitfall 1**: [Description]
  - **Solution**: [How to fix]

## Additional Resources

### Internal References
- **[@{skill-name}/REFERENCE.md](./REFERENCE.md)** - Complete API documentation
- **[@{skill-name}/EXAMPLES.md](./EXAMPLES.md)** - More examples and patterns
- **[@{skill-name}/FORMS.md](./FORMS.md)** - Templates and checklists

### External Resources
- [Official documentation]
- [Community resources]
- [Related tools]
```

---

## Type 4: Philosophy-Driven Template (1,000-3,000 words)

```markdown
---
name: creative-approach-name
description: "This skill should be used when the user asks to \"creative task 1\", \"design approach 2\", \"conceptual work 3\", mentions \"philosophy keyword\". Conceptual framework for creative/judgment tasks."
---

# Creative Approach Name

## Philosophy / Approach

[2-3 paragraphs explaining:
- The conceptual framework
- The underlying philosophy
- Why this approach matters
- The mindset required]

### Core Principles

1. **Principle 1**: [Explanation]
2. **Principle 2**: [Explanation]
3. **Principle 3**: [Explanation]
4. **Principle 4**: [Explanation]

## When to Use This Skill

This approach is valuable for:
- **Creative scenario 1**: [When judgment and creativity are key]
- **Design challenge 2**: [When conceptual thinking is needed]
- **Problem-solving 3**: [When framework guidance helps]
- **Decision-making 4**: [When philosophy informs choices]

## Core Concepts

### Concept 1: [Theoretical Foundation]
[Deep explanation connecting theory to practice]

### Concept 2: [Key Framework Element]
[Deep explanation connecting theory to practice]

### Concept 3: [Critical Understanding]
[Deep explanation connecting theory to practice]

## Implementation Workflow

### Phase 1: [Concept Application]
[How to apply the conceptual framework in practice]

**Steps**:
1. [Action based on philosophy]
2. [Action based on philosophy]
3. [Action based on philosophy]

### Phase 2: [Execution]
[How to execute with the framework in mind]

**Steps**:
1. [Action with judgment]
2. [Action with creativity]
3. [Action with evaluation]

### Phase 3: [Validation]
[How to validate results against philosophy]

**Steps**:
1. [Evaluation criterion]
2. [Quality check]
3. [Refinement process]

## Patterns / Examples

### Example 1: [Concept Demonstration]

**Context**: [Situation requiring this approach]

**Approach**:
```
[Demonstration of philosophy in action]
[May include code, design, or process examples]
```

**Reasoning**: [Why this approach embodies the philosophy]

### Example 2: [Judgment Application]

**Context**: [Situation requiring judgment]

**Approach**:
```
[Demonstration of judgment-based decision]
```

**Reasoning**: [How philosophy guided this decision]

## Best Practices

### Conceptual Level
- **Practice 1**: [Philosophical guidance]
- **Practice 2**: [Mindset recommendation]

### Implementation Level
- **Practice 1**: [Practical application]
- **Practice 2**: [Execution guidance]

### Evaluation Level
- **Practice 1**: [Quality assessment]
- **Practice 2**: [Refinement approach]

## Common Pitfalls

### Conceptual Traps
- **Pitfall 1**: [Philosophical mistake]
  - **Solution**: [How to maintain true to philosophy]

- **Pitfall 2**: [Conceptual error]
  - **Solution**: [How to stay on track]

### Implementation Traps
- **Pitfall 1**: [Execution mistake]
  - **Solution**: [How to implement correctly]

- **Pitfall 2**: [Practical error]
  - **Solution**: [How to avoid]

## Additional Resources

### Philosophical Foundations
- [Related philosophy/theory]
- [Foundational concepts]

### Practical Applications
- [Case studies]
- [Real-world examples]

### Related Approaches
- [Similar frameworks]
- [Complementary methods]
```

---

## Example Quality Levels

When collecting examples in Phase 2, offer quality level selection:

### Basic Level
```language
# Syntax demonstration only
function example() {
  return "minimal example";
}
```

### Standard Level (Recommended)
```language
// Working code with actual values
function processUserData(user) {
  const validated = validateInput(user);
  const processed = transformData(validated);
  return processed;
}
```

### Production Level
```language
// Error handling + edge cases
async function processUserData(user) {
  try {
    // Validation
    if (!user || typeof user !== 'object') {
      throw new Error('Invalid user object');
    }

    // Processing
    const validated = await validateInput(user);
    const processed = transformData(validated);

    // Edge case handling
    if (processed.length === 0) {
      console.warn('No data to process');
      return null;
    }

    return processed;
  } catch (error) {
    console.error('Processing failed:', error);
    throw error;
  }
}
```

---

## Formatting Styles

### Emoji Markers (Optional)
Use sparingly for section separation:
- 🚀 Quick Start
- 📋 Core Concepts
- ⚠️ Common Pitfalls
- ✅ Best Practices
- 💡 Tips

### Visual Markers
For best practices vs pitfalls:
- ✅ Do this
- ❌ Don't do this

### Tables
For quick reference information:

| Column 1 | Column 2 | Column 3 |
|----------|----------|----------|
| Data     | Data     | Data     |

**Default**: Minimal emoji use, only when it genuinely improves clarity.

---

## Progressive Disclosure File References

When creating Level 3+ skills, reference supporting files:

```markdown
## Additional Resources

### Reference Files

For detailed information, consult:
- **[@{skill-name}/REFERENCE.md](./REFERENCE.md)** - Complete API documentation
- **[@{skill-name}/EXAMPLES.md](./EXAMPLES.md)** - Comprehensive examples
- **[@{skill-name}/FORMS.md](./FORMS.md)** - Templates and checklists

### Example Files

Working examples in `examples/`:
- **[example1.sh](./examples/example1.sh)** - Basic usage
- **[example2.py](./examples/example2.py)** - Advanced patterns
```

---

## Location-Specific File Paths

### Project Scope
```bash
.claude/skills/{skill-name}/
├── SKILL.md
├── REFERENCE.md
├── EXAMPLES.md
└── examples/
```

### Global Scope
```bash
~/.claude/skills/{skill-name}/
├── SKILL.md
├── REFERENCE.md
└── references/
```

### Current Directory (Plugin Development)
```bash
./{skill-name}/
├── SKILL.md
└── references/
```

---

## Template Usage Notes

1. **Choose the right type**: See skill-types.md for type selection guidance
2. **Customize sections**: Not all sections are required for all types
3. **Progressive disclosure**: For longer skills, split content into references/
4. **Imperative form**: Write all instructions in imperative/infinitive form
5. **Third-person description**: Frontmatter description uses third person
6. **Specific triggers**: Include exact phrases users would say

## Quality Checklist

Before finalizing:
- ✅ Frontmatter complete and properly formatted
- ✅ Description includes specific trigger phrases
- ✅ Required sections present (When to Use, Core Concepts)
- ✅ Appropriate length for skill type
- ✅ Progressive disclosure applied if > 1,500 words
- ✅ Examples are complete and working
- ✅ Best practices and pitfalls included
- ✅ Imperative form used throughout body
- ✅ References to supporting files included (if applicable)
