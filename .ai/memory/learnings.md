# Learnings — Leçons apprises accumulées

Ce fichier accumule les leçons apprises au fil des sessions, pour éviter de répéter les mêmes erreurs et capitaliser les surprises.

## Format

```
### [DATE] — [Sujet]
- Learning : [ce qu'on a appris]
- Contexte : [quand ça s'est produit]
- Application : [comment l'appliquer à l'avenir]
```

## Learnings

### 2026-07-23 — Force de Cursor
- Learning : La "perfection" de Cursor vient du déclenchement contextuel (globs sur les fichiers touchés) + codebase awareness (read-before-write) + diff review. Ce n'est pas une grosse instruction system, c'est une stack de règles scopées qui s'activent au bon moment.
- Contexte : Analyse de awesome-cursorrules (PatrickJS) après question user sur le feeling Cursor.
- Application : Toujours scoper les règles par globs plutôt que `alwaysApply: true`. Privilégier read-before-write à de grandes instructions de contexte. Imiter l'existant plutôt que standardiser.

### 2026-07-23 — SKILL.md massif
- Learning : Les skills peuvent être très lourds (avoid-ai-writing = 76KB, 57 catégories). Les stocker localement plutôt qu'en URL distante pour garder le bootstrap autonome.
- Contexte : Intégration du skill avoid-ai-writing. Tentative de pointer vers URL distante, corrigée après remarque user.
- Application : Pour tout skill > 5KB, le copier localement dans `.ai/skills/<nom>/SKILL.md`. Documenter la source dans `.ai/plugins/<nom>.md`.

### 2026-07-23 — Windows + git plugins
- Learning : Sur Windows, les plugins git+https dans opencode.json peuvent échouer (cache Bun, git.exe introuvable). Workaround : `npm install` en local + pointer vers `node_modules/`.
- Contexte : Installation de Superpowers pour opencode.
- Application : Sur Windows, toujours essayer l'install npm locale en fallback si le plugin git ne charge pas.

### 2026-07-23 — Bootstrap pas actif pendant sa propre création
- Learning : Pendant qu'on construit le bootstrap, ses règles ne sont pas encore actives dans la session. L'agent peut halluciner parce que `.ai/rules/anti-hallucination.md` n'est pas chargé. Il faut vérifier la doc de l'outil cible (OpenCode) avant de répercuter la doc d'un plugin tiers (Context7 GitHub README qui disait "API key recommended" vs doc OpenCode qui disait "optionnel").
- Contexte : J'ai documenté Context7 avec une API key "recommandée" en me basant sur le README GitHub, alors que la doc OpenCode montrait la config sans clé. L'utilisateur a corrigé.
- Application : Toujours cross-référencer la doc de l'outil cible avant d'écrire une config plugin. Le README d'un plugin n'est pas la source de vérité pour l'intégration dans un outil spécifique. Une fois le bootstrap actif dans un projet, `anti-hallucination.md` protégera contre ça.

### 2026-07-23 — OpenCode perd le contexte cross-session contrairement à Cursor
- Learning : Cursor garde l'historique de chat + un index de codebase natifs, donc en revenant sur un sujet on a le contexte. OpenCode repart de zéro à chaque session — la règle `memory.md` existait mais "lire au démarrage" était noyé dans la règle et pas repris comme étape obligatoire dans AGENTS.md. Conséquence : l'agent oublie tout le travail précédent même quand MEMORY.md est bien rempli.
- Contexte : User a remarqué qu'en revenant sur le projet, je n'avais aucun souvenir du travail fait sur les plugins, contrairement à Cursor.
- Application : La lecture mémoire au démarrage doit être (1) une section dédiée en haut d'AGENTS.md, pas une sous-puce d'une règle, (2) explicitement obligatoire et non-négociable, (3) confirmée avec l'user ("Je reprends sur Y ?") avant de continuer. Même les questions triviales doivent passer par cette lecture.