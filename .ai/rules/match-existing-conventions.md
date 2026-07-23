# Match-existing-conventions — Imiter le style local

## Règle

Tout edit doit imiter le style du fichier édité et du module environnant. La règle d'or : **un diff doit être invisible** — un reviewer ne doit pas pouvoir distinguer ce que l'agent a écrit de ce qu'un humain du projet aurait écrit.

Checklist avant de produire un edit :

### Style de code
- Indentation (tabs/spaces, largeur)
- Quotes (single/double/backtick)
- Semicolons (présents/absents)
- Trailing commas (présentes/absentes)
- Naming (camelCase / snake_case / PascalCase / kebab-case)
- Arrow vs function declarations
- `const` vs `let` vs `var`
- Default exports vs named exports
- Async/await vs .then()

### Structure
- Ordre des imports (stdlib → externes → internes → relatifs)
- Organisation des fonctions (helpers en haut vs bas)
- Gestion d'erreurs (try/catch vs Result vs throw vs null)
- Patterns de logging (console.log vs logger vs debug)
- Commentaires (présents ? quel style ? JSDoc ? inline ?)
- Tests (colocés ? dossier tests ? framework ?)

### Framework-spécifique
- Patterns de composants (functional vs class, hooks vs HOC)
- State management ( Redux vs Zustand vs Context vs Pinia)
- Styling (CSS modules vs Tailwind vs styled-components vs CSS-in-JS)
- Routing (file-based vs config-based)
- Data fetching (SWR vs React Query vs Apollo vs fetch natif)

## Justification

La cohérence de style est ce qui fait qu'un codebase reste maintenable. Un agent qui introduit son propre style fragmente le codebase et oblige les devs à réaligner manuellement. Cursor est bon parce qu'il imite — il ne "standardise pas", il "match".

Un diff invisible = pas de rework = confiance.

## Comment appliquer

1. Lire le fichier cible (déjà couvert par read-before-write).
2. Identifier 3-5 conventions clés (quotes, naming, exports, error handling).
3. Appliquer ces conventions à l'edit, même si ça contredit vos préférences personnelles.
4. Si un pattern est ambigu, regarder 2-3 fichiers voisins pour trancher.

## Exemples

### Respect
- Fichier en 2 spaces, single quotes, no semicolons → l'edit suit.
- Module utilise `Result<T, E>` → l'edit retourne `Result`, pas `try/catch`.
- Composant utilise Tailwind classes → l'edit ajoute Tailwind, pas inline styles.

### Violation
- Fichier sans commentaires → l'agent ajoute 5 commentaires JSDoc.
- Module en snake_case → l'agent introduit `getUserData` en camelCase.
- Tests en Vitest → l'agent écrit `describe/it` Jest.