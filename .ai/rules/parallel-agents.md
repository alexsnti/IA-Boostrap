# Parallel-agents — Coordination multi-agents sur même codebase

## Règle

Quand plusieurs agents IA travaillent en parallèle sur la même codebase (via git worktrees ou sessions séparées), ils doivent se coordonner via la mémoire partagée.

## Setup : Git worktrees

Chaque agent travaille dans un worktree isolé :

```bash
# Créer un worktree par agent
git worktree add ../project-feature-a feature/a
git worktree add ../project-feature-b feature/b

# Lancer un agent par worktree
cd ../project-feature-a && opencode   # Agent A
cd ../project-feature-b && opencode   # Agent B
```

Avantages :
- Pas de conflit de fichiers (dossiers séparés)
- Pas de conflit git (branches séparées)
- Merge/PR indépendants à la fin
- Même repo, même historique

## Coordination via mémoire partagée

Le dossier `.ai/memory/` est partagé si les worktrees partagent le même repo (ou symlink). Chaque agent doit :

### Au démarrage
1. Lire `MEMORY.md` pour le contexte global.
2. Lire `.ai/memory/progress.md` pour voir ce que les autres agents font.
3. Déclarer son workstream dans `progress.md` :

```markdown
## [Workstream] — [Nom agent]
- Statut : in-progress
- Worktree : ../project-feature-a
- Branche : feature/a
- Tâches : [ce que cet agent fait]
- Fichiers touchés : [liste des fichiers en cours d'edit]
```

### Pendant le travail
- Mettre à jour `progress.md` avec les fichiers en cours d'édition.
- Si décision archi → ajouter ADR dans `decisions.md` (les autres agents le verront).
- Si leçon apprise → `learnings.md`.

### Conflit de fichiers

Si deux agents doivent toucher le même fichier :

1. **Prévenir** : déclarer le fichier dans `progress.md` avant de l'éditer.
2. **Sérialiser** : un agent attend que l'autre finisse (vérifier `progress.md`).
3. **Ou scinder** : diviser le fichier en deux si possible.
4. **Ou fusionner après** : chaque agent dans sa branche, merge manuel à la fin.

### En fin de session
1. Mettre à jour `progress.md` (statut: done).
2. Écrire résumé dans `MEMORY.md`.
3. Indiquer si merge nécessaire.

## Lock files (optionnel)

Pour les fichiers critiques, utiliser un lock file simple :

```
.ai/memory/locks/
  utils.ts.agent-a.lock   # Agent A édite utils.ts
```

Avant d'éditer un fichier, vérifier `.ai/memory/locks/`. Si un lock existe pour ce fichier, attendre ou négocier.

## Checklist multi-agents

- [ ] Un worktree par agent (branches séparées)
- [ ] Chaque agent déclare son workstream dans `progress.md`
- [ ] Chaque agent liste les fichiers qu'il touche
- [ ] Vérifier `progress.md` avant de toucher un fichier déjà déclaré
- [ ] Merge/PR à la fin, un par workstream
- [ ] Mettre à jour `MEMORY.md` en fin de session

## Justification

Sans coordination, deux agents en parallèle :
- Éditent le même fichier → conflit au merge.
- Contournent le travail de l'autre → rework.
- Prennent des décisions contradictoires → ADR en conflit.

Avec worktrees + mémoire partagée, chaque agent est isolé mais informé.