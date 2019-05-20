# Remove from Coverage
> Author: [Jack Warren](https://jackwarren.info)

Manipulate `lcov.info` coverage files to ignore files matching given patterns. This can be used to exclude generated files from coverage reports.

```text
Remove files with paths matching given PATTERNs from the lcov.info FILE
-f, --file=<FILE>         the target lcov.info file to manipulate
-r, --remove=<PATTERN>    a pattern of paths to exclude from coverage
-h, --help                show this help
```

The patterns are used to construct `RegExp` objects, so `-r '.g.dart$'` becomes `RegExp('.g.dart$')`. Multiple patterns may be provided by either providing `-r` multiple times or by using comma-separation, like `-r '.g.dart$', 'main.dart'`. When evaluating a file's entry in the `lcov.info`, if its path matches any pattern it will be excluded. The paths are relative from the project's root.

If a `lcov.info` file is not provided via thr `-f` flag, `stdin` input is run through the program's filter and is sent to `stdout`.

## Example
Suppose you want to remove generated files ending in `.g.dart` from code coverage reports:

- We'll target the `lcov.info` file in the `coverage` directory
- We'll use `.g.dart$` as the pattern

If [`pub global` scripts are on your path](https://dart.dev/tools/pub/cmd/pub-global#running-a-script-from-your-path), you can use the following:

```bash
remove_from_coverage -f coverage/lcov.info -r '.g.dart$'
```

Otherwise, [you can use the following](https://dart.dev/tools/pub/cmd/pub-global#running-a-script-using-pub-global-run):

```bash
pub global run remove_from_coverage:remove_from_coverage -f coverage/lcov.info -r '.g.dart$'
```

An entire example project where `remove_from_coverage` is useful exists in the [`example` directory of the package](https://github.com/jack-r-warren/remove_from_coverage/tree/master/example).

## How it works
Code coverage reports are generated from an `lcov.info` file, which is generated by test coverage packages. 

In some cases, it might make sense to have some files excluded from code coverage reports, like generated or experimental files. Some generators of `lcov.info` files might support excluding files, but `remove_from_coverage` provides another route: after a `lcov.info` file has been generated, it can be manipulated to remove data relating to ignored files.

This means that `remove_from_coverage` is agnostic to both how the `lcov.info` file is generated or how it is used. It may be used for non-Dart projects, and it has been tested on both Linux and Windows.
