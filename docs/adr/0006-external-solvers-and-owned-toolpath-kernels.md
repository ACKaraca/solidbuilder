# 0006 External open solvers, owned toolpath kernels and a documented post-processor format

- Status: Accepted
- Deciders: maintainers of this repository

## Context

Simulation needs mature finite element solvers and meshers; the open ones that cover static, modal,
thermal, buckling and nonlinear analysis are file-driven programs under copyleft licenses that cannot
be linked into an Apache-2.0 application. Manufacturing preparation needs toolpath generation that is
fast, multi-threaded and integrated with the model and its persistent references, and post-processors
that users can read and adapt for their machines.

## Decision

1. **Solvers and meshers run as external processes.** SolidBuilder writes solver input decks and
   reads result files through adapters in `solidbuilder-sim`. Solver executables are installed
   separately or provided by an Arc Hub worker; they are never linked. Every adapter declares the solver
   versions it was validated against, and no core modeling workflow depends on a solver being present.
2. **Toolpath kernels are owned.** Toolpath strategies, stock simulation and collision checking are
   implemented in Rust in `solidbuilder-cam` on top of `arc-geom` and `arc-mesh`, running headless on
   worker threads and on Hub workers.
3. **Post-processors are documented and sandboxed.** A machine definition is a declarative,
   versioned data file (kinematics, limits, controller dialect). A post-processor is a WebAssembly
   module that receives a documented intermediate toolpath representation and returns NC text, with no
   file system or network access. The intermediate format and the post-processor interface are
   specified in this repository.

## Consequences

- Solver availability and licensing are the user's or the Hub operator's choice; the product reports
  when a solver is missing.
- Toolpath correctness is verified by owned tests (stock simulation against reference geometry) rather
  than by a third-party engine.
- Post-processors written in any language that compiles to WebAssembly can be shared safely.

## Alternatives considered

- Linking solver libraries: license incompatibility and crash coupling.
- Embedding an existing open CAM engine: licensing, threading model and associativity with persistent
  references do not fit.
- Script-language post-processors with host access: harder to sandbox; rejected by the suite plug-in
  model.

## Trigger to reopen

A permissively licensed solver with the required coverage becomes available as a library, or the
WebAssembly post-processor interface cannot express a controller dialect that users need.
