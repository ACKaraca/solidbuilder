# 0005 Stable modeling API and canonical text form of the feature history

- Status: Accepted
- Deciders: maintainers of this repository

## Context

Design automation engineers keep part libraries in version control, review changes as text and
regenerate models in CI. The suite already requires every change to be a command reachable through the
Arc Automation Protocol. What is missing is a stable contract for scripts and a textual representation
of models that is readable and diffable.

## Decision

1. The modeling commands and queries exposed over the automation protocol form a versioned API with
   semantic versioning; within a major version, changes are additive, and deprecated calls keep working
   for at least one further major version.
2. Each component definition has a canonical text form: a declarative listing of parameters, sketches,
   constraints, features and their inputs in history order, using stable IDs and persistent reference
   expressions. Serializing an unchanged model always yields identical text; reading the text yields the
   same model and the same persistent names.
3. The text form is an exchange and review format. The SQLite document remains the native storage;
   the text form is not a scripting language and contains no control flow. Generative logic lives in
   scripts and rules that emit commands.

## Consequences

- The text grammar is versioned with the API and published in this repository when designed.
- Every feature type must define its text serialization; round-trip tests cover each type.
- Users can review and diff designs without the application, and build documents from source in CI.

## Alternatives considered

- Making the text form the native file: loses the document package guarantees (assets, journal,
  partial loading, recovery).
- A full modeling language with control flow: duplicates scripting and makes the round trip from
  interactive edits lossy.
- No text form, scripts only: models made interactively could not be reviewed as text.

## Trigger to reopen

Round-trip fidelity cannot be maintained for a class of features (for example plug-in features or
freeform bodies), or users require control flow in the text form often enough to justify a language.
