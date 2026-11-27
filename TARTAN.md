# Tartan Studios' selene releases

This fork publishes upstream's own release tags, unchanged, for every
platform Tartan's machines and Rokit install on: Linux arm64 and x86_64, macOS arm64 and x86_64, and Windows x86_64. Upstream
publishes no Linux arm64 build, which Tartan's Graviton CI machines need
(TAR-2656).

This fork's `main` is upstream's plus this file and
`.github/workflows/tartan-release.yml`. Upstream has no tag-triggered release
workflow, so pushing an upstream tag releases nothing by itself. The workflow checks out an
upstream tag, builds selene with Rust 1.99.0 and the tag's own `Cargo.lock`, and releases it under that same tag as
`selene-<version>-<os>-<arch>.zip` zips, each holding the executable and upstream's licence, with a
`.sha256` beside each. Rokit picks a zip by the OS and architecture words in
its name.

## Releasing an upstream tag

1. Make sure the tag exists here: fetch upstream's tags and push the one you
   want, `git push origin refs/tags/<tag>`.
2. Run the workflow from `main`:
   `gh workflow run tartan-release.yml -R Tartan-Studios/selene --ref main -f tag=<tag>`.
   Pass `-f dry-run=true` to build every zip and publish nothing.
3. Pin it, as `selene = "Tartan-Studios/selene@<version>"` in a `rokit.toml`
   or by the release's zip and its `.sha256`.

A pull request into `main` dry-runs the workflow on its default tag,
0.29.0. Never change the code an upstream tag holds: a release here must be upstream's.
