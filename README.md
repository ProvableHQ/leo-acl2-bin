# leo-acl2-bin

This repo contains binary bundles built in `leo-acl2`, uploaded as releases.  This file records the releases.

As of 2021-08-07, the `leo-acl2` repo is private, and the build process is idiosyncratic.  The plan is to improve the `leo-acl2` build and make the repo public, with its own releases.  Once that is done this `leo-acl2-bin` repo can be removed.

## Release Tarball

The release tarball contains a bundle of 4 files that work together to run leo-acl2 theorem generation and checking (tgc).  It is currently targeted to CI tests of the Leo compiler.  

## Running

To run tgc, you must have a pair of JSON files that are before and after snapshots of a Leo compiler phase.  Currently the canonicalization phase is supported.
```
    /path/to/tgc canonicalization initial_ast.json canonicalization_ast.json canonicalization-theorem.lisp
```
The first argument to tgc is the compiler phase.  The next two arguments are the before and after snapshots.  The last argument is the name of a new file that will contain the theorem of correctness.  It must end in ".lisp".

The tgc script returns an exit status of 0 if the theorem was successfully generated and checked (proved).

## Releases

| Release version |    Date    | leo-acl2 commit | acl2 commit | changelog |
|:---------------:|:----------:|:---------------:|:-----------:|-----------|
| 0.1.0           | 2021-07-30 |  --             |  --         | adapt to changes in AST representation for countdown loops and end bound inclusivity
| 0.1.1           | 2021-08-09 |  583f03b        |  20c0f9464  | reduce amount of output from tgc

