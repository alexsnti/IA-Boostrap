# Avoid AI Writing — Anti AI-isms

## Règle

Tout contenu généré par l'IA doit être audité et nettoyé des patterns d'écriture IA ("AI-isms") avant publication.

Patterns principaux à détecter et corriger :

### Formatting
- Em dashes (— et --), bold excessif, emoji dans headers, listes à puces excessives, guillemets curly en plain-text.

### Structure
- "It's not X — it's Y" / négation-révélation → réécrire en affirmation directe.
- Intensificateurs creux : genuine, truly, quite frankly, it's worth noting → couper.
- Endossement vague : "worth reading/checking out" → dire *pourquoi*.
- Hedging : perhaps, could potentially → faire le point directement.
- Triades compulsives (règle de trois) → varier (2, 4, ou phrase complète).

### Mots à remplacer (3 tiers)

**Tier 1 — Toujours remplacer** : delve, landscape (métaphore), tapestry, realm, paradigm, embark, beacon, testament to, robust, comprehensive, cutting-edge, leverage (verbe), pivotal, underscores, meticulous, seamless, game-changer, utilize, watershed moment, nestled, vibrant, thriving, showcasing, deep dive, unpack, bustling, intricate, complexities, ever-evolving, holistic, actionable, impactful, learnings, thought leader, best practices, synergy, interplay, in order to, due to the fact that, serves as, features (verbe), boasts, commence, ascertain, endeavor, symphony, embrace (métaphore).

**Tier 2 — Flag en cluster (2+ même paragraphe)** : harness, navigate, foster, elevate, unleash, streamline, empower, bolster, spearhead, resonate, revolutionize, facilitate, underpin, nuanced, crucial, multifaceted, ecosystem (métaphore), myriad, plethora, catalyze, reimagine, galvanize, augment, cultivate, illuminate, juxtapose, cornerstone, paramount, poised, burgeoning, nascent, quintessential, overarching, deeply (collocations), underpinning.

**Tier 3 — Flag par densité (~3%+ des mots)** : significant, innovative, effective, dynamic, scalable, compelling, unprecedented, exceptional, remarkable, sophisticated, instrumental, world-class.

### Content patterns
- **Inflation de significance** : "marking a pivotal moment" → state what happened.
- **Name-dropping de notoriété** : piles de citations prestigieuses → une référence avec contexte.
- **Attributions vagues** : "experts believe", "studies show" → citer la source ou couper.
- **Validation tierce vague** : "independent testing confirms" → nommer source + test + résultat.
- **Analyses -ing superficielles** : "symbolizing… reflecting… showcasing…" → faits spécifiques ou couper.
- **Langage promotionnel** : "nestled within", "vibrant hub" → description simple.
- **Défis formulaires** : "Despite challenges… continues to thrive" → nommer le défi et la réponse.

### Communication patterns
- **Artefacts chatbot** : "I hope this helps!", "Certainly!", "Great question!" → retirer.
- **"Let's"** : "Let's explore", "Let's break this down" → commencer par le point.
- **Disclaimers de cutoff** : "As of my last update" → trouver l'info ou couper.
- **Conclusions génériques** : "The future looks bright", "Only time will tell" → fermeture spécifique ou couper.
- **Flatline émotionnelle** : "What surprised me most" → gagner l'émotion ou couper.
- **Artefacts de raisonnement** : "Let me think step by step" → conclusion puis evidence.
- **Ton sycophantique** : "Great question!", "You're absolutely right!" → retirer.
- **Boucles d'acknowledgment** : "You're asking about", "To answer your question" → répondre directement.
- **Calibration de confiance** : "Interestingly", "Notably", "Certainly" → laisser le fait parler.

### Meta patterns
- **Structure excessive** : 5 headers dans 200 mots → fusionner.
- **Rythme uniforme** : toutes phrases 15-25 mots → varier court/long/fragments.
- **Over-polishing** : irrégularités sandées → garder disfluence naturelle.
- **Seuil rewrite-vs-patch** : 5+ flags vocab + 3+ catégories + rythme uniforme → réécriture complète.

### AI-tool fingerprints
- **Placeholders non remplis** : `[Your Name]`, `2025-XX-XX` → fill or delete.
- **Citation markup** : `citeturn0search0`, `oai_citation` → strip.
- **Paramètres URL IA** : `utm_source=chatgpt.com` → strip.

## Justification

Les patterns d'écriture IA sont statistiquement plus fréquents en sortie LLM, mais aussi produits par des humains en pression de deadline ou en seconde langue. False-positive rates >60% sur non-native English writers (Liang et al., Stanford 2023).

**Signaux, pas preuves.** À utiliser pour nettoyer son propre writing et évaluer si un texte sonne IA, jamais comme base unique d'une décision conséquente.

## Exemple

### Before (AI)
> Certainly! Acme Analytics, a vibrant startup nestled in the heart of Boulder's thriving tech ecosystem, has secured $40M in Series B funding — marking a watershed moment for the observability landscape.

### After (clean)
> Acme Analytics raised a $40M Series B led by Sequoia. The Boulder-based startup makes an observability platform that runs queries in under a second.