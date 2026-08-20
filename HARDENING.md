<!-- markdownlint-disable -->

# Hardening Report: rui314--setup-mold/v1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **rui314--setup-mold/v1** was hardened automatically. 6 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The composite action's run: block directly interpolates ${{ inputs.mold-version }} and ${{ inputs.make-default }} inside shell command strings (rule a). This allows an attacker who controls these inputs to inject arbitrary shell commands. Offending lines:
- Line 15: echo "mold ${{ inputs.mold-version }}"
- Line 17: wget ... v${{ inputs.mold-version }}/mold-${{ inputs.mold-version }}-... | $SUDO tar ...
- Line 18: test ${{ inputs.make-default }} = true ...
Fix: move inputs into env: variables and reference them as quoted shell variables (e.g., "$MOLD_VERSION").

Locations:

- `action.yml:15`
- `action.yml:17`
- `action.yml:18`

### unpinned-uses (severity: high)

The workflow uses actions/checkout@v2, which is pinned to a mutable version tag rather than an immutable 40-character commit SHA. A tag can be moved to point to a different (potentially malicious) commit, enabling supply-chain attacks. Fix: pin to a full SHA, e.g. actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v2

Locations:

- `.github/workflows/update-version.yml:16`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.mold-version }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:15`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.mold-version }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:17`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.mold-version }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:17`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.make-default }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:18`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, unpinned-uses

**Notes:**

1. action.yml: Added an env: block to the composite step with MOLD_VERSION=${{ inputs.mold-version }} and MAKE_DEFAULT=${{ inputs.make-default }}. Replaced all direct ${{ inputs.* }} interpolations in the run: block with quoted shell variable references ($MOLD_VERSION, $MAKE_DEFAULT, ${MOLD_VERSION}). Also fixed the unsafe wget | tar pipe by downloading to a mktemp file first, then extracting separately, then removing the temp file. 2. .github/workflows/update-version.yml: Pinned actions/checkout@v2 to its full commit SHA (0717577d45739eb3c851188b29f50ed6c0b2194e) with a # v2 comment.

