---
name: topoglyph-save-module
description: Use to save/persist a TopoGlyph module developed in the current conversation.
---

Pick a short, hyphenated filename based on the module's name, then ask the user where to save it:

1. **Globally** (default; available to all projects): `~/.topoglyph/modules/<module-name>.md`
2. **Project-local** (specific to the current project): `<project-path>/.topoglyph/modules/<module-name>.md`

The global location (`~/.topoglyph/modules/`) is also the path registered by `initialize-topoglyph` as a project reference path.

#### Required frontmatter

```yaml
---
muid: <unique module identifier, e.g. "my-module-v1">
refs:                              # optional, list of related module paths
  - ~/.topoglyph/modules/topoglyph.md
---
```

When `refs` references other TopoGlyph modules, use `~/.topoglyph/...` paths (or absolute paths) so they resolve consistently across projects — never repo-relative paths like `modules/foo.md`.

#### After writing

Confirm to the user:
- Where the file was saved
- The `muid`
- Any cross-references declared in the `refs` field