# 0004 One solver for joints and assembly constraints

- Status: Accepted
- Deciders: maintainers of this repository

## Context

Users relate occurrences either with joints (kinematic pairs with limits) or with geometric
constraints (mate, flush, angle). Systems that solve the two with separate engines produce
order-dependent results and confusing failures when both are present. `arc-solve` provides a 3D
geometric constraint solver.

## Decision

Joints, assembly constraints, rigid groups and motion couplings are translated into one system of
equations over occurrence transforms and joint variables and solved by `arc-solve`. The system is
decomposed into independent clusters (rigid subassemblies, flexible subassemblies, disconnected sets)
that are solved in parallel. On failure, the solver returns a minimal conflicting set of relations.
Solving is deterministic for a given document revision and independent of creation order.

## Consequences

- Any mix of joints and constraints behaves predictably.
- `arc-solve` must support assembly-scale problems, cluster decomposition and conflict extraction;
  SolidBuilder contributes these requirements to `arc-core`.
- Kinematic drives, contact sets and motion studies use the same solver state.

## Alternatives considered

- Joints only: insufficient for many existing workflows and imported assemblies.
- Separate joint and constraint engines: order dependence and duplicated failure handling.

## Trigger to reopen

Assembly solve time for the large-assembly performance target is not met because a single general
system cannot exploit joint structure, and a specialized joint solver shows a clear gain.
