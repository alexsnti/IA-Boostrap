# Anti-hallucination — Vérifier avant d'invoquer

## Règle

Ne **jamais** invoquer une API, fonction, import, propriété, ou méthode sans avoir vérifié qu'elle existe.

Avant d'écrire un appel :

1. **Imports** : vérifier que le module/package est dans `package.json`, `requirements.txt`, `Cargo.toml`, etc. Ne pas inventer un import.
2. **Fonctions/methodes** : grep le nom exact dans le codebase. Si introuvable, ne pas l'inventer — chercher l'alternative réelle.
3. **APIs stdlib/framework** : si incertain, lire la doc ou grep un exemple dans le codebase plutôt que deviner la signature.
4. **Propriétés d'objets** : lire le type/interface avant d'accéder à `.foo`. Ne pas supposer qu'un objet a un champ.
5. **Versions** : vérifier la version installée avant d'utiliser une API récente. Ne pas supposer "last version".
6. **Si incertain après vérification** : dire "Je ne suis pas sûr que X existe, je vérifie" plutôt qu'écrire un appel plausible mais faux.

## Patterns à bloquer

- **Imports fantômes** : `import { fancyHook } from 'react'` qui n'existe pas dans React.
- **Signatures inventées** : `array.filter(predicate, thisArg, extraOption)` où `extraOption` n'existe pas.
- **APIs dépréciées présentées comme actuelles** : utiliser `componentWillMount` en 2026.
- **Mix de versions** : utiliser une API v5 sur une install v4.
- **Noms proches mais faux** : `useEffet` au lieu de `useEffect`, `Array.flaten` au lieu de `Array.flat`.
- **Props inventées** : `<Button variant="ghostly">` quand le composant ne supporte que `ghost`.

## Justification

Les hallucinations sont la cause #1 de perte de confiance en les agents IA. Un seul appel inventé casse la build et oblige l'utilisateur à devenir reviewer au lieu d'être reviewer-passif. L'impression de "qualité Cursor" vient du fait que ses outputs compilent et passent les types.

## Comment vérifier

| Type | Vérification |
|------|--------------|
| Import stdlib | Read doc / grep dans node_modules |
| Import local | grep `from '@/path'` dans codebase |
| Méthode d'objet | Read le type / interface |
| API framework | grep un usage existant dans le codebase |
| Package externe | Read `package.json` + grep dans `node_modules/<pkg>` |
| Version | Read lock file / `pkg --version` |

## Exemples

### Respect
- "Vérifions que `useTransition` existe dans cette version de React" → grep → oui → utiliser.
- "Ce composant Button accepte-t-il `size`?" → read Button.tsx → non → ne pas utiliser.

### Violation
- Écrire `import { useResource } from 'react'` sans vérifier (ça n'existe pas).
- Écrire `db.query(sql, { timeout: 5000 })` sans vérifier que `query` accepte `timeout`.