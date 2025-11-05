# Sub-Agent Available Tools

A document listing all tools available for sub-agents, with descriptions, use cases, and security considerations for each tool.

---

## 📚 Complete Tool List

| Tool | Description | Main Use | Security Level |
|------|-------------|----------|----------------|
| **Read** | Read files | Code analysis, validation, review | 🟢 Safe |
| **Write** | Write/create files | File creation, content writing | 🟡 Caution |
| **Edit** | Edit files | Modify existing files | 🟡 Caution |
| **Bash** | Execute commands | Tool execution, build, test | 🔴 Dangerous |
| **Grep** | Pattern search | Code search, pattern finding | 🟢 Safe |
| **Glob** | File search | File pattern matching | 🟢 Safe |
| **WebFetch** | Fetch web pages | Document lookup, information gathering | 🟢 Safe |
| **WebSearch** | Web search | Information search, find documentation | 🟢 Safe |

---

## 🔧 Tool Detailed Descriptions

### Read
**Description**: Reads file contents.

**Use Cases**:
- Code analysis and review
- Configuration file validation
- Document reading
- Test file verification

**Security Considerations**:
- ✅ Read-only, safe
- ✅ No file system changes

**Recommended Agent Types**: All types

---

### Write
**Description**: Creates new files or completely overwrites existing files.

**Use Cases**:
- Create new files
- Write reports
- Create configuration files
- Write template files

**Security Considerations**:
- ⚠️ Can overwrite existing files
- ⚠️ Potential unintended file loss
- 💡 Recommended to check existing files with Read first

**Recommended Agent Types**: Specialist (formatter), Orchestrator (workflow management)

---

### Edit
**Description**: Modifies specific parts of existing files.

**Use Cases**:
- Code modification
- Version number updates
- Configuration value changes
- Refactoring

**Security Considerations**:
- ⚠️ File content modification
- 💡 Accurate pattern matching required
- 💡 Backup recommended before changes

**Recommended Agent Types**: Specialist (code formatter), Orchestrator (version management)

---

### Bash
**Description**: Executes shell commands.

**Use Cases**:
- Run tests (npm test, pytest)
- Build commands (npm run build)
- Run linters (eslint, black)
- Git commands (git status, git add)
- Package management (npm install, pip install)

**Security Considerations**:
- 🔴 Can execute arbitrary commands
- 🔴 Can modify file system
- 🔴 Can make network requests
- 💡 Command scope must be clearly limited
- 💡 Prohibit dangerous commands (rm -rf, sudo)

**Recommended Agent Types**: Specialist (tool execution), Analyst (validation), Orchestrator (build/deployment)

---

### Grep
**Description**: Searches for patterns in file contents.

**Use Cases**:
- Find specific code patterns
- Search for security vulnerabilities
- Find TODO/FIXME
- Search dependencies

**Security Considerations**:
- ✅ Read-only, safe
- ✅ No file system changes

**Recommended Agent Types**: Analyst (code review, security audit)

---

### Glob
**Description**: Searches for files using file path patterns.

**Use Cases**:
- Find files with specific extensions (*.ts, *.py)
- Explore directory structure
- Generate file lists

**Security Considerations**:
- ✅ Read-only, safe
- ✅ No file system changes

**Recommended Agent Types**: Analyst, Orchestrator

---

### WebFetch
**Description**: Fetches web page contents.

**Use Cases**:
- Look up official documentation
- Check API documentation
- Collect external information

**Security Considerations**:
- ✅ Read-only
- ⚠️ External network requests
- 💡 Recommended to use only trusted domains

**Recommended Agent Types**: Analyst (information gathering), Orchestrator (document checking)

---

### WebSearch
**Description**: Searches for information on the web.

**Use Cases**:
- Search for latest technical information
- Find error message solutions
- Look up best practices

**Security Considerations**:
- ✅ Read-only
- ⚠️ External network requests
- 💡 Search result validation required

**Recommended Agent Types**: Analyst, Orchestrator

---

## 🔐 Principle of Least Privilege

It's recommended to allow only the minimum tools necessary for the agent's role.

### Recommended Tool Combinations by Role

#### Read-Only Analysis (code review, security audit)
```yaml
tools: Read,Grep
```
**Reason**: Safe as it only reads and searches files

#### Analysis + Tool Execution (test execution, linters)
```yaml
tools: Read,Grep,Bash
```
**Reason**: Analysis followed by tool execution for validation

#### File Modification (formatting, code cleanup)
```yaml
tools: Read,Write,Edit
```
**Reason**: Need to read and modify files

#### Complex Workflow (build, deployment, release)
```yaml
tools: Read,Write,Bash
```
**Reason**: File read/write + command execution needed

#### Comprehensive Analysis (comprehensive review)
```yaml
tools: Read,Grep,Bash
```
**Reason**: Code analysis + test/linter execution

---

## ⚠️ Tool Suitability Validation by Type

### Specialist (single task specialist)

**Recommended Tools**:
- Formatting: `Read,Write,Bash`
- Validation: `Read,Bash`
- Link Checking: `Read,Bash,WebFetch`

**Warning Scenarios**:
```yaml
# ❌ Grep unnecessary for formatter
specialist_formatter:
  tools: Read,Write,Bash,Grep  # Grep warning
  reason: "Search not needed for formatting"

# ✅ Appropriate tool combination
specialist_formatter:
  tools: Read,Write,Bash
```

---

### Analyst (analysis and review)

**Recommended Tools**:
- Code Review: `Read,Grep,Bash`
- Security Audit: `Read,Grep`
- Performance Analysis: `Read,Grep,Bash`

**Warning Scenarios**:
```yaml
# ❌ Write inappropriate for analysis tasks (should be read-only)
analyst_reviewer:
  tools: Read,Grep,Write  # Write warning
  reason: "Analysis should be read-only"

# ✅ Appropriate tool combination
analyst_reviewer:
  tools: Read,Grep,Bash
```

---

### Orchestrator (complex workflow)

**Recommended Tools**:
- Deployment: `Read,Write,Bash`
- Release Management: `Read,Write,Edit,Bash`
- CI/CD Pipeline: `Read,Write,Bash,Grep`

**Warning Scenarios**:
```yaml
# ⚠️ Allowing all tools not recommended
orchestrator_release:
  tools: "*"  # All tools - excessive permissions
  reason: "Should specify only necessary tools"

# ✅ Appropriate tool combination
orchestrator_release:
  tools: Read,Write,Bash
```

---

## 🛡️ Security Checklist

Check the following when creating agents:

- [ ] **Least Privilege**: Are only the minimum tools necessary for the role allowed?
- [ ] **Read-Only**: Do analysis tasks use only Read, Grep?
- [ ] **Write/Edit Justification**: Is file modification really necessary?
- [ ] **Bash Command Scope**: Are the commands to execute clearly defined?
- [ ] **Dangerous Command Prohibition**: Not using dangerous commands like rm -rf, sudo?
- [ ] **Avoid All Tools**: Avoided using tools: "*"?

---

## 📖 References

- **Validation Criteria**: `validation-criteria.md`
- **Type System**: `type-system.md`
- **Best Practices**: `best-practices.md`
- **Templates**: `templates/` directory
