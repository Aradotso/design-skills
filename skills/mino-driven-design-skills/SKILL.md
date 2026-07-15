---
name: mino-driven-design-skills
description: Apply Mino-san's design principles for problem framing, domain modeling, contract-driven design, and reproducible development workflows
triggers:
  - apply mino design principles to this feature
  - check domain model completeness using mino skill
  - frame this problem using mino problem framing
  - validate design by contract using mino approach
  - separate interface from implementation following mino
  - use mino reproducible development workflow
  - audit architecture quality with mino strategy
  - apply design skills from mino suite
---

# mino-driven-design-skills

> Skill by [ara.so](https://ara.so) — Design Skills collection.

This skill suite applies design principles extracted from Mino-san's public materials, restructured for AI-assisted development. It helps you move from ambiguous requirements to verifiable contracts, complete domain models, and reproducible implementations.

**Not official**: This is an unofficial project inspired by publicly available materials. It is not supervised, endorsed, or recommended by Mino-san or any affiliated organization.

**Goal**: Produce grounded, verifiable design artifacts—not mimic speaking style or conclusions. The suite helps you check problems, purpose, context, requirements, contracts, models, quality, and public boundaries before implementation.

## Installation

### Clone the Repository

```bash
git clone https://github.com/my-take-dev/inspired-mino-design-skills.git
cd inspired-mino-design-skills
```

### Validate the Suite

Run structural validation on your platform:

**Linux / macOS:**
```bash
bash .agents/validate-suite.sh
```

**Windows PowerShell:**
```powershell
pwsh .agents/validate-suite.ps1
```

**Windows Command Prompt:**
```cmd
powershell -ExecutionPolicy Bypass -File .agents/validate-suite.ps1
```

Validation checks:
- YAML frontmatter structure
- Required sections (Purpose, Scope, Exclusions, Gates, Workflow)
- Platform-specific script compatibility

### Configure Skill Routing

Add this to your project's `AGENTS.md` or `.github/AGENTS.md`:

```markdown
# Skill Composition

When a request matches multiple Skills:

- Use the Skill that best matches the primary outcome as the basic workflow.
- Add only relevant language-, framework-, or tool-specific Skills to supplement that workflow.
- Let the basic Skill control scope, changes, validation, and the final response; specialized Skills provide their domain-specific guidance.
- Preserve every applicable Skill's exclusions, hard gates, and safety constraints.
- Follow the user's explicitly named Skills and do not add unrelated Skills.
```

## Current Status

```text
Status: Experimental / Preview
Suite version: 0.9.0
Structural validation: pass on Linux and PowerShell-over-WSL; fail on native macOS
macOS support: implemented but not passing (run 29397674053, exit 2)
Behavioral release: not ready
```

**Behavioral release** means a designated evaluation owner has executed frozen versioned cases with isolated oracles in fresh contexts, meeting representative case, negative case, regression, and required platform release gates. Experimental distribution is permitted, but stable release requires maintainer approval of all Evidence.

## Available Skills

| Skill | Use When | Primary Artifacts |
|-------|----------|-------------------|
| [`mino-core`](.agents/skills/mino-core/SKILL.md) | Other Skills need shared problem definition, evidence, requirement tracing, decision rules. Don't call directly. | Problem Frame, Context Packet, Requirement Catalog, shared decisions |
| [`mino-problem-framing`](.agents/skills/mino-problem-framing/SKILL.md) | Starting from tech-first proposals or ambiguous requirements—need to separate observations, assumptions, problems, goals, success criteria before design | Problem Framing Package |
| [`mino-domain-model-completeness`](.agents/skills/mino-domain-model-completeness/SKILL.md) | Auditing use case for missing concepts, states, constraints, failures, authority | Completeness Package |
| [`mino-design-by-contract`](.agents/skills/mino-design-by-contract/SKILL.md) | Converting natural language requirements into preconditions, postconditions, invariants, failure guarantees, contract tests | Contract Package |
| [`mino-interface-implementation-separation`](.agents/skills/mino-interface-implementation-separation/SKILL.md) | Finding caller-side branching or tech leakage—designing boundaries around purpose and contract | Boundary Package |
| [`mino-architecture-quality-strategy`](.agents/skills/mino-architecture-quality-strategy/SKILL.md) | Designing multi-module systems, data ownership, system-wide quality trade-offs, migration, recovery | Architecture Strategy Package |
| [`mino-reproducible-development`](.agents/skills/mino-reproducible-development/SKILL.md) | Medium-to-large design, implementation, review, reproducibility verification—integrating multiple specialized artifacts with independent verification | Implementation Spec, Verified Change, Review Result, or Reproduction Report |

### Choosing by Development Phase

| Phase | Skill | When to Use |
|-------|-------|-------------|
| Design | `mino-problem-framing` | Before implementation—clarify problem, purpose, assumptions, success criteria |
| Design | `mino-domain-model-completeness` | Check for missing business concepts, states, constraints, behaviors |
| Design | `mino-design-by-contract` | Convert requirements into testable normal/exceptional conditions |
| Design | `mino-interface-implementation-separation` | Separate public operations from internal implementation methods |
| Architecture | `mino-architecture-quality-strategy` | Design system composition, data management, migration, recovery |
| Design + Implementation + Review | `mino-reproducible-development` | Medium+ changes requiring multiple design viewpoints through implementation and verification |
| Internal | `mino-core` | Used by other Skills—developers don't call directly |

**For new features or major changes**: Start with `mino-problem-framing` to establish design premises, then pick one specialized design Skill. Use `mino-reproducible-development` only when integrating multiple viewpoints through implementation and review.

**For small mechanical changes** (rename, approved baseline): You don't need this suite.

## Common Workflows

### 1. Frame a New Feature Problem

```bash
# In your AI coding agent (Claude Code, Cursor, etc.)
# Say: "Apply mino problem framing to this feature"
```

The agent will produce a **Problem Framing Package** containing:

```markdown
# Problem Frame

## Observation
[What users/systems currently do, concrete data]

## Assumption
[What we believe but haven't verified]

## Problem
[Gap between current state and desired state]

## Purpose / Goal
[What success looks like, measurable]

## Success Criteria
[How we'll know we've solved it]

## Context Packet
- Stakeholder: [who needs this]
- Constraint: [tech, time, policy limits]
- Risk: [what could go wrong]
- Dependency: [what must exist first]
```

**Example (Shell script context):**

```markdown
## Observation
validate-suite.sh passes on Linux runners but exits 2 on native macOS /bin/bash 3.2 at fixture runner portable rewrite.

## Assumption
macOS /bin/bash 3.2 has stricter parameter expansion or array handling than Linux bash.

## Problem
Native macOS developers can't verify fixture runner structural correctness before commit.

## Purpose
All required platforms pass structural validation and fixture execution.

## Success Criteria
- validate-suite.sh exits 0 on macOS 15.7.7 ARM64 with /bin/bash 3.2
- solver-nested-metadata fixture completes without parse errors
- CI job "Native macOS /bin/bash" shows green
```

### 2. Audit Domain Model Completeness

```bash
# Say: "Check domain model completeness using mino skill"
```

The agent produces a **Completeness Package**:

```markdown
# Domain Model Completeness Audit

## Concepts
- [Business entity]: attributes, lifecycle
- **MISSING**: [concept users mention but not modeled]

## States
- [Entity]: [state1] → [state2] → [state3]
- **MISSING**: [intermediate state, error state]

## Constraints
- Invariant: [what must always be true]
- **MISSING**: [business rule not enforced]

## Failures
- [Operation]: [failure mode], [guarantee level (strong/basic/none)]
- **MISSING**: [unhandled error case]

## Authority
- [Role] can [operation] on [entity]
- **MISSING**: [permission check, ownership model]

## Writer / Reader Coverage
- Writer: [who modifies this data]
- Reader: [who queries this data]
- **MISSING**: [unclaimed data ownership]
```

**Example (validation suite context):**

```markdown
## Concepts
- Skill: name, triggers, sections
- Evidence: platform, runner, exit code, artifacts
- **MISSING**: Baseline (approved frozen version for regression)

## States
- Skill: draft → structural-valid → behavioral-verified → stable-release
- **MISSING**: deprecated, sunset states

## Constraints
- Invariant: Every Skill has exactly one SKILL.md
- **MISSING**: Maximum trigger count, minimum trigger length

## Failures
- validate-suite.sh: exits 1 on parse error (basic guarantee: no partial write)
- **MISSING**: fixture runner failure isolation (could corrupt shared state)

## Authority
- Maintainer: can approve stable release
- Evaluation owner: can freeze versioned case
- **MISSING**: Who can add new required platform?
```

### 3. Design by Contract

```bash
# Say: "Validate design by contract using mino approach"
```

Produces a **Contract Package**:

```markdown
# Contract Specification

## Operation: [function/command name]

### Preconditions
- [What must be true before calling]
- [Input validation rules]

### Postconditions (Normal)
- [What is guaranteed after success]
- [State changes, return value properties]

### Postconditions (Exceptional)
- [Failure mode]: [what is guaranteed even on failure]

### Invariants
- [What remains true before and after]

### Contract Tests
```[language]
# Test: precondition violation rejected
# Test: postcondition holds on success
# Test: invariant preserved
# Test: failure guarantee holds
```
```

**Example (Shell validation script):**

```markdown
## Operation: validate-suite.sh

### Preconditions
- Working directory contains .agents/skills/
- /bin/bash or bash is available
- Read permission on all SKILL.md files

### Postconditions (Normal)
- Exit code 0
- All SKILL.md files have valid YAML frontmatter
- All required sections present
- STDOUT shows "✓" for each check

### Postconditions (Exceptional)
- Exit code 1 on parse error: no files modified
- Exit code 2 on missing section: STDERR shows which file, which section
- Partial progress printed before failure (basic guarantee)

### Invariants
- Script never modifies SKILL.md files
- Script never writes outside .agents/

### Contract Tests
```bash
# Test: exits 0 when all Skills valid
bash .agents/validate-suite.sh
assert_exit_code 0

# Test: exits 1 on malformed YAML
echo "invalid: [" > .agents/skills/test/SKILL.md
bash .agents/validate-suite.sh
assert_exit_code 1

# Test: files unmodified after validation
checksum_before=$(md5sum .agents/skills/*/SKILL.md)
bash .agents/validate-suite.sh
checksum_after=$(md5sum .agents/skills/*/SKILL.md)
assert_equal "$checksum_before" "$checksum_after"
```
```

### 4. Separate Interface from Implementation

```bash
# Say: "Separate interface from implementation following mino"
```

Produces a **Boundary Package**:

```markdown
# Interface-Implementation Separation

## Public Interface
[Operations visible to callers, in terms of their purpose]

## Implementation Options
[How to achieve those operations—hidden from caller]

## Leaked Abstractions Found
- Caller checks [internal detail] before calling
- Return value exposes [implementation choice]

## Proposed Boundary
[Operations named by purpose, contracts independent of how]

## Migration Path
[How to move existing callers to new boundary]
```

**Example (validation suite):**

```markdown
## Public Interface (Current)
- validate-suite.sh: caller must know script path, shell variant
- Caller must choose .sh vs .ps1 based on OS

## Implementation Options
- Bash script for POSIX systems
- PowerShell script for Windows
- Unified entry point that detects platform

## Leaked Abstractions Found
- Caller branches on OS to pick script extension
- Caller must know .agents/ directory structure
- Script assumes /bin/bash path (macOS), not generic bash

## Proposed Boundary
```bash
# Single entry point, platform-agnostic from caller POV
mino-validate
# Internally detects platform, dispatches to .sh or .ps1
```

## Migration Path
1. Add mino-validate wrapper script to repo root
2. Update CI workflows to call mino-validate (no OS check)
3. Deprecate direct .agents/validate-suite.sh calls
4. Keep .sh/.ps1 as internal implementation
```

### 5. Architecture Quality Strategy

```bash
# Say: "Audit architecture quality with mino strategy"
```

Produces an **Architecture Strategy Package**:

```markdown
# Architecture Quality Strategy

## Product Value
[What users/business gain, in measurable terms]

## Quality Portfolio
- Optimize: [quality attributes we maximize]
- Satisfy: [quality attributes we meet threshold]
- Ignore: [quality attributes we intentionally don't optimize]

## Trade-offs
- Choosing [approach A] over [approach B] because [product value priority]

## Module Structure
- [Module]: responsibility, boundaries, dependencies

## Data Ownership
- [Entity]: owned by [module], readers are [modules]

## Migration Strategy
- Current state: [system-wide description]
- Target state: [desired architecture]
- Transition: [incremental steps, rollback points]

## Recovery Strategy
- [Failure scenario]: [detection method], [recovery procedure]
```

**Example (validation suite architecture):**

```markdown
## Product Value
Developers verify Skill structural correctness before push, reducing CI feedback time from 5min to <10sec locally.

## Quality Portfolio
- Optimize: fast local validation (<10sec), platform coverage (Linux, macOS, Windows)
- Satisfy: error message clarity (show file + line), exit code consistency
- Ignore: performance on 1000+ Skills (current scope <10), internationalization

## Trade-offs
- Bash 3.2 subset over Bash 4+ features: macOS compatibility more valuable than associative arrays
- Duplicate .sh/.ps1 scripts over unified runtime: zero dependency install more valuable than single source

## Module Structure
- validate-suite.*: entry point, orchestrates checks
- skill-parser.*: YAML frontmatter extraction
- section-checker.*: required section validation
- fixture-runner.*: oracle execution

## Data Ownership
- SKILL.md: owned by skill author, read by validator
- Evidence/: owned by CI, read by release gatekeeper
- mino-doc/: owned by suite maintainer (reference material)

## Migration Strategy
- Current: manual platform selection by caller
- Target: unified entry point with platform detection
- Transition:
  1. Add mino-validate wrapper (non-breaking)
  2. Update CI to new entry point
  3. Deprecate direct script calls after 2 releases
  4. Keep .sh/.ps1 as internal implementation

## Recovery Strategy
- Native test failure: captured in Evidence/, blocks release (not rollback—no deploy yet)
- Structural regression: gated by required Evidence before merge
- Breaking change to SKILL.md format: version field in frontmatter triggers migration script
```

### 6. Reproducible Development Workflow

Use when you need to **integrate multiple design artifacts through implementation and independent verification**.

```bash
# Say: "Use mino reproducible development workflow for this change"
```

The agent will:

1. **Choose mode** based on change scope:
   - `new`: new feature, new Skill
   - `fix`: bug fix, contract violation
   - `refactor`: internal change, contract preserved
   - `review`: verify existing implementation

2. **Produce Implementation Spec** (for new/fix/refactor):

```markdown
# Implementation Spec

## Problem Frame
[From mino-problem-framing]

## Contract
[From mino-design-by-contract]

## Domain Model
[From mino-domain-model-completeness]

## Boundary
[From mino-interface-implementation-separation]

## Quality Strategy (if system-wide)
[From mino-architecture-quality-strategy]

## Implementation Plan
- [ ] Step 1: [atomic change, verifiable]
- [ ] Step 2: [atomic change, verifiable]
- [Contract test must pass before merge]
```

3. **Execute and verify**:
   - Implement each step
   - Run contract tests
   - Capture Evidence (exit codes, platform, artifacts)
   - Produce **Verified Change** package

4. **Review Result** (for review mode):

```markdown
# Review Result

## Contract Violations Found
- [Operation]: precondition not checked
- [Operation]: postcondition not guaranteed

## Model Gaps Found
- Missing concept: [what users need but not modeled]
- Missing state: [intermediate or error state]

## Boundary Leakage Found
- Caller must know [implementation detail]

## Recommendations
- [Specific change to restore contract]
- [Specific addition to model]
- [Specific boundary adjustment]
```

**Example (fixing macOS validation failure):**

```markdown
# Implementation Spec

## Problem Frame
**Observation**: validate-suite.sh exits 2 on macOS /bin/bash 3.2 at solver-nested-metadata fixture.
**Problem**: Native macOS developers can't verify correctness before commit.
**Purpose**: All required platforms pass structural validation.

## Contract
**Operation**: validate-suite.sh
**Precondition**: Bash 3.2+ available
**Postcondition (normal)**: exit 0, all checks pass
**Postcondition (exceptional)**: exit 1 on parse error, exit 2 on structural error, STDERR shows location
**Invariant**: Never modifies SKILL.md files

## Domain Model
**Concept**: Platform (Linux, macOS, Windows)
**State**: Evidence: not-run → executed → pass/fail
**Constraint**: All required platforms must reach pass before release

## Boundary
**Public**: mino-validate (platform-agnostic)
**Internal**: validate-suite.sh, validate-suite.ps1 (implementation)

## Implementation Plan
- [ ] Reproduce failure: run validate-suite.sh on macOS 15.7.7 ARM64
- [ ] Isolate: run solver-nested-metadata fixture standalone
- [ ] Fix: rewrite parameter expansion to Bash 3.2 compatible form
- [ ] Verify: validate-suite.sh exits 0 on macOS
- [ ] Evidence: capture CI run with macOS job green
- [ ] Contract test: assert exit 0 on all required platforms
```

## Platform Support

The suite targets:

- **Linux**: Bash 3.2+, case-sensitive filesystem, POSIX permissions
- **macOS**: `/bin/bash` 3.2 (system default), BSD userland, native filesystem
- **Windows**: PowerShell 5.1+ or PowerShell 7, path separators, line endings
- **WSL**: Linux semantics for filesystem/process, Windows boundary for interop

**Platform parity**: Not considered verified until all required platforms have native runtime Evidence. Unsupported OS is documented with reason and missing runner details.

## Validation Commands

### Structural Validation

**Linux / macOS:**
```bash
bash .agents/validate-suite.sh
# Expected output:
# ✓ Validating SKILL.md structure...
# ✓ mino-core: valid
# ✓ mino-problem-framing: valid
# [... all skills ...]
# ✓ All Skills passed validation
# Exit code: 0
```

**Windows PowerShell:**
```powershell
pwsh .agents/validate-suite.ps1
# Expected output:
# [✓] Validating SKILL.md structure...
# [✓] mino-core: valid
# [✓] mino-problem-framing: valid
# [... all skills ...]
# [✓] All Skills passed validation
# Exit code: 0
```

### Fixture Runner (if available)

```bash
bash .agents/run-fixtures.sh
# Executes oracle test cases, captures Evidence
```

## Configuration

### Skill Triggers

Each Skill defines 6-8 natural language triggers in its YAML frontmatter:

```yaml
triggers:
  - apply mino problem framing
  - frame this problem using mino
  - check problem definition
```

Your AI coding agent matches these triggers to user requests.

### Exclusions and Gates

Every Skill declares:

- **Exclusions**: What it does NOT handle
- **Hard Gates**: Conditions that must pass before proceeding

Example from `mino-problem-framing`:

```yaml
exclusions:
  - Does not write implementation code
  - Does not choose technical solutions
  - Does not approve requirements (human authority required)

hard_gates:
  - User has provided initial request or requirement text
  - Observation and assumption are separated
  - Problem statement exists before proposing solutions
```

Agents must respect these constraints.

## Environment Variables

The suite itself does not require secrets. If you integrate with external tools:

```bash
# Example: If extending with API-based validation
export MINO_VALIDATOR_API_KEY="your-key-here"  # Don't commit!

# Example: If publishing Evidence to remote storage
export EVIDENCE_UPLOAD_TOKEN="your-token"      # Use secrets manager
```

**Never commit secrets.** Use environment variables or secret managers.

## Troubleshooting

### Validation Fails on macOS

**Symptom**: `validate-suite.sh` exits 2 on macOS, passes on Linux.

**Cause**: Bash 3.2 compatibility issue (macOS ships with Bash 3.2 from 2007).

**Fix**:
1. Check if error mentions array subscript or parameter expansion
2. Rewrite using Bash 3.2 compatible syntax (no `[[` with `=~`, no associative arrays)
3. Test on macOS before pushing

**Workaround**: Use Docker with Linux bash:
```bash
docker run --rm -v $(pwd):/work -w /work bash:5 bash .agents/validate-suite.sh
```

### Skill Not Triggering

**Symptom**: Agent doesn't recognize your request as matching a Skill.

**Fix**:
1. Check `.agents/skills/*/SKILL.md` frontmatter `triggers` field
2. Use one of the documented trigger phrases exactly
3. If AI agent caches Skills, reload/restart agent

**Example**: Instead of "help me design", say "apply mino problem framing to this feature".

### Multiple Skills Activate

**Symptom**: Agent tries to apply all design Skills at once.

**Fix**: Use **Skill Composition** rules in your `AGENTS.md`:

```markdown
# Skill Composition

When a request matches multiple Skills:

- Use the Skill that best matches the primary outcome as the basic workflow.
- Add only relevant Skills to supplement that workflow.
- Let the basic Skill control scope, changes, validation, and response.
```

Then be specific: "Use only mino-problem-framing for this request".

### Evidence Not Generated

**Symptom**: CI passes but no Evidence files appear.

**Fix**:
1. Check if `.agents/evidence/` directory exists
2. Check if runner has write permission
3. Check if CI workflow captures artifacts:

```yaml
# GitHub Actions example
- name: Upload Evidence
  uses: actions/upload-artifact@v3
  with:
    name: validation-evidence
    path: .agents/evidence/
```

### Contract Test Fails After Refactor

**Symptom**: You changed implementation, now contract tests fail.

**Fix**:
1. **Don't change the contract** unless requirements changed
2. If requirements did change: update Problem Frame first, then contract, then implementation
3. If requirements didn't change: your refactor broke the contract—revert or fix

**Principle**: Refactor preserves external contract, even if internal structure changes completely.

## Code Examples

### Example: Validating a New Skill File

**Add a new Skill:**

```bash
mkdir -p .agents/skills/my-new-skill
cat > .agents/skills/my-new-skill/SKILL.md <<'EOF'
---
name: my-new-skill
description: Does something useful
triggers:
  - apply my new skill
  - use my skill
---

# my-new-skill

## Purpose
[what this skill achieves]

## Scope
[what it covers]

## Exclusions
[what it does not do]

## Gates
[required conditions]

## Workflow
[step-by-step process]

## Evidence
[how to verify]
EOF
```

**Validate structure:**

```bash
bash .agents/validate-suite.sh
# Should show:
# ✓ my-new-skill: valid
```

**If validation fails:**

```bash
# Check specific file
bash -x .agents/validate-suite.sh 2>&1 | grep my-new-skill
# Look for parse errors, missing sections
```

### Example: Problem Framing in Shell Script Context

**User request:** "Add color output to validation script"

**Agent applies `mino-problem-framing`:**

```markdown
# Problem Framing Package

## Observation
- validate-suite.sh outputs plain text: "✓ skill: valid"
- Errors are mixed with successes in STDOUT/STDERR
- Users must read full output to find failures

## Assumption
- Users have color-capable terminals (TERM=xterm-256color or similar)
- Color improves scan speed for pass/fail detection

## Problem
Users spend extra time visually scanning validation output to find which Skill failed.

## Purpose
Users immediately see pass/fail status without reading full output.

## Success Criteria
- Green ✓ for pass, red ✗ for fail
- Passes validation on all required platforms (Linux, macOS, Windows PowerShell)
- Degrades gracefully on non-color terminals

## Context Packet
- Stakeholder: Skill authors running local validation
- Constraint: Bash 3.2 compatibility (macOS), PowerShell 5.1 compatibility (Windows)
- Risk: ANSI codes break on some CI runners (must detect and disable)
- Dependency: Requires terminal capability detection
```

**Then proceed to Contract (not shown here, would specify color output contract) and Implementation.**

### Example: Domain Model Completeness for Validation Suite

**Agent applies `mino-domain-model-completeness`:**

```markdown
# Completeness Package

## Concepts
- **Skill**: name, description, triggers, sections (Purpose, Scope, Exclusions, Gates, Workflow, Evidence)
- **Platform**: OS (Linux, macOS, Windows), shell (bash, pwsh), runner (native, Docker, WSL)
- **Evidence**: platform, runner, exit code, timestamp, artifacts (logs, coverage)
- **MISSING**: Baseline (approved frozen version for regression comparison)

## States
- Skill: draft → structural-valid → behavioral-verified → stable-release
- Evidence: not-run → executed → pass → fail
- **MISSING**: deprecated state (for sunset Skills)

## Constraints
- Invariant: Every Skill has exactly one SKILL.md
- Invariant: Evidence timestamp is immutable after creation
- **MISSING**: Maximum number of triggers per Skill (could affect performance)

## Failures
- validate-suite.sh parse error: exits 1, STDERR shows file+line
- validate-suite.sh structural error: exits 2, STDERR shows missing section
- **MISSING**: Partial fixture failure (does one bad oracle stop others? No guarantee documented)

## Authority
- Maintainer: can merge to main, approve stable release
- Evaluation owner: can freeze versioned case for regression
- Contributor: can propose Skill, cannot approve
- **MISSING**: Who can add new required platform? (Policy gap)

## Writer / Reader Coverage
- **Skill author** writes SKILL.md → **Validator** reads → **CI** reads → **Agent runtime** reads
- **CI runner** writes Evidence/ → **Release gatekeeper** reads
- **MISSING**: Who owns mino-doc/ updates? (Reference material lifecycle unclear)
```

**Action**: Add missing concepts (Baseline), states (deprecated), constraints (trigger limit), failure guarantees (fixture isolation), authority (platform approval policy), and data ownership (mino-doc lifecycle).

### Example: Design by Contract for Color Output

**Agent applies `mino-design-by-contract`:**

```markdown
# Contract Package

## Operation: validate-suite.sh (with color output)

### Preconditions
- Bash 3.2+ available (Linux, macOS)
- .agents/skills/ directory contains at least one SKILL.md
- TERM environment variable exists (may be empty)

### Postconditions (Normal)
- Exit code 0
- STDOUT contains "✓" (green) or "✗" (red) for each Skill
- If TERM is empty or "dumb", fallback to plain text
- All SKILL.md files remain unmodified

### Postconditions (Exceptional)
- Exit 1 on YAML parse error: STDERR shows file+line, no color (avoid confusion)
- Exit 2 on missing section: STDERR shows Skill+section, no color
- Partial progress printed before failure (basic guarantee: see which Skills passed before error)

### Invariants
- Script never writes to .agents/skills/
- Color codes only emitted if terminal is capable (detected via TERM or tput)

### Contract Tests

```bash
#!/bin/bash
# Test: precondition—Bash 3.2 works
bash --version | grep -q "version [3-9]"
assert_exit_code 0

# Test: postcondition normal—exit 0 on valid Skills
bash .agents/validate-suite.sh >/tmp/out.txt 2>&1
assert_exit_code 0
grep -q "✓" /tmp/out.txt  # At least one pass symbol

# Test: postcondition normal—green color on capable terminal
export TERM=xterm-256color
bash .agents/validate-suite.sh | grep -q $'\033\[32m'  # ANSI green
assert_exit_code 0

# Test: postcondition normal—plain text on dumb terminal
export TERM=dumb
bash .agents/validate-suite.sh >/tmp/plain.txt
assert_exit_code 0
! grep -q $'\033' /tmp/plain.txt  # No ANSI codes

# Test: postcondition exceptional—exit 1 on parse error, no color in STDERR
echo "bad: [yaml" > .agents/skills/bad/SKILL.md
bash .agents/validate-suite.sh 2>/tmp/err.txt
assert_exit_code 1
! grep -q $'\033' /tmp/err.txt  # Error messages are plain

# Test: invariant—files unmodified
checksum_before=$(md5sum .agents/skills/*/SKILL.md | sort)
bash .agents/validate-suite.sh >/dev/null 2>&1
checksum_after=$(md5sum .agents/skills/*/SKILL.md | sort)
assert_equal "$checksum_before" "$checksum_after"
```
```

**Implementation** (not shown) would add color functions, terminal detection, and color-safe error handling.

## Integration with AI Coding Agents

### Claude Code / Cursor / Copilot

1. **Install Skill suite** in your project:
   ```bash
   git submodule add https://github.com/my-take-dev/inspired-mino-design-skills.git .agents/skills/mino
   ```

2. **Configure agent** to load Skills from `.agents/skills/mino/.agents/skills/*/SKILL.md`.

3. **Trigger in conversation**:
   - "Apply mino problem framing to this feature request"
   - "Check domain model completeness using mino skill"
   - "Use mino reproducible development workflow for this refactor"

4. **Agent will**:
   - Match trigger to Skill
   - Follow Skill's Workflow section
   - Respect Exclusions and Gates
   - Produce structured artifacts (Problem Frame, Contract, etc.)

### Custom Agent Configuration

If your agent supports
