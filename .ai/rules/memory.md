# Memory — Gestion de la mémoire cross-session

## Règle

L'agent IA doit maintenir une mémoire cross-session pour ne pas perdre le contexte entre les sessions.

### Au démarrage d'une session (OBLIGATOIRE, avant toute action)

Cette lecture est **non-négociable**. Sans elle, l'agent repart de zéro et perd tout le travail précédent — c'est l'équivalent de l'historique de chat + l'index de codebase que Cursor a nativement.

1. **Lire `MEMORY.md`** à la racine du projet — dernière session, "prochaine étape", learnings récents.
2. **Lire `.ai/memory/progress.md`** — avancement par workstream, tâches cochées, blocages.
3. Si des décisions architecturales sont en jeu → lire `.ai/memory/decisions.md`.
4. Si le sujet touche à un learning précédent → lire `.ai/memory/learnings.md`.
5. Identifier la dernière session et la "prochaine étape" à reprendre.
6. **Confirmer avec l'utilisateur** avant de repartir : « Dernière session : X. Prochaine étape : Y. Je reprends sur Y ? »

Ne pas sauter cette étape même si la question paraît triviale — c'est exactement dans ces cas qu'on oublie qu'on a déjà fait le travail.

### Pendant la session

- Mettre à jour `.ai/memory/progress.md` au fur et à mesure (cocher les tâches, ajouter les nouvelles).
- Si une décision architecturale est prise → ajouter une entrée ADR dans `.ai/memory/decisions.md`.
- Si une leçon est apprise → ajouter une entrée dans `.ai/memory/learnings.md`.

### En fin de session

Écrire un résumé dans `MEMORY.md` :

```markdown
## [DATE] — [Objectif de la session]
- Fait : [ce qui a été accompli]
- Décisions : [décisions prises, avec pourquoi]
- En cours : [ce qui reste en cours / blocages]
- Prochaine étape : [ce que l'agent doit reprendre]
- Learnings : [leçons apprises, surprises]
```

### Archive

Si `MEMORY.md` dépasse 10 sessions actives :
1. Déplacer la plus ancienne dans `.ai/memory/sessions/YYYY-MM-DD.md`.
2. Garder les 10 plus récentes dans `MEMORY.md`.

## Quand utiliser quoi

| Scope | Outil | Quand |
|-------|-------|-------|
| Session courte (< 30 min, 1-2 tâches) | `MEMORY.md` seul | Pas de décision archi, pas de multi-workstream |
| Session longue ou projet complexe | `MEMORY.md` + `.ai/memory/` | Décisions archi, multiples workstreams, long terme |
| Décision architecturale | `.ai/memory/decisions.md` | Toujours, quelle que soit la longueur de session |
| Leçon apprise | `.ai/memory/learnings.md` | Toujours, même en session courte |

## Justification

Sans mémoire cross-session, l'agent perd :
- Ce qu'il a fait → répète ou oublie des étapes.
- Les décisions prises → re-questionne ou contredit.
- Où il s'est arrêté → ne sait pas reprendre.
- Les leçons apprises → répète les mêmes erreurs.

Cursor gère ça via l'index du codebase + l'historique de chat. Un agent CLI doit le faire explicitement via fichiers.

## Exemples

### Démarage de session
```
[Lit MEMORY.md]
"Dernière session : 2026-07-23, BootstrapIA core. Prochaine étape : rules framework TS."
→ "Je reprends sur les rules framework TypeScript ?"
```

### Fin de session
```
[Écrit dans MEMORY.md]
## 2026-07-24 — Rules framework TypeScript
- Fait : Créé typescript-conventions.md, react-conventions.md
- Décisions : Prefer named exports (ADR-005)
- En cours : Python conventions pas encore faites
- Prochaine étape : Python conventions
- Learnings : Les rules framework doivent être courtes (< 50 lignes), pas des cours
```