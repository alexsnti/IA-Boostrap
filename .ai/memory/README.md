# Memory — Mémoire cross-session

Ce dossier contient la mémoire structurée de l'agent IA, pour les projets longs et les décisions architecturales.

## Structure

| Fichier | Rôle |
|---------|------|
| `decisions.md` | Registre des décisions techniques (ADR-like) |
| `progress.md` | Avancement par workstream |
| `learnings.md` | Leçons apprises accumulées |
| `sessions/` | Archives des anciennes sessions |

## Quand utiliser memory/ vs MEMORY.md

- **`MEMORY.md`** (racine) : sessions courtes (< 30 min), 1-2 tâches, pas de décisions architecturales. L'agent lit au démarrage, écrit à la fin.
- **`memory/`** (ce dossier) : projets longs, décisions architecturales, multiples workstreams. L'agent met à jour les fichiers pertinents au fur et à mesure.

## Cycle de vie

1. Au démarrage : lire `MEMORY.md` + `progress.md` pour savoir où on s'est arrêté.
2. Pendant la session : mettre à jour `progress.md` au fur et à mesure.
3. Si décision architecturale : ajouter une entrée dans `decisions.md`.
4. Si leçon apprise : ajouter une entrée dans `learnings.md`.
5. En fin de session : écrire le résumé dans `MEMORY.md`.
6. Si `MEMORY.md` dépasse 10 sessions : archiver la plus ancienne dans `sessions/`.