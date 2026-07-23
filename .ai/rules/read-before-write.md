# Read-before-write — Lire avant d'écrire

## Règle

Avant toute modification d'un fichier, **lire le fichier et ses voisins** pour comprendre le pattern existant.

Étapes obligatoires :
1. Lire le fichier cible en entier (pas juste la fonction à modifier).
2. Identifier les conventions du fichier : nommage, structure, imports, error handling, style.
3. Lire 1-2 fichiers voisins du même module pour confirmer le pattern.
4. Chercher les callers/callees de la fonction touchée (grep).
5. Match le pattern existant dans l'edit.

Ne **jamais** écrire du code qui introduit un style différent du reste du fichier :
- Si le fichier utilise des arrow functions → arrow functions.
- Si le fichier utilise `const` → `const`.
- Si le fichier n'a pas de commentaires → pas de commentaires.
- Si le fichier gère les erreurs avec Result/Either → Result/Either.

## Justification

Cursor est fort parce qu'il match l'existant. La cohérence avec le codebase est ce qui donne l'impression que "ça marche du premier coup". Un edit qui casse le style local = rework = perte de confiance.

L'agent qui lit avant d'écrire :
- Ne réinvente pas un pattern déjà présent.
- Ne duplique pas une helper qui existe 3 lignes plus haut.
- Respecte les conventions implicites (async vs sync, named vs default exports, etc.).

## Exemples

### Respect
- Fichier cible utilise `interface` → utiliser `interface`, pas `type`.
- Fichier cible importe avec `@/` → continuer avec `@/`.
- Fonction touchée a 3 callers → lire les 3 avant de changer la signature.

### Violation
- Ajouter un `try/catch` dans un fichier qui utilise Result type partout.
- Renommer en camelCase un fichier snake_case.
- Ajouter un commentaire JSDoc dans un fichier sans aucun commentaire.