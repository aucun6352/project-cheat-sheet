# Command Validation Criteria

This document defines the common validation criteria for evaluating the quality of Claude Code command files.
Used by both `/create-command` and `/evaluate-command` commands.

---

## 📊 Scoring System

**Total Score: 100 points**

| Validation Item | Points | Description |
|-----------------|--------|-------------|
| 1. Structure Validation | 20 points | File existence, location, naming convention |
| 2. Frontmatter Validation | 15 points | name, description format |
| 3. Required Section Validation | 25 points | Triggers, Usage/Flow, Boundaries |
| 4. Content Quality Validation | 25 points | Code blocks, section hierarchy, example quality |
| 5. Command Type Appropriateness | 10 points | Word count and type matching, workflow complexity |
| 6. Consistency and Completeness | 5 points | Overall quality, typos, completeness |

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
- Including comments and examples

**Calculation Method:**
```
Total word count = (all words separated by spaces)
```

**Example:**
```markdown
---
name: example-command
---

# Command Title

This is a command. (4 words)

```bash
echo "hello"  (2 words)
```

Total word count = 6
```

---

## ✅ Validation 1: Structure Validation (20 points)

### Items

**1.1 File Existence and Location (10 points)**
- ✅ Exists in `.claude/commands/` or `plugins/*/commands/`: 10 points
- ⚠️ Exists in other location: 5 points
- ❌ File does not exist: 0 points

**1.2 Filename Convention (10 points)**

**Required Conditions:**
- kebab-case format (lowercase + hyphens)
- `.md` extension
- Regex: `^[a-z0-9]+(-[a-z0-9]+)*\.md$`
- 3-64 characters

**Score Calculation:**
- All conditions met: 10 points
- Format errors present: 5 points
- Severe format errors: 0 points

**Examples:**
```bash
# ✅ Correct examples
deploy-production.md
run-tests.md
create-feature-branch.md

# ❌ Incorrect examples
Deploy-Production.md  # Uppercase
run_tests.md          # Underscore
cmd.md                # Too short
```

---

## ✅ Validation 2: Frontmatter Validation (15 points)

### Items

**2.1 name Field (10 points)**

**Required Conditions:**
- Existence
- kebab-case format (lowercase + hyphens)
- 3-64 characters
- Regex: `^[a-z0-9]+(-[a-z0-9]+)*$`
- Should match filename (excluding extension)

**Score Calculation:**
- All conditions met: 10 points
- Exists but format error: 5 points
- Does not exist: 0 points

**Examples:**
```yaml
# ✅ Correct examples
name: deploy-production
name: run-integration-tests
name: analyze-code-quality

# ❌ Incorrect examples
name: Deploy Production  # Spaces, uppercase
name: run_tests          # Underscore
name: cmd                # Too short
```

**2.2 description Field (5 points)**

**Required Conditions:**
- Existence
- 10-500 characters
- Specify command purpose and main tasks
- Concise and clear expression

**Score Calculation:**
- All conditions met: 5 points
- Exists but too short or too long: 3 points
- Does not exist: 0 points

**Examples:**
```yaml
# ✅ Correct example
description: "Complete workflow to build and deploy application to production environment"

# ✅ Acceptable example
description: "Run integration tests and generate report"

# ⚠️ Needs improvement
description: "Deploy"  # Too short
```

---

## ✅ Validation 3: Required Section Validation (25 points)

### Items

**3.1 H1 Heading (5 points)**
- ✅ Exactly 1 exists: 5 points
- ⚠️ 0 or 2 or more: 0 points

**3.2 Triggers Section (7 points)**

**Requirements:**
- Section exists
- Specify situations/conditions when command should execute
- Minimum 1 trigger example

**Score Calculation:**
- Section exists + specific trigger description: 7 points
- Section only with insufficient content: 3 points
- Section missing: 0 points

**3.3 Usage or Behavioral Flow Section (8 points)**

**Requirements:**
- At least one of Usage or Behavioral Flow section exists
- Explain how to execute command or step-by-step behavior
- Execution flow is clear

**Score Calculation:**
- Section exists + clear flow explanation: 8 points
- Section only with insufficient content: 4 points
- Section missing: 0 points

**3.4 Boundaries Section (5 points)**

**Requirements:**
- Section exists
- Specify what command should not do
- Include constraints or precautions

**Score Calculation:**
- Section exists + specific boundary description: 5 points
- Section only with insufficient content: 2 points
- Section missing: 0 points

---

## ✅ Validation 4: Content Quality Validation (25 points)

### Items

**4.1 Code Block Language Specification (10 points)**

**Inspection Method:**
- Find all ` ``` ` code blocks
- Check for language tag presence (e.g., ` ```bash `, ` ```python `)

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

**4.3 Example Quality (10 points)**

**Inspection Items:**
- Include executable examples
- Cover various usage scenarios
- Sufficient explanation in examples

**Score Calculation:**
- 3 or more specific and practical examples: 10 points
- 1-2 examples or abstract: 5 points
- No examples: 0 points

---

## ✅ Validation 5: Command Type Appropriateness (10 points)

### Command Type Standards

| Type | Word Count Range | Complexity | Main Features |
|------|------------------|------------|---------------|
| **Simple Task** | 50-150 | Low | Single task, simple automation |
| **Workflow Pipeline** | 150-400 | Medium | Multi-step workflow, sequential execution |
| **Complex Orchestration** | 400-800 | High | Complex conditional branching, multiple tool coordination |

### Score Calculation

**5.1 Word Count Appropriateness (5 points)**
- Within recommended range for command type: 5 points
- Deviates within 20%: 3 points
- Deviates more than 20%: 0 points

**5.2 Workflow Complexity Match (5 points)**
- Type matches actual complexity: 5 points
- Slight mismatch: 3 points
- Severe mismatch (e.g., Simple but very complex): 0 points

**Examples:**
```markdown
# Simple Task (50-150 words)
- File formatting
- Code cleanup
- Single test execution

# Workflow Pipeline (150-400 words)
- Build → Test → Deploy
- Code review automation
- CI/CD pipeline

# Complex Orchestration (400-800 words)
- Multi-environment deployment
- Complex rollback strategy
- Multiple service coordination
```

---

## ✅ Validation 6: Consistency and Completeness (5 points)

### Items

**6.1 Typos and Grammar (3 points)**
- 0 severe typos: 3 points
- 1-2 typos: 2 points
- 3 or more: 0 points

**6.2 Completeness (2 points)**
- All sections complete: 2 points
- "TODO", "WIP" or other incomplete markers present: 1 point
- Multiple empty sections: 0 points

---

## 🔄 Validation Procedure

### Step 1: Read File
```
Read command file with Read tool
→ Fail immediately if file does not exist
```

### Step 2: Extract Basic Information
```
- Parse Frontmatter (YAML)
- Calculate word count
- Count H1/H2/H3
- Count code blocks
- Count examples
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
- Specific improvement suggestions
```

---

## 📝 Validation Result Example

```markdown
📊 Command Validation Result

## 📌 Executive Summary
**Grade: B (83/100)**
**Key Issue: 3 code blocks without language specification, insufficient Boundaries section**
**Recommended Action: Can achieve 90+ points with auto-correction**

---

## Score Details

| Validation Item | Score | Max | Notes |
|-----------------|-------|-----|-------|
| 1. Structure Validation | 20 | 20 | ✅ |
| 2. Frontmatter Validation | 15 | 15 | ✅ |
| 3. Required Section Validation | 20 | 25 | ⚠️ Insufficient Boundaries section |
| 4. Content Quality Validation | 18 | 25 | ⚠️ 30% code blocks without language |
| 5. Command Type Appropriateness | 10 | 10 | ✅ |
| 6. Consistency and Completeness | 0 | 5 | ❌ 5 typos found |
| **Total** | **83** | **100** | **B (Good)** |

---

## ✅ Strengths
- Perfect file structure and naming convention
- Accurate Frontmatter format
- Complexity appropriate for command type

## ⚠️ Needs Improvement
1. **Code blocks without language specification** (Line 45, 89, 123)
2. **Insufficient Boundaries section content** (Line 156)
3. **5 typos** (Line 23, 67, 102, 134, 178)

## 💡 Improvement Suggestions
- Add language to code blocks with auto-correction → +7 points
- Specify Boundaries section → +5 points
- Fix typos → +5 points
- Expected score after improvements: **100 points (A)**
```

---

## 🛠️ How to Use This Document

### In /create-command Command
```markdown
Phase 4: File Creation and Validation

**Execute Validation:**
Follow validation procedure from @shared/command/validation-criteria.md.

**Generate Report:**
Generate simple summary report based on validation results.
```

### In /evaluate-command Command
```markdown
Phase 1: Command Evaluation

**Execute Validation:**
Follow validation procedure from @shared/command/validation-criteria.md.

**Generate Detailed Report:**
Generate complete report including Executive Summary + detailed validation results + improvement suggestions.
```

---

This validation criteria ensures quality consistency of Claude Code command files and enables objective evaluation.
