# AGENTS.md - Agent Coding Guidelines

Repository: skale-skills - Collection of installable Agent Skills for SKALE Network workflows.

## Repository Purpose

This is a **documentation/skills repository** - not a code project. Contains structured markdown files that get installed via:

```bash
npx -y skills add skalenetwork/skale-skills --skill <skill-name>
```

## Build/Lint/Test Commands

No build system. This is markdown-only.

## Repository Structure

```
skills/
  <skill>/
    SKILL.md           # Root entry point - MUST have frontmatter
    rules/             # Must-follow constraints
    references/        # Conceptual docs and lookup tables
    examples/          # Concrete patterns and commands
    scripts/           # Optional executable helpers
    agents/            # UI metadata (openai.yaml)
```

## File Conventions

### SKILL.md Requirements

Every skill root MUST have frontmatter:

```yaml
---
name: skill-name
description: One-line description. Use for X, Y, Z. Triggers: keyword1, keyword2.
---
```

### Section Headers

- `## Rules (Must Follow)` or `## Rules Index`
- `## References (Concepts + Lookup)` or `## References Index`
- `## Examples (Concrete Patterns)` or `## Examples Index`

### Dynamic Discovery Pattern

Each SKILL.md must include:

```markdown
## Workflow

1. Identify request type
2. Check `rules/` first for mandatory constraints
3. Use `references/` for decision-making context
4. Use `examples/` for concrete patterns
```

## Naming Conventions

### Brand Name: SKALE (All Caps)

Always use "SKALE" not "skale", "Skale", or variants:

```markdown
<!-- Correct -->
SKALE Network, SKALE Europa, SKALE Calypso

<!-- Incorrect -->
skale network, Skale Europa
```

### File Naming

- Directories: kebab-case (`bite-development`, `skale-cli`)
- Markdown: kebab-case (`chain-naming.md`, `contracts-deployment.md`)
- JSON: kebab-case (`chains.json`)

### In Code Examples

```typescript
// Constants: SKALE_ prefix with caps
const SKALE_CHAIN = "0x727cd5b7...";
const SKALE_RPC = "https://rpc...";

// Variables: camelCase with SKALE caps in compound words
const skaleProvider = new Provider();
const isSKALE = true;

// Functions: SKALE caps
function connectToSKALE() {}

// UI text: Always "SKALE"
<h1>Connect to SKALE</h1>
```

## Markdown Style

### Rules Files (rules/)

Format: Problem → Incorrect → Correct → Context

```markdown
# Rule: rule-name

## Why It Matters
<Brief explanation>

## Incorrect
<Code examples of what NOT to do>

## Correct
<Code examples of best practice>

## Context
<Additional details, tables, references>
```

### Examples Files (examples/)

Format: Incorrect → Correct → Full Implementation

```markdown
# Example: example-name

## Incorrect
<Anti-patterns>

## Correct
<Best practice snippets>

## Full Implementation
<Complete working code>

## References
<External links>
```

### References Files (references/)

Format: Conceptual documentation, lookup tables, API references

```markdown
# Reference: topic-name

## Overview
<Concept explanation>

## Details
<Structured information>

## Tables
<Lookup data if applicable>
```

## Code Examples in Markdown

### TypeScript/JavaScript

```typescript
// Use const assertions for config
const CHAIN_CONFIG = {
    skale_europa: {
        chainId: 0x727cd5b7,
        name: "SKALE Europa"
    }
} as const;

// Prefer type-safe patterns
type ChainName = keyof typeof CHAIN_CONFIG;
```

### Solidity

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SKALEExample {
    // Use SKALE in contract names
    bytes32 private constant SKALE_CHAIN_ID = 0x...;
}
```

### Shell Commands

```bash
# Chain-targeted commands require --chain
skale read <contract> <method> --chain europa

# Network-targeted commands require --network
skale ima chain-id --network mainnet
```

## Error Handling

### In Documentation

Document error taxonomy when applicable:

```markdown
## Error Taxonomy

- `MISSING_TARGET`: Required flag not provided
- `INVALID_ADDRESS`: Not a valid Ethereum address
- `INVALID_CONTRACT`: Contract not supported for target
```

### In Code Examples

```typescript
// Always validate chain ID before operations
const currentChainId = await ethers.provider.getNetwork()
    .then(n => Number(n.chainId));

if (currentChainId !== config.chainId) {
    throw new Error(`Chain ID mismatch! Expected ${config.chainId}`);
}
```

## Chains Reference

Source of truth: `skills/skale/references/chains.json`

SKALE Chains (`--chain`):
- calypso, europa, nebula, titan, strayshot
- calypso-testnet, europa-testnet, nebula-testnet, titan-testnet

Ethereum Networks (`--network`):
- mainnet, holesky

## External Documentation

SKALE docs endpoints:
- `https://docs.skale.space/llms.txt`
- `https://docs.skale.space/llms-full.txt`
- `https://docs.skale.space/llms-small.txt`

Add `.md` suffix for raw markdown.

## Git Conventions

### .gitignore

```
venv/
.venv/
__pycache__/
*.pyc
```

### Commits

- Keep markdown changes atomic
- Update SKILL.md indexes when adding new files
- Verify cross-references work

## Validation Checklist

Before adding/modifying skills:

- [ ] SKILL.md has proper frontmatter (name, description)
- [ ] All SKALE references use correct capitalization
- [ ] File cross-references exist and are correct
- [ ] Code examples are syntactically valid
- [ ] Rules follow problem/incorrect/correct format
- [ ] Examples follow incorrect/correct/implementation format

## Related Skills

Companion skills are cross-linked:

- `skale` → general SKALE development
- `bite-development` → BITE protocol implementation
- `skale-cli` → CLI command playbooks
