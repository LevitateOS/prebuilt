# prebuilt

Prebuilt parts of the LevitateOS and AcornOS builds: our own programs (the
install tools, the TUI binary, the desktop apps), published so a build can
download them instead of compiling.

Nothing here is edited by hand. `distro-builder prebuilt publish` creates one
release per part, tagged `<family>-<first 16 hex of key>`, with two assets:

- `prebuilt.json`: the full key, the part's family and name, its source
  commits, and the SHA-256 and size of every file.
- the archive of the part's files.

The key is a SHA-256 over everything the part is compiled from (its source
tree, target, compile recipe, compiler image and Recipe's declared behaviour
versions). A build computes the same key from its checkout, downloads only a
release whose `prebuilt.json` carries that exact key, checks every file's
digest, and compiles when there is no match. Release builds always compile.

See `docs/prebuilt-downloads.md` in the
[LevitateOS repository](https://github.com/LevitateOS/LevitateOS).
