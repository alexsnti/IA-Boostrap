# BootstrapIA

Un bootstrap de développement IA **agnostique**, conçu pour reproduire l'expérience Cursor avec n'importe quel agent IA (opencode, Claude Code, Codex, Copilot, Windsurf, Cursor, Gemini CLI, etc.).

## Pourquoi

Cursor est excellent parce qu'il combine trois choses :
1. **Codebase awareness** — il connaît le projet avant d'écrire
2. **Déclenchement contextuel** — les règles s'activent selon les fichiers touchés
3. **Diff review + fast apply** — l'utilisateur garde le contrôle

BootstrapIA reproduit ça de façon agnostique, via une structure `.ai/` portable et des règles qui s'appliquent à tous les agents.

## Démarrage rapide

1. Copiez le dossier `.ai/`, `AGENTS.md`, et `opencode.json` à la racine de votre projet.
2. L'agent fait un **cold start** : il analyse la codebase et remplit `.ai/context/`.
3. C'est tout. L'agent a maintenant les superpowers.

```bash
# Pour opencode
opencode

# L'agent lit AGENTS.md, applique les règles, fait le cold start
```

## Structure

```
.ai/
├── rules/        Règles always-on + manuelles (comme .cursor/rules .mdc)
├── skills/       Compétences IA réutilisables (SKILL.md)
├── plugins/      Configuration MCP servers et extensions
├── context/      Documentation remplie au cold start (archi, stack, conventions)
└── memory/       Mémoire cross-session (décisions, progress, learnings)
AGENTS.md         Point d'entrée lu par tous les agents
MEMORY.md         Journal de session (lu au démarrage, écrit en fin)
opencode.json     Config OpenCode (plugins + MCP)
```

## Règles always-on

| Règle | Rôle |
|-------|------|
| `cold-start.md` | Analyser et documenter la codebase à la première session |
| `auto-plan-mode.md` | Détecte la complexité, propose un plan (avec confirmation) |
| `ponytail.md` | Anti-over-engineering (échelle de paresse : YAGNI → réutiliser → stdlib → natif → une ligne) |
| `read-before-write.md` | Lire fichier + voisins + callers avant d'éditer |
| `anti-hallucination.md` | Vérifier qu'une API/fonction existe avant de l'invoquer |
| `match-existing-conventions.md` | Imiter le style local (diff invisible) |
| `memory.md` | Gestion mémoire cross-session |
| `scoped-rules.md` | Déclenchement contextuel par globs |
| `parallel-agents.md` | Coordination multi-agents via git worktrees |

## Règles manuelles

| Règle | Rôle |
|-------|------|
| `avoid-ai-writing.md` | Audit et réécriture anti AI-isms (57 catégories, 111 entrées) |
| `skillspector.md` | Scanner de sécurité des skills tiers avant installation |

## Plugins

| Plugin | Rôle |
|--------|------|
| `ponytail.md` | [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) — lazy senior dev mode |
| `avoid-ai-writing.md` | [conorbronsdon/avoid-ai-writing](https://github.com/conorbronsdon/avoid-ai-writing) — anti AI-isms |
| `skillspector.md` | [NVIDIA/SkillSpector](https://github.com/NVIDIA/SkillSpector) — sécurité des skills |
| `superpowers.md` | [obra/superpowers](https://github.com/obra/superpowers) — méthodologie dev (TDD, brainstorming, subagents) |
| `vercel-mcp.md` | MCP server Vercel officiel (déploiements, logs, projects) |
| `context7.md` | [upstash/context7](https://github.com/upstash/context7) — doc à jour pour LLMs |

## Skills

| Skill | Rôle |
|-------|------|
| `avoid-ai-writing/SKILL.md` | 76KB, 57 catégories de patterns, 3 tiers de vocabulaire, modes rewrite/detect/edit |

## Compatibilité

| Outil | Config |
|-------|--------|
| **OpenCode** | `opencode.json` + `AGENTS.md` |
| **Claude Code** | `AGENTS.md` + `/plugin install` |
| **Codex CLI** | `AGENTS.md` + `codex mcp add` |
| **Cursor** | `.cursor/rules/` (porter les règles en `.mdc`) |
| **Windsurf** | `.windsurf/rules/` |
| **Cline** | `.clinerules/` |
| **GitHub Copilot** | `.github/copilot/instructions.md` |
| **Gemini CLI** | `AGENTS.md` + `~/.gemini/settings.json` |

## Workflow de l'agent

1. **Cold start** — scan codebase → remplit `.ai/context/` (architecture, tech-stack, conventions, glossaire)
2. **Prompt utilisateur** — l'agent évalue la complexité (`auto-plan-mode`)
3. **Si plan mode** — demande confirmation → produit un plan → validation
4. **Avant d'écrire** — `read-before-write` + `anti-hallucination` + `match-existing-conventions`
5. **Pendant l'écriture** — `ponytail` (échelle de paresse) + vérification doc via `context7`
6. **En fin de session** — écrit `MEMORY.md` + met à jour `.ai/memory/`

## Multi-agents

Plusieurs agents en parallèle sur la même codebase via git worktrees :

```bash
git worktree add ../project-feature-a feature/a
git worktree add ../project-feature-b feature/b

cd ../project-feature-a && opencode   # Agent A
cd ../project-feature-b && opencode   # Agent B
```

Coordination via `.ai/memory/progress.md` (chaque agent déclare son workstream + fichiers touchés).

## Licence

MIT