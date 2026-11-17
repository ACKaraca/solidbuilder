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
   worker threads and on Hub workers. Multi-axis strategies and slicing may first be delivered
   through an adapter around an external open engine and then follow decision 4.
3. **Post-processors are documented and sandboxed.** A machine definition is a declarative,
   versioned data file (kinematics, limits, controller dialect). A post-processor is a WebAssembly
   module that receives a documented intermediate toolpath representation and returns NC text, with no
   file system or network access. The intermediate format and the post-processor interface are
   specified in this repository.
4. **Native kernels replace wrapped engines.** Finite-element solvers, meshers, slicers and
   multi-axis toolpath engines that are first delivered through an external engine are replaced by
   native Rust kernels in `solidbuilder-sim` and `solidbuilder-cam` once a kernel passes the same
   validation suites as the engine it replaces, with results inside the stated tolerances. The adapter
   interface and the user-visible functions stay the same; the roadmap lists the order.

## Consequences

- Solver availability and licensing are the user's or the Hub operator's choice; the product reports
  when a solver is missing.
- Toolpath correctness is verified by owned tests (stock simulation against reference geometry) rather
  than by a third-party engine.
- A native kernel is accepted only against the validation suites of the engine it replaces; until then the
  wrapped engine stays the default.
- Post-processors written in any language that compiles to WebAssembly can be shared safely.

## Alternatives considered

- Linking solver libraries: license incompatibility and crash coupling.
- Embedding an existing open CAM engine: licensing, threading model and associativity with persistent
  references do not fit.
- Script-language post-processors with host access: harder to sandbox; rejected by the suite plug-in
  model.

## Trigger to reopen

A permissively licensed solver with the required coverage becomes available as a library, a native
kernel cannot reach the validation results of its wrapped engine, or the
WebAssembly post-processor interface cannot express a controller dialect that users need.
