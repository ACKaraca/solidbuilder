# Feature history

This document specifies how SolidBuilder records, orders and regenerates features, names topology
persistently, reports and repairs failures, records direct edits, branches and merges history, and
rebuilds on many threads. The user-facing behavior is in
[solid features](../product/capabilities/03-solid-features.md).

## The feature tree

Each component definition with history enabled owns one feature history: an ordered list of entries.
An entry is a sketch, a reference geometry item, a feature, a feature group boundary or a direct edit.
Entries are typed objects in the `arc-doc` store with stable IDs; the history holds their order and the
rollback marker position.

| Concept | Rule |
|---|---|
| Order | The list order is the evaluation order. An entry may use only outputs of entries before it, parameters, and geometry of other components through cross-component references. |
| Inputs | Every input is explicit: a parameter expression, a persistent reference, a sketch profile reference or an explicit geometric query. There are no hidden inputs such as "the last created body". |
| Outputs | A feature produces or modifies bodies and declares the persistent name of every face, edge and vertex it creates or modifies. |
| Rollback marker | Entries after the marker are not evaluated and their outputs are absent. New entries are inserted at the marker. |
| Groups | A feature group is a named contiguous range; groups nest and can be suppressed as a whole. |
| Suppression | A suppressed entry is skipped. Entries that depend on its outputs fail with a named missing input. |
| Base feature | An imported or history-free body snapshot that starts or continues a history. |

The model tree shows the history together with bodies, sketches and reference geometry; the compact
history bar shows the same list in order.

## Regeneration order and dependency tracking

The `arc-doc` dependency graph holds one node per entry, parameter, occurrence, assembly cluster,
drawing view, table, toolpath and study. Edges come from declared inputs.

1. When a transaction modifies an object, its node and every node reachable from it are marked dirty.
2. Inside one history, the order constraint adds an implicit edge from each entry to the next entry
   that consumes the same body. Entries that act on different bodies and do not share inputs have no
   edge between them and can evaluate concurrently.
3. A dirty entry evaluates only after all its inputs are clean for the current revision.
4. Evaluation results (bodies, naming tables, tessellation) are cached per entry with the revision and
   a hash of the inputs. If the input hash is unchanged after an upstream edit (for example a
   parameter changed back), the cached result is reused and downstream entries stay clean.
5. A change to a component definition marks occurrences of it dirty in every open assembly; external
   references in closed documents are marked out of date when those documents open.

| Change | Recomputes |
|---|---|
| Parameter value | Entries whose expressions read it, then everything downstream of them |
| Sketch geometry or constraint | Sketch solve, then features using its profiles or curves |
| Feature input | That feature and downstream entries on the same bodies or reading its outputs |
| Reorder | All entries from the earlier of the two positions |
| Rollback marker | Nothing is recomputed when moving backwards (cached results are shown); moving forwards evaluates entries that are not cached for the revision |
| Component definition | Occurrences, assembly clusters containing them, views, tables, BOM, downstream manufacturing and studies |
| Configuration selection | The component's history under the configuration's overrides, with a separate cache per configuration |

## Persistent topological naming

The decision is recorded in [ADR 0002](../adr/0002-persistent-naming.md). The naming model is:

- **Generated names.** When a feature creates topology, `arc-brep` records for each new face, edge and
  vertex the generating feature ID and the generating input: the profile curve that swept a side face,
  the cap side (start or end), the tool face that produced a cut face, or the pair of faces that
  produced an edge. The name is the tuple (feature ID, role, input names).
- **Modification history.** When an operation splits a face, both pieces keep the original name plus a
  split discriminator ordered by a stable geometric rule; when faces merge, the result records all
  source names. The kernel returns this history for every operation.
- **Signature.** Each name is stored with a geometric signature: surface or curve type, normal or axis
  direction, area or length, centroid, and the names of adjacent entities.
- **References.** A persistent reference stores the name, the signature and, for user picks, the pick
  location in the entity's parameter space.

Resolution after each regeneration:

1. Exact name match: one entity with the same name. Accept.
2. Name history: follow splits and merges; if the reference pointed to a face that was split, choose
   the piece containing the stored pick location; if several remain, the reference is ambiguous.
