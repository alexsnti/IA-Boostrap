# Vercel MCP — Configuration

MCP server officiel de Vercel pour gérer projets, déploiements, logs, env vars et rechercher la doc Vercel depuis n'importe quel agent IA.

Source : [Vercel MCP docs](https://vercel.com/docs/mcp/vercel-mcp)

## Endpoint

```
https://mcp.vercel.com
```

MCP server remote avec OAuth. Implémente MCP Authorization + Streamable HTTP.

## Outils disponibles

- **Publics (sans auth)** : recherche dans la doc Vercel
- **Authentifiés** : gestion teams, projects, deployments, logs, env vars, DNS

## Configuration par client

### Claude Code
```bash
claude mcp add --transport http vercel https://mcp.vercel.com
# Authentifier via /mcp dans la session
```

### Cursor (`.cursor/mcp.json`)
```json
{
  "mcpServers": {
    "vercel": {
      "url": "https://mcp.vercel.com"
    }
  }
}
```

### Windsurf (`mcp_config.json`)
```json
{
  "mcpServers": {
    "vercel": {
      "serverUrl": "https://mcp.vercel.com"
    }
  }
}
```

### VS Code with Copilot
```json
{
  "mcpServers": {
    "vercel": {
      "url": "https://mcp.vercel.com"
    }
  }
}
```

### Codex CLI
```bash
codex mcp add vercel --url https://mcp.vercel.com
```

### Gemini CLI / Code Assist (`~/.gemini/settings.json`)
```json
{
  "mcpServers": {
    "vercel": {
      "command": "npx",
      "args": ["mcp-remote", "https://mcp.vercel.com"]
    }
  }
}
```

### Claude.ai / Claude Desktop
- Settings → Connectors → Add custom connector
- Name : `Vercel`
- URL : `https://mcp.vercel.com`

### ChatGPT
- Settings → Connectors → Developer mode → Create connector
- Name : `Vercel`
- MCP server URL : `https://mcp.vercel.com`
- Auth : `OAuth`

### Devin
- Settings > MCP Marketplace → rechercher "Vercel" → Install

### Raycast
- Install Server command → Name: `Vercel`, Transport: HTTP, URL: `https://mcp.vercel.com`

### Goose
- One-click installation via la doc Goose

### Installation multi-clients (auto-detect)
```bash
npx add-mcp https://mcp.vercel.com
# -y : skip confirmation, installe sur tous les agents détectés
# -g : installation globale
```

## Sécurité

- **Endpoint officiel** : toujours `https://mcp.vercel.com`
- **OAuth** : consentement explicite requis pour chaque client (confused deputy protection)
- **Human confirmation** : garder activé pour valider chaque action
- **Scope** : l'agent a le même accès que votre compte Vercel user
- **Prompt injection** : attention aux instructions malveillantes dans les logs/contenus

## Critères de succès

- Le MCP server est connecté et authentifié.
- L'agent peut déployer, gérer projets, lire logs, gérer env vars.
- Le human confirmation reste activé pour les actions sensibles.