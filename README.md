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

## How parts get here

`.github/workflows/publish.yml` runs every hour (at minute 17) and on demand (Actions >
Publish prebuilt parts > Run workflow; its optional `part` input builds one part, written
as `distro-builder prebuilt list` prints it, such as `tools recab`). It checks out
LevitateOS with its submodules at the heads of their branches, lists every part, computes
each key and skips the parts whose release already holds both assets. For the rest it
produces the pinned compiler image and compiles the part with `distro-builder prebuilt
build` (the recipe an ISO build uses, as root through `sudo`), then publishes it with
`distro-builder prebuilt publish` as the runner user. A release is never replaced. The
run's summary lists each part as skipped, published or failed, and the run fails if any
part failed. This workflow has not run yet.

See `docs/prebuilt-downloads.md` in the
[LevitateOS repository](https://github.com/LevitateOS/LevitateOS).