3. Signature match: among entities of the same type produced by the same feature, score the signature;
   accept only if the best score exceeds the threshold and the margin over the second-best exceeds a
   second threshold.
4. Otherwise the reference is **broken**.

Explicit geometric queries ("the planar face of feature F with normal +Z") are an alternative input
form for users and scripts; they resolve by evaluation rather than by name.

### How references survive upstream edits

| Upstream edit | Effect on names |
|---|---|
| Dimension change that keeps topology | All names unchanged |
| Profile gains a segment | Existing side faces keep their profile-curve names; the new segment produces a new face; downstream references are unaffected |
| Profile segment removed | Faces generated by that segment disappear; references to them break and are reported |
| Cut now splits a face | References follow the piece containing the pick location |
| Feature reordered | Names do not depend on position; they resolve if their generating features still evaluate |
| Sketch replaced by a new sketch | Names follow the mapping between old and new profile curves established by the replace command |
| Imported base feature refreshed | Names come from source identifiers where the format has them, then from signatures |

The target for the naming regression corpus is that at least 99 percent of references in scripted
upstream edits resolve to the intended entity, and that the remainder are reported broken rather than
resolved to a wrong entity. A wrong resolution is a release-blocking defect.

## Failure states and repair

| State | Meaning | Display |
|---|---|---|
| OK | Evaluated for the current revision | Normal |
| Warning | Evaluated, but with an issue: a reference resolved by signature match, a tolerance relaxed, a pattern instance skipped | Warning marker with message |
| Error | Could not evaluate: missing or broken input, kernel failure, invalid parameters | Error marker; the previous valid result stays visible in a muted style |
| Blocked | Not evaluated because an input entry is in error | Blocked marker naming the failing upstream entry |
| Out of date | Not yet evaluated for the current revision (deferred update, cancelled rebuild) | Pending marker |

Rules:

- An error never deletes downstream entries. Downstream entries with intact inputs from other sources
  still evaluate.
- Every error has a cause code and the inputs involved; the health list collects all warnings and
  errors of a document with navigation to each.
- Repair actions: edit the entry, remap a broken reference by one pick (applied to all references with
  the same broken name on request), roll back to the entry, suppress it, or delete it with a choice for
  dependents.
- A reference audit lists every reference that resolved by signature match so users can confirm them.

## Direct edits inside a history

Direct edits (push-pull, move face, delete face, replace face, change radius) on a history-enabled
component are recorded as direct-edit entries at the marker. A direct-edit entry stores the targeted
faces as persistent references and the operation parameters (offset, transform, replacement face),
plus active live rules. On regeneration it re-resolves its targets and reapplies the operation. If a
target disappears, the entry fails like any other feature. Direct-edit entries merge, reorder and
suppress like features. Components with history disabled store their bodies directly; direct edits
modify them in place and are journaled at body level.

## Branching and merging

History objects are ordinary journaled objects ([ADR 0003](../adr/0003-history-as-journal-with-feature-merge.md)).

- A branch is a line of versions starting from a base version; versions are journal ranges.
- A merge computes, for each side, the set of changed objects since the common base. Disjoint changes
  apply directly. Two changes to the same property of the same object, or incompatible order changes in
  the same history, are conflicts.
- Order conflicts are resolved by a three-way merge of the ordered lists; entries inserted on both
  sides at the same position are placed in a deterministic order and flagged for review.
- After applying the merge, the history regenerates; new failures appear in the health list of the
  merge result before it is committed.
- Conflicts keep both values; the user chooses per object. Nothing is discarded until commit.

## Multi-threaded rebuild

- The scheduler evaluates dirty nodes as soon as their inputs are clean, using a work-stealing pool.
  Independent bodies, components and configurations rebuild concurrently.
- Each evaluation is a pure function of its inputs: feature parameters, resolved references and input
  bodies. Bodies are immutable values shared between evaluations; a feature produces new bodies.
- Results carry the revision; when a newer revision exists, older results are dropped on arrival.
- Cancellation stops scheduling new work and lets in-flight evaluations finish or abort at kernel
  checkpoints; the document keeps the last complete result.
- Determinism: naming and topology must not depend on thread scheduling. Tests regenerate the corpus on
  one and many threads and compare names and topology exactly.
