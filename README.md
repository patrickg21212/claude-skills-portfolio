# Claude Code Skills

Custom skills built for Anthropic's Claude Code Agent Skills framework. These are production skills, not demos.

## Skills

### [`pptx-generator/`](pptx-generator/)
Generate brand-compliant PowerPoint presentations from markdown text input. Parses raw content into slides, applies corporate fonts/colors/layouts from a JSON brand config, and outputs .pptx files using python-pptx. Supports custom templates. Includes a working generation script, sample brand config, and sample content file.

```bash
python scripts/generate_pptx.py \
  --config assets/sample_brand_config.json \
  --input assets/sample_content.md \
  --output presentation.pptx
```

### [`upwork-job/`](upwork-job/)
Full Upwork freelance lifecycle management. 6-phase workflow covering job evaluation (difficulty rating, platform compatibility check, fit scoring, red/green flag analysis), proposal generation with cover letters, client messaging with tone calibration, scoping and pricing with three-point estimates, job execution planning, and review requests. 430+ lines of structured skill logic.

### [`stay-hard/`](stay-hard/)
David Goggins-style accountability review. Analyzes recent activity against goals using six Goggins frameworks (Accountability Mirror, Callus the Mind, Cookie Jar, 40% Rule, Taking Souls, Stay Hard vs Stay Soft), then projects 30-day consequences of current behavior and generates a specific call to action.

## Skill architecture

Every skill follows Anthropic's SKILL.md pattern:

```
skill-name/
  SKILL.md          # YAML frontmatter (name, description) + markdown instructions
  scripts/          # Executable code (python, bash)
  references/       # Documentation loaded on-demand
  assets/           # Templates, configs, sample files
```

The frontmatter `description` field controls when the skill triggers. The body loads only after activation. Reference files load on-demand. This progressive disclosure keeps context window usage efficient.

Built by Patrick Gibbs / [Epiphany Dynamics](https://epiphanydynamics.ai)
