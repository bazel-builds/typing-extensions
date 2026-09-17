# typing_extensions (Bazel build)

This repo vendors [python/typing_extensions](https://github.com/python/typing_extensions)
as a git subtree and adds Bazel build/test targets for it, so it can be
consumed as a Bazel dependency without relying on pip.

## Layout

- `typing_extensions/` — upstream source, imported via `git subtree` at tag
  `4.16.0`. Don't hand-edit files under here; see "Updating the vendored
  source" below.
- `BUILD` — Bazel targets for the library and its test suite.
- `MODULE.bazel` / `MODULE.bazel.lock` — bzlmod dependencies (`rules_python`)
  and the hermetic Python toolchain registration.
- `.bazelversion` — pins the Bazel version via [Bazelisk](https://github.com/bazelbuild/bazelisk).
- `.bazelrc` — build flags (see "Bootstrap flag" below).
- `LICENSE` — copied verbatim from `typing_extensions/LICENSE` (PSF License
  Version 2).

## Requirements

- [Bazelisk](https://github.com/bazelbuild/bazelisk) installed as `bazel` on
  your `PATH` (e.g. `brew install bazelisk`). Bazelisk reads `.bazelversion`
  and downloads the matching Bazel release automatically — no local Bazel
  install needed.

No local Python installation is required either: `MODULE.bazel` registers a
hermetic CPython 3.12 toolchain that Bazel downloads and uses for all builds
and tests, so results don't depend on whatever Python happens to be on your
machine.

## Building

```sh
bazel build //:typing_extensions
```

## Testing

```sh
bazel test //:typing_extensions_test
```

This runs upstream's `test_typing_extensions.py` test suite (500+ tests)
against the vendored source, using the hermetic Python 3.12 toolchain.

## Bootstrap flag

`.bazelrc` sets:

```
build --@rules_python//python/config_settings:bootstrap_impl=script
```

`rules_python`'s default bootstrap (`system_python`) wires up `sys.path`
in-process without exporting `PYTHONPATH`, so tests that spawn a subprocess
(e.g. via `subprocess.run([sys.executable, ...])`) can't find
`typing_extensions` in the child process. The legacy `script` bootstrap sets
`PYTHONPATH` as a real environment variable, which subprocesses inherit.
Without this flag, three of the upstream tests fail with
`ModuleNotFoundError: No module named 'typing_extensions'`.

## Updating the vendored source

To pull a newer release of `typing_extensions`:

```sh
git subtree pull --prefix=typing_extensions https://github.com/python/typing_extensions <tag> --squash
```

Then re-run `bazel test //:typing_extensions_test` and update `BUILD` if
upstream added/removed/renamed source files.
