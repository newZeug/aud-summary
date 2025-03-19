# aud-summary

## Markdown Viewer Features

The markdown viewer is a powerful tool that supports standard markdown syntax along with various extended features:

### LaTeX Support

The viewer can render LaTeX expressions using both KaTeX and MathJax:

1. **Inline Math**: Use single dollar signs `$...$` for inline math expressions
   - Example: $f(x) = x^2$

2. **Display Math**: Use double dollar signs `$$...$$` for centered block equations
   - Example: $$\sum_{i=1}^n i = \frac{n(n+1)}{2}$$

3. **Flalign Environment**: For aligned equations with left alignment
   ```latex
   \begin{flalign*}
   y &= mx + b \\
   y &= 2x + 1
   \end{flalign*}
   ```

4. **TikZ Diagrams**: For creating mathematical diagrams
   ```latex
   \begin{tikzpicture}
   \draw (0,0) -- (1,1) -- (2,0);
   \end{tikzpicture}
   ```

5. **Tables using Tabular**: For complex table layouts
   ```latex
   \begin{tabular}{|c|c|}
   1 & 2 \\
   3 & 4
   \end{tabular}
   ```

6. **Tile Rows**: For creating tile-based layouts
   ```latex
   \begin{tilerow}
   1 & 2 & 3
   \end{tilerow}
   ```

### Code Block Features

1. **Syntax Highlighting**: Supports multiple programming languages
2. **Line Numbers**: Automatic line numbering (disable with `.noLineNumbers`)
3. **Copy Button**: One-click code copying

### Extended Markdown Syntax

1. **Footnotes**: 
   ```md
   Here's a note[^1]
   [^1]: This is the footnote
   ```

2. **Text Formatting**:
   - Subscript: Use `~text~` for ~subscript~
   - Superscript: Use `^text^` for ^superscript^
   - Highlight: Use `==text==` for ==highlighted text==

3. **Symbols**:
   - Copyright: (c)
   - Registered: (r)
   - Trademark: (tm)
   - Plus/minus: +-

4. **Emojis**: Supports both emoji codes and emoticons
   - `:wink:` → 😉
   - `:-)` → 😊

### Image Handling

1. **Automatic Loading**: Images are loaded with a loading state
2. **Size Attributes**: Supports width and height via curly brackets
   ```md
   ![Alt text](image.png){width="300px", max-height="200px"}
   ```

### Navigation Features

1. **Table of Contents**: Automatically generated from headers
2. **Breadcrumb Navigation**: Shows current location in document
3. **Search**: Full-text search with context highlighting
4. **Section Management**: Drag and drop section reordering

### Chat Integration

1. **Context-Aware AI**: Ask questions about selected text
2. **Multiple AI Providers**:
   - Google Gemini
   - OpenAI (GPT)

### Export Options

1. **HTML**: Export as self-contained HTML file
2. **PDF**: Print-optimized layout
3. **File Management**: Add, delete, and reorder sections

### Customization

1. **Theme Support**: Light and dark modes
2. **Language**: Multiple UI language support
3. **Source Integration**: Load from various sources (GitHub, etc.)