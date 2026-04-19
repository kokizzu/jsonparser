# ReqProof Initial Coverage

This note records the current ReqProof coverage tranche added to `jsonparser`.

Covered stakeholder needs:

- `STK-REQ-001`: key-path lookup over JSON byte slices, including missing-path and error cases
- `STK-REQ-002`: decoded string retrieval with escapes and Unicode handling
- `STK-REQ-003`: typed numeric and boolean helper access
- `STK-REQ-004`: array/object traversal and multi-path lookup
- `STK-REQ-005`: experimental mutation through Set and Delete with deterministic edge-case behavior
- `STK-REQ-006`: unsafe raw-value retrieval through `GetUnsafeString`
- `STK-REQ-007`: raw scalar token parsing through `ParseBoolean`, `ParseFloat`, `ParseString`, and `ParseInt`

Covered system slices:

- `SYS-REQ-001`: `Get`
- `SYS-REQ-002`: `GetString`
- `SYS-REQ-003`: `GetInt`
- `SYS-REQ-004`: `GetFloat`
- `SYS-REQ-005`: `GetBoolean`
- `SYS-REQ-006`: `ArrayEach`
- `SYS-REQ-007`: `ObjectEach`
- `SYS-REQ-008`: `EachKey`
- `SYS-REQ-009`: `Set`
- `SYS-REQ-010`: `Delete`
- `SYS-REQ-011`: `GetUnsafeString`
- `SYS-REQ-012`: `ParseBoolean`
- `SYS-REQ-013`: `ParseFloat`
- `SYS-REQ-014`: `ParseString`
- `SYS-REQ-015`: `ParseInt`
- `SYS-REQ-016`: `Get` returns the defined not-found tuple for well-formed missing-path lookups
- `SYS-REQ-017`: `Get` reports parse errors for incomplete or truncated lookup input
- `SYS-REQ-018`: `Get` returns the closest complete root value when no key path is provided
- `SYS-REQ-019`: `Get` returns the defined not-found tuple for empty input with a key path
- `SYS-REQ-020`: `Get` resolves object-member path segments only within the current structural scope
- `SYS-REQ-021`: `Get` resolves valid in-bounds array-index path segments
- `SYS-REQ-022`: `Get` reports the defined not-found tuple for malformed array-index syntax
- `SYS-REQ-023`: `Get` reports the defined not-found tuple for out-of-bounds array indexes
- `SYS-REQ-024`: `Get` resolves escaped JSON object keys after decoding the path segment
- `SYS-REQ-025`: `Get` returns raw string contents without quotes and without JSON unescaping
- `SYS-REQ-026`: `Get` preserves best-effort results when malformed input is outside the addressed token
- `SYS-REQ-027`: `Get` returns a value-type error for invalid addressed token shapes

Current traced artifacts for this tranche:

- Implementation: `Get`, `GetString`, `GetInt`, `GetFloat`, `GetBoolean`, `ArrayEach`, `ObjectEach`, `EachKey`, `Set`, `Delete`, `GetUnsafeString`, `ParseBoolean`, `ParseFloat`, `ParseString`, `ParseInt`
- Verification: `TestGet`, `TestGetRequirementSlices`, `TestGetString`, `TestGetUnsafeString`, `TestGetInt`, `TestGetFloat`, `TestGetBoolean`, `TestArrayEach`, `TestArrayEachWithWhiteSpace`, `TestObjectEach`, `TestEachKey`, `TestSet`, `TestSetCreatesMissingEntryInExistingArray`, `TestDelete`, `TestParseBoolean`, `TestParseFloat`, `TestFuzzSetHarnessCoverage`, `TestFuzzParseFloatHarnessCoverage`, `TestParseString`, `TestParseInt`
