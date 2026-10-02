# CLAUDE.md

Guide for maintaining the HTML & JavaScript interactive course.

## Project Overview

A self-contained, single-file HTML course teaching HTML and JavaScript fundamentals to programmers with Java/C++/Python background. No build tools, no server - just open `index.html` in a browser.

## File Structure

```
html-js-course/
├── index.html    # The entire course (HTML + CSS + JS)
├── README.md     # User-facing documentation
└── CLAUDE.md     # This file
```

## Architecture

Everything lives in `index.html`:

```
┌─────────────────────────────────────┐
│ <head>                              │
│   - Meta tags, title                │
│   - Highlight.js CDN links          │
│   - <style> block (all CSS)         │
│ </head>                             │
├─────────────────────────────────────┤
│ <body>                              │
│   - <header> with progress bar      │
│   - <aside class="toc-sidebar">     │
│   - <main class="main-content">     │
│       - 10 lesson divs              │
│       - nav buttons                 │
│   - <script> block (all JS)         │
│ </body>                             │
└─────────────────────────────────────┘
```

## Key Patterns

### Lesson Structure

Each lesson is a div with `data-lesson` attribute:

```html
<div class="lesson" data-lesson="4">
    <h2>Lesson 4: Topic Name (XX min)</h2>

    <p>Intro text...</p>

    <h3>Subsection</h3>
    <!-- content -->

    <div class="interactive-demo">
        <!-- demo content -->
    </div>

    <div class="further-reading">
        <h4>Further Reading</h4>
        <ul>
            <li><a href="...">Resource</a></li>
        </ul>
    </div>
</div>
```

### Code Blocks

```html
<div class="code-block">
    <div class="code-header">
        <span>filename.js</span>
    </div>
    <pre><code class="language-javascript">// code here</code></pre>
</div>
```

Supported languages: `language-javascript`, `language-html`, `language-css`

### Interactive Demos

Pattern with "Show Code" functionality:

```html
<div class="interactive-demo">
    <div class="demo-title">Demo: Description</div>
    <div id="demoNElement">
        <!-- Interactive elements -->
    </div>
    <button onclick="demoN()">Run Demo</button>
    <div class="output" id="demoNOutput"></div>

    <div class="output-controls">
        <button class="view-source-btn"
                onclick="showDemoSource('demoNElement', 'demoNSource', 'demoN')">
            Show Code
        </button>
    </div>
    <div class="source-view" id="demoNSource"></div>
</div>
```

The JS function `showDemoSource(elementId, sourceId, jsFunctionName)` auto-generates HTML/JS source view with copy buttons.

### Special Content Boxes

| Class | Purpose | Color |
|-------|---------|-------|
| `.key-concept` | Important concept callouts | Yellow |
| `.comparison` | Analogies to Java/C++/Python | Blue |
| `.further-reading` | External resource links | Green |

### ASCII Diagrams

Use box-drawing characters for diagrams (per user's CLAUDE.md preferences):

```
─ │ ┌ ┐ ┤ ┘ └
```

Example from DOM tree visualization in Lesson 4.

## JavaScript Functions

### Navigation
- `changeLesson(direction)` - Move forward/backward through lessons
- `jumpToLesson(n)` - Go directly to lesson n
- `updateProgress()` - Update progress bar

### Demo Support
- `demoN()` - Each interactive demo has a numbered function (demo1, demo2, etc.)
- `showDemoSource(elementId, sourceId, jsFunctionName)` - Generate source code view
- `createOutputWithSource(html, id, fullDocument)` - Create output with view-source toggle
- `copyGeneratedCode(codeId)` - Copy code to clipboard
- `showLivePreview(button)` - Toggle live HTML preview in iframe

### State
- `currentLesson` - Tracks active lesson (1-10)
- `totalLessons` - Constant: 10
- `todos[]` - State for Lesson 9 todo app demo

## Adding Content

### New Lesson
1. Add lesson div in `.main-content` section
2. Add TOC entry in `.toc-sidebar`
3. Update `totalLessons` if extending beyond 10

### New Interactive Demo
1. Create HTML structure following the pattern above
2. Add corresponding `demoN()` function in the script block
3. Use unique IDs for elements, outputs, and source views

### New Code Block
Just use the `.code-block` pattern. Highlight.js runs on page load (`hljs.highlightAll()`).

## Dependencies

Only external dependency: **Highlight.js** (loaded from CDN)
- CSS: `atom-one-dark.min.css`
- JS: `highlight.min.js`, `javascript.min.js`, `xml.min.js`

## Testing

1. Open `index.html` in browser
2. Navigate through all lessons
3. Test each interactive demo
4. Verify "Show Code" and "Live Preview" buttons work
5. Test on mobile viewport (responsive layout)

## Gotchas

- When adding code examples with HTML, escape `<` and `>` as `&lt;` and `&gt;`
- Demo IDs must be unique across the entire file
- The TOC links use `onclick="jumpToLesson(N)"` - keep N matching `data-lesson`
- Progress bar calculation assumes lessons are numbered 1 through `totalLessons`
