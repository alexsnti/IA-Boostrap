# Superpowers — Complete dev methodology for coding agents

Méthodologie de développement complète pour agents IA : brainstorming → plan → TDD → subagents → review.

Source : [obra/superpowers](https://github.com/obra/superpowers)

## Contexte d'usage

Superpowers s'active automatiquement dès que l'agent détecte qu'on construit quelque chose. Au lieu de sauter dans le code, il :
1. Questionne pour extraire une spec (brainstorming)
2. Présente le design par chunks validables
3. Crée un plan d'implémentation détaillé (tâches 2-5 min)
4. Lance un processus subagent-driven-development
5. Enforce le TDD red/green
6. Code review entre tâches
7. Vérifie tests + propose merge/PR/keep/discard

## Installation par outil

| Outil | Installation |
|-------|-------------|
| **OpenCode** | `"plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]` dans `opencode.json` (voir `opencode.json` à la racine) |
| Claude Code | `/plugin install superpowers@claude-plugins-official` |
| Antigravity | `agy plugin install https://github.com/obra/superpowers` |
| Codex App | Plugins sidebar → Superpowers → `+` |
| Codex CLI | `/plugins` → rechercher "superpowers" → Install |
| Cursor | `/add-plugin superpowers` dans Agent chat |
| Factory Droid | `droid plugin marketplace add https://github.com/obra/superpowers` puis `droid plugin install superpowers@superpowers` |
| GitHub Copilot CLI | `copilot plugin marketplace add obra/superpowers-marketplace` puis `copilot plugin install superpowers@superpowers-marketplace` |
| Kimi Code | `/plugins` → Marketplace → Superpowers |
| Pi | `pi install git:github.com/obra/superpowers` |

### Windows (OpenCode fallback)

Si le plugin git+https ne charge pas (cache Bun / git.exe introuvable) :

```powershell
npm install superpowers@git+https://github.com/obra/superpowers.git --prefix "$HOME\.config\opencode"
```

Puis dans `opencode.json` :
```json
{
  "plugin": ["~/.config/opencode/node_modules/superpowers"]
}
```

## Skills inclus

### Testing
- **test-driven-development** — Cycle RED-GREEN-REFACTOR (écrit test qui échoue, code minimal qui passe, refactor, commit). Supprime le code écrit avant les tests.

### Debugging
- **systematic-debugging** — Process root cause en 4 phases (root-cause-tracing, defense-in-depth, condition-based-waiting)
- **verification-before-completion** — Vérifier que c'est vraiment fixé avant de déclarer fini

### Collaboration
- **brainstorming** — Raffinement Socratic du design, présenté par chunks validables
- **writing-plans** — Plans d'implémentation avec chemins de fichiers exacts, code complet, étapes de vérification
- **executing-plans** — Exécution par batch avec checkpoints humains
- **dispatching-parallel-agents** — Workflows subagents concurrents
- **requesting-code-review** — Checklist pre-review
- **receiving-code-review** — Réponse aux feedbacks
- **using-git-worktrees** — Branches de dev parallèles isolées
- **finishing-a-development-branch** — Décision merge/PR/keep/discard
- **subagent-driven-development** — Itération rapide avec two-stage review (spec compliance, puis code quality)

### Meta
- **writing-skills** — Créer de nouveaux skills en suivant les best practices
- **using-superpowers** — Intro au système de skills

## Workflow de base

1. **brainstorming** — Avant d'écrire du code. Affine l'idée par questions, explore alternatives, sauvegarde un doc de design.
2. **using-git-worktrees** — Après validation du design. Crée workspace isolé sur nouvelle branche.
3. **writing-plans** — Découpe en tâches 2-5 min avec paths exacts, code complet, étapes de vérification.
4. **subagent-driven-development** / **executing-plans** — Dispatch un subagent frais par tâche avec two-stage review.
5. **test-driven-development** — Pendant l'implémentation. Enforce RED-GREEN-REFACTOR.
6. **requesting-code-review** — Entre tâches. Issues par sévérité. Critical bloque.
7. **finishing-a-development-branch** — Vérifie tests, propose options, cleanup worktree.

## Philosophie

- **TDD first** — Toujours écrire les tests d'abord
- **Systematic over ad-hoc** — Processus > guessing
- **Complexity reduction** — Simplicité comme objectif primaire
- **Evidence over claims** — Vérifier avant de déclarer succès

## Telemetry

Optionnel (logo Prime Radiant sur le visual companion). Pour désactiver :
```bash
export SUPERPOWERS_DISABLE_TELEMETRY=1
# ou
export DISABLE_TELEMETRY=1
export CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1
```

## Critères de succès

- L'agent brainstorm avant de coder.
- TDD enforced (test échoue → code → test passe → commit).
- Plan détaillé avec tâches 2-5 min avant exécution.
- Code review entre tâches, critical issues bloquent.
- Tests verts avant merge/PR.