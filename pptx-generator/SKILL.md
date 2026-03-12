---
name: pptx-generator
description: |
  Generate brand-compliant PowerPoint presentations from raw text or markdown input.
  Enforces corporate fonts, colors, and layouts defined in a JSON brand config.
  Uses python-pptx to produce .pptx files. Supports custom templates.
  Use when asked to create a presentation, slide deck, or .pptx file from text content.
---

# PPTX Generator — Branded Presentation Skill

Generate .pptx presentations that enforce corporate brand standards from raw text input.

## How it works

1. **Brand config** (`assets/sample_brand_config.json`) defines fonts, colors, and layout mappings
2. **Markdown input** uses `#` for title slides and `##` for content slides, with `-` for bullets
3. **The script** (`scripts/generate_pptx.py`) parses the input, applies brand rules, and outputs a .pptx

## Quick start

```bash
python scripts/generate_pptx.py \
  --config assets/sample_brand_config.json \
  --input assets/sample_content.md \
  --output presentation.pptx
```

To use a client's existing .pptx template as the base (preserves their master slides):

```bash
python scripts/generate_pptx.py \
  --config brand_config.json \
  --input content.md \
  --output presentation.pptx \
  --template client_template.pptx
```

## Workflow for new clients

### Step 1: Analyze the template
When a client provides their .pptx template, extract the brand system:

```python
from pptx import Presentation
from pptx.util import Pt

prs = Presentation("client_template.pptx")

# Extract master slide layouts
for i, layout in enumerate(prs.slide_layouts):
    print(f"Layout {i}: {layout.name}")
    for ph in layout.placeholders:
        print(f"  Placeholder {ph.placeholder_format.idx}: {ph.name} ({ph.width}, {ph.height})")

# Extract theme colors and fonts from the first slide
slide = prs.slides[0] if prs.slides else None
if slide:
    for shape in slide.shapes:
        if shape.has_text_frame:
            for para in shape.text_frame.paragraphs:
                for run in para.runs:
                    print(f"Font: {run.font.name}, Size: {run.font.size}, Bold: {run.font.bold}")
```

### Step 2: Build the brand config
Create a JSON config that maps the extracted values:

```json
{
  "company": "Client Name",
  "fonts": {
    "heading": { "name": "Their Font", "size": 36, "bold": true, "color": "#HEX" },
    "body": { "name": "Their Font", "size": 18, "bold": false, "color": "#HEX" }
  },
  "colors": {
    "primary_bg": "#HEX",
    "accent": "#HEX"
  }
}
```

### Step 3: Generate presentations
Once the config exists, any text input produces brand-compliant slides via the slash command.

## Input format

```markdown
# Presentation Title
Optional subtitle text

## Slide title
- Bullet point one
- Bullet point two

## Another slide
Body paragraph text instead of bullets.
```

- `#` creates a title slide (layout 0)
- `##` creates a content slide (layout 1)
- `-` or `*` creates bullet points
- Plain text after `##` becomes body content
- First line after `#` becomes the subtitle

## Brand config reference

See `references/brand-config-spec.md` for the full configuration schema.

## Extending

The script is designed to be extended per client:
- Add new layout types (two-column, image+text, data table)
- Add chart generation via python-pptx chart API
- Add image insertion with auto-sizing
- Add speaker notes from content annotations
