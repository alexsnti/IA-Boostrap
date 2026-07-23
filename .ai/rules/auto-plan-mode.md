# Auto-détection du plan mode

## Règle

Avant d'exécuter un prompt, évaluer sa complexité pour décider du mode :

### Mode PLAN (plan d'abord, validation avant exécution)

Déclencher le plan mode si le prompt présente **au moins un** de ces signaux :

- **Multi-fichiers** : touche 3+ fichiers ou 2+ modules distincts
- **Multi-étapes** : implique une séquence logique (ex: "ajoute auth + tests + migration")
- **Refactor** : restructure du code existant (renommage, extraction, déplacement)
- **Architecture** : ajoute/supprime une couche, un pattern, une dépendance
- **Incertain** : la spec est floue, plusieurs approches possibles
- **Mots-clés** : "plan", "design", "comment tu ferais", "approche", "stratégie", "refactor", "architecture", "migration", "rewrite", "redesign"
- **Impact élevé** : sécurité, data migration, breaking change, déploiement
- **Effort estimé** : > 15 min de travail

**Action en plan mode** :
1. **Demander confirmation d'activation** : "Plan mode détecté pour cette tâche. Tu veux que je planifie d'abord avant de coder ?"
2. Si l'utilisateur refuse → mode DIRECT (exécuter immédiatement)
3. Si l'utilisateur accepte → NE PAS écrire de code immédiatement
4. Explorer le codebase concerné (grep, glob, read)
5. Produire un plan structuré :
   - Objectif (1 phrase)
   - Approche choisie + alternatives écartées (pourquoi)
   - Étapes numérotées avec fichiers touchés
   - Risques et mitigations
   - Critères de validation (tests, checks)
6. Demander validation explicite du plan ("Tu valides ce plan ?")
7. Après validation → exécuter (ou déléguer à subagents)

### Mode DIRECT (exécution immédiate)

Exécuter directement si le prompt est :
- **Single-file** : 1-2 fichiers, scope local
- **Trivial** : typo, rename local, ajout d'une petite fonction, fix obvious
- **Exploratoire** : "où est X", "comment marche Y", "montre-moi Z"
- **Effort estimé** : < 15 min
- **Pas de risque** : pas de breaking change, pas de migration, pas de sécurité

**Action en direct mode** : faire la modif, lancer lint/typecheck, confirmer.

### Mode MIXTE (rare)

Si le prompt est simple mais touche un système critique (sécurité, données), annoncer brièvement l'approche en 2-3 lignes avant d'exécuter, sans plan complet.

## Justification

- Évite le gaspillage : un plan complet pour un typo = overhead inutile.
- Évite les catastrophes : exécuter direct sur un refactor multi-fichiers = bugs en cascade.
- Reproduit le comportement plan/act de Cursor sans config supplémentaire.
- Le plan mode permet à l'utilisateur de corriger l'approche avant tout coût de code.

## Signaux de détection (résumé decisionnel)

```
PROMPT → complexité ?
├── simple (1-2 fichiers, <15min, pas de risque) → DIRECT
├── complexe (3+ fichiers, refactor, archi, spec floue) → PLAN
└── critique + simple → MIXTE (annonce 2-3 lignes puis execute)
```

## Exemples

### DIRECT
- "Renomme `foo` en `bar` dans utils.ts" → 1 fichier, trivial, direct
- "Ajoute un TODO comment dans le handler" → 1 fichier, direct
- "Où est défini `connectToServer` ?" → exploratoire, pas de code, direct

### PLAN
- "Ajoute l'authentification OAuth2 + refresh tokens + tests" → multi-étapes, sécurité → PLAN
- "Refactorise le module DB en repository pattern" → refactor archi → PLAN
- "Migrer de Redux à Zustand" → refactor global, breaking → PLAN
- "Comment tu ferais un système de cache distribué ?" → mot-clé "comment tu ferais" → PLAN

### MIXTE
- "Corrige la requête SQL injectée dans `getUserById`" → critique (sécurité) mais 1 fichier → annonce "Je sanitize le paramètre avec un prepared statement" puis exécute

## Comportement attendu de l'agent

- Si plan mode détecté : afficher "Plan mode détecté" puis demander "Tu veux que je planifie d'abord avant de coder ?" et attendre la réponse avant de continuer.
- Si l'utilisateur refuse l'activation → basculer en DIRECT et exécuter.
- Si l'utilisateur accepte → produire le plan puis demander validation du plan lui-même.
- Ne jamais sauter le plan mode si un signal de complexité est présent, même si l'utilisateur dit "juste fais-le" → proposer le plan d'abord et demander confirmation.
- En cas de doute sur la complexité → privilégier PLAN, mais toujours demander à l'utilisateur.