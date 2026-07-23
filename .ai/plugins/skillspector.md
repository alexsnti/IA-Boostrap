# SkillSpector — Security scanner for AI agent skills

Scanner de sécurité pour les skills d'agents IA. Détecte vulnérabilités, patterns malveillants et risques de sécurité avant installation.

Source : [NVIDIA/SkillSpector](https://github.com/NVIDIA/SkillSpector)

## Contexte d'usage

À utiliser **avant d'installer un skill** tiers pour répondre à : "Est-ce que ce skill est safe à installer ?"

Recherche NVIDIA 2026 : 26.1% des skills contiennent des vulnérabilités, 5.2% montrent une intention probablement malveillante. Les skills avec scripts exécutables sont 2.12x plus vulnérables.

## Installation

```bash
# Avec uv (CLI-only)
uv tool install git+https://github.com/NVIDIA/skillspector.git

# Avec MCP extra (pour usage MCP server)
uv tool install 'skillspector[mcp] @ git+https://github.com/NVIDIA/skillspector.git'

# Docker (sans Python)
docker build -t skillspector .
docker run --rm -v "$PWD:/scan" skillspector scan ./my-skill/ --no-llm
```

## Utilisation

```bash
# Scanner un dossier de skill
skillspector scan ./my-skill/

# Scanner un fichier SKILL.md
skillspector scan ./SKILL.md

# Scanner un repo Git
skillspector scan https://github.com/user/my-skill

# Sans LLM (analyse statique only, plus rapide)
skillspector scan ./my-skill/ --no-llm

# Output JSON (CI/CD)
skillspector scan ./my-skill/ --format json --output report.json

# Output SARIF (IDE/CI)
skillspector scan ./my-skill/ --format sarif --output report.sarif

# Baseline (supprimer false positives)
skillspector baseline ./my-skill/ -o .skillspector-baseline.yaml
skillspector scan ./my-skill/ --baseline .skillspector-baseline.yaml
```

## MCP Server

```bash
# Stdio (agents locaux : Claude Code, Codex, Gemini)
skillspector mcp

# HTTP (remote / A2A)
skillspector mcp --transport http --host 127.0.0.1 --port 8000

# Enregistrer dans Claude Code
claude mcp add skillspector -- skillspector mcp
```

Outil exposé : `scan_skill(target, use_llm=true, output_format="json")` → renvoie `risk_score` (0-100), `severity`, `recommendation`, `safe_to_install`, `findings`.

## Patterns détectés (68 sur 17 catégories)

| Catégorie | Patterns | Sévérité |
|-----------|----------|----------|
| Prompt Injection | 5 (override, hidden, exfil, behavior, harmful) | HIGH-CRITICAL |
| Anti-Refusal | 3 (refusal/disclaimer suppression, policy nullification) | HIGH |
| Data Exfiltration | 4 (external transmission, env harvesting, fs enum, context leak) | MEDIUM-HIGH |
| Privilege Escalation | 3 (excessive perms, sudo/root, credential access) | LOW-HIGH |
| Supply Chain | 6 (unpinned, curl\|bash, obfuscated, CVEs, abandoned, typosquat) | LOW-HIGH |
| Excessive Agency | 4 (unrestricted tools, autonomous, scope creep, unbounded) | MEDIUM-HIGH |
| Output Handling | 3 (unvalidated, cross-context, unbounded) | MEDIUM-HIGH |
| System Prompt Leakage | 3 (direct, indirect, tool-based) | MEDIUM-HIGH |
| Memory Poisoning | 3 (persistent injection, stuffing, manipulation) | MEDIUM-HIGH |
| Tool Misuse | 3 (param abuse, chaining, unsafe defaults) | MEDIUM-HIGH |
| Rogue Agent | 2 (self-modification, session persistence) | HIGH-CRITICAL |
| Trigger Abuse | 3 (overly broad, shadow command, keyword baiting) | MEDIUM-HIGH |
| Dangerous Code (AST) | 9 (exec, eval, import, subprocess, os.system, compile, getattr, chains) | MEDIUM-CRITICAL |
| Taint Tracking | 5 (direct, variable-mediated, cred exfil, file→network, input→exec) | MEDIUM-CRITICAL |
| YARA Signatures | 4 (malware, webshell, cryptominer, hack tool) | HIGH-CRITICAL |
| MCP Least Privilege | 4 (underdeclared, wildcard, missing, overdeclared) | LOW-HIGH |
| MCP Tool Poisoning | 4 (hidden instructions, unicode deception, param injection, mismatch) | MEDIUM-HIGH |

## Scoring

| Score | Severity | Recommendation |
|-------|----------|----------------|
| 0-20 | LOW | SAFE |
| 21-50 | MEDIUM | CAUTION |
| 51-80 | HIGH | DO NOT INSTALL |
| 81-100 | CRITICAL | DO NOT INSTALL |

- CRITICAL: +50 pts, HIGH: +25, MEDIUM: +10, LOW: +5. Scripts exécutables: x1.3.

## Providers LLM (optionnel, stage 2)

| Provider | Variable | Endpoint |
|----------|----------|----------|
| openai | `OPENAI_API_KEY` | api.openai.com |
| anthropic | `ANTHROPIC_API_KEY` | api.anthropic.com |
| bedrock | `AWS_PROFILE`/`AWS_REGION` | AWS Bedrock |
| nv_build | `NVIDIA_INFERENCE_KEY` | build.nvidia.com |
| claude_cli | (local CLI auth) | local claude binary |
| codex_cli | (local CLI auth) | local codex binary |
| Ollama/local | `OPENAI_BASE_URL` + `OPENAI_API_KEY=ollama` | localhost |

## Exit codes (CI/CD)

| Code | Signification |
|------|---------------|
| 0 | Scan OK, score ≤ 50 (SAFE/CAUTION) |
| 1 | Scan OK, score > 50 (DO NOT_INSTALL) |
| 2 | Erreur |

## Critères de succès

- Le skill est validé safe (score ≤ 20) avant installation.
- Les skills avec score > 50 sont bloqués.
- Les skills avec scripts exécutables sont systématiquement scannés.