# Progressive Disclosure Levels

Progressive Disclosure is a design principle that presents information in stages, loading only what's needed when it's needed. This keeps context windows efficient while maintaining comprehensive coverage.

## Core Principle

**Load information in three levels:**
1. **Metadata** (name + description) - Always in context (~100 words)
2. **SKILL.md body** - When skill triggers (~1,500-2,000 words ideal)
3. **Supporting resources** - As Claude needs them (unlimited)

## Level Selection by Word Count

| Level | Word Count | Files | When to Use |
|-------|------------|-------|-------------|
| 1 | < 400 | SKILL.md only | Simple, single-purpose skills |
| 2 | 400-1,500 | SKILL.md + examples/ | Standard skills with code examples |
| 3 | 1,500-3,500 | + REFERENCE.md | Complex skills needing detailed docs |
| 4 | > 3,500 | + EXAMPLES.md + FORMS.md | Comprehensive toolkits |

---

## Level 1: Single File (< 400 words)

**When to use**: Simple, straightforward skills without complex examples or extensive documentation.

### File Structure
```bash
{location_path}/{skill-name}/
└── SKILL.md          # All content included
```

### Creation Command
```bash
mkdir -p {location_path}/{skill-name}
# Write SKILL.md with all content
```

### Content Organization
**SKILL.md contains**:
- Frontmatter (name, description)
- Overview
- When to Use
- Core workflow/concepts (brief)
- Quick examples (inline)
- Best practices (brief list)
- Common pitfalls (brief list)

### Example Use Cases
- Simple file format converters
- Quick validation tools
- Basic automation workflows
- Single-purpose utilities

### Quality Standards
- ✅ Complete in < 400 words
- ✅ Immediately actionable
- ✅ No external references needed
- ❌ No complex examples or extensive docs

---

## Level 2: With Examples Directory (400-1,500 words)

**When to use**: Standard skills that need working code examples but don't require extensive documentation.

### File Structure
```bash
{location_path}/{skill-name}/
├── SKILL.md          # Overview + core guide
└── examples/         # Detailed examples directory
    ├── example1.sh
    ├── example2.py
    └── config-template.json
```

### Creation Commands
```bash
mkdir -p {location_path}/{skill-name}/examples
# Write SKILL.md with core content
# Write example files in examples/
```

### Content Organization

**SKILL.md contains** (~800-1,200 words):
- Frontmatter
- Overview
- When to Use
- Core Concepts (moderate detail)
- How to Use (workflow)
- Pattern overview (reference examples/)
- Best Practices
- Common Pitfalls
- References to examples/

**examples/ contains**:
- Complete, working code examples
- Configuration templates
- Sample input/output files
- Each example should be runnable

### Example Use Cases
- API integration workflows
- Testing strategies with examples
- Deployment procedures with scripts
- Code transformation patterns

### Referencing Examples in SKILL.md

```markdown
## Examples

Working examples are provided in the `examples/` directory:

### Basic Usage
See **[example-basic.sh](./examples/example-basic.sh)** for a simple implementation.

### Advanced Pattern
See **[example-advanced.py](./examples/example-advanced.py)** for error handling and edge cases.

### Configuration
Use **[config-template.json](./examples/config-template.json)** as a starting point.
```

### Quality Standards
- ✅ SKILL.md is focused and < 1,500 words
- ✅ Examples are complete and working
- ✅ Examples are referenced in SKILL.md
- ✅ Each example has clear purpose
- ❌ Don't duplicate example code in SKILL.md

---

## Level 3: With Reference Documentation (1,500-3,500 words)

**When to use**: Complex skills requiring detailed technical documentation, API references, or extensive pattern catalogs.

### File Structure
```bash
{location_path}/{skill-name}/
├── SKILL.md          # Overview + When to Use + simple examples
├── REFERENCE.md      # Detailed API documentation
└── examples/         # Examples and templates
    ├── example1.sh
    └── example2.py
```

### Creation Commands
```bash
mkdir -p {location_path}/{skill-name}/examples
# Write SKILL.md (overview, ~800-1,000 words)
# Write REFERENCE.md (detailed content, ~1,000-2,000 words)
# Write example files
```

### Content Organization

**SKILL.md contains** (~800-1,000 words):
- Frontmatter
- Overview
- When to Use
- Core Concepts (high-level)
- Quick Start or Quick Reference table
- How to Use (workflow overview)
- Simple inline examples
- References to REFERENCE.md and examples/

**REFERENCE.md contains** (~1,000-2,500 words):
- Detailed API documentation
- Comprehensive pattern catalog
- Advanced techniques
- Edge cases and troubleshooting
- In-depth explanations
- Complex examples with detailed breakdowns

**examples/ contains**:
- Basic usage examples
- Advanced pattern demonstrations
- Configuration templates
- Real-world use case examples

### Example Use Cases
- Database query toolkits
- Cloud platform SDKs
- Complex framework integration
- System administration tools

### Referencing in SKILL.md

```markdown
## Quick Reference

| Task | Method | See |
|------|--------|-----|
| Basic operation | `method1()` | REFERENCE.md §2.1 |
| Advanced pattern | `method2()` | REFERENCE.md §3.2 |
| Configuration | See examples/ | config-template.json |

## Core Concepts

[Brief overview of key concepts]

For detailed API documentation and advanced patterns, see [@{skill-name}/REFERENCE.md](./REFERENCE.md).

## Additional Resources

### Internal Documentation
- **[@{skill-name}/REFERENCE.md](./REFERENCE.md)** - Complete API reference and advanced patterns

### Examples
- **[example-basic.sh](./examples/example-basic.sh)** - Basic usage
- **[example-advanced.py](./examples/example-advanced.py)** - Advanced patterns
```

### REFERENCE.md Structure

```markdown
# {Skill Name} - Complete Reference

## Table of Contents
1. [API Overview](#api-overview)
2. [Component Reference](#component-reference)
3. [Advanced Patterns](#advanced-patterns)
4. [Troubleshooting](#troubleshooting)

## API Overview
[Comprehensive API documentation]

## Component Reference
### Component 1
[Detailed documentation]

### Component 2
[Detailed documentation]

## Advanced Patterns
### Pattern 1: [Name]
[In-depth explanation with examples]

### Pattern 2: [Name]
[In-depth explanation with examples]

## Troubleshooting
[Common issues and solutions]
```

### Quality Standards
- ✅ SKILL.md is concise overview (< 1,200 words)
- ✅ REFERENCE.md has detailed content
- ✅ No content duplication between files
- ✅ Clear references from SKILL.md to REFERENCE.md
- ✅ Examples demonstrate key patterns
- ❌ Don't put all details in SKILL.md

---

## Level 4: Comprehensive Documentation (> 3,500 words)

**When to use**: Extensive toolkits, complete frameworks, or skills requiring multiple reference documents and comprehensive templates.

### File Structure
```bash
{location_path}/{skill-name}/
├── SKILL.md          # Overview (with reference links)
├── REFERENCE.md      # API/technical documentation
├── EXAMPLES.md       # Detailed examples with explanations
├── FORMS.md          # Templates/checklists
└── examples/         # Working code files
    ├── basic/
    │   ├── example1.sh
    │   └── example2.py
    └── advanced/
        ├── example3.py
        └── example4.js
```

### Creation Commands
```bash
mkdir -p {location_path}/{skill-name}/examples/{basic,advanced}
# Write SKILL.md (overview only, ~600-800 words)
# Write REFERENCE.md (technical docs, ~1,500-2,000 words)
# Write EXAMPLES.md (detailed examples, ~1,000-1,500 words)
# Write FORMS.md (templates, ~500-1,000 words)
# Write example files in examples/
```

### Content Organization

**SKILL.md contains** (~600-800 words):
- Frontmatter
- Overview (what and why)
- When to Use
- Quick Reference table
- Core Concepts (brief)
- Getting Started (3-5 steps)
- References to all supporting files

**REFERENCE.md contains** (~1,500-2,000 words):
- Complete API documentation
- Component architecture
- Configuration options
- Advanced patterns
- Technical specifications
- Performance considerations

**EXAMPLES.MD contains** (~1,000-1,500 words):
- Comprehensive example walkthroughs
- Use case demonstrations
- Pattern implementations with explanations
- Real-world scenarios
- Step-by-step breakdowns

**FORMS.md contains** (~500-1,000 words):
- Checklists
- Templates
- Boilerplate code
- Configuration templates
- Decision trees

**examples/ contains**:
- Organized by complexity (basic/, advanced/)
- Complete, runnable examples
- Each example referenced in EXAMPLES.md

### Example Use Cases
- Complete framework toolkits
- Enterprise-level system integrations
- Comprehensive methodology guides
- Multi-component platforms

