# Codex Skills

Personal reusable Codex skills.

## Skills

- `lab-report-polisher`: Polish Chinese university lab report DOCX templates, especially programming-question sections with code, outputs, figures, and analysis while preserving teacher review areas and template structure.

## Install From GitHub

After pushing this repository to GitHub, another Codex environment can install the skill with:

```bash
python /path/to/skill-installer/scripts/install-skill-from-github.py \
  --repo <your-github-username>/codex-skills \
  --path lab-report-polisher
```

If using a browser or another agent that supports skill installation by GitHub URL, point it to:

```text
https://github.com/<your-github-username>/codex-skills/tree/main/lab-report-polisher
```

Restart Codex after installation so the skill appears in the available skill list.

