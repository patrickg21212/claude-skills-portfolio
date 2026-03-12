# Brand config specification

The brand config JSON controls all visual output. Every field is optional with sensible defaults.

## Schema

```json
{
  "company": "string — company name (used in footer/metadata)",
  "fonts": {
    "heading": {
      "name": "string — font family (default: Calibri)",
      "size": "number — point size (default: 36)",
      "bold": "boolean (default: true)",
      "italic": "boolean (default: false)",
      "color": "string — hex color (default: #1B2A4A)"
    },
    "subtitle": {
      "name": "string (default: Calibri)",
      "size": "number (default: 20)",
      "bold": "boolean (default: false)",
      "color": "string (default: #5A6B7C)"
    },
    "body": {
      "name": "string (default: Calibri)",
      "size": "number (default: 18)",
      "bold": "boolean (default: false)",
      "color": "string (default: #333333)"
    }
  },
  "colors": {
    "primary_bg": "string — title slide background (default: #FFFFFF)",
    "slide_bg": "string — content slide background (default: #FFFFFF)",
    "accent": "string — bullets, highlights, borders (default: #2563EB)",
    "body_text": "string — body text color (default: #333333)",
    "heading_text": "string — heading text color (default: #1B2A4A)"
  },
  "layouts": {
    "title_slide": "number — slide layout index for title slides (default: 0)",
    "content_slide": "number — slide layout index for content slides (default: 1)",
    "section_header": "number — slide layout index for section headers (default: 2)",
    "two_column": "number — slide layout index for two-column slides (default: 3)"
  }
}
```

## Extracting values from a client template

Use python-pptx to programmatically extract font names, sizes, colors, and layout indices
from the client's existing .pptx template. See SKILL.md Step 1 for the extraction script.

## Font availability

python-pptx embeds the font name as a string reference. The font must be installed on the
machine that opens the .pptx for it to render correctly. If the font isn't available,
PowerPoint falls back to a default. For maximum compatibility, stick to fonts included
with Microsoft Office (Calibri, Arial, Segoe UI) unless the client confirms their font
is available on all target machines.
