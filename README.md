# FOLIO Claude Code Plugin

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A [Claude Code](https://claude.com/claude-code) plugin that provides access to [FOLIO](https://openlegalstandard.org), the Federated Open Legal Information Ontology — 18,000+ legal concepts covering areas of law, document types, legal entities, and more.

## Installation

Add to your project's `.mcp.json`:

```json
{
  "mcpServers": {
    "folio": {
      "command": "uvx",
      "args": ["folio-mcp"]
    }
  }
}
```

Or install globally in `~/.claude/settings.json`:

```json
{
  "mcpServers": {
    "folio": {
      "command": "uvx",
      "args": ["folio-mcp"]
    }
  }
}
```

Requires Python 3.10+ and [uv](https://docs.astral.sh/uv/) installed. The `uvx folio-mcp` command fetches [folio-mcp](https://pypi.org/project/folio-mcp/) from PyPI automatically.

## Available Tools

| Tool | Description |
|---|---|
| `search_concepts` | Search concepts by label (name) — primary entry point |
| `search_definitions` | Search concepts by definition text |
| `query_concepts` | Advanced query with composable text and structural filters |
| `query_properties` | Query OWL object properties with filters |
| `get_concept` | Get full details for a concept by IRI |
| `export_concept` | Export concept as markdown, JSON-LD, or OWL XML |
| `list_branches` | List all 24 taxonomy branches with concept counts |
| `get_taxonomy_branch` | Get all concepts in a branch |
| `get_children` | Get child concepts |
| `get_parents` | Get parent concepts |
| `get_properties` | List all OWL object properties |
| `find_connections` | Find semantic connections between concepts |

## Usage Examples

Once installed, Claude Code automatically uses FOLIO tools when relevant. You can also invoke them directly:

```
> Search FOLIO for patent infringement concepts

> What areas of law does FOLIO define?

> Find the FOLIO classification for "software licensing agreement"

> Export the "Landlord Tenant Law" concept as JSON-LD
```

### Workflow

1. **Search by name**: `search_concepts("bankruptcy")` — fuzzy label matching
2. **Refine by definition**: `search_definitions("court-ordered debt relief")` — definition text search
3. **Advanced query**: `query_concepts(label="contract", branch="document_artifacts")` — composable filters
4. **Browse branches**: `list_branches()` → `get_taxonomy_branch("areas_of_law")`
5. **Navigate hierarchy**: `get_children(iri)` / `get_parents(iri)`
6. **Export**: `export_concept(iri, format="markdown")`

## Architecture

```
Claude Code → stdio → folio-mcp → HTTPS → folio.openlegalstandard.org
```

The plugin runs `folio-mcp` as a stdio MCP server. By default it calls the public FOLIO REST API at `folio.openlegalstandard.org`. No API key required — the FOLIO API is freely available with open CORS.

For local/offline use, install `folio-python[search]` and run with `--local`:

```json
{
  "mcpServers": {
    "folio-local": {
      "command": "uvx",
      "args": ["folio-mcp", "--local"]
    }
  }
}
```

## Links

- [FOLIO Ontology](https://openlegalstandard.org)
- [FOLIO API](https://folio.openlegalstandard.org/docs)
- [folio-mcp on PyPI](https://pypi.org/project/folio-mcp/)
- [folio-python on PyPI](https://pypi.org/project/folio-python/)
- [ALEA Institute](https://aleainstitute.ai)

## License

MIT — see [LICENSE](LICENSE).
