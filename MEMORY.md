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

---

## 2026-07-23 — Initialisation BootstrapIA
- Fait : Structure agnostique créée, 4 plugins intégrés (ponytail, avoid-ai-writing, skillspector, superpowers), 4 règles always-on (auto-plan-mode, read-before-write, anti-hallucination, match-existing-conventions, scoped-rules), 1 règle manuelle (avoid-ai-writing), 1 MCP config (vercel-mcp).
- Décisions : Structure `.ai/` agnostique plutôt qu'outil-spécifique. SKILL.md stocké localement plutôt qu'URL distante. Plan mode demande confirmation avant activation.
- En cours : Rules framework (TS/React/Python) pas encore créées. Context (architecture, glossaire) pas encore rempli.
- Prochaine étape : Compléter rules framework selon la stack du projet cible, ou ajouter d'autres plugins/skills.
- Learnings : La force de Cursor vient du déclenchement contextuel (globs) + codebase awareness (read-before-write) + diff review. Reproduire ça demande des règles scopées, pas une grosse instruction.

---

## 2026-07-23 — Plugin goal + fix mémoire cross-session
- Fait : Ajout plugin `@prevalentware/opencode-goal-plugin` (`.ai/plugins/opencode-goal-plugin.md` + README plugins mis à jour). Ajout règle obligatoire 7 (goal mode) dans AGENTS.md. Renforcement règle `memory.md` : lecture OBLIGATOIRE au démarrage de chaque session, pas seulement en fin. Ajout section "Démarrage de session" obligatoire dans AGENTS.md (lire MEMORY.md + progress.md + decisions.md + learnings.md avant toute action). Réécriture MEMORY.md (problème encodage).
- Décisions : La lecture mémoire au démarrage doit être explicite et confirmée avec l'user ("Je reprends sur Y ?"), parce qu'OpenCode n'a pas d'historique de chat natif contrairement à Cursor. Le goal plugin est recommandé pour les tâches longues (refactor, migration, review, fix tests).
- En cours : Rules framework (TS/React/Python) toujours pas faites. Context (architecture, glossaire) toujours pas rempli.
- Prochaine étape : Compléter rules framework selon stack cible, ou continuer plugins/skills.
- Learnings : OpenCode perd le contexte entre sessions contrairement à Cursor qui garde l'historique de chat. La règle memory.md existait mais n'était pas assez forte — "lire au démarrage" était noyé dans la règle et pas repris dans AGENTS.md comme étape obligatoire. Fix : expliciter la lecture au démarrage dans AGENTS.md + confirmer avec l'user avant de repartir.