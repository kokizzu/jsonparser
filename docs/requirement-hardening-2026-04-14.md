# Requirement Hardening 2026-04-14

This note documents the requirement decomposition added during the April 14, 2026 hardening pass.
It exists as explicit documentation evidence for the new requirement rows introduced after reviewing mutation robustness, traversal edge cases, and parse error classes.

## ArrayEach

- `SYS-REQ-028`: well-formed empty arrays produce no callbacks.
- `SYS-REQ-029`: malformed or otherwise unusable array input returns an error.

## ObjectEach

- `SYS-REQ-030`: well-formed empty objects produce no entry callbacks.
- `SYS-REQ-031`: malformed or otherwise unusable object input returns an error.
- `SYS-REQ-032`: callback errors are propagated back to the caller.

## Delete

- `SYS-REQ-033`: deleting an existing addressed target removes that target from usable input.
- `SYS-REQ-034`: deleting a missing addressed target in otherwise usable input preserves the original payload.
- `SYS-REQ-035`: malformed, truncated, or otherwise unusable input preserves the original payload and does not panic.

## Parse Helpers

- `SYS-REQ-036`: ParseBoolean returns the documented malformed-value error for invalid boolean tokens.
- `SYS-REQ-037`: ParseFloat returns the documented malformed-value error for malformed numeric tokens.
- `SYS-REQ-038`: ParseString returns the documented malformed-value error for malformed encoded string literals.
- `SYS-REQ-039`: ParseInt returns the documented overflow error for out-of-range integer tokens.
- `SYS-REQ-040`: ParseInt returns the documented malformed-value error for malformed non-overflow tokens.

## Why These Rows Exist

The hardening goal was to stop hiding distinct externally visible behaviors inside umbrella requirements.
The new rows separate:

- empty success cases from non-empty success cases
- malformed-input failure from normal success behavior
- missing-target behavior from unusable-input robustness
- overflow from malformed-token rejection

That structure matches the verification model used elsewhere in this repo and makes the behavior easier to review, test, and audit.
