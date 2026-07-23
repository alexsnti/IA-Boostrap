# Ponytail — Anti-over-engineering

## Règle

Appliquer le mode "lazy senior dev" : avant d'écrire du code, grimper l'échelle de paresse et s'arrêter au premier barreau qui tient :

1. Est-ce que ça a besoin d'exister ? (YAGNI)
2. Existe déjà dans le codebase ? Réutiliser.
3. La stdlib le fait ? L'utiliser.
4. Une feature native de la plateforme le couvre ? L'utiliser.
5. Une dépendance déjà installée le résout ? L'utiliser.
6. Ça tient en une ligne ? Une ligne.
7. Sinon : le minimum qui marche.

## Justification

Moins de code = moins de bugs, moins de maintenance, moins de coût, moins de latence.
Mesuré : ~54% de code en moins, ~20% moins cher, ~27% plus rapide, 100% safe.
La paresse porte sur la solution, jamais sur la compréhension du problème.

## Exemples

### Respect
- Besoin d'un date picker → `<input type="date">` (natif), pas flatpickr + wrapper + CSS.
- Une fonction existe déjà dans `utils.ts` → l'appeler, ne pas la réécrire.
- Bug sur un appelant → grep tous les appelants, fixer la fonction partagée une fois.

### Violation
- Ajouter une abstraction "au cas où on en ait besoin plus tard".
- Installer une lib pour faire un `Array.map`.
- Wrapper un composant natif dans un composant custom pour "consistance".

## Ce qui n'est jamais paresseux

- Compréhension du problème (lire le code, tracer le flux).
- Validation des entrées aux frontières de confiance.
- Gestion d'erreurs qui prévient la perte de données.
- Sécurité.
- Accessibilité.
- Calibration matérielle réelle.
- Tout ce qui est explicitement demandé.
- Non-trivial logic laisse UN check exécutable (assert ou test minimal).