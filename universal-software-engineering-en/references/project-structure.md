# Project structure

Goal: easy to manage, bugs easy to localize, and the AI only needs to read the relevant part. Principle: once a feature is large enough, give it one directory; inside that directory, split files by responsibility, not by individual function.

If the project already has its own structure, follow it and apply this file only to new code where it makes sense.

## 1. Organize by feature

- One directory per feature once it is large enough to split. A small feature stays in one file (section 6). That directory holds its own logic, data types, data access, and tests.
- Editing or removing a feature should only touch its own directory.
- Name files by role (`service`, `models`, `repository`, `handlers`...), not by individual function name.

## 2. Do not split "one function per file"

Functions that serve the same purpose live in the same file. Splitting every function makes the file count explode, forces constant jumping around to understand one flow, and adds declaration/import overhead (especially in Rust, where each new file needs a `mod` declaration). Move a function into its own file only when it is large, complex, or shared by several features.

## 3. Warning thresholds

| Item | Warn at | Definitely split at |
|---|---|---|
| File | about 300 lines | about 500 lines |
| Function | about 50 lines | when the function does two jobs, or is about 100 lines and cannot be given one name that covers all of its work |

These are signals to take a look, not hard rules. The signs that a split is needed matter more than the numbers:
- The file does two or more unrelated things (describing it requires the word "and").
- Part of the file is used by another feature.
- Groups of functions change for different reasons (for example business logic mixed with DB access).
- You have to scroll a lot to find a function.

A 350-line file that is tightly cohesive with a single responsibility does not have to be split.

**Thresholds do not apply to:** test files, generated code, config/static data files, and entry points (`main`) that merely wire things together.

**When a threshold is exceeded:** tell the user and propose how to split (into which files, and why). Do not refactor on your own, since this is a structural change that needs confirmation first.

## 4. Rules between features

- A feature **must not import** the internals of another feature. If something is shared, move the shared part into `shared/` (or `common/`), and only when there are at least 3 real places that use it.
- If feature A must call feature B, go through a small public interface of B (a few clearly exported functions), never reach into its internal files.
- Each feature has its own tests, inside or next to its directory.
- Avoid circular dependencies (A calls B, B calls A back); if it happens, it is a sign the feature boundary is wrong.

## 5. Examples by language

The trees below are the shape of a feature that already has several different responsibilities. Do not use them as a scaffold for a new project or a still-small feature. Section 6 is how to start.

### Python
```
src/
├── features/
│   ├── auth/
│   │   ├── __init__.py      # exports the public interface only
│   │   ├── service.py       # business logic
│   │   ├── models.py        # data types
│   │   ├── repository.py    # DB access
│   │   └── test_auth.py
│   └── billing/
│       └── ...
├── shared/                  # shared code (used in 3+ places)
└── main.py
```

### Rust
```
src/
├── main.rs                  # only wires things together
├── features/
│   ├── mod.rs
│   ├── auth/
│   │   ├── mod.rs           # declares modules + pub use of the public interface
│   │   ├── service.rs
│   │   ├── models.rs
│   │   └── repository.rs
│   └── billing/
│       └── ...
└── shared/
    └── mod.rs
```
Every new file must be declared in the parent directory's `mod.rs`; remember this step each time you add a file. For a very large project, consider splitting into multiple crates in a workspace instead of adding hundreds of modules.

### TypeScript / JavaScript
```
src/
├── features/
│   ├── auth/
│   │   ├── index.ts         # public interface
│   │   ├── service.ts
│   │   ├── types.ts
│   │   └── auth.test.ts
│   └── billing/
└── shared/
```

### Go
Organize by **package**: one directory/package per feature, with several files in the same package. Do not split packages too small.
```
internal/
├── auth/
│   ├── service.go
│   ├── repository.go
│   └── service_test.go
└── billing/
```

## 6. When creating a new project

Start simple: if there are only one or two small features, a flat directory is enough. Move to the feature-based structure when the project grows clearly separate parts. Do not scaffold empty directory skeletons for things that do not exist yet.
