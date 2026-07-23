# BootstrapIA

Un bootstrap de développement IA agnostique, conçu pour fonctionner aussi bien que Cursor avec n'importe quel agent IA (opencode, Claude Code, Copilot, Windsurf, etc.).

## Démarrage rapide

1. Copiez le dossier `.ai/` et le fichier `AGENTS.md` à la racine de votre projet.
2. Ajoutez vos règles dans `.ai/rules/`.
3. Ajoutez vos skills dans `.ai/skills/`.
4. Ajoutez vos plugins dans `.ai/plugins/`.
5. Documentez votre architecture dans `.ai/context/`.

## Structure

```
.ai/
├── rules/      Règles et conventions
├── skills/     Compétences IA réutilisables
├── plugins/    Configuration MCP et extensions
└── context/    Documentation de contexte
AGENTS.md       Point d'entrée pour les agents IA
```

## Compatibilité

| Outil | Fichier de compat |
|-------|-------------------|
| Cursor | `.cursorrules` |
| Cline | `.clinerules` |
| GitHub Copilot | `.github/copilot/instructions.md` |
| opencode | `AGENTS.md` + `.opencode/` |
| Claude Code | `AGENTS.md` + `.claude/` |
| Windsurf | `.windsurfrules` |

## Licence

MIT