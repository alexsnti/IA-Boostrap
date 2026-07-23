# Progress — Avancement par workstream

Ce fichier suit l'avancement des travaux en cours, organisé par workstream.

## Format

```
## [Workstream]
- Statut : not-started | in-progress | blocked | done
- Tâches :
  - [ ] tâche 1
  - [x] tâche 2
- Prochaine étape : [ce qu'il faut faire ensuite]
- Blocages : [ce qui bloque, et depuis quand]
```

## Workstreams

## BootstrapIA core
- Statut : in-progress
- Tâches :
  - [x] Structure agnostique `.ai/`
  - [x] AGENTS.md point d'entrée
  - [x] Plugin ponytail (anti-over-engineering)
  - [x] Plugin avoid-ai-writing (anti AI-isms)
  - [x] Plugin skillspector (sécurité skills)
  - [x] Plugin superpowers (méthodologie dev)
  - [x] Plugin vercel-mcp (config MCP)
  - [x] Règle auto-plan-mode
  - [x] Règle read-before-write
  - [x] Règle anti-hallucination
  - [x] Règle match-existing-conventions
  - [x] Règle scoped-rules
  - [x] Système de mémoire hybride
  - [ ] Rules framework (TS/React/Python selon stack cible)
  - [ ] Context : architecture.md, glossaire.md
  - [ ] Fichiers de compatibilité (`.cursorrules`, `.clinerules`, etc.)
- Prochaine étape : Compléter selon les besoins du projet cible
- Blocages : aucun