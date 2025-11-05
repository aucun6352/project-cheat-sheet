# SKILL.md Validation Criteria

This document defines the common validation criteria for evaluating the quality of SKILL.md files.
Used by both `/create-skill` and `/evaluate-skill` commands.

---

## 📊 Scoring System

**Total Score: 100 points**

| Validation Item | Points | Description |
|-----------------|--------|-------------|
| 1. Structure Validation | 20 points | File existence, Progressive Disclosure appropriateness |
| 2. Frontmatter Validation | 15 points | name, description, license format |
| 3. Required Section Validation | 20 points | H1, When to Use, Core Concepts, Best Practices |
| 4. Content Quality Validation | 25 points | Code blocks, section hierarchy, links, terminology consistency |
| 5. Skill Type Appropriateness | 10 points | Word count and type matching, recommended sections |
| 6. Consistency and Completeness | 10 points | Overall quality, typos, completeness |

---

## 📈 Grade Standards

| Grade | Score | Assessment | Action |
|-------|-------|------------|--------|
| **A** | 90-100 | Excellent | Maintain or fine-tune |
| **B** | 80-89 | Good | Minor improvements recommended |
| **C** | 70-79 | Fair | Improvements needed |
| **D** | 60-69 | Poor | Major improvements required |
| **F** | < 60 | Fail | Rewrite recommended |

---

## 📏 Word Count Calculation Method

**Calculation Target:**
- Excluding Frontmatter (section wrapped in ---)
- Including markdown body text only
- Including code block content
- Including comments

**Calculation Method:**
```
Total word count = (all words separated by spaces)
```

**Example:**
```markdown
---
name: example
---

# Title

This is a test. (4 words)

```python
def hello():  (2 words)
    pass      (1 word)
```

Total word count = 7
```

---

## ✅ Validation 1: Structure Validation (20 points)

### Items

**1.1 SKILL.md File Existence (5 points)**
- ✅ SKILL.md file exists
- ❌ File missing or different name

**1.2 Progressive Disclosure Level Appropriateness (15 points)**

Recommended file structure based on word count:

| Word Count | Level | Recommended Structure | Points |
|------------|-------|----------------------|--------|
| < 400 | 1 | Single `SKILL.md` file | 15 points |
| 400-1,500 | 2 | `SKILL.md` + `examples/` | 15 points |
| 1,500-3,500 | 3 | + `REFERENCE.md` | 15 points |
| > 3,500 | 4 | + `EXAMPLES.md`, `FORMS.md` | 15 points |

**Score Calculation:**
- Exactly matches recommended level: 15 points
- 1 level difference (e.g., Level 2 but Level 1): 10 points
- 2 or more levels difference: 5 points

---

## ✅ Validation 2: Frontmatter Validation (15 points)

### Items

**2.1 name Field (10 points)**

**Required Conditions:**
- Existence
- kebab-case format (lowercase + hyphens)
- 64 characters or less
- Regex: `^[a-z0-9]+(-[a-z0-9]+)*$`

**Score Calculation:**
- All conditions met: 10 points
- Exists but format error: 5 points
- Does not exist: 0 points

**Examples:**
```yaml
# ✅ Correct examples
name: java-design-principles
name: quick-start-guide
name: api-reference-v2

# ❌ Incorrect examples
name: Java Design Principles  # Spaces, uppercase
name: api_reference  # Underscore
name: guide  # Too short (generally 2+ words recommended)
```

**2.2 description Field (5 points)**

**Required Conditions:**
- Existence
- 1024 characters or less
- Third-person expression recommended
- "What+When" specification recommended

**Score Calculation:**
- All conditions met: 5 points
- Exists but exceeds 1024 characters: 3 points
- Does not exist: 0 points

**Examples:**
```yaml
# ✅ Correct example
description: Comprehensive guide providing essential design principles and best practices for Java development

# ✅ Acceptable example
description: Provides essential design principles for Java development

# ⚠️ Needs improvement
description: Java principles  # Too short, missing "when"
```

**2.3 license Field (Optional, bonus points)**

- Present: +2 points (bonus)
- Absent: 0 points (no deduction)
- Recommended format: "Complete terms in LICENSE.txt"

---

## ✅ Validation 3: Required Section Validation (20 points)

### Items

**3.1 H1 Heading (5 points)**
- ✅ Exactly 1 exists: 5 points
- ⚠️ 0 or 2 or more: 0 points

**3.2 When to Use This Skill Section (5 points)**
- ✅ Section exists + has content: 5 points
- ⚠️ Section only without content: 2 points
- ❌ Section missing: 0 points

**3.3 Core Concepts Section (5 points)**
- ✅ Section exists + has content: 5 points
- ⚠️ Section only without content: 2 points
- ❌ Section missing: 0 points

**3.4 Best Practices or Common Pitfalls Section (5 points)**
- ✅ At least one exists + has content: 5 points
- ⚠️ Section only without content: 2 points
- ❌ Both missing: 0 points

---

## ✅ Validation 4: Content Quality Validation (25 points)

### Items

**4.1 Code Block Language Specification (10 points)**

**Inspection Method:**
- Find all ` ``` ` code blocks
- Check for language tag presence (e.g., ` ```python `, ` ```java `)

**Score Calculation:**
```
Specification rate = (code blocks with language specified) / (total code blocks) × 100%

