# Architecture Specialist Checklist

Review for structural soundness, coupling, extensibility, and design principles.

---

## Checklist

### SOLID Principles

- [ ] **Single Responsibility:** Each module/class has one reason to change
- [ ] **Open/Closed:** New behavior is added through extension, not modification of existing code
- [ ] **Liskov Substitution:** Subtypes can replace their base types without breaking behavior
- [ ] **Interface Segregation:** Consumers depend only on the interfaces they actually use
- [ ] **Dependency Inversion:** High-level modules do not depend on low-level modules; both depend on abstractions

### Coupling and Cohesion

- [ ] Modules are loosely coupled — changes in one module do not force changes in others
- [ ] Modules are highly cohesive — elements within a module are strongly related
- [ ] Dependencies point inward (core logic does not depend on infrastructure)
- [ ] No circular dependencies between modules
- [ ] Shared state is minimized; communication through well-defined interfaces

### Extensibility

- [ ] New features can be added without modifying existing code (or with minimal modification)
- [ ] Configuration-driven behavior replaces hardcoded decisions where appropriate
- [ ] Plugin or strategy patterns are used for varying behavior (not if/else chains)
- [ ] Public APIs are stable; internal implementation details are hidden
- [ ] Future requirements mentioned in the charter are not precluded by current design

### Error Architecture

- [ ] Error types are meaningful and distinguishable (not all generic Error)
- [ ] Error propagation follows a consistent pattern across the changeset
- [ ] Boundary layers (API edge, database edge) translate internal errors to appropriate external representations
- [ ] Retry logic is centralized, not duplicated per call site
- [ ] Circuit breakers or fallbacks exist for external service calls

### Testability

- [ ] Business logic can be tested without infrastructure (database, network, filesystem)
- [ ] Dependencies are injectable, enabling test doubles
- [ ] Test fixtures are straightforward to construct (no 10-step setup for a simple test)
- [ ] Side effects are isolated and observable in tests
- [ ] Integration tests cover critical paths; unit tests cover logic

---

## Report Format

```
ARCHITECTURE REVIEW
===================

Status: [PASS | CONDITIONAL | FAIL]

Findings:
---
[ARCH-1] [CRITICAL] [file:line] [Principle] Description
  Violation: [which principle and how]
  Remediation: [structural change needed]

[ARCH-2] [IMPORTANT] [file:line] Description
  Concern: [what degrades over time if unchanged]
  Remediation: [refactoring direction]

[ARCH-3] [SUGGESTION] [file:line] Description
  Observation: [pattern noticed]
  Opportunity: [potential improvement]
---

Summary: [1-2 sentences on the architectural quality of this changeset]
```
