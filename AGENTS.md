# AGENTS.md — Point d'entrée pour tous les agents IA

Ce fichier est lu par la plupart des assistants IA (Cursor, opencode, Claude Code, Copilot, Windsurf, etc.).
Il décrit comment travailler sur ce projet et où trouver les ressources IA.

## Structure des ressources IA

Toutes les ressources IA sont centralisées dans `.ai/` :

| Dossier | Rôle |
|---------|------|
| `.ai/rules/` | Règles métier, conventions de code, guidelines à respecter |
| `.ai/skills/` | Compétences custom : prompts réutilisables, templates, workflows |
| `.ai/plugins/` | Configuration des extensions et MCP servers |
| `.ai/context/` | Documentation de contexte : architecture, décisions, glossaire |

## Règles obligatoires

0. **Cold start** : à la première session sur un projet, analyser la codebase et documenter dans `.ai/context/` avant toute tâche. Voir `.ai/rules/cold-start.md`.
1. **Lire `.ai/rules/`** avant toute modification de code.
2. **Consulter `.ai/context/`** pour comprendre l'architecture et les décisions techniques.
3. **Utiliser les skills** de `.ai/skills/` quand un cas d'usage correspond.
4. **Respecter les conventions** de commit, de nommage et de test définies dans les rules.
5. **Déclenchement contextuel** : voir `.ai/rules/scoped-rules.md` pour savoir quelles règles s'appliquent selon les fichiers touchés.
6. **Vérifier la doc via Context7** : pour tout sujet concernant une librairie, framework, ou API, utiliser `context7` (`use context7` dans le prompt ou l'outil MCP `context7`) pour récupérer la doc à jour avant d'écrire du code. Ne pas se fier aux connaissances d'entraînement seules — elles peuvent être obsolètes ou hallucinées. Voir `.ai/plugins/context7.md`.

## Règles disponibles

### Always-on (s'appliquent à chaque prompt)

| Règle | Rôle |
|-------|------|
| `auto-plan-mode.md` | Détecte la complexité du prompt, propose un plan si besoin |
| `cold-start.md` | Analyser et documenter la codebase à la première session |
| `ponytail.md` | Anti-over-engineering (échelle de paresse) |
| `read-before-write.md` | Lire fichier + voisins avant d'éditer |
| `anti-hallucination.md` | Vérifier qu'une API/fonction existe avant de l'invoquer |
| `match-existing-conventions.md` | Imiter le style du fichier édité |
| `memory.md` | Gestion de la mémoire cross-session |
| `scoped-rules.md` | Mapping des règles par globs |

### Manuelles (sur invocation explicite)

| Règle | Rôle |
|-------|------|
| `avoid-ai-writing.md` | Audit et réécriture anti AI-isms |
| `skillspector.md` | Scanner de sécurité des skills avant install |

## Compatibilité

Des fichiers de compatibilité sont fournis pour les outils qui nécessitent un chemin spécifique :

- `.cursorrules` — Cursor (symlink logique vers `.ai/rules/`)
- `.clinerules` — Cline
- `.github/copilot/instructions.md` — GitHub Copilot

## Comment ajouter une compétence

1. Créer un fichier `.ai/skills/<nom>.md` avec le format standard (voir `.ai/skills/README.md`).
2. Décrire le contexte d'usage, le prompt type, et les critères de succès.
3. Référencer le skill dans `AGENTS.md` si pertinent.

## Comment ajouter une règle

1. Créer un fichier `.ai/rules/<domaine>.md`.
2. Énoncer la règle, sa justification, et des exemples.
3. Mettre à jour la liste ci-dessus si nécessaire.