Score = specification rate / 10
```

**Examples:**
- 10 out of 10 specified: 100% → 10 points
- 8 out of 10 specified: 80% → 8 points
- 5 out of 10 specified: 50% → 5 points

**4.2 Section Hierarchy Structure (5 points)**

**Rules:**
- H1 (# ) : Only 1
- H2 (## ) : After H1
- H3 (### ) : After H2 (should not come directly after H1)

**Score Calculation:**
- Perfect hierarchy: 5 points
- 1-2 violations: 3 points
- 3 or more violations: 0 points

**4.3 Link Relative Path Usage (5 points)**

**Inspection:**
- Internal file links use relative paths (./REFERENCE.md, ./examples/)
- External links can use absolute paths

**Score:**
- All internal links use relative paths: 5 points
- Some absolute paths used: 3 points
- Mostly absolute paths: 0 points

**4.4 Terminology Consistency (5 points)**

**Inspection Items:**
- MCP format: `ServerName:tool_name` (e.g., `Serena:find_symbol`)
- Time-independent expressions (use "latest", "current" instead of "2024")
- Consistent terminology use (e.g., unify "user" vs "users")

**Score:**
- Maintains consistency: 5 points
- Minor inconsistencies: 3 points
- Severe inconsistencies: 0 points

---

## ✅ Validation 5: Skill Type Appropriateness (10 points)

### Word Count Standards by Skill Type

| Type | Word Count Range | Main Sections |
|------|------------------|---------------|
| **Quick Workflow** | 200-400 | Quick Start, Detailed Workflow |
| **Comprehensive Guide** | 600-1,500 | Overview, How to Use, Patterns, Examples |
| **Technical Reference** | 1,500-5,000+ | Quick Reference, API Reference, Examples |
| **Philosophy-Driven** | 1,000-3,000 | Philosophy/Approach, Implementation Workflow |

### Score Calculation

**5.1 Word Count Appropriateness (5 points)**
- Within recommended range: 5 points
- Deviates within 20%: 3 points
- Deviates more than 20%: 0 points

**5.2 Recommended Sections Included (5 points)**
- All type-specific recommended sections included: 5 points
- 50% or more included: 3 points
- Less than 50%: 0 points

---

## ✅ Validation 6: Consistency and Completeness (10 points)

### Items

**6.1 Typos and Grammar (5 points)**
- 0 severe typos: 5 points
- 1-3 typos: 3 points
- 4 or more: 0 points

**6.2 Completeness (5 points)**
- All sections complete: 5 points
- "TODO", "WIP" or other incomplete markers present: 2 points
- Multiple empty sections: 0 points

---

## 🔄 Validation Procedure

### Step 1: Read File
```
Read SKILL.md file with Read tool
→ Fail immediately if file does not exist
```

### Step 2: Extract Basic Information
```
- Parse Frontmatter (YAML)
- Calculate word count
- Count H1/H2/H3
- Count code blocks
```

### Step 3: Execute 6 Validations
```
For each validation item:
1. Check conditions
2. Calculate score
3. Generate improvement suggestions
```

### Step 4: Calculate Total Score and Assign Grade
```
Total score = validation1 + validation2 + ... + validation6
Grade = A/B/C/D/F based on total score
```

### Step 5: Generate Report
```
- Executive Summary (3-line summary)
- Score and grade
- Strengths
- Areas needing improvement
- Detailed validation results
- Improvement suggestions
```

---

## 📝 Validation Result Example

```markdown
📊 SKILL.md Validation Result

## 📌 Executive Summary
**Grade: B (85/100)**
**Key Issue: 5 code blocks without language specification, duplicate H1 heading**
**Recommended Action: Can achieve 90+ points with auto-correction**

---

## Score Details

| Validation Item | Score | Max | Notes |
|-----------------|-------|-----|-------|
| 1. Structure Validation | 20 | 20 | ✅ |
| 2. Frontmatter Validation | 13 | 15 | ⚠️ description length exceeded |
| 3. Required Section Validation | 20 | 20 | ✅ |
| 4. Content Quality Validation | 17 | 25 | ⚠️ 50% code blocks without language |
| 5. Skill Type Appropriateness | 10 | 10 | ✅ |
| 6. Consistency and Completeness | 5 | 10 | ⚠️ 4 typos found |
| **Total** | **85** | **100** | **B (Good)** |

---

## ✅ Strengths
- Perfect Progressive Disclosure application (Level 3)
- All required sections included
- Word count appropriate for skill type

## ⚠️ Needs Improvement
1. **Code blocks without language specification** (Line 125, 234, 456, 567, 789)
2. **Frontmatter description length exceeded** (1050 chars / 1024 chars)
3. **4 typos** (Line 45, 123, 345, 678)

## 💡 Improvement Suggestions
- Add language to code blocks with auto-correction → +8 points
- Trim description → +2 points
- Expected score after improvements: **95 points (A)**
```

---

## 🛠️ How to Use This Document

### In /create-skill Command
```markdown
Phase 4: Validation and Completion

**Execute Validation:**
Follow validation procedure from @shared/skill/validation-criteria.md.

**Generate Report:**
Generate simple summary report based on validation results.
```

### In /evaluate-skill Command
```markdown
Phase 1: SKILL.md Evaluation

**Execute Validation:**
Follow validation procedure from @shared/skill/validation-criteria.md.

**Generate Detailed Report:**
Generate complete report including Executive Summary + detailed validation results + improvement suggestions.
```

---

This validation criteria ensures quality consistency of SKILL.md and enables objective evaluation.
