# CLAUDE.md

## Project Overview

This is Anthropic's **Agent Skills** repository — a collection of self-contained, reusable skills that extend Claude's capabilities. Skills are folders containing instructions (`SKILL.md`), scripts, references, and assets that Claude loads dynamically to perform specialized tasks.

The repository is **not** a traditional software application. It is a curated library of skill definitions organized for use in Claude Code, Claude.ai, and the Claude API.

- Specification: https://agentskills.io/specification
- Docs: https://support.claude.com/en/articles/12512176-what-are-skills

## Repository Structure

```
CLAUDE.md                        # This file — AI assistant guide
README.md                        # Repository overview and getting started
THIRD_PARTY_NOTICES.md           # Third-party license attributions
.claude-plugin/marketplace.json  # Plugin definitions for Claude Code
spec/                            # Links to Agent Skills specification
template/                        # Skill template for new skills
skills/                          # All skills live here (16 total)
├── algorithmic-art/             # p5.js generative art with seeded randomness
├── brand-guidelines/            # Anthropic brand colors and typography
├── canvas-design/               # Visual art in .png and .pdf using design philosophy
├── doc-coauthoring/             # Structured workflow for co-authoring documentation
├── docx/                        # Word document creation/editing (proprietary)
├── frontend-design/             # Production-grade frontend interfaces
├── internal-comms/              # Internal communications (status reports, newsletters, FAQs)
├── mcp-builder/                 # MCP server development guide
├── pdf/                         # PDF manipulation (proprietary)
├── pptx/                        # PowerPoint creation/editing (proprietary)
├── skill-creator/               # Framework for creating new skills
├── slack-gif-creator/           # Animated GIF creation for Slack
├── theme-factory/               # Pre-set themes (colors/fonts) for artifacts
├── web-artifacts-builder/       # React/Vite artifact builder
├── webapp-testing/              # Playwright web app testing
└── xlsx/                        # Excel spreadsheet operations (proprietary)
```

## Skill Anatomy

Every skill follows this structure:

```
skill-name/
├── SKILL.md           # REQUIRED — YAML frontmatter + markdown instructions
├── LICENSE.txt        # License (Apache 2.0 for examples, proprietary for doc skills)
├── scripts/           # Optional executable code (Python/Bash/JS)
├── references/        # Optional documentation loaded on-demand
└── assets/            # Optional files used in output (templates, fonts, images)
```

### SKILL.md Format

```yaml
---
name: skill-name          # Required: kebab-case identifier
description: ...          # Required: what it does + when to trigger it
license: ...              # Optional: license reference
---
```

The markdown body below the frontmatter contains instructions. Only loaded after the skill triggers.

## Plugin Groups

Defined in `.claude-plugin/marketplace.json`:

- **document-skills**: `docx`, `pptx`, `pdf`, `xlsx`
- **example-skills**: all other 12 skills

## Key Conventions

### Naming
- **Skill directories**: kebab-case (`web-artifacts-builder`, `slack-gif-creator`)
- **Python files**: snake_case (`extract_form_field_info.py`)
- **Reference files**: kebab-case or snake_case markdown (`docx-js.md`, `mcp_best_practices.md`)

### Licensing
- Example skills: Apache 2.0 (open source)
- Document skills (`docx`, `pptx`, `pdf`, `xlsx`): Proprietary / source-available

### Design Principles
1. **Conciseness**: Context window is shared — every paragraph must justify its token cost
2. **Progressive disclosure**: Metadata always loaded (~100 words) → SKILL.md body on trigger (<5k words) → bundled resources on demand
3. **Appropriate degrees of freedom**: High freedom for creative tasks, low freedom for fragile operations
4. **No extraneous files**: No README, CHANGELOG, or auxiliary docs inside skills — only SKILL.md, scripts, references, and assets

### Writing Style
- Use imperative/infinitive form in SKILL.md instructions
- SKILL.md body should stay under 500 lines; split into reference files when approaching this limit
- Reference files >100 lines should include a table of contents
- Keep references one level deep from SKILL.md (no deeply nested references)

## Development Workflow

### Creating a New Skill

```bash
# 1. Initialize from template
python skills/skill-creator/scripts/init_skill.py <skill-name> --path <output-dir>

# 2. Edit SKILL.md and add scripts/references/assets

# 3. Validate and package
python skills/skill-creator/scripts/package_skill.py <path/to/skill-folder>
```

### Editing an Existing Skill
1. Modify `SKILL.md` or bundled resources directly
2. Test by using the skill in Claude
3. Re-package if distributing: `python skills/skill-creator/scripts/package_skill.py <skill-folder>`

### Validation
```bash
python skills/skill-creator/scripts/quick_validate.py <skill-folder>
```

Validates: SKILL.md presence, frontmatter format (name, description fields), and directory structure.

## Common Script Patterns

All paths below are relative to the repository root.

### OOXML Document Processing (docx/pptx)

Both `skills/docx/` and `skills/pptx/` contain an `ooxml/scripts/` subdirectory with identical tooling:

```bash
python skills/docx/ooxml/scripts/unpack.py <file> <output-dir>   # Unzip to XML
# Edit XML files
python skills/docx/ooxml/scripts/pack.py <input-dir> <output-file>  # Repack
python skills/docx/ooxml/scripts/validate.py <file>                  # Validate
# Same scripts exist under skills/pptx/ooxml/scripts/
```

### PDF Operations
```bash
python skills/pdf/scripts/extract_form_field_info.py <pdf>
python skills/pdf/scripts/fill_fillable_fields.py <pdf> <output>
python skills/pdf/scripts/check_bounding_boxes.py <pdf>
```

### Web Artifacts (React/Vite)
```bash
bash skills/web-artifacts-builder/scripts/init-artifact.sh <project-name>
bash skills/web-artifacts-builder/scripts/bundle-artifact.sh   # Produces bundle.html
```

### Web App Testing (Playwright)
```bash
python skills/webapp-testing/scripts/with_server.py --server "npm run dev" --port 5173 -- python automation.py
```

### Excel Recalculation
```bash
python skills/xlsx/recalc.py <file>   # Recalculate formulas (requires LibreOffice)
```

## Dependencies

No unified package manager at the repo root. Each skill manages its own dependencies:

- **Python**: pypdf, pdfplumber, reportlab, python-pptx, python-docx, openpyxl, PIL/Pillow, pandas, numpy, playwright, imageio
- **JavaScript/Node**: React 18, TypeScript, Vite, Parcel, Tailwind CSS, shadcn/ui, p5.js
- **System**: FFmpeg, Pandoc, LibreOffice (for recalculation)

## Testing

No centralized test suite. Testing is per-skill:

- `skills/pdf/scripts/check_bounding_boxes_test.py` — unit tests for PDF form field detection
- `skills/webapp-testing/` — Playwright-based integration testing
- `skills/mcp-builder/scripts/evaluation.py` — evaluation framework for MCP servers
- `skills/skill-creator/scripts/quick_validate.py` — validates skill structure and SKILL.md format
- Skills are primarily tested through actual usage in Claude

## Git Conventions

- Structured commit messages with issue references (e.g., `Add doc-coauthoring skill (#134)`)
- No pre-commit hooks or CI/CD pipelines configured
- `.gitignore` excludes: `.DS_Store`, `__pycache__/`, `.idea/`, `.vscode/`
