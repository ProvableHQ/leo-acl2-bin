# leo-acl2-bin

This repo contains binary bundles built in `leo-acl2`, uploaded as releases.  This file records the releases.

As of 2021-09-16, the `leo-acl2` repo is private, and the build process is idiosyncratic.  The plan is to improve the `leo-acl2` build and make the repo public, with its own releases.  Once that is done this `leo-acl2-bin` repo can be removed.

## Release Tarball

The release tarball contains a bundle of 5 files that work together to run leo-acl2 theorem generation and checking (tgc).  It is currently targeted to CI tests of the Leo compiler.

## Running

Prior to running tgc, you must have a pair of JSON files that are before and after snapshots of a Leo compiler phase.  The leo compiler has flags to generate these JSON files.  For example,
```
    leo run --enable-all-ast-snapshots
```

Currently supported phases are canonicalization and type-inference.
```
    /path/to/tgc canonicalization initial_ast.json canonicalization_ast.json canonicalization-theorem.lisp
    /path/to/tgc type-inference canonicalization_ast.json type_inferenced_ast.json type-inference-theorem.lisp
```
The first argument to tgc is the compiler phase.  The next two arguments are the before and after snapshots.  The last argument is the name of a new file that will contain the theorem of correctness.  It must end in ".lisp".

The tgc script returns an exit status of 0 if the theorem was successfully generated and checked (proved).

## Releases

| Release version |    Date    | leo-acl2 commit | acl2 commit | changelog |
|:---------------:|:----------:|:---------------:|:-----------:|-----------|
| v0.1.0          | 2021-07-30 |  --             |  --         | adapt to changes in AST representation for countdown loops and end bound inclusivity
| v0.1.1          | 2021-08-09 |  583f03b        |  20c0f9464  | reduce amount of output from tgc
| v0.1.2          | 2021-08-18 |  0683a5c(*1)    |  8d24d938c  | add tgc for type-inference; split .tl file into two, one for each phase
| v0.1.3          | 2021-08-30 |  c29e5dc        |  0b52b2fbc  | relax array type checking; handle circuit expression types; tolerate other changes in AST format
| v0.1.4          | 2021-09-16 |  eac143d        |  6420eb29f  | add alias types; relaxations in  type checks; add file hashes to theorem files


## Footnotes

(*) final tgc script in release tarball is slightly modified from this commit,
    due to more strict bash processing on github action ubuntu than a workstation ubuntu (requires '[[' instead of '[').
