# SST Marketplace

A curated catalog of Claude Code plugins from SuperScript. This repository is a **thin registry** — no plugin code lives here. Each plugin maintains full independence with its own repository, release cycle, and maintainers.

## Quick Start

### Add the marketplace

```
/plugin marketplace add sst/sst-marketplace
```

### Install a plugin

```
/plugin install <name>@sst-marketplace
```

## Available Plugins

| Plugin | Description |
|--------|-------------|
| [cli-skill](https://github.com/superscript/cli-skill) | Guide creation of Claude Code skills that wrap CLI tools |
| [five-whys](https://github.com/superscript/five-whys) | Facilitate structured Five Whys root-cause analysis sessions |
| [kak-script](https://github.com/superscript/kak-script) | Write kakoune script files adhering to SuperScript standards |
| [makefile-writing](https://github.com/superscript/makefile-writing) | Write Makefiles adhering to SuperScript standards |
| [plugin-ops](https://github.com/superscript/plugin-ops) | Provide various plugin-management operations |
| [planning](https://github.com/superscript/planning) | Plan logging and resumption skills |
| [ports-dev](https://github.com/superscript/ports-dev) | Skills for creating and maintaining FreeBSD ports |
| [project-setup](https://github.com/superscript/project-setup) | Create new project repositories of various types |
| [scrip-programming](https://github.com/superscript/scrip-programming) | Programming conventions for use of the scrip project |
| [shell-programming](https://github.com/superscript/shell-programming) | POSIX shell programming conventions for sh scripts |
| [unix-programming](https://github.com/superscript/unix-programming) | Guidelines for programming in UNIX style |

## Repository Structure

```
.claude-plugin/marketplace.json   — the plugin registry
scripts/validate-marketplace      — validation script (POSIX sh + jq)
```

## Design Philosophy

This marketplace follows a **thin registry pattern**: the entire registry is a single JSON file that maps plugin names to their external sources. No plugin code, build artifacts, or vendored dependencies live in this repository. The registry provides discovery, not distribution.

This design has several advantages over monorepo or artifact-hosting approaches:

- **Plugin author autonomy.** Authors retain full control over their release cycle, versioning, and hosting. Publishing a new plugin version requires no action in the marketplace — the source repository is the source of truth.

- **Minimal review surface.** Pull requests to the marketplace are small JSON diffs. They are easy to review and unlikely to introduce bugs, since there is no executable code to evaluate.

- **No single point of failure.** If the marketplace repository is unavailable, plugins continue to exist independently at their sources. The registry is an index, not a host.

- **Low-friction contributions.** Adding a plugin requires appending a JSON object to one file. There is no monorepo build system to conform to, no CI pipeline to satisfy, and no packaging step.

- **No artifact storage.** The registry stores no binaries, tarballs, or code. This keeps the repository tiny and eliminates supply-chain concerns at the registry level — there is nothing here to tamper with beyond metadata.

By contrast, monorepo approaches (where all plugin code lives in one tree) couple every plugin's release to the registry's merge process, and artifact-hosting registries (like npm or PyPI) must store, serve, and secure package contents. A thin registry separates discovery from distribution entirely, keeping each concern simple.

## Contributing

### Requirements

The validation script requires:

- **jq** — used for all JSON parsing and validation checks
- **POSIX-compatible `/bin/sh`** — the script targets `sh`, not `bash`

### Adding a Plugin

1. Fork this repository
2. Add your plugin entry to `.claude-plugin/marketplace.json` (keep plugins in alphabetical order)
3. Run validation: `scripts/validate-marketplace`
4. Open a pull request

For the full marketplace and plugin entry schema, see the [Claude Code plugin marketplace documentation](https://docs.anthropic.com/en/docs/claude-code/plugin-marketplaces).

### Plugin Entry Fields

Each plugin entry requires `name`, `source`, and `description`. The following fields are optional: `version`, `author`, `license`, `keywords`, `category`.

### Plugin Entry Templates

**GitHub source:**
```json
{
  "name": "my-plugin",
  "source": {
    "source": "github",
    "repo": "owner/repo",
    "ref": "v1.0.0"
  },
  "description": "What the plugin does.",
  "version": "1.0.0",
  "author": "your-name",
  "license": "MIT",
  "keywords": ["keyword1", "keyword2"],
  "category": "category-name"
}
```

**Git URL source:**
```json
{
  "name": "my-plugin",
  "source": {
    "source": "url",
    "url": "https://gitlab.com/org/repo.git",
    "ref": "main"
  },
  "description": "What the plugin does.",
  "version": "1.0.0",
  "author": "your-name",
  "license": "MIT",
  "keywords": ["keyword1", "keyword2"],
  "category": "category-name"
}
```

**npm source:**
```json
{
  "name": "my-plugin",
  "source": {
    "source": "npm",
    "package": "@scope/package-name",
    "version": "^1.0.0"
  },
  "description": "What the plugin does.",
  "version": "1.0.0",
  "author": "your-name",
  "license": "MIT",
  "keywords": ["keyword1", "keyword2"],
  "category": "category-name"
}
```

**pip source:**
```json
{
  "name": "my-plugin",
  "source": {
    "source": "pip",
    "package": "package-name",
    "version": ">=1.0.0"
  },
  "description": "What the plugin does.",
  "version": "1.0.0",
  "author": "your-name",
  "license": "MIT",
  "keywords": ["keyword1", "keyword2"],
  "category": "category-name"
}
```

### Entry Requirements

- Plugin names must be kebab-case (e.g., `my-plugin-name`)
- Each plugin must have `name`, `source`, and `description` fields
- Sources must be objects with a `source` type field — no relative paths or string sources
- Keep plugins in alphabetical order

## Validation

Run the validation script before submitting a PR:

```
scripts/validate-marketplace
```

The script checks:
- File exists and contains valid JSON
- Required top-level fields are present (`name`, `owner`, `plugins`)
- Plugin entries have required fields (`name`, `source`, `description`)
- Plugin names are valid kebab-case and unique
- Source objects have correct fields for their type (e.g., `repo` for GitHub, `package` for npm/pip)
- Plugins are in alphabetical order (warning only)

Exit codes:
- **0** — validation passed (warnings may still be shown)
- **111** — fatal validation error

## Team Auto-Install

To have your team automatically use this marketplace, add it to your project's `.claude/settings.json`:

```json
{
  "plugins": {
    "marketplaces": ["sst/sst-marketplace"]
  }
}
```

## License

BSD 3-Clause — see [LICENSE](LICENSE).
