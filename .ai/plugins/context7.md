# Context7 — Doc à jour pour LLMs

MCP server qui connecte l'agent à la documentation à jour de milliers de librairies/frameworks. Évite l'hallucination sur les APIs.

Source : [upstash/context7](https://github.com/upstash/context7)

## Contexte d'usage

Quand l'agent doit écrire du code qui utilise une librairie/framework, il peut demander "use context7" pour récupérer la doc à jour au lieu de se fier à ses données d'entraînement (potentiellement obsolètes).

Sans Context7 : APIs hallucinées, exemples outdated, versions anciennes.
Avec Context7 : doc version-spécifique injectée directement dans le prompt.

## Installation

### Setup universel (recommandé)

```bash
npx ctx7 setup
```

Authentifie via OAuth, génère une API key, installe le skill. Options : `--cursor`, `--claude`, `--opencode`.

### MCP server (manuel)

URL : `https://mcp.context7.com/mcp`
Pas de clé API requise (optionnelle pour des rate limits plus élevées via [context7.com/dashboard](https://context7.com/dashboard)).

#### Par client

**Claude Code** :
```bash
claude mcp add context7 --transport http https://mcp.context7.com/mcp
```

**Cursor** (`.cursor/mcp.json`) :
```json
{
  "mcpServers": {
    "context7": {
      "url": "https://mcp.context7.com/mcp"
    }
  }
}
```

**OpenCode** (`opencode.json`) :
```json
{
  "mcp": {
    "context7": {
      "type": "remote",
      "url": "https://mcp.context7.com/mcp"
    }
  }
}
```

**VS Code / Windsurf** :
```json
{
  "mcpServers": {
    "context7": {
      "url": "https://mcp.context7.com/mcp"
    }
  }
}
```

**Codex CLI** :
```bash
codex mcp add context7 --url https://mcp.context7.com/mcp
```

#### Avec API key (optionnel, rate limits plus élevés)

Ajouter le header `CONTEXT7_API_KEY` à la config si vous avez un compte :
```json
"headers": { "CONTEXT7_API_KEY": "{env:CONTEXT7_API_KEY}" }
```

### CLI + Skills (sans MCP)

```bash
npx ctx7 setup --opencode   # ou --cursor, --claude
```

Installe un skill qui guide l'agent à utiliser les commandes `ctx7` CLI au lieu d'un MCP server.

## Utilisation

Dans le prompt, ajouter "use context7" :

```
Create a Next.js middleware that checks for a valid JWT in cookies
and redirects unauthenticated users to /login. use context7
```

### Préciser la librairie (si connue)

```
Implement basic authentication with Supabase. use library /supabase/supabase
```

La syntaxe `/owner/repo` dit à Context7 exactement quelle lib charger.

### Préciser la version

```
How do I set up Next.js 14 middleware? use context7
```

## Outils MCP exposés

| Outil | Description |
|-------|-------------|
| `resolve-library-id` | Résout un nom de lib en ID Context7 (`query` + `libraryName` requis) |
| `query-docs` | Récupère la doc pour un library ID (`libraryId` + `query` requis) |

## CLI commands

```bash
ctx7 library <name> <query>     # Cherche une lib par nom
ctx7 docs <libraryId> <query>   # Récupère la doc (ex: /vercel/next.js)
```

## Règle recommandée

Ajouter dans `.ai/rules/` ou CLAUDE.md :

> Always use Context7 when I need library/API documentation, code generation, setup or configuration steps without me having to explicitly ask.

## Critères de succès

- L'agent ne hallucine pas d'APIs : il vérifie via Context7.
- La doc récupérée correspond à la version installée.
- Le code généré compile avec la version réelle de la lib.