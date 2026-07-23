# SkillSpector — Sécurité des skills IA

## Règle

Tout skill tiers doit être scanné par SkillSpector **avant installation**. La recherche NVIDIA (2026) montre que 26.1% des skills contiennent des vulnérabilités et 5.2% montrent une intention probablement malveillante.

### Obligations

1. **Scanner avant install** : `skillspector scan <path-or-url>` sur tout skill tiers.
2. **Bloquer si score > 50** : recommendation DO_NOT_INSTALL → ne pas installer.
3. **Warn si 21-50** : CAUTION → avertir l'utilisateur avant install.
4. **Scripts exécutables** : scan systématique (2.12x plus vulnérables).

### Patterns critiques à bloquer

- **Prompt injection** : instructions cachées, override de safety, exfiltration de contexte.
- **Anti-refusal** : suppression de refus/disclaimers, nullification de guardrails.
- **Data exfiltration** : env harvesting, transmission externe, context leakage.
- **Rogue agent** : self-modification, persistence via cron/startup.
- **Dangerous code (AST)** : exec(), eval(), subprocess, os.system, dynamic import.
- **Taint tracking** : credentials → network, file → network, input → exec.
- **Supply chain** : curl|bash, obfuscated code, CVEs connus, typosquatting.
- **MCP tool poisoning** : hidden instructions, unicode deception, param injection.

### Scoring

| Score | Action |
|-------|--------|
| 0-20 (LOW) | Autoriser |
| 21-50 (MEDIUM) | Avertir |
| 51-80 (HIGH) | Bloquer |
| 81-100 (CRITICAL) | Bloquer |

## Justification

Les skills s'exécutent avec une confiance implicite et un vetting minimal. SkillSpector fait du defense-in-depth : analyse statique (regex + AST + YARA) + optionnellement LLM sémantique. Il n'exécute jamais le skill scanné.

- Sans LLM (`--no-llm`) : statique only, high recall, false positives possibles.
- Avec LLM : precision ~87%, envoie le contenu du fichier au provider configuré.

## Exemples

### Respect
```bash
# Skill tiers à évaluer
skillspector scan https://github.com/user/some-skill --format json
# Score: 12/100 → SAFE → autoriser
```

### Violation
```bash
# Skill avec env harvesting + exfil externe
skillspector scan ./suspicious-skill/
# Score: 78/100 → DO NOT_INSTALL → bloquer
```

## Note

SkillSpector est defense-in-depth, pas un sandbox. Il ne contient pas un skill qu'on choisit d'installer quand même. Il ne remplace pas la revue humaine.