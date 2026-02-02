---
name: proximity-principles
description: Use when writing or reviewing code to keep related logic, decisions, and context close together
---

# Proximity Principles

## Overview

Aggressive proximity fights the natural entropy of code organization. Related logic drifts apart, context gets lost, decisions become mysteries. These 10 principles keep everything that changes together, together.

## The Principles

### 1. Decision Archaeology

Document *why* at the implementation point. Include rationale, alternatives considered, and measurements.

```
// Bad
cache.setTTL(3600)

// Good
// DECISION: 1-hour cache TTL for user data
// Why: Balance between freshness and API load
// Alternative: 5-min TTL rejected — 10x API cost increase
// Measured: Reduces API calls by 85%
USER_CACHE_TTL = 3600
```

### 2. Three-Strikes Abstraction

Copy-paste is fine for the first two occurrences. Abstract on the third. Premature abstraction is worse than duplication.

```
// Occurrence 1: write it inline
// Occurrence 2: copy it (yes, really)
// Occurrence 3: NOW extract a shared function
```

### 3. Test Colocation

Test files live next to source files, not in a separate test tree.

```
// Bad
src/services/payment.js
test/services/payment.test.js

// Good
services/payment.js
services/payment.test.js
```

### 4. Error Context at Source

Handle errors where they're understood. Generic catch-all messages are an anti-pattern.

```
// Bad
if err: log("error occurred")

// Good
// ERROR_CONTEXT: DB query failure in user lookup
// Common cause: Connection pool exhausted during peak
raise Error("user lookup failed (query={sql}): {err}")
```

### 5. Configuration Near Usage

Constants live next to the code that uses them, with a comment explaining the choice.

```
// Bad — all unrelated config in one file
database.pool_size: 50
api.rate_limit: 100
cache.ttl: 3600

// Good — in the database module
// DECISION: Pool size 50 — handles 99th percentile (measured: 47 concurrent)
DB_POOL_SIZE = 50
```

### 6. Behavior Over Structure

Group files by feature/domain, not by technical layer.

```
// Bad
src/controllers/
src/models/
src/validators/

// Good
src/user-authentication/
src/payment-processing/
```

### 7. Temporal Proximity

Code that changes together should live together. If two files always appear in the same commits, they belong closer.

### 8. Security Annotations

Document security mitigations at the enforcement point.

```
// SECURITY: DOMPurify with strict whitelist to prevent XSS
// Threat: User-supplied HTML in comments
// Tested: OWASP XSS test vectors
sanitized = purify(input, allowedTags: ['b', 'i', 'em'])
```

### 9. Performance Annotations

Document performance tradeoffs at the implementation point with benchmarks.

```
// PERFORMANCE: Vectorized operation for 100x speedup
// Benchmark: Loop = 2.3s, vectorized = 0.023s for 1M elements
// Tradeoff: 50MB additional memory
```

### 10. Deprecation at Source

Deprecation notices, migration paths, and removal timelines on the deprecated code itself.

```
// DEPRECATED since 2.0.0 — use processAsync() instead
// Migration: Replace process() with processAsync().await()
// Removal: Version 3.0.0 (2025-06-01)
```

## Quick Reference

| Principle | One-liner |
|-----------|-----------|
| Decision Archaeology | Document *why* at the code, not in a wiki |
| Three-Strikes | Abstract on third occurrence, not before |
| Test Colocation | `foo.test.ts` next to `foo.ts` |
| Error Context | Rich errors where the error is understood |
| Config Near Usage | Constants next to their consumers |
| Behavior Over Structure | `user-auth/` not `controllers/` |
| Temporal Proximity | Co-changing files live together |
| Security Annotations | Security rationale at enforcement point |
| Performance Annotations | Benchmarks at optimization point |
| Deprecation at Source | Migration path on the deprecated code |

## Common Mistakes

- **Over-documenting** — Not every line needs a decision comment. Focus on non-obvious choices.
- **Premature colocation** — Don't force unrelated code together. Proximity is about relationships.
- **All-or-nothing** — Incremental adoption is fine. Apply principles where they add clarity.
