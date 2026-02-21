# AI Agent Instructions: AIM-Workshop Conic Sections

## Project Overview
This is a **PreTeXt educational textbook** on conic sections (Chapter 9) for the AIM Workshop. PreTeXt is an XML-based authoring framework that compiles to accessible HTML websites and PDF.

**Key outputs:**
- Web version: `pretext build web` → HTML website
- Print version: `pretext build print` → PDF file
- Preview locally: `pretext view web`

## Architecture & File Structure

### Source Organization
All content lives in `source/` using XML with XInclude relationships:
- `main.ptx` - root document that includes all others
- `ch-chapter-conics.ptx` - single chapter including three sections
- `sec-section-*.ptx` - individual sections (ellipse, hyperbola, polar)
- `docinfo.ptx` - document metadata, LaTeX macros, and preambles (do not modify `<document-id>` lightly)
- `frontmatter.ptx` / `backmatter.ptx` - front/back matter
- `publication/publication.ptx` - output format configuration

**XInclude pattern** (critical):
```xml
<xi:include href="./file.ptx" />
```
Files must be well-formed XML snippets that are included, not standalone documents.

### Key Files
- `project.ptx` - PreTeXt project manifest (target definitions)
- `requirements.txt` - specifies `pretext == 2.18.0` CLI version
- `publication/publication.ptx` - controls HTML/PDF styling, TOC depth, exercise visibility

## Content Patterns

### Mathematical Macros
Defined in `docinfo.ptx` macros section:
```
\N = ℕ, \Z = ℤ, \Q = ℚ, \R = ℝ
```
Always use these semantic macros rather than `\mathbb` directly for consistency.

### LaTeX Images & Diagrams
`docinfo.ptx` includes TikZ packages for generating images:
```xml
<latex-image-preamble>
  \usepackage{tikz, pgfplots}
  \usetikzlibrary{positioning,matrix,arrows,shapes,decorations}
</latex-image-preamble>
```
Use within `<latex-image>` tags in content. If new TeX packages are needed, add to `.devcontainer/installLatex.sh`.

## Development Workflow

### Editing Content
1. Edit `.ptx` files in `source/` (XML format)
2. Run `pretext view web` to see live preview (auto-rebuilds on save)
3. Check HTML output in `output/web/` directory

### Building Outputs
```bash
pretext build web      # Creates output/web/
pretext build print    # Creates PDF (requires full TeXLive)
pretext deploy         # Pushes to GitHub Pages
```

### Common Issues
- **Missing LaTeX packages**: Use `tlmgr install <package>` in terminal; add to `.devcontainer/installLatex.sh` for persistence
- **SageMath plots**: Not installed by default. Use command palette "PreTeXt: Install SageMath" if needed
- **Won't build after edits**: Check XML well-formedness (missing closing tags, unclosed quotes)

## Editing Guidelines

### XML Structure Rules
- All files must be valid XML snippets (tags properly closed)
- Use `xml:id` attributes for cross-referenceable elements (chapters: `ch-*`, sections: `sec-*`, etc.)
- Indentation: 2 spaces, preserve consistency with existing files

### Content Organization
- One chapter per file (`ch-chapter-conics.ptx`)
- One section per file (`sec-section-*.ptx`)
- Follow naming: `sec-section-{topic}.ptx` (all lowercase, hyphens)
- Section titles match content (e.g., "Ellipse", "Hyperbola")

### Adding New Content
1. Create `sec-section-{topic}.ptx` following the template in `sec-section-ellipse.ptx`
2. Add `<xi:include href="sec-section-{topic}.ptx" />` to `ch-chapter-conics.ptx`
3. Use standard PreTeXt tags: `<p>`, `<title>`, `<definition>`, `<example>`, `<exercise>`, `<image>`, etc.

## External Dependencies
- `pretext == 2.18.0` (Python CLI) - manages all builds and compilation
- **No database**, **no APIs**, **no backend services** - pure static content generation
- TeXLive (for LaTeX image generation and PDF output)
- Minimal additional dependencies specified in `requirements.txt`

## Cross-References & Links
- Use `<xref>` tag: `<xref ref="sec-section-ellipse" text="type-global" />`
- Cross-references work across all sections and chapters automatically
