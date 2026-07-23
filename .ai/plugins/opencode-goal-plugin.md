# OpenCode Goal Plugin — Goal mode Codex-style pour OpenCode

Plugin OpenCode qui ajoute un mode "goal" long-running façon Codex : commande `/goal`, état persistant, evidence de complétion, continuation auto sur idle, et indicateur de goal dans le TUI.

Source : [prevalentWare/opencode-goal-plugin](https://github.com/prevalentWare/opencode-goal-plugin)
npm : [`@prevalentware/opencode-goal-plugin`](https://www.npmjs.com/package/@prevalentware/opencode-goal-plugin)

## Contexte d'usage

Quand on veut qu'OpenCode reste focus sur un objectif long (refactor, migration, review, fix de tests) au lieu de traiter prompt par prompt. Le goal reste visible, survit à la compaction de session, peut continuer auto quand la session devient idle, et ne se ferme qu'avec evidence explicite ou un blocker concret.

Sans le plugin : l'agent perd le fil sur les tâches longues, oublie l'objectif après compaction.
Avec le plugin : objectif persistant, continuation auto, fermeture conditionnée par evidence.

## Installation

### Via commande OpenCode (recommandé)

```bash
opencode plugin @prevalentware/opencode-goal-plugin
```

Global :

```bash
opencode plugin -g @prevalentware/opencode-goal-plugin
```

OpenCode détecte les entrypoints package et écrit le plugin dans la config server + TUI.

### Config manuelle

Ajouter le package dans **les deux** fichiers de config.

`opencode.json` :
```json
{
  "plugin": ["@prevalentware/opencode-goal-plugin"]
}
```

`tui.json` :
```json
{
  "plugin": ["@prevalentware/opencode-goal-plugin"]
}
```

## Options server

Configurables dans `opencode.json` :

```json
{
  "plugin": [
    [
      "@prevalentware/opencode-goal-plugin",
      {
        "auto_continue": true,
        "defer_while_tasks_active": true,
        "max_auto_turns": 25,
        "min_continue_interval_seconds": 3,
        "max_turn_time": 300,
        "max_prompt_failures": 3,
        "default_token_budget": 200000,
        "max_goal_duration_seconds": 1800,
        "no_progress_token_threshold": 50,
        "max_no_progress_turns": 2,
        "restricted_agents": ["plan"],
        "allow_goal_execution_from_plan": false
      }
    ]
  ]
}
```

### Defaults

| Option | Default | Rôle |
|--------|---------|------|
| `auto_continue` | `true` | Continuation auto sur idle |
| `defer_while_tasks_active` | `true` | Attend les Task children + orchestrator avant continuation |
| `max_auto_turns` | `25` | Budget de turns auto |
| `min_continue_interval_seconds` | `3` | Intervalle min entre continuations |
| `max_turn_time` | unset | Watchdog : retry un prompt de continuation si un turn reste busy X secondes |
| `max_prompt_failures` | `3` | Échecs de prompt avant pause |
| `default_token_budget` | unset | Budget tokens hérité par les nouveaux goals |
| `max_goal_duration_seconds` | unset | Limite de temps écoulé |
| `no_progress_token_threshold` | `50` | Seuil output-tokens pour juger le progress d'un turn |
| `max_no_progress_turns` | `2` | Turns low-progress consécutifs avant pause |
| `register_command` | `true` | Enregistre la commande `/goal` |
| `command_name` | `"goal"` | Nom de la slash command |
| `restricted_agents` | `["plan"]` | Agents traités comme planning-only |
| `allow_goal_execution_from_plan` | `false` | Désactive les restrictions Plan-mode si `true` (non recommandé) |

## Commandes `/goal`

| Commande | Action |
|----------|--------|
| `/goal <objective>` | Crée un goal long-running |
| `/goal` | Reporte l'état courant |
| `/goal history` | Historique du lifecycle + checkpoints récents |
| `/goal edit <objective>` | Met à jour l'objectif courant |
| `/goal pause` | Pause sans clear |
| `/goal resume` | Reprend |
| `/goal clear` | Clear (aliases : `stop`, `off`, `reset`, `none`, `cancel`) |

Le TUI expose aussi une entrée `Goal` dans la command palette (view, refresh, pause, resume, history, clear).

L'agent peut aussi formuler l'objectif lui-même via `set_goal` quand on lui demande ("set your own goal to finish this refactor safely").

## Outils agent exposés

| Outil | Description |
|-------|-------------|
| `get_goal` | État courant |
| `get_goal_history` | Historique + checkpoints |
| `create_goal` | Crée un goal |
| `set_goal` | Définit l'objectif (goal créé seulement si explicitement demandé) |
| `update_goal_objective` | Met à jour l'objectif |
| `update_goal` | Met à jour l'état (`complete` + evidence, `unmet` + blocker) |
| `clear_goal` | Clear |

### Fermeture d'un goal

- `status: "complete"` → requiert `evidence` (vérification réelle : fichiers, commandes, tests, PR).
- `status: "unmet"` → requiert `blocker` concret.

### États de safety

- `budgetLimited` — budget tokens épuisé.
- `usageLimited` — budget auto-turns ou temps écoulé.
- `paused` — user pause, échecs répétés, ou low-progress répété.

Sur safety limit, le plugin envoie un wrap-up prompt demandant un handoff concis (pas de continuation silencieuse infinie).

## Plan Mode safety

Le mode Plan d'OpenCode est une boundary user-controlled ; le goal mode ne doit pas devenir une escape hatch :

- Goals créés depuis l'agent `plan` → enregistrés `paused` (stop reason `plan mode`), jamais actifs.
- Auto-continuation supprimée si le dernier prompt vient d'un agent restreint.
- `resume` refusé depuis Plan mode (pas d'auto-escalade via prompt injecté).
- Continuation prompts pinés à l'agent du dernier user prompt.
- Le goal system reminder devient planning-only après un prompt Plan-mode.

`restricted_agents` (default `["plan"]`) définit les agents planning-only. `allow_goal_execution_from_plan: true` opt-out total (défaut sécurisé `false`).

## State

Stocké à :
```
$XDG_DATA_HOME/opencode-goal-plugin/goals.json
# fallback
~/.local/share/opencode-goal-plugin/goals.json
```

Variable `OPENCODE_GOAL_STATE_PATH` pour un fichier custom. Écriture atomique avec permissions owner-only. Les goals actifs recover depuis le disk avec objectif, budget, history, checkpoints.

## Compaction

Pendant la compaction, le plugin désactive l'auto-continue générique d'OpenCode quand un goal actif existe — le continuation prompt goal-specific reste autoritaire. Le goal actif est préservé dans le context de compaction.

## TUI sidebar

Affiche : statut, temps écoulé, usage tokens, auto-continue count, dernier checkpoint, dernier status message, stop reason, objectif. Les goals fermés restent visibles brièvement (achieved / unmet) via le latest tool state.

## Critères de succès

- L'agent reste focus sur un objectif long sans perdre le fil après compaction.
- `/goal` crée/pilote un goal persistant dans TUI, desktop, et web.
- Un goal ne se ferme qu'avec evidence vérifiée ou un blocker concret.
- Plan mode reste une boundary : pas d'auto-escalade vers Build.
- Continuation auto respecte budgets (turns, tokens, temps, no-progress).