---
name: gzh-design-skill-wechat-markdown-formatter
description: Transform Markdown into paste-ready WeChat Official Account HTML with 6 themes, auto-numbering, keyword highlighting, and compliance validation
triggers:
  - "format this markdown for WeChat official account"
  - "convert markdown to gongzhonghao HTML"
  - "create WeChat article from this markdown"
  - "generate WeChat official account post"
  - "style this article for WeChat public account"
  - "make this markdown ready for 公众号"
  - "apply moyu-green theme to this article"
  - "create custom WeChat article theme"
---

# gzh-design-skill: WeChat Official Account Markdown Formatter

> Skill by [ara.so](https://ara.so) — Design Skills collection.

This skill transforms Markdown documents into production-ready HTML for WeChat Official Accounts (微信公众号) with professional typesetting, automatic section numbering, keyword highlighting, and strict platform compliance. Ships with 6 battle-tested themes plus a theme generator.

## What It Does

- **One-click formatting**: Markdown → inline-styled HTML that pastes cleanly into WeChat's editor
- **6 curated themes**: Moyu Green (default), Red-White, Graphite Minimal, Zen Whitespace, Moyu Ticket, Olive Notes
- **Smart typography**: Auto section numbering, keyword underlining (1-3 per paragraph), full-width punctuation
- **Platform compliance**: All styles inline, text wrapped in `<span leaf="">`, no `<div>/<style>/class/position/grid`
- **Rich components**: Code blocks (light/dark), images, GIF badges, quotes, lists, product labels, intro cards
- **Quality gates**: Dual validation (source linting + output validation) ensures nothing breaks

## Installation

### Quick Install (Recommended)

```bash
npx skills add https://github.com/isjiamu/gzh-design-skill
```

### Manual Clone

```bash
# For Claude Code
git clone https://github.com/isjiamu/gzh-design-skill.git ~/.claude/skills/gzh-design

# For Cursor
git clone https://github.com/isjiamu/gzh-design-skill.git ~/.cursor/skills/gzh-design

# For Codex
git clone https://github.com/isjiamu/gzh-design-skill.git ~/.codex/skills/gzh-design
```

### Let AI Install It

Simply tell your agent:

> "Install the skill from https://github.com/isjiamu/gzh-design-skill"

## Core Workflow

The skill follows this deterministic pipeline defined in `SKILL.md`:

### 1. Theme Selection

```markdown
User: "Format this article for WeChat using moyu-green theme"
```

The agent will:
- Read `references/theme-index.md` to map theme name to file
- Load the theme component library (e.g., `references/theme-moyu-green.md`)
- Load `references/common-components.md` for universal components (code blocks, images)

### 2. Content Analysis

```markdown
User: "Convert article.md to WeChat HTML, auto-detect best theme"
```

The agent analyzes content type and recommends:
- **Tutorial/Guide** → Moyu Green (step labels, code blocks)
- **Opinion/Analysis** → Red-White or Graphite Minimal (emphasis on text hierarchy)
- **Product Review** → Moyu Ticket (card-based layout)
- **Minimalist Essay** → Zen Whitespace (breathing room)

### 3. HTML Assembly

The agent will:
- Parse Markdown structure (headings, paragraphs, code, images, lists)
- Apply automatic section numbering (末章 marked with ∞ or ///)
- Highlight 1-3 keywords per paragraph with theme-specific underlines
- Convert punctuation to full-width (正文 only, preserves half-width in code)
- Wrap all text nodes in `<span leaf="">` for paste safety
- Inline all styles (no external CSS)

### 4. Validation

Two-checkpoint quality gate:

```bash
# Source checkpoint: Lint component library
python3 scripts/component_lint.py .

# Output checkpoint: Validate final HTML
python3 scripts/validate_gzh_html.py output.html
```

Must achieve:
- 0 ERRORs in both checks
- 0 WARNs for half-width punctuation in body text

### 5. Delivery

Generates two files:
- `article_clean.html` — Clean HTML for direct paste
- `article_preview.html` — Preview with "Copy to WeChat" button (one-click copy to clipboard)

## Theme System

### Using Built-in Themes

```markdown
# Default (Moyu Green)
User: "Format article.md for WeChat"

# Specific theme
User: "Use red-white theme for this markdown"
User: "Apply zen-whitespace style to my essay"
```

### Theme Quick Reference

| Theme ID | Primary Color | Use Case | Underline Color |
|----------|---------------|----------|-----------------|
| `moyu-green` | `#059669` | Tutorials, tools, reviews | `#10b981` |
| `red-white` | `#dc2626` | Opinion, analysis | `#ef4444` |
| `graphite-minimal` | `#374151` | Design, tech commentary | `#6b7280` |
| `zen-whitespace` | `#64748b` | Essays, minimalist content | `#94a3b8` |
| `moyu-ticket` | `#059669` | Tool comparisons, creative reviews | `#10b981` |
| `olive-notes` | `#65a30d` | Case studies, internal reports | `#84cc16` |

Full specs in `references/theme-index.md`.

### Generating Custom Themes

```markdown
User: "Create a new WeChat theme inspired by tech magazines, 
use deep blue (#1e40af) as primary color, clean and professional"
```

The agent will:
1. Read `references/theme-generator.md` for generation workflow
2. Collect requirements (description, colors, fonts)
3. Generate complete component library (30+ components)
4. Save to `references/theme-custom-{name}.md`
5. Generate preview blocks in `assets/theme-previews/`

From a reference image:

```markdown
User: "Generate a WeChat theme matching this screenshot: [attach image]
Call it 'emerald-tech'"
```

## Component Usage

### Code Blocks

Markdown:
```markdown
​```python
def hello_wechat():
    return "内联样式,等宽字体,不折行"
​```
```

Renders with:
- Monospace font (Consolas/Monaco)
- Light background (`#f9fafb`) or dark (`#1f2937`) based on theme
- Horizontal scroll (no line wrapping)
- Preserved half-width punctuation

### Images

```markdown
![Alt text](https://example.com/image.jpg)
```

Auto-formatted with:
- Max width 100%, responsive
- Border radius from theme
- Optional caption below

GIFs get an animated corner badge.

### Keyword Highlighting

Automatic in body text:

```markdown
这段讲**核心概念**和实现方式。
```

Becomes:
- Bold text gets theme-colored underline
- 1-3 keywords per paragraph max (not every bold)
- Uses `border-bottom` (not `text-decoration` for better mobile support)

### Section Numbering

```markdown
## First Chapter
Content...

## Second Chapter
Content...

## Conclusion
Final thoughts...
```

Auto-numbered as:
- 一、First Chapter
- 二、Second Chapter
- ∞ Conclusion (last chapter gets ∞ or ///)

### Intro Cards & TOC

```markdown
User: "Add intro card and table of contents to this article"
```

Agent extracts:
- Intro card: First 2-3 compelling sentences
- TOC: All H2 headers with jump links

### Product Badges

```markdown
User: "Add tech stack badges for React, TypeScript, Tailwind"
```

Renders inline pill badges with theme colors.

## Validation & Quality

### Platform Compliance Checks

WeChat Official Account editor strips/breaks:
- ❌ `<style>`, `<script>`, `<div>` tags
- ❌ `class`, `id` attributes
- ❌ `position: fixed/absolute/sticky`
- ❌ `display: grid`, `float`
- ❌ `@media`, `@keyframes`, CSS variables
- ❌ External fonts (must use system stack)

This skill ensures:
- ✅ All styles inline on elements
- ✅ Text wrapped in `<span leaf="">`
- ✅ Only safe tags: `<section>`, `<p>`, `<span>`, `<h*>`, `<pre>`, `<code>`, `<img>`, `<blockquote>`, `<ul>`, `<ol>`, `<li>`

### Running Validation

```bash
# Check component library source
python3 scripts/component_lint.py .

# Check final HTML output
python3 scripts/validate_gzh_html.py output.html
```

**Component Lint** catches:
- `white-space: pre` (causes layout breaks)
- Dotted borders around body text (visual noise)
- Forbidden CSS properties

**HTML Validation** catches:
- Missing `<span leaf="">` wrappers
- Half-width punctuation in body (should be full-width: 。、！？)
- Forbidden tags/attributes
- External styles

Both must show **0 ERRORs** before delivery.

### Reproducible Quality Loop

```bash
# 1. Edit component library
vim references/theme-moyu-green.md

# 2. Validate source
python3 scripts/component_lint.py .

# 3. Generate test HTML
# (via agent: "format test article")

# 4. Validate output
python3 scripts/validate_gzh_html.py test_output.html

# 5. Fix until both pass
```

Documented in `references/eval-cases.md`.

## Real-World Examples

### Example 1: Tutorial Article

**Input**: `tutorial.md`
```markdown
# 如何用 Claude Code 自动化排版

本文教你把 Markdown 一键转成公众号 HTML。

## 安装技能

先克隆仓库...

## 选择主题

推荐用**摸鱼绿**主题...

​```bash
npx skills add https://github.com/isjiamu/gzh-design-skill
​```

## 生成 HTML

运行命令后...
```

**Command**:
```markdown
User: "Format tutorial.md for WeChat, use moyu-green theme, 
add step labels and intro card"
```

**Output**: HTML with:
- Auto-numbered sections (一、二、三)
- Keywords underlined in green (`#10b981`)
- Code block with light background
- Intro card extracted from first paragraph
- All punctuation converted to full-width

### Example 2: Opinion Essay

**Input**: `opinion.md`
```markdown
# AI 工具的本质是放大人的意图

很多人误解了 AI...

**核心观点**:工具不会取代思考...
```

**Command**:
```markdown
User: "Convert opinion.md to WeChat HTML, recommend best theme"
```

**Agent response**:
```
Analyzing content... This is an opinion/analysis piece.
Recommended theme: red-white (classic editorial style)
Proceed? [yes]
```

**Output**: HTML with:
- Red underlines on key terms (`#ef4444`)
- Pull quotes for 核心观点
- Generous line spacing (1.8)
- Centered bold quote blocks

### Example 3: Custom Theme Generation

**Command**:
```markdown
User: "Generate a WeChat theme called 'tech-purple' with:
- Primary: #7c3aed (purple)
- For developer tools content
- Modern, clean, slightly playful
- Code blocks with dark background"
```

**Agent workflow**:
1. Loads `references/theme-generator.md`
2. Generates component library with:
   - Purple heading underlines
   - Dark code blocks (`#1f2937` bg)
   - Purple link hover states
   - 30+ styled components
3. Saves to `references/theme-custom-tech-purple.md`
4. Generates preview: `assets/theme-previews/tech-purple-preview.html`

**Usage after generation**:
```markdown
User: "Format devtools.md using tech-purple theme"
```

### Example 4: Batch Conversion

**Command**:
```markdown
User: "Convert all markdown files in ./articles/ to WeChat HTML,
use moyu-green for tutorials, red-white for opinions.
Detect type automatically."
```

**Agent process**:
```python
for article in articles:
    content_type = analyze_content_type(article)
    theme = "moyu-green" if content_type == "tutorial" else "red-white"
    generate_wechat_html(article, theme)
    validate_output(output_file)
```

## Configuration Files

### Theme Index

`references/theme-index.md` — Single source of truth for themes:

```markdown
| 主题名称 | 文件 | 主色 | 适用场景 |
|---------|------|------|---------|
| 摸鱼绿 | theme-moyu-green.md | #059669 | 教程/测评 |
| 红白色系 | theme-red-white.md | #dc2626 | 观点/深度 |
```

### Theme Component Library Structure

Each `theme-*.md` contains:

```markdown
## 设计变量
- 主色: #059669
- 正文色: #374151
- 背景色: #ffffff
- 强调色: #10b981

## 文字组件
### 正文段落
<p style="margin:16px 0;line-height:1.8;color:#374151">
  <span leaf="">正文内容</span>
</p>

### 小标题 (H3)
<h3 style="font-size:18px;font-weight:600;color:#059669;margin:24px 0 12px">
  <span leaf="">小标题</span>
</h3>

## 配方表（按文章类型）
- 教程类: step-label + 代码块 + 编号列表
- 测评类: tool-label + 对比卡片 + 数据框
```

### Common Components

`references/common-components.md` — Cross-theme components:

```markdown
## 代码块（浅色）
<pre style="background:#f9fafb;padding:16px;border-radius:8px;overflow-x:auto">
<code style="font-family:Consolas,Monaco,monospace;font-size:14px;white-space:pre">
<span leaf="">代码内容</span>
</code>
</pre>

## 动图标记
<div style="position:relative;display:inline-block">
  <img src="URL" style="max-width:100%">
  <span style="position:absolute;top:8px;right:8px;background:rgba(0,0,0,0.6);
               color:#fff;padding:4px 8px;border-radius:4px;font-size:12px">
    <span leaf="">GIF</span>
  </span>
</div>
```

## Troubleshooting

### Issue: Styles disappear after pasting

**Cause**: Used `<div>` or external `<style>` tags (WeChat strips them)

**Fix**: Ensure `validate_gzh_html.py` passes. All styles must be inline, structure must use `<section>`/`<p>`/`<span>`.

```bash
python3 scripts/validate_gzh_html.py output.html
# Look for ERRORs about forbidden tags
```

### Issue: Code blocks render with broken layout

**Cause**: `white-space: pre` without `overflow-x: auto`, or missing monospace font

**Fix**: Component lint will catch this:

```bash
python3 scripts/component_lint.py .
# ERROR: Found white-space:pre without overflow-x
```

Use the corrected code block component from `common-components.md`.

### Issue: Half-width punctuation warnings

**Cause**: Body text contains `,` `.` instead of `，` `。`

**Fix**: Validation shows WARNings:

```bash
python3 scripts/validate_gzh_html.py output.html
# WARN: Half-width comma at line 45
```

The agent should auto-convert during generation. If not, add explicit instruction:

```markdown
User: "Format article.md, ensure all punctuation in body is full-width"
```

### Issue: Keywords not highlighted

**Cause**: Markdown uses italics instead of bold, or too many bold items

**Fix**: Use `**keyword**` for emphasis. Agent auto-selects 1-3 per paragraph. If not triggering:

```markdown
User: "Highlight these keywords with underlines: 核心概念, 实现方式, 质量保证"
```

### Issue: Section numbering wrong

**Cause**: Markdown uses H1 for sections (should be H2)

**Fix**: Use H2 (##) for main sections, H3 (###) for subsections:

```markdown
## 第一章 (becomes 一、第一章)
### 小节 (becomes plain H3, no number)
```

### Issue: Custom theme not available

**Cause**: Theme generation created file but agent didn't index it

**Fix**: Regenerate theme and explicitly reference file:

```markdown
User: "Use the theme from references/theme-custom-tech-purple.md 
to format this article"
```

Or update `theme-index.md` to add your custom theme.

## Best Practices

1. **Start with theme selection** — Let agent recommend based on content type before generating
2. **Validate every output** — Run both linters, 0 ERRORs is non-negotiable
3. **Keep component libraries DRY** — Reuse from `common-components.md`, don't duplicate in themes
4. **Test paste before publishing** — Open preview HTML, click "Copy to WeChat", paste in editor, check on mobile
5. **Use theme recipes** — Each theme's "配方表" section defines proven component combinations per article type
6. **Generate themes conservatively** — Built-in 6 themes cover 90% use cases; custom themes add maintenance burden
7. **Lock punctuation** — Body text must use full-width, code blocks must preserve half-width (linter enforces this)
8. **Minimize color usage** — Primary color should appear ≤5 times per article (headings, key underlines), rely on grayscale for body

## Advanced Usage

### Normalizing Non-Markdown Inputs

```markdown
User: "Convert report.docx to WeChat HTML using olive-notes theme"
```

Agent workflow:
1. Reads `references/format-normalize.md`
2. Extracts text from DOCX
3. Normalizes to Markdown (headings, lists, emphasis)
4. Applies theme and generates HTML

Supports: `.docx`, `.pdf`, plain text with ad-hoc formatting.

### Batch Processing with Validation

```bash
# Process all files, fail fast on validation errors
for file in *.md; do
  echo "Processing $file..."
  # (agent generates HTML via skill)
  python3 scripts/validate_gzh_html.py "${file%.md}.html" || exit 1
done
```

### Theme Variants

Create theme variants by editing:

```markdown
User: "Create a dark mode variant of moyu-green called moyu-green-dark"
```

Agent duplicates `theme-moyu-green.md`, inverts colors:
- Background: `#1f2937` → `#ffffff`
- Text: `#374151` → `#f9fafb`
- Primary stays: `#059669`

(Note: WeChat doesn't support true dark mode, but useful for preview screenshots)

## Integration with AI Agents

### Claude Code

```markdown
# In Claude Code chat
User: "Load gzh-design skill and format my-article.md with moyu-green theme"
```

Claude reads `SKILL.md`, loads theme, generates HTML, runs validation.

### Cursor

```markdown
# In Cursor composer
User: "@gzh-design format article.md for WeChat, auto-detect theme"
```

Cursor references skill via `@` mention.

### Codex / Custom Agents

Add skill to agent's system prompt:

```python
system_prompt = f"""
You have access to gzh-design-skill for WeChat formatting.
Skill docs: {skill_md_content}
Theme index: {theme_index_content}
Always validate output with validate_gzh_html.py before delivering.
"""
```

## File Structure Reference

```
gzh-design/
├── SKILL.md                      # Main workflow (this skill's source)
├── references/
│   ├── theme-index.md            # Theme registry (single source)
│   ├── theme-moyu-green.md       # Default theme library
│   ├── theme-red-white.md        # Editorial theme
│   ├── theme-graphite-minimal.md # Minimal theme
│   ├── theme-zen-whitespace.md   # Zen theme
│   ├── theme-moyu-ticket.md      # Ticket-style theme
│   ├── theme-olive-notes.md      # Notes theme
│   ├── theme-generator.md        # Custom theme creation workflow
│   ├── common-components.md      # Universal components (code, images)
│   ├── format-normalize.md       # DOCX/PDF → Markdown conversion
│   └── eval-cases.md             # Test cases + quality loop
├── scripts/
│   ├── validate_gzh_html.py      # Output validation (ERROR/WARN)
│   └── component_lint.py         # Source linting (anti-patterns)
├── assets/
│   ├── sample-article.md         # Demo input
│   └── theme-previews/           # Generated theme previews
└── docs/
    ├── all-themes.md             # Full theme screenshots
    └── gallery/
        └── index.html            # Interactive theme browser
```

## Version & Compatibility

- **HTML Output**: Tested on WeChat Official Account editor (微信公众平台) as of 2026-07
- **Python Scripts**: Require Python 3.7+
- **Agent Compatibility**: Claude Code, Cursor, Codex, any agent that can read Markdown docs and run shell commands

## License

AGPL-3.0 — See [LICENSE](LICENSE) file.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for:
- Adding new themes
- Improving component libraries
- Reporting validation edge cases
- Submitting test articles

## Links

- **GitHub**: https://github.com/isjiamu/gzh-design-skill
- **Theme Previews**: `docs/gallery/index.html` (open in browser)
- **Validation Docs**: `references/eval-cases.md`
- **Example Output**: `docs/all-themes.md`

---

**Quick Start Checklist**:
- [ ] Install skill: `npx skills add https://github.com/isjiamu/gzh-design-skill`
- [ ] Verify files: `ls ~/.claude/skills/gzh-design/references/`
- [ ] Format test article: "Format assets/sample-article.md using moyu-green"
- [ ] Validate output: `python3 scripts/validate_gzh_html.py output.html`
- [ ] Open preview: `open output_preview.html`
- [ ] Paste in WeChat editor and check mobile preview

You're ready to ship production-quality WeChat articles. 🚀
