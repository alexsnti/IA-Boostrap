# Scoped-rules — Déclenchement contextuel par globs

## Règle

Les règles ne s'appliquent pas toutes en permanence. Chaque règle a un **scope de déclenchement** basé sur les fichiers touchés, comme les `.mdc` Cursor.

Format d'en-tête attendu pour chaque fichier de règle :

```markdown
# <Nom règle>

## Scope
Globs: **/*.ts, **/api/**, src/components/**
Trigger: manual | glob | always
```

### Types de trigger

| Trigger | Quand s'active |
|---------|----------------|
| `always` | À chaque prompt, quel que soit le fichier. Ex: auto-plan-mode, ponytail. |
| `glob` | Quand un fichier match le glob est touché. Ex: règles TS sur `**/*.ts`. |
| `manual` | Quand l'utilisateur ou l'agent invoque explicitement la règle. Ex: avoid-ai-writing. |

### Mapping actuel

| Règle | Trigger | Globs |
|-------|---------|-------|
| auto-plan-mode | always | — |
| ponytail | always | — |
| read-before-write | always | — |
| anti-hallucination | always | — |
| match-existing-conventions | always | — |
| avoid-ai-writing | manual | `**/*.md`, `**/*.mdx`, `**/*.txt` |
| skillspector | manual | — |

### Règles à créer par framework (exemples)

| Règle | Trigger | Globs |
|-------|---------|-------|
| typescript-conventions | glob | `**/*.ts`, `**/*.tsx` |
| react-conventions | glob | `**/*.tsx`, `**/*.jsx` |
| python-conventions | glob | `**/*.py` |
| rust-conventions | glob | `**/*.rs` |
| nextjs-conventions | glob | `**/app/**`, `**/pages/**` |
| testing-conventions | glob | `**/*.test.*`, `**/*.spec.*`, `**/tests/**` |

Ces règles framework sont à créer au besoin selon la stack du projet cible.

## Justification

Cursor n'active pas toutes ses règles en permanence — il attache le contexte utile selon les fichiers édités. Ça évite le bruit (règle Rust active sur un projet TS) et garde chaque prompt léger.

L'agent doit, avant chaque prompt :
1. Identifier les fichiers touchés.
2. Charger les règles `always` + les règles `glob` qui matchent.
3. Ignorer les autres.

## Exemple

### Prompt : "ajoute une fonction util dans utils.ts"
- Fichiers touchés : `utils.ts` → globs `**/*.ts`
- Règles chargées : auto-plan-mode, ponytail, read-before-write, anti-hallucination, match-existing-conventions, typescript-conventions (si existe)
- Règles ignorées : avoid-ai-writing (pas .md), python-conventions (pas .py)

### Prompt : "nettoie le README"
- Fichiers touchés : `README.md` → globs `**/*.md`
- Règles chargées : auto-plan-mode, ponytail, read-before-write, avoid-ai-writing, match-existing-conventions
- Règles ignorées : typescript-conventions, etc.