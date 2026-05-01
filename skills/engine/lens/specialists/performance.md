# Performance Specialist Checklist

Performance review for latency, memory, query efficiency, and caching.

---

## Checklist

### Database Queries

- [ ] No N+1 query patterns (separate queries in loops instead of batch fetches)
- [ ] Queries use appropriate indexes (check EXPLAIN plans for new queries)
- [ ] No full table scans on large tables
- [ ] Pagination is implemented for all list endpoints (no unbounded result sets)
- [ ] Database connections are properly pooled and released
- [ ] Transactions cover the minimum necessary scope (no long-running locks)

### Memory Usage

- [ ] No loading entire datasets into memory when streaming or pagination is possible
- [ ] Large objects are not held in memory longer than necessary
- [ ] No memory leaks from event listeners, subscriptions, or closures that capture large scope
- [ ] Buffers and streams are properly cleaned up after use
- [ ] Caches have size limits and eviction policies (no unbounded caches)

### Computational Efficiency

- [ ] Loops are not doing redundant work inside iterations (move invariants out)
- [ ] Sorting and filtering happen at the database level, not in application code
- [ ] Expensive operations (cryptography, compression, image processing) are not repeated for the same input
- [ ] Algorithm complexity is appropriate for the expected data size (no O(n^2) on large inputs)
- [ ] Expensive computations are deferred until actually needed (lazy evaluation where applicable)

### Network and I/O

- [ ] No synchronous file I/O in request handling paths
- [ ] External API calls have reasonable timeouts set
- [ ] Redundant API calls are batched or deduplicated
- [ ] Responses are compressed for large payloads
- [ ] Static assets use appropriate caching headers

### Caching Strategy

- [ ] Frequently accessed, rarely changing data is cached
- [ ] Cache invalidation is handled correctly when source data changes
- [ ] Cache keys are deterministic and collision-free
- [ ] Stale-while-revalidate or fallback strategies are in place for cache misses
- [ ] No caching of user-specific data under shared keys

---

## Report Format

```
PERFORMANCE REVIEW
==================

Status: [PASS | CONDITIONAL | FAIL]

Findings:
---
[PERF-1] [CRITICAL] [file:line] [Category] Description
  Impact: [estimated latency/memory cost]
  Optimization: [specific change recommendation]

[PERF-2] [IMPORTANT] [file:line] Description
  Impact: [estimated cost under load]
  Optimization: [specific change recommendation]
---

Summary: [1-2 sentences on the performance characteristics of this changeset]
```