### Referencing in SKILL.md

```markdown
# {Skill Name}

## Overview
[Brief introduction]

## When to Use This Skill
[Usage scenarios]

## Quick Reference

| Need | Document | Section |
|------|----------|---------|
| API docs | REFERENCE.md | API Reference |
| Examples | EXAMPLES.md | Use Cases |
| Templates | FORMS.md | Templates |

## Getting Started

1. **Understand the concepts**: Read Core Concepts below
2. **Review the API**: See [@{skill-name}/REFERENCE.md](./REFERENCE.md)
3. **Study examples**: See [@{skill-name}/EXAMPLES.md](./EXAMPLES.md)
4. **Use templates**: See [@{skill-name}/FORMS.md](./FORMS.md)
5. **Start building**: Adapt examples to your needs

## Core Concepts

[Brief overview only]

For complete technical documentation, see [@{skill-name}/REFERENCE.md](./REFERENCE.md).

## Additional Resources

### Complete Documentation
- **[@{skill-name}/REFERENCE.md](./REFERENCE.md)** - Complete API and technical reference
- **[@{skill-name}/EXAMPLES.md](./EXAMPLES.md)** - Comprehensive examples and use cases
- **[@{skill-name}/FORMS.md](./FORMS.md)** - Templates, checklists, and boilerplate

### Working Examples
- **[basic/](./examples/basic/)** - Basic usage examples
- **[advanced/](./examples/advanced/)** - Advanced patterns and techniques
```

### Document-Specific Content

**REFERENCE.md structure**:
```markdown
# Complete Technical Reference

## API Overview
[Comprehensive API documentation]

## Component Architecture
[System design and structure]

## Configuration Reference
[All configuration options]

## Advanced Patterns
[Complex usage patterns]

## Performance Optimization
[Performance guidelines]

## Security Considerations
[Security best practices]
```

**EXAMPLES.md structure**:
```markdown
# Comprehensive Examples

## Basic Examples
### Example 1: [Use Case]
[Complete walkthrough]

### Example 2: [Use Case]
[Complete walkthrough]

## Advanced Examples
### Example 3: [Complex Use Case]
[Detailed walkthrough]

### Example 4: [Integration Scenario]
[Complete implementation guide]

## Real-World Scenarios
### Scenario 1: [Production Use Case]
[End-to-end example]
```

**FORMS.md structure**:
```markdown
# Templates and Checklists

## Checklists
### Pre-Implementation Checklist
- [ ] Item 1
- [ ] Item 2

### Quality Checklist
- [ ] Item 1
- [ ] Item 2

## Configuration Templates
### Template 1: [Purpose]
```yaml
[Template content]
```

### Template 2: [Purpose]
```json
[Template content]
```

## Boilerplate Code
### Boilerplate 1: [Purpose]
```language
[Boilerplate code]
```
```

### Quality Standards
- ✅ SKILL.md is concise entry point (< 800 words)
- ✅ Each reference document has clear purpose
- ✅ No content duplication across files
- ✅ Comprehensive cross-references
- ✅ Examples/ is well-organized
- ✅ All documents referenced in SKILL.md
- ❌ Don't create documents without clear need

---

## Decision Tree: Which Level to Use?

```
What is the total word count (including examples and documentation)?

├─ < 400 words
│  └─ Level 1: SKILL.md only
│
├─ 400-1,500 words
│  └─ Level 2: SKILL.md + examples/
│
├─ 1,500-3,500 words
│  └─ Level 3: SKILL.md + REFERENCE.md + examples/
│
└─ > 3,500 words
   └─ Level 4: SKILL.md + REFERENCE.md + EXAMPLES.md + FORMS.md + examples/
```

## Content Distribution Guidelines

### What Goes in SKILL.md?
**Always include:**
- Overview and purpose
- When to Use section
- Core Concepts (at appropriate depth)
- Quick Reference (for Level 3+)
- Getting Started (for Level 4)
- References to supporting files

**Keep it lean:**
- Level 1: Everything (< 400 words)
- Level 2: Core content (< 1,500 words)
- Level 3: Overview + pointers (< 1,200 words)
- Level 4: Entry point + navigation (< 800 words)

### What Goes in REFERENCE.md? (Level 3+)
- Complete API documentation
- Technical specifications
- Advanced patterns and techniques
- Configuration reference
- Architecture details
- Performance optimization
- Security considerations

