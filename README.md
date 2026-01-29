# TLA+ Skills and Rules for AI

## Introduction

A collection of agent skills for working with TLA+ specifications. These skills enable the AI assistant to perform common TLA+ modeling tasks with proper structure and conventions.

Note 1: "Agent skills" is meant in a general sense, and is not tied to any specific agent infrastructure (e.g., Claude or Cursor Skills). Contrbutions are welcome for any skills, rules, prompts or workflows that proved to be useful.

Note 2: The [TLA+ VSCode/Cursor extension](https://github.com/tlaplus/vscode-tlaplus) already bundles several [MCP](https://modelcontextprotocol.io/specification/2025-06-18/server/resources) knowledge base [articles](https://github.com/tlaplus/vscode-tlaplus/tree/master/resources/knowledgebase). Eventually, we should find a way to unify them.

Note 3: The [TLA+ VSCode/Cursor extension](https://github.com/tlaplus/vscode-tlaplus) also contains [TLA+ MCP server](https://github.com/tlaplus/vscode-tlaplus/blob/master/src/lm/MCPServer.ts) with lots of useful tools. Contributions are very welcome to add additional tools to it.


## Overview

| Skill | Description |
|-------|-------------|
| [tlaplus-add-variable](#tlaplus-add-variable) | Add a new variable to an existing TLA+ specification |
| [tlaplus-split-action](#tlaplus-split-action) | Split a TLA+ action into two sequential actions |
| [tlaplus-from-source](#tlaplus-from-source) | Generate a TLA+ model from source code |

## Skills

### tlaplus-add-variable

Add a new variable to an existing TLA+ specification without changing its semantics.

**When to use:**
- Adding a new state variable to track additional information
- Introducing auxiliary variables for verification
- Extending a model with new features

**What it handles:**
- `VARIABLE` declaration block
- `Init` predicate initialization
- All `UNCHANGED` statements (including nested and conditional branches)
- `vars` tuple definition
- `TypeOk` invariant (if present)

**Example prompt:**
> "Add a `debug_log` variable to track all operations in this specification"

---

### tlaplus-split-action

Split a TLA+ action into two sequential actions by introducing a new program counter (pc) state.

**When to use:**
- Modeling finer-grained atomicity
- Adding intermediate steps to an action sequence
- Breaking down complex actions for clearer verification

**What it handles:**
- Creating new PC state constants
- Updating PC state sets
- Modifying the original action's destination
- Creating the new intermediate action
- Renumbering subsequent actions (if using numbered naming conventions)
- Updating `TypeOk`, `Next`, and fairness constraints

**Example prompt:**
> "Split `ConnectAction_1` into two actions - the first should acquire the lock, the second should update the connection state"

---

### tlaplus-from-source

Generate a high-level TLA+ model from source code (C, C++, Rust, etc.).

**When to use:**
- Creating formal specifications from existing implementations
- Modeling concurrent or distributed algorithms
- Verifying correctness properties of production code

**Workflow:**
1. **Analyze** - Understands the code's purpose, key functions, and state
2. **Abstract** - Maps implementation details to TLA+ concepts
3. **Specify** - Writes the TLA+ specification with proper structure
4. **Propose** - Suggests safety invariants and liveness properties

**What it produces:**
- Complete TLA+ module with constants, variables, and actions
- `TypeOk` invariant for type checking
- `Init` and `Next` definitions
- Proposed safety invariants and liveness properties
- Comments referencing original source code

**Example prompt:**
> "Create a TLA+ model for this connection management code, focusing on the state machine and reference counting"

## Usage Tips

1. **Open the relevant file** - Have your TLA+ spec or source code open before invoking the skill

2. **Be specific** - Provide details about what you want:
   - For `add-variable`: Specify the variable name, type, and initial value
   - For `split-action`: Specify which action to split and how to divide the logic
   - For `from-source`: Describe what aspects of the code to model

3. **Review the changes** - The AI will make systematic updates; verify the modifications are correct

4. **Run TLC** - After modifications, run the TLA+ model checker to verify the specification still works

