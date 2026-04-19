# Deep Specification Hardening 2026-04-14

This note documents the deep-spec requirement decomposition covering truncation safety, sentinel handling, typed getter edge cases, and boundary-value correctness for the jsonparser library.

## Get Truncation Safety

- `SYS-REQ-041`: Get returns error for input truncated at a value boundary (e.g., `{"a":1` with no closing brace) without panicking.
- `SYS-REQ-042`: Get returns error for input truncated mid-structure (e.g., unclosed objects/arrays) without panicking.
- `SYS-REQ-043`: Get returns error for input truncated mid-key (e.g., `{"a` with unterminated key string) without panicking.

## Internal Sentinel Handling

- `SYS-REQ-044`: tokenEnd sentinel boundary -- all callers treat len(data) return as end-of-input, not as an unchecked index.
- `SYS-REQ-045`: stringEnd sentinel boundary -- all callers treat -1 return as malformed-string condition.
- `SYS-REQ-046`: blockEnd sentinel boundary -- all callers treat -1 return as malformed-structure condition.

## Negative Array Index

- `SYS-REQ-047`: Get returns not-found for negative array indexes (e.g., `[-1]`) because negative indexing is not supported.

## Delete Robustness

- `SYS-REQ-048`: Delete returns original input unchanged for input truncated at a value boundary, without panicking.
- `SYS-REQ-049`: Delete uses internalGet errors to short-circuit to safe fallback rather than discarding them.
- `SYS-REQ-050`: Delete returns original input unchanged for truncated array input, without panicking.
- `SYS-REQ-056`: Delete returns original input unchanged for mid-structure truncated input, without panicking.

## Set Robustness

- `SYS-REQ-051`: Set returns error for truncated input rather than producing corrupt output or panicking.
- `SYS-REQ-068`: Set returns error when path resolves beyond EOF.
- `SYS-REQ-069`: Set correctly creates missing nested structure for multi-level paths.
- `SYS-REQ-070`: Set returns KeyPathNotFoundError when called without any key path.

## ArrayEach Error Handling

- `SYS-REQ-052`: ArrayEach propagates element-level Get errors to the caller.
- `SYS-REQ-053`: ArrayEach returns error for truncated array elements without panicking.
- `SYS-REQ-055`: ArrayEach returns MalformedArrayError for malformed delimiters between elements.
- `SYS-REQ-083`: ArrayEach returns error for input truncated at value boundary without panicking.

## ObjectEach Error Handling

- `SYS-REQ-054`: ObjectEach returns error for truncated object entries without panicking.
- `SYS-REQ-084`: ObjectEach returns error for mid-structure truncated input without panicking.

## EachKey Sentinel Handling

- `SYS-REQ-085`: EachKey treats tokenEnd sentinel as end-of-input and returns safely.

## ParseBoolean Edge Cases

- `SYS-REQ-057`: ParseBoolean returns MalformedValueError for partial literals like "tru" or "fals".
- `SYS-REQ-066`: ParseBoolean returns MalformedValueError for empty input.

## ParseInt Boundary Values

- `SYS-REQ-058`: ParseInt returns correct value at exact int64 boundaries (max 9223372036854775807, min -9223372036854775808).
- `SYS-REQ-059`: ParseInt returns OverflowIntegerError for values exactly one beyond int64 range.
- `SYS-REQ-064`: ParseInt returns MalformedValueError for empty input.

## ParseFloat Edge Cases

- `SYS-REQ-065`: ParseFloat returns MalformedValueError for empty input.

## ParseString Edge Cases

- `SYS-REQ-060`: ParseString returns MalformedValueError for truncated escape sequences.
- `SYS-REQ-061`: ParseString returns MalformedValueError for high surrogate without valid low surrogate.
- `SYS-REQ-062`: ParseString returns MalformedValueError for high surrogate followed by sub-range low surrogate.
- `SYS-REQ-063`: ParseString returns MalformedValueError for string ending with lone backslash.
- `SYS-REQ-067`: ParseString returns empty string without error for empty input.

## GetString Edge Cases

- `SYS-REQ-071`: GetString propagates Get errors for malformed input.
- `SYS-REQ-072`: GetString returns error for truncated escape sequences in addressed values.
- `SYS-REQ-073`: GetString returns type-mismatch error for non-string values.
- `SYS-REQ-074`: GetString returns not-found for empty input.

## GetInt Edge Cases

- `SYS-REQ-075`: GetInt propagates Get errors for malformed input.
- `SYS-REQ-076`: GetInt returns overflow error for values exceeding int64 range.
- `SYS-REQ-077`: GetInt returns type-mismatch error for non-number values.
- `SYS-REQ-078`: GetInt returns not-found for empty input.

## GetBoolean Edge Cases

- `SYS-REQ-079`: GetBoolean returns error for partial boolean literals due to truncation.

## GetUnsafeString Edge Cases

- `SYS-REQ-080`: GetUnsafeString propagates Get errors for malformed input.
- `SYS-REQ-081`: GetUnsafeString returns not-found for empty input.
- `SYS-REQ-082`: GetUnsafeString propagates error for input truncated at value boundary.