### What Goes in EXAMPLES.md? (Level 4)
- Comprehensive example walkthroughs
- Use case demonstrations
- Real-world scenarios
- Step-by-step breakdowns
- Pattern implementations with detailed explanations

### What Goes in FORMS.md? (Level 4)
- Checklists
- Configuration templates
- Boilerplate code
- Decision trees
- Standard forms
- Quick-start templates

### What Goes in examples/?
- Complete, runnable code
- Working examples
- Configuration files
- Sample data
- Each file should be self-contained when possible

---

## Best Practices

### Progressive Loading Strategy
1. **User triggers skill** → Loads SKILL.md (metadata + body)
2. **Claude needs details** → Loads REFERENCE.md
3. **Claude needs examples** → Loads EXAMPLES.md or examples/ files
4. **Claude needs templates** → Loads FORMS.md

### Cross-Reference Format
Use consistent reference format:

```markdown
See [@{skill-name}/REFERENCE.md](./REFERENCE.md) for detailed API documentation.
See [@{skill-name}/EXAMPLES.md](./EXAMPLES.md) for comprehensive examples.
See **[example.sh](./examples/example.sh)** for a working implementation.
```

### Avoid Content Duplication
- Don't repeat detailed content in multiple files
- SKILL.md gives overview, references detailed docs
- REFERENCE.md has technical depth
- EXAMPLES.md has example depth
- examples/ has working code

### Navigation Clarity
Make it easy for Claude to find information:
- Clear section headings
- Table of contents in long documents
- Descriptive file names
- Explicit cross-references

### Testing Progressive Disclosure
1. Can task be completed with just SKILL.md? (Level 1-2)
2. Is SKILL.md a good entry point? (All levels)
3. Are references easy to find? (Level 3-4)
4. Is information discoverable without loading everything? (All levels)

---

## Common Mistakes

### Mistake 1: Everything in SKILL.md
❌ **Bad**: 5,000-word SKILL.md with all content
✅ **Good**: 800-word SKILL.md + REFERENCE.md + EXAMPLES.md

### Mistake 2: No Cross-References
❌ **Bad**: REFERENCE.md exists but not mentioned in SKILL.md
✅ **Good**: Clear references with section pointers

### Mistake 3: Duplicated Content
❌ **Bad**: Same examples in both SKILL.md and EXAMPLES.md
✅ **Good**: Overview in SKILL.md, details in EXAMPLES.md

### Mistake 4: Unclear File Purposes
❌ **Bad**: Three files with overlapping content
✅ **Good**: Each file has distinct, clear purpose

### Mistake 5: Premature Level 4
❌ **Bad**: Creating FORMS.md with only 2 templates
✅ **Good**: Use Level 3 until truly need comprehensive templates

---

## Evolution Path

Skills naturally evolve:

1. **Start Level 1**: Single file, simple skill
2. **Grow to Level 2**: Add examples/ when patterns emerge
3. **Expand to Level 3**: Add REFERENCE.md when API grows
4. **Mature to Level 4**: Add EXAMPLES.md + FORMS.md for comprehensive coverage

**Don't skip levels unnecessarily**. Start simple, evolve based on actual needs.

---

## Quality Checklist by Level

### Level 1
- ✅ SKILL.md < 400 words
- ✅ Complete and self-contained
- ✅ No external references needed

### Level 2
- ✅ SKILL.md < 1,500 words
- ✅ examples/ directory created
- ✅ Examples are working and referenced
- ✅ No detailed docs in SKILL.md

### Level 3
- ✅ SKILL.md < 1,200 words
- ✅ REFERENCE.md created with detailed content
- ✅ Clear references from SKILL.md
- ✅ examples/ organized
- ✅ No content duplication

### Level 4
- ✅ SKILL.md < 800 words (entry point only)
- ✅ All reference docs created (REFERENCE, EXAMPLES, FORMS)
- ✅ Each doc has clear, distinct purpose
- ✅ Comprehensive cross-references
- ✅ examples/ well-organized (basic/, advanced/)
- ✅ Navigation is clear and logical

---

## Summary

Progressive Disclosure ensures:
- **Efficiency**: Load only what's needed
- **Clarity**: Each file has clear purpose
- **Scalability**: Skills can grow without bloating
- **Discoverability**: Information is easy to find
- **Maintainability**: Content is organized logically

Choose the right level based on total content volume, and structure files to make information progressively discoverable as Claude needs it.
