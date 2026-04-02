# AGENTS.md

## Cursor Cloud specific instructions

### Project overview

SchemaStore is the world's largest collection of independent JSON Schemas. It is a single-project Node.js repository (not a monorepo). There are no Docker services, databases, or external API dependencies — everything runs locally with Node.js and npm.

### Key commands

All standard commands are documented in `CONTRIBUTING.md` and `package.json` scripts. Quick reference:

- **Install dependencies:** `npm clean-install`
- **Validate a single schema:** `node ./cli.js check --schema-name=<schemaName.json>`
- **Validate all schemas:** `node ./cli.js check` (~70s)
- **Lint CLI:** `npm run eslint`
- **Type check:** `npm run typecheck`
- **Format check:** `npm run prettier` (~30s, checks all files)
- **Format fix:** `npm run prettier:fix`
- **Scaffold new schema:** `node cli.js new-schema` (interactive, reads from stdin)
- **Build website:** `node ./cli.js build-website` (outputs to `./website/`, not needed for schema development)

### Non-obvious caveats

- The `node ./cli.js check` command validates all 753+ schemas with Ajv and runs positive/negative tests. It takes ~70 seconds. For faster iteration, use `--schema-name=<name>.json` to validate a single schema.
- The `npm run prettier` check takes ~30 seconds because it checks all files in the repository including hundreds of schema and test files.
- When adding a new schema file, it must also have a corresponding entry in `src/api/json/catalog.json` (or be listed in `missingCatalogUrl` in `src/schema-validation.jsonc`), otherwise `node ./cli.js check` will fail.
- The `new-schema` command is interactive (prompts for schema name via stdin). Pipe a name to avoid blocking: `echo "my-schema" | node cli.js new-schema`.
- Node.js >=18 is required (see `engines` in `package.json`).
