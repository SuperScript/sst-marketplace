# SST Marketplace

A curated catalog of Claude Code plugins maintained by the SST community. This repository is a **thin registry** — no plugin code lives here. Each plugin maintains full independence with its own repository, release cycle, and maintainers.

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

No plugins yet. See [Contributing](#contributing) to add the first one.

## Design Philosophy

This marketplace follows a **thin registry pattern**:

- **No plugin code** lives in this repository
- Each entry in `marketplace.json` points to an external source (GitHub repo, git URL, npm package, or pip package)
- Plugins are installed directly from their source — this repo only provides discovery
- Plugin authors maintain full control over their release cycle and versioning

## Contributing

### Adding a Plugin

1. Fork this repository
2. Add your plugin entry to `.claude-plugin/marketplace.json` (keep plugins in alphabetical order)
3. Run validation: `scripts/validate-marketplace`
4. Open a pull request

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

### Requirements

- Plugin names must be kebab-case (e.g., `my-plugin-name`)
- Each plugin must have `name`, `source`, and `description` fields
- Sources must be objects with a `source` type field — no relative paths or string sources
- Keep plugins in alphabetical order

## Team Auto-Install

To have your team automatically use this marketplace, add it to your project's `.claude/settings.json`:

```json
{
  "plugins": {
    "marketplaces": ["sst/sst-marketplace"]
  }
}
```

## Validation

Run the validation script to check `marketplace.json` before submitting a PR:

```
scripts/validate-marketplace
```

## License

BSD 3-Clause — see [LICENSE](LICENSE).
