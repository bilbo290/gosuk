# Gosuk

Verification-first implementation skill for Codex.

Use this when you want Codex to define and run a deterministic verifier before editing code, then rerun the same verifier after the implementation. For UI or artifact-producing work, Gosuk also asks Codex to capture visible proof when possible.

## Install

Ask Codex:

```text
Install this skill: https://github.com/bilbo290/gosuk/tree/main/skills/gosuk
```

Or run the installer directly:

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --url https://github.com/bilbo290/gosuk/tree/main/skills/gosuk
```

Restart Codex after installing so the skill index reloads.

## Contents

```text
skills/gosuk/
  SKILL.md
  agents/
    openai.yaml
```
