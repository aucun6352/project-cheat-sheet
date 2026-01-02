---
description: "Create well-formatted commits with conventional commit messages and emoji"
allowed-tools: Bash(git:*), Read
model: claude-haiku-4-5
---

<!--
BEST PRACTICES FOR COMMITS:
  - Atomic commits: Each commit should contain related changes serving a single purpose
  - Split large changes: If changes touch multiple concerns, split into separate commits
  - Conventional commit format: <emoji> <type>: <description>
  - Present tense, imperative mood: "add feature" not "added feature"
  - Concise first line: Keep under 72 characters

COMMIT TYPES:
  - feat: A new feature
  - fix: A bug fix
  - docs: Documentation changes
  - style: Code style changes (formatting, etc)
  - refactor: Code changes that neither fix bugs nor add features
  - perf: Performance improvements
  - test: Adding or fixing tests
  - chore: Changes to the build process, tools, etc.

SPLITTING CRITERIA:
When analyzing the diff, consider splitting commits based on:
  1. Different concerns: Changes to unrelated parts of the codebase
  2. Different types of changes: Mixing features, fixes, refactoring, etc.
  3. File patterns: Changes to different types of files (e.g., source code vs documentation)
  4. Logical grouping: Changes that would be easier to understand or review separately
  5. Size: Very large changes that would be clearer if broken down

EXAMPLES OF GOOD COMMIT MESSAGES:
  - ✨ feat: add user authentication system
  - 🐛 fix: resolve memory leak in rendering process
  - 📝 docs: update API documentation with new endpoints
  - ♻️ refactor: simplify error handling logic in parser
  - 🚨 fix: resolve linter warnings in component files
  - 🧑‍💻 chore: improve developer tooling setup process
  - 👔 feat: implement business logic for transaction validation
  - 🩹 fix: address minor styling inconsistency in header
  - 🚑️ fix: patch critical security vulnerability in auth flow
  - 🎨 style: reorganize component structure for better readability
  - 🔥 fix: remove deprecated legacy code
  - 🦺 feat: add input validation for user registration form
  - 💚 fix: resolve failing CI pipeline tests
  - 📈 feat: implement analytics tracking for user engagement
  - 🔒️ fix: strengthen authentication password requirements
  - ♿️ feat: improve form accessibility for screen readers

EXAMPLE OF SPLITTING COMMITS:
  - First commit: ✨ feat: add new solc version type definitions
  - Second commit: 📝 docs: update documentation for new solc versions
  - Third commit: 🔧 chore: update package.json dependencies
  - Fourth commit: 🏷️ feat: add type definitions for new API endpoints
  - Fifth commit: 🧵 feat: improve concurrency handling in worker threads
  - Sixth commit: 🚨 fix: resolve linting issues in new code
  - Seventh commit: ✅ test: add unit tests for new solc version features
  - Eighth commit: 🔒️ fix: update dependencies with security vulnerabilities

COMPLETE EMOJI REFERENCE:
  Core types (used in main workflow):
    ✨ feat: New feature
    🐛 fix: Bug fix
    📝 docs: Documentation
    💄 style: Formatting/style
    ♻️ refactor: Code refactoring
    ⚡️ perf: Performance improvements
    ✅ test: Tests
    🔧 chore: Tooling, configuration
    🚀 ci: CI/CD improvements
    🗑️ revert: Reverting changes

  Extended emoji mappings (available for specific cases):
    🧪 test: Add a failing test
    🚨 fix: Fix compiler/linter warnings
    🔒️ fix: Fix security issues
    👥 chore: Add or update contributors
    🚚 refactor: Move or rename resources
    🏗️ refactor: Make architectural changes
    🔀 chore: Merge branches
    📦️ chore: Add or update compiled files or packages
    ➕ chore: Add a dependency
    ➖ chore: Remove a dependency
    🌱 chore: Add or update seed files
    🧑‍💻 chore: Improve developer experience
    🧵 feat: Add or update code related to multithreading or concurrency
    🔍️ feat: Improve SEO
    🏷️ feat: Add or update types
    💬 feat: Add or update text and literals
    🌐 feat: Internationalization and localization
    👔 feat: Add or update business logic
    📱 feat: Work on responsive design
    🚸 feat: Improve user experience / usability
    🩹 fix: Simple fix for a non-critical issue
    🥅 fix: Catch errors
    👽️ fix: Update code due to external API changes
    🔥 fix: Remove code or files
    🎨 style: Improve structure/format of the code
    🚑️ fix: Critical hotfix
    🎉 chore: Begin a project
    🔖 chore: Release/Version tags
    🚧 wip: Work in progress
    💚 fix: Fix CI build
    📌 chore: Pin dependencies to specific versions
    👷 ci: Add or update CI build system
    📈 feat: Add or update analytics or tracking code
    ✏️ fix: Fix typos
    ⏪️ revert: Revert changes
    📄 chore: Add or update license
    💥 feat: Introduce breaking changes
    🍱 assets: Add or update assets
    ♿️ feat: Improve accessibility
    💡 docs: Add or update comments in source code
    🗃️ db: Perform database related changes
    🔊 feat: Add or update logs
    🔇 fix: Remove logs
    🤡 test: Mock things
    🥚 feat: Add or update an easter egg
    🙈 chore: Add or update .gitignore file
    📸 test: Add or update snapshots
    ⚗️ experiment: Perform experiments
    🚩 feat: Add, update, or remove feature flags
    💫 ui: Add or update animations and transitions
    ⚰️ refactor: Remove dead code
    🦺 feat: Add or update code related to validation
    ✈️ feat: Improve offline support
-->

# Git Commit Workflow

Execute the following steps to create a well-formatted conventional commit with emoji.

## Step 1: Check Repository Status

Run `git status` to identify:
- Currently staged files
- Unstaged changes
- Untracked files

Display the output to understand the current state.

## Step 2: Handle File Staging

**If no files are staged:**
- Automatically stage all modified and new files with `git add .`
- Inform user that all changes have been staged

**If files are already staged:**
- Proceed with those files
- Note which files will be included in the commit

## Step 3: Review Changes

Execute `git diff --cached` to review the staged changes.

Analyze the diff to understand:
- What changed in each file
- The overall purpose of the changes
- Whether multiple distinct logical changes exist

## Step 4: Determine Commit Strategy

**If multiple distinct logical changes are detected:**
- List the separate concerns identified
- Suggest splitting into multiple commits
- For each logical group:
  - Specify which files belong to it
  - Suggest the commit type and message
  - Guide user through selective staging with `git reset` and `git add`

**If single logical change detected:**
- Proceed directly to commit message creation

## Step 5: Create Commit Message

Based on your analysis, determine the appropriate commit type and emoji:

**Core emoji mappings (use these for most commits):**
- ✨ **feat**: New feature
- 🐛 **fix**: Bug fix
- 📝 **docs**: Documentation only
- ♻️ **refactor**: Code restructuring without changing behavior
- ⚡️ **perf**: Performance improvement
- ✅ **test**: Adding or updating tests
- 🔧 **chore**: Tooling, configuration, dependencies
- 🚨 **fix**: Fix linter/compiler warnings
- 🔒️ **fix**: Security fix
- 🔥 **refactor**: Remove code or files

**Format:** `<emoji> <type>: <description>`

**Guidelines:**
- Use imperative mood: "add" not "added"
- Keep description under 72 characters
- Be specific about what changed, not how
- Focus on the "why" if not obvious from "what"

**Extended mappings** (see HTML comments above for complete list):
- Use more specific emojis when appropriate
- Default to core mappings when uncertain

Draft the commit message following this format.

## Step 6: Execute Commit

Create the commit using the drafted message:
- Use `git commit -m "message"` for single-line commits
- For multi-line commits with body, guide user or use heredoc format

After committing:
- Display the commit hash
- Show success confirmation
- If multiple commits were planned, guide through remaining commits

## Step 7: Post-Commit Summary

Provide a brief summary:
- Number of commits created
- Commit hash(es)
- Brief description of what was committed
- Suggest next steps if applicable (e.g., review, push, create PR)

---

**Important Notes:**
- Always review the diff before creating commit messages
- Ensure commit message accurately reflects the changes
- When in doubt, prefer smaller, focused commits over large ones
- If changes include sensitive information, warn before committing
