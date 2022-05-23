# leo-acl2-bin

This repo contains binary bundles built in `leo-acl2`, uploaded as releases.  This file records the releases.

As of 2022-05-22, the `leo-acl2` repo is private, and the build process is idiosyncratic.  The plan is to improve the `leo-acl2` build and make the repo public, with its own releases.  Once that is done this `leo-acl2-bin` repo can be removed.

## Release Tarball

The release tarball contains a bundle of 5 files that work together to run leo-acl2 theorem generation and checking (tgc).  It is currently targeted to CI tests of the Leo compiler.

## Preparing to run tgc

Prior to running tgc, you must have a pair of files that are before and after snapshots of a Leo compiler phase.

For the moment, only parsing is supported.  The before snapshot is the Leo source file and the after snapshot
is a JSON file that is produced by the `parser` executable.

To create the parser executable, do the following:
```
    cd leo
    cargo update
    cargo build --release
    cd compiler/parser
    cargo install --path . --example parser
```
That should install the `parser` executable in your `.cargo/bin` directory,
which you can add to your `PATH`.

To run thre parser executable, simply call it with the `.leo` source file name on the command line.
A file with the same name but the `.json` extension will be created.  For example
```
    cd leo/tests/compiler/function
    parser return.leo
```

### Preparing to run tgc for other phases in the future

The leo compiler has flags to generate these JSON files.  For example,
```
    leo run --enable-all-ast-snapshots
```

## Running tgc

Currently supported phase is parsing.
```
    /path/to/tgc parsing source.leo source.json parsing-theorem.lisp
```
The first argument to tgc is the compiler phase.  The next two arguments are the before and after snapshots.  The last argument is the name of a new file that will contain the theorem of correctness.  It must end in `.lisp`.

The tgc script returns an exit status of 0 if the theorem was successfully generated and checked (proved).

### Running tgc for other phases in the future

Supported phases will be canonicalization and type-inference and others.
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
| v0.1.5          | 2022-05-22 |  e0001ac        |  85a4b2e38f | simplified testnet3 Leo; support parsing phase; removed support for other phases

## Footnotes

(*1) final tgc script in release tarball is slightly modified from this commit,
     due to more strict bash processing on github action ubuntu than a workstation ubuntu (requires '[[' instead of '[').
