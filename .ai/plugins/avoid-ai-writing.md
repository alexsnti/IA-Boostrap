# Avoid AI Writing — Audit & Rewrite

Skill qui audite et réécrit le contenu pour retirer les patterns d'écriture IA ("AI-isms").

Source : [conorbronsdon/avoid-ai-writing](https://github.com/conorbronsdon/avoid-ai-writing)

## Contexte d'usage

Utiliser ce skill quand on demande de :
- "remove AI-isms"
- "clean up AI writing"
- "edit writing for AI patterns"
- "audit writing for AI tells"
- "make this sound less like AI"

## Modes

| Mode | Action | Déclencheur |
|------|--------|-------------|
| `rewrite` (défaut) | Flag les AI-isms et réécrit le texte | par défaut |
| `detect` | Flag seulement, sans réécriture | "detect", "flag only", "audit only", "scan" |
| `edit` | Édite un fichier in-place avec changements minimaux | "fix file.md in place", "clean up this file directly" |

**Voix optionnelles** : casual / professional / technical / warm / blunt

## Installation par outil

| Outil | Installation |
|-------|-------------|
| Claude Code | `git clone https://github.com/conorbronsdon/avoid-ai-writing ~/.claude/skills/avoid-ai-writing` OU plugin : `/plugin marketplace add conorbronsdon/avoid-ai-writing` puis `/plugin install avoid-ai-writing@conorbronsdon-skills` |
| OpenClaw | `clawhub install avoid-ai-writing` |
| Cursor | `curl -o .cursor/rules/avoid-ai-writing.mdc https://raw.githubusercontent.com/conorbronsdon/avoid-ai-writing/main/cursor-rules/avoid-ai-writing.mdc` |
| Hermes | `curl -o ~/.hermes/skills/writing/avoid-ai-writing/SKILL.md https://raw.githubusercontent.com/conorbronsdon/avoid-ai-writing/main/SKILL.md` |
| Codex | `curl -o .agents/skills/avoid-ai-writing/SKILL.md https://raw.githubusercontent.com/conorbronsdon/avoid-ai-writing/main/SKILL.md` |
| Windsurf | Copier dans `.windsurf/rules/avoid-ai-writing.md` |
| Cline | Copier dans `.clinerules/avoid-ai-writing.md` |
| Copilot (VS Code) | Coller dans `.github/copilot-instructions.md` |
| Claude.ai Projects | Coller SKILL.md dans les custom instructions |
| ChatGPT Custom GPTs | Coller SKILL.md dans le champ Instructions |

## SKILL.md local

Le SKILL.md complet est stocké localement dans :
`.ai/skills/avoid-ai-writing/SKILL.md`

Il contient le ruleset complet :
- 57 catégories de patterns avec before/after
- 111 entrées de remplacement de mots sur 3 tiers
- Détection en deux passes (pass 2 rattrape ce qui survit au pass 1)
- Modes rewrite / detect / edit
- Profils de contexte : linkedin / blog / technical-blog / investor-email / docs / casual
- Déterminateur optionnel : `detector/` (Node >=18, zéro dépendance)

Les agents IA doivent lire ce fichier avant tout audit ou réécriture de contenu.

## Critères de succès

- Le texte réécrit ne contient plus les AI-isms identifiés.
- Les passages déjà humains sont préservés (mode edit).
- Le citation markup, placeholders, et paramètres URL IA sont retirés.
- Une seconde passe confirme qu'aucun pattern ne survit.