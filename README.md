# sidenode

Core sidenote service: domain models, API, SDK, and CLI for contextual annotations

## Architecture

`sidenode` is the **core service repository** for the Sidenode system, separate from the marketing site (`sidenode-site`). This repo contains all the domain logic, API services, and developer tooling needed to build and integrate sidenotes into applications.

## Repository Structure

This is a **pnpm monorepo** organized with workspaces:

```
sidenode/
├── packages/
│   ├── core/           # Domain models, business logic
│   ├── api/            # REST/GraphQL API service (Cloudflare Workers)
│   ├── sdk/            # TypeScript/JavaScript client SDK
│   ├── cli/            # Command-line tool for dev workflow
│   └── widget/         # (Optional) UI components for embedding
├── package.json        # Monorepo root with workspaces
└── README.md           # This file
```

## Packages

### `@halema/sidenode-core`
Core domain library containing:
- Sidenote models (blocks, references, tags)
- Storage abstractions (KV, D1, Postgres)
- Business logic independent of runtime
- Pluggable for Cloudflare Workers, Node, Deno, etc.

### `@halema/sidenode-api`
HTTP API service:
- REST or JSON-RPC endpoints
- Create, query, link sidenotes
- Authentication/authorization hooks
- Deployable to Cloudflare Workers, Node, Docker

### `@halema/sidenode-sdk`
Client SDK for apps:
- `sidenode.init(config)`
- `sidenode.create(note)`
- `sidenode.listForPage(url)`
- TypeScript types and autocomplete

### `@halema/sidenode-cli`
Developer CLI:
- `sidenode init` - Bootstrap new project
- `sidenode dev` - Local development server
- `sidenode deploy` - Deploy to production
- Manage schemas, migrations, config

### `@halema/sidenode-widget` (Optional)
Embeddable UI components:
- React/Vue/Svelte components
- Drop-in annotation widgets
- Customizable themes

## Design Principles

1. **Separation of Concerns**
   - `sidenode` = core service (this repo)
   - `sidenode-site` = marketing + docs
   
2. **Runtime Agnostic Core**
   - Domain logic works everywhere
   - Storage abstraction for different backends
   
3. **Multiple Integration Paths**
   - SDK for JavaScript apps
   - API for any language/platform
   - CLI for developer workflows
   
4. **Goodstack/Halema Integration**
   - Auth hooks for Goodstack properties
   - Multi-tenant support
   - Permission scoping by project

## Getting Started

### Prerequisites
- Node.js ≥18
- pnpm ≥8

### Installation

```bash
# Clone the repo
git clone https://github.com/Halema-org/sidenode.git
cd sidenode

# Install dependencies
pnpm install

# Build all packages
pnpm build

# Run tests
pnpm test

# Start local dev
pnpm dev
```

### Usage Example

```typescript
import { Sidenode } from '@halema/sidenode-sdk';

const sidenode = new Sidenode({
  apiUrl: 'https://api.sidenode.io',
  apiKey: process.env.SIDENODE_API_KEY
});

// Create a sidenote
const note = await sidenode.create({
  content: 'This is a contextual annotation',
  context: {
    url: 'https://example.com/article',
    selector: '#paragraph-3'
  },
  tags: ['feedback', 'important']
});

// Query sidenotes for a page
const notes = await sidenode.listForPage({
  url: 'https://example.com/article'
});
```

## Relationship to `sidenode-site`

- **`sidenode`** (this repo): Core app, API, SDK, CLI
- **`sidenode-site`**: Marketing site, docs, embed snippets

The site consumes `@halema/sidenode` packages (via npm or Git submodule) but contains no domain logic. All features are built here first, then documented on the site.

## Development Workflow

1. **Core changes** → `packages/core`
2. **API updates** → `packages/api`
3. **SDK enhancements** → `packages/sdk`
4. **CLI features** → `packages/cli`
5. **Version & publish** → Update package versions, publish to npm
6. **Update site** → Bump SDK version in `sidenode-site`, update docs

## Deployment

### API Service
```bash
cd packages/api
pnpm deploy  # Deploys to Cloudflare Workers
```

### SDK (npm)
```bash
cd packages/sdk
pnpm publish
```

### CLI (npm)
```bash
cd packages/cli
pnpm publish
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for development guidelines.

## License

MIT © Halema Foundation
