# Cold-start — Analyser et documenter la codebase au démarrage

## Règle

Quand le bootstrap est ajouté sur un projet (ou à la première session sur un projet existant), le **premier réflexe** de l'agent doit être d'analyser la codebase et de documenter ce qu'il trouve — avant toute tâche de code.

### Déclencheur

- Première session sur un projet où le bootstrap vient d'être ajouté.
- `MEMORY.md` vide ou sans session enregistrée.
- `progress.md` sans workstream actif.
- Utilisateur qui dit "fais un cold start", "analyse le projet", "on commence".

### Procédure de cold start

#### 1. Scan de la codebase

Lire et analyser dans cet ordre :

1. **Fichiers racine** : `package.json`, `requirements.txt`, `Cargo.toml`, `go.mod`, `pyproject.toml`, `pom.xml`, etc. → identifier la stack technique, les dépendances, les scripts.
2. **Structure de dossiers** : `glob` les fichiers principaux, identifier l'organisation (src/, lib/, app/, pages/, tests/, etc.).
3. **Config** : `tsconfig.json`, `.eslintrc`, `vite.config`, `next.config`, `Dockerfile`, `docker-compose.yml`, CI/CD (`.github/workflows/`, `.gitlab-ci.yml`).
4. **Entry points** : `main.ts`, `index.js`, `app.tsx`, `manage.py`, `main.go` — comprendre comment l'app démarre.
5. **README** et docs existantes : `README.md`, `CONTRIBUTING.md`, `docs/`.
6. **Tests** : identifier le framework de test, où sont les tests, comment les lancer.
7. **Git** : `git log --oneline -20` pour voir l'historique récent et les conventions de commit.

#### 2. Documentation

Écrire dans `.ai/context/` les fichiers suivants :

**`architecture.md`** :
- Stack technique (langages, frameworks, librairies clés)
- Structure des dossiers (schéma en arbre simplifié)
- Entry points et flux principal
- Patterns architecturaux identifiés (MVC, repository, services, etc.)
- Base de données / persistence
- API (REST/GraphQL/RPC) si applicable
- Authentification / autorisation si applicable

**`tech-stack.md`** :
- Langage(s) et version(s)
- Framework(s) et version(s)
- Librairies clés (avec versions)
- Outils de build / bundler
- Outils de test
- CI/CD
- Hosting / déploiement
- Base de données

**`conventions.md`** :
- Style de code (quotes, indentation, naming)
- Structure des fichiers (colocés vs séparés)
- Conventions de commit (ex: conventional commits)
- Conventions de test (framework, naming, placement)
- Patterns de gestion d'erreurs
- Patterns de logging

**`glossaire.md`** (si domaine métier) :
- Termes métier
- Acronymes
- Concepts spécifiques au projet

#### 3. Vérification doc externe

Pour chaque techno identifiée, utiliser **Context7** pour vérifier que la doc correspond à la version installée :
```
Check the docs for Next.js 14 with context7
```

Cela évite de documenter des patterns outdated.

#### 4. Mémoire initiale

Écrire dans `MEMORY.md` :
```markdown
## [DATE] — Cold start
- Fait : Analyse codebase, documentation architecture/tech-stack/conventions
- Stack : [résumé en 1 ligne]
- Décisions : [aucune pour l'instant]
- Prochaine étape : [attendre tâche utilisateur]
```

Mettre à jour `progress.md` avec un workstream "Documentation" en statut done.

### Quand NE PAS faire un cold start

- `MEMORY.md` a déjà une session → la codebase est déjà documentée, skip.
- L'utilisateur demande une tâche spécifique urgente → faire la tâche d'abord, cold start après.
- Projet vide/nouveau sans code → pas de codebase à analyser, skip.

## Justification

Un agent qui démarre sans comprendre la codebase :
- Hallucine la stack ("je suppose que c'est du React" → c'est du Vue).
- Casse les conventions (introduit du camelCase dans du snake_case).
- Réinvente ce qui existe déjà (crée une helper qui existe 3 fichiers plus loin).
- Rate le contexte critique (ignore qu'il y a une DB, une auth, un middleware).

Le cold start coûte 5-10 minutes et évite des heures de rework. C'est ce que fait Cursor implicitement via son index — un agent CLI doit le faire explicitement.

## Exemple

### Cold start sur un projet Next.js + Prisma + PostgreSQL

```
[Scan package.json → Next.js 14, React 18, Prisma 5, Tailwind 3, Vitest]
[Scan structure → app/, lib/, prisma/, tests/]
[Scan entry → app/layout.tsx, app/page.tsx]
[Scan config → next.config.js, tailwind.config.ts, prisma/schema.prisma]
[Scan git log → conventional commits, 200 commits]

→ Écrit .ai/context/architecture.md
  "Next.js App Router + Prisma + PostgreSQL. Layout-based routing.
   Auth via NextAuth. API routes dans app/api/. Services dans lib/services/."

→ Écrit .ai/context/tech-stack.md
  "Next.js 14.2, React 18, Prisma 5.1, PostgreSQL 16, Tailwind 3.4,
   Vitest 1.6, Playwright 1.45, Vercel hosting."

→ Écrit .ai/context/conventions.md
  "Single quotes, 2 spaces, no semicolons. Named exports.
   Conventional commits. Tests colocés (*.test.ts). Error handling via Result type."

→ Écrit MEMORY.md
  "Cold start done. Stack: Next.js 14 + Prisma + PostgreSQL."
```

## Checklist cold start

- [ ] Scan package.json / requirements.txt / etc.
- [ ] Scan structure de dossiers
- [ ] Scan config (build, test, CI/CD)
- [ ] Scan entry points
- [ ] Scan README + docs
- [ ] Scan tests
- [ ] Scan git log
- [ ] Écrire architecture.md
- [ ] Écrire tech-stack.md
- [ ] Écrire conventions.md
- [ ] Écrire glossaire.md (si métier)
- [ ] Vérifier doc via Context7 pour chaque techno
- [ ] Écrire MEMORY.md
- [ ] Mettre à jour progress.md