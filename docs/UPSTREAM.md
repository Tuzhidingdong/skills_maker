# GSD Core Upstream Pin

- Upstream repository: https://github.com/open-gsd/gsd-core
- Selected release: `v1.21.0`
- Release page: https://github.com/open-gsd/gsd-core/releases/tag/v1.21.0
- Git commit: `357508df233ccca0dcf4332b385d6bee9b113da9`
- Local source path: `vendor/gsd-core`
- Runtime package: `@opengsd/gsd-core@1.21.0`

GSD is included as a Git submodule, not a rewritten or flattened copy. This preserves attribution, upstream history and a clear upgrade path while avoiding a brittle partial copy of thousands of upstream files.

Use the official npm installer for runtime installation. The upstream README explicitly says not to copy `agents/` or `commands/` manually because each runtime needs a different schema and directory layout.

The upstream package manifest declares Node.js `>=24.0.0` and npm `>=10.0.0`; use those exact minimums.

The upstream Antigravity installer target is supported. The exact installed hook coverage and enforcement behavior still need to be checked on the workstation after installation. A file existing in the submodule is not evidence that the host runtime registered or executed it.
