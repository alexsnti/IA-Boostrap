# MEMORY.md — Journal de session

Ce fichier est la mémoire court-terme de l'agent IA. Il est lu au démarrage de chaque session et mis à jour à la fin.

## Format

### Dernière session

```
## [DATE] — [Objectif de la session]
- Fait : [ce qui a été accompli]
- Décisions : [décisions prises, avec pourquoi]
- En cours : [ce qui reste en cours / blocages]
- Prochaine étape : [ce que l'agent doit reprendre]
- Learnings : [leçons apprises, surprises]
```

## Archives

Les sessions anciennes sont archivées dans `.ai/memory/sessions/` quand `MEMORY.md` dépasse 10 sessions actives.

## Quand utiliser le dossier memory/ plutôt que MEMORY.md

- **MEMORY.md** : sessions courtes (< 30 min), 1-2 tâches, pas de décisions architecturales.
- **memory/ structuré** : projets longs, décisions architecturales, multiples workstreams.

Le dossier `.ai/memory/` contient :
- `decisions.md` — Registre des décisions (ADR-like)
- `progress.md` — Avancement par workstream
- `learnings.md` — Leçons apprises accumulées
- `sessions/` — Archives des anciennes sessions

## État actuel

_Pas encore de session enregistrée._

---

## [2026-07-23] — Initialisation BootstrapIA
- Fait : Structure agnostique créée, 4 plugins intégrés (ponytail, avoid-ai-writing, skillspector, superpowers), 4 règles always-on (auto-plan-mode, read-before-write, anti-hallucination, match-existing-conventions, scoped-rules), 1 règle manuelle (avoid-ai-writing), 1 MCP config (vercel-mcp).
- Décisions : Structure `.ai/` agnostique plutôt qu'outil-spécifique. SKILL.md stocké localement plutôt qu'URL distante. Plan mode demande confirmation avant activation.
- En cours : Rules framework (TS/React/Python) pas encore créées. Context (architecture, glossaire) pas encore rempli.
- Prochaine étape : Compléter rules framework selon la stack du projet cible, ou ajouter d'autres plugins/skills.
- Learnings : La force de Cursor vient du déclenchement contextuel (globs) + codebase awareness (read-before-write) + diff review. Reproduire ça demande des règles scopées, pas une grosse instruction.