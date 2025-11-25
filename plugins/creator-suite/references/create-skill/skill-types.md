# Skill Types - Detailed Guide

## Overview

This document provides detailed guidance for each of the 4 skill types. Each type has specific characteristics, word count targets, and optimal use cases.

## Type 1: Quick Workflow (200-400 words)

### Characteristics
- **Purpose**: Single-purpose sequential process
- **Target word count**: 200-400 words
- **Structure**: Minimal, action-focused
- **Best for**: Simple tools, quick automation, straightforward workflows

### When to Use
- Simple, repeatable processes
- Tools with clear, linear workflows
- Tasks requiring immediate execution without complex decision-making
- Automation of routine operations

### Template Structure
```markdown
---
name: quick-tool-name
description: "Brief description of what and when"
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

### Examples
- File format converters
- Simple data transformations
- Quick validation tools
- Basic automation scripts

---

## Type 2: Comprehensive Guide (600-1,500 words)

### Characteristics
- **Purpose**: Multi-functional with examples
- **Target word count**: 600-1,500 words
- **Structure**: Balanced overview, patterns, best practices
- **Best for**: Standard development workflows, common tasks

### When to Use
- Multi-step workflows with multiple paths
- Standard development tasks requiring context
- Skills needing pattern examples
- Tasks with best practices and pitfalls

### Template Structure
```markdown
---
name: comprehensive-skill-name
description: "Detailed description of purpose and context"
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

### Examples
- API integration workflows
- Testing strategies
- Code review processes
- Deployment procedures

---

## Type 3: Technical Reference (1,500-5,000+ words)

### Characteristics
- **Purpose**: In-depth API documentation
- **Target word count**: 1,500-5,000+ words
- **Structure**: Comprehensive reference with quick lookup
- **Best for**: Complex toolkits, libraries, frameworks

### When to Use
- Complex APIs requiring detailed documentation
- Toolkits with multiple components
- Libraries with extensive functionality
- Technical domains requiring reference material

### Template Structure
```markdown
---
name: technical-toolkit-name
description: "Comprehensive toolkit for specific technical domain"
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

### Examples
- Database query libraries
- Cloud platform SDKs
- Complex framework documentation
- System administration toolkits

### Progressive Disclosure Strategy
For Technical Reference skills:
- SKILL.md: Overview + Quick Reference + Core Concepts
- REFERENCE.md: Detailed API documentation
- EXAMPLES.md: Comprehensive examples
- FORMS.md: Templates and checklists (if needed)

---

## Type 4: Philosophy-Driven (1,000-3,000 words)

### Characteristics
- **Purpose**: Conceptual framework + implementation
- **Target word count**: 1,000-3,000 words
- **Structure**: Philosophy/approach, concepts, implementation
- **Best for**: Creative processes, judgment-based tasks

### When to Use
- Tasks requiring conceptual understanding
- Creative workflows
- Judgment-based decision-making
- Processes emphasizing approach over mechanics

### Template Structure
```markdown
---
name: creative-approach-name
description: "Conceptual framework for creative/judgment tasks"
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

### Examples
- Design thinking processes
- Code architecture approaches
- Problem-solving frameworks
- User experience methodologies

---

## Type Selection Decision Tree

```
Start here: What is the primary purpose of this skill?

├─ Single, straightforward process?
│  └─ YES → Quick Workflow (200-400 words)
│
├─ Standard workflow with patterns?
│  └─ YES → Comprehensive Guide (600-1,500 words)
│
├─ Complex technical documentation?
│  └─ YES → Technical Reference (1,500-5,000+ words)
│
└─ Conceptual framework or creative process?
   └─ YES → Philosophy-Driven (1,000-3,000 words)
```

## Hybrid Approaches

Sometimes a skill may not fit perfectly into one type. Consider these hybrid strategies:

### Quick Workflow + Technical Reference
- Start with Quick Workflow template
- Add REFERENCE.md for detailed technical info
- Keep main SKILL.md action-focused

### Comprehensive Guide + Philosophy-Driven
- Use Comprehensive Guide structure
- Add "Philosophy" section at the beginning
- Balance concepts with practical patterns

### Technical Reference + Philosophy-Driven
- Use Technical Reference structure
- Include philosophy section in Core Concepts
- Emphasize both theory and practice

## Word Count Guidelines

### Target Ranges
- **Quick Workflow**: 200-400 words
- **Comprehensive Guide**: 600-1,500 words
- **Technical Reference**: 1,500-5,000+ words
- **Philosophy-Driven**: 1,000-3,000 words

### When to Split Content
If word count exceeds these thresholds:
- **400+ words**: Consider adding examples/ directory
- **1,500+ words**: Add REFERENCE.md for detailed content
- **3,500+ words**: Use full Progressive Disclosure (REFERENCE.md, EXAMPLES.md, FORMS.md)

### Progressive Disclosure by Word Count
See `progressive-disclosure.md` for detailed file structure at each level.

## Quality Standards by Type

### Quick Workflow
- ✅ Clear, actionable steps
- ✅ Minimal cognitive load
- ✅ Immediate usability
- ❌ No complex decision trees

### Comprehensive Guide
- ✅ Balanced overview and detail
- ✅ Multiple examples
- ✅ Clear best practices
- ✅ Common pitfalls addressed

### Technical Reference
- ✅ Comprehensive API coverage
- ✅ Quick reference table
- ✅ Multiple complete examples
- ✅ Progressive disclosure applied

### Philosophy-Driven
- ✅ Clear conceptual framework
- ✅ Theory-to-practice connection
- ✅ Judgment guidance
- ✅ Flexible implementation patterns

## Common Mistakes by Type

### Quick Workflow Mistakes
- ❌ Too much explanation
- ❌ Complex decision trees
- ❌ Excessive background theory
- ✅ Keep it simple and direct

### Comprehensive Guide Mistakes
- ❌ Too brief (< 600 words)
- ❌ Missing examples
- ❌ No best practices
- ✅ Balance all sections

### Technical Reference Mistakes
- ❌ Everything in SKILL.md (no Progressive Disclosure)
- ❌ Missing quick reference
- ❌ Incomplete API coverage
- ✅ Use references/ for details

### Philosophy-Driven Mistakes
- ❌ Too theoretical (no implementation)
- ❌ Vague concepts
- ❌ Missing practical guidance
- ✅ Connect philosophy to practice

## Recommendations

### For Most Skills
Start with **Comprehensive Guide** (Type 2). It provides:
- Good balance of overview and detail
- Room for examples and patterns
- Best practices and pitfalls
- Flexible structure

### When Specialized Types Are Better
- **Quick Workflow**: Simple automation, clear linear process
- **Technical Reference**: Complex APIs, extensive toolkits
- **Philosophy-Driven**: Creative work, judgment-heavy tasks

### Evolution Path
Skills often evolve:
1. Start as Quick Workflow (prototype)
2. Expand to Comprehensive Guide (add patterns)
3. Grow into Technical Reference (full documentation)
4. Or shift to Philosophy-Driven (conceptual focus)

Allow skills to evolve naturally based on usage patterns.
