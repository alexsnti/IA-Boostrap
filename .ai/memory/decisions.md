# Decisions — Registre des décisions techniques (ADR-like)

Ce fichier accumule les décisions architecturales et techniques du projet, au format ADR (Architecture Decision Record).

## Format

```
### ADR-XXX — [Titre]
- Date : YYYY-MM-DD
- Statut : proposed | accepted | deprecated | superseded
- Contexte : [pourquoi on doit décider]
- Décision : [ce qu'on a choisi]
- Alternatives : [ce qu'on a écarté, et pourquoi]
- Conséquences : [impact, tradeoffs]
```

## Décisions

### ADR-001 — Structure agnostique `.ai/`
- Date : 2026-07-23
- Statut : accepted
- Contexte : Le bootstrap doit fonctionner avec n'importe quel agent IA (Cursor, opencode, Claude Code, Copilot, Windsurf, etc.).
- Décision : Centraliser les ressources dans `.ai/` (rules, skills, plugins, context, memory) + `AGENTS.md` comme point d'entrée.
- Alternatives : Structure spécifique par outil (`.cursor/`, `.opencode/`, `.claude/`) — écarté car verrouille l'outil et duplique le contenu.
- Conséquences : Un seul dossier à maintenir. Compatibilité via fichiers de pont (`.cursorrules`, `.clinerules`, `.github/copilot/instructions.md`).

### ADR-002 — SKILL.md stocké localement
- Date : 2026-07-23
- Statut : accepted
- Contexte : Le skill avoid-ai-writing a un SKILL.md de 76KB. URL distante vs copie locale ?
- Décision : Copier le SKILL.md localement dans `.ai/skills/avoid-ai-writing/SKILL.md`.
- Alternatives : Pointer vers l'URL distante — écarté car le bootstrap doit rester autonome, sans dépendance réseau.
- Conséquences : Mise à jour manuelle nécessaire. Le bootstrap est portable et offline-ready.

### ADR-003 — Plan mode demande confirmation
- Date : 2026-07-23
- Statut : accepted
- Contexte : Le plan mode peut être activé automatiquement selon la complexité du prompt.
- Décision : L'agent demande confirmation avant d'activer le plan mode ("Tu veux que je planifie d'abord avant de coder ?").
- Alternatives : Activation automatique sans confirmation — écarté car trop intrusif pour les users qui veulent de la vitesse.
- Conséquences : Un prompt supplémentaire pour les tâches complexes, mais l'utilisateur garde le contrôle.

### ADR-004 — Mémoire hybride MEMORY.md + memory/
- Date : 2026-07-23
- Statut : accepted
- Contexte : Besoin de mémoire cross-session sans surcharger les petites sessions.
- Décision : `MEMORY.md` à la racine pour les sessions courtes + dossier `.ai/memory/` structuré pour les projets longs.
- Alternatives : Uniquement MEMORY.md (trop plat pour gros projet), uniquement memory/ (trop lourd pour petite session).
- Conséquences : L'agent choisit selon la complexité. Archive automatique quand MEMORY.md dépasse 10 sessions.