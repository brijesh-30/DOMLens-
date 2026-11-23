# DOMLens ⚡

<p align="center">
  <img src="assets/logo.svg" alt="DOMLens Logo" width="96" height="96" />
</p>

<h3 align="center">Point. Inspect. Edit with AI. Generate. Build.</h3>

<p align="center">
  A Production-Grade Visual HTML Inspector, Computed Style Analyzer, React (Tailwind JSX) &amp; HTML+CSS Component Generator, Asset Tree Scanner, and Natural Language LLM Component Editor for Chrome (Manifest V3).
</p>

<p align="center">
  <a href="#-key-features"><img src="https://img.shields.io/badge/Chrome%20Extension-Manifest%20V3-4285F4?style=for-the-badge&logo=googlechrome&logoColor=white" alt="Manifest V3" /></a>
  <a href="#-technology-stack"><img src="https://img.shields.io/badge/JavaScript-ES2023-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" alt="ES2023" /></a>
  <a href="#-zero-dependencies"><img src="https://img.shields.io/badge/Dependencies-0%20(Pure%20Vanilla)-success?style=for-the-badge" alt="Zero Dependencies" /></a>
  <a href="#-multi-provider-ai-engine"><img src="https://img.shields.io/badge/AI%20Engine-Gemini%20%7C%20OpenAI%20%7C%20Groq%20%7C%20OpenRouter-blueviolet?style=for-the-badge" alt="AI Multi-Provider" /></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge" alt="License: MIT" /></a>
</p>

---

## 📖 Table of Contents

- [Overview](#-overview)
- [Why DOMLens?](#-why-domlens)
- [9 Ordered Feature Tabs](#-9-ordered-feature-tabs)
- [Architecture & State Management](#-architecture--state-management)
- [Multi-Provider AI Engine](#-multi-provider-ai-engine)
- [Technology Stack](#-technology-stack)
- [Zero Dependencies](#-zero-dependencies)
- [Installation & Quick Start](#-installation--quick-start)
- [Keyboard Shortcuts](#-keyboard-shortcuts)
- [Project Directory Structure](#-project-directory-structure)
- [Technical Documentation Matrix](#-technical-documentation-matrix)
- [Contributing](#-contributing)
- [License & Academic Credits](#-license--academic-credits)

---

## 🌟 Overview

**DOMLens** is a lightweight, high-precision Chrome Extension built on **Manifest V3** that transforms any website into an interactive design and engineering workbench. 

Rather than sifting through convoluted browser DevTools panels, DOMLens lets you click any element on **any webpage** to extract clean computed styles, box model metrics, color palettes, and typography tokens. It synthesizes clean, production-ready **React (Tailwind JSX)** or **HTML+CSS** code with one click, scans nested asset trees (including YouTube thumbnails and SVGs), and lets you rewrite component markup and styling on the fly using natural-language AI prompts.

---

## 💡 Why DOMLens?

| Traditional DevTools | DOMLens |
| :--- | :--- |
| Buried under hundreds of inherited CSS rules | Filters down to essential, computed visual properties instantly |
| Manual copy-pasting of messy DOM fragments | Generates clean, isolated **React (Tailwind JSX)** or **HTML+CSS** components |
| Clumsy manual CSS editing to test design tweaks | **Natural language AI prompts** ("make modern dark mode", "add glassy blur") |
| Hard to extract nested SVGs, images, and thumbnails | **Deep Subtree Asset Scanner** with format filters and instant download/copy |
| Pollutes global host page styles | 100% encapsulated inside **Shadow DOM** and theme-isolated sandboxed iframe |

---

## 🚀 9 Ordered Feature Tabs

```text
┌────────────────────────────────────────────────────────────────────────┐
│  👁️ Preview │ ⓘ Info │ 🔤 Type │ 🎨 Color │ 📐 Box │ 📄 Code │ 💬 AI │ 🖼️ Assets │ ⚙️ Setup │
└────────────────────────────────────────────────────────────────────────┘
```

1. 👁️ **Live Preview**:
   - Isolated sandbox `<iframe>` rendering with **Canvas Background Isolation** (toggle `#000000` Dark vs `#FFFFFF` Light while preserving component backgrounds).
   - Canvas zoom controls (`-`, `Fit`, `100%`, `+`) with true intrinsic aspect-ratio preservation.
2. ⓘ **Overview**:
   - Element tag, computed dimensions, unique CSS selector path, DOM hierarchy depth level, and quick inspection actions.
3. 🔤 **Typography**:
   - Real-time detection of font family, font size, weight, line-height, letter-spacing, text alignment, and text transforms.
4. 🎨 **Colors**:
   - Text color, background color, border color, and visual color swatch pills with one-click HEX/RGB clipboard copy.
5. 📐 **Layout (Box Model)**:
   - Interactive visual box model diagram illustrating computed margin, padding, border, and content dimensions with flexbox and CSS grid telemetry.
6. 📄 **Code Synthesizer**:
   - Clean component generator supporting strictly two export targets:
     - **React (Tailwind JSX)**: Web-component sanitized, JSX-attribute compliant (`className`, `htmlFor`), mapped to Tailwind utility classes.
     - **HTML + CSS**: Clean structural selectors (`${rootSelector} > ${relPath}`) preventing selector collision.
7. 💬 **Edit with AI**:
   - Natural language LLM component editor. Describe what you want to change (e.g., *"Make this button look like a glassy neumorphic card"*), preview the result live, and revert anytime with the **Reset** rollback buffer.
8. 🖼️ **Assets Scanner**:
   - Deep DOM subtree media extractor: detects YouTube high-res thumbnails, `<img>`, `srcset`, inline `<svg>`, CSS `background-image` URLs, and `<video>` posters.
   - Format filter pills: `All`, `Images`, `SVG`, `PNG`, `JPG`, `WEBP`, `GIF`, and `Other`.
9. ⚙️ **Settings**:
   - Multi-provider AI configuration (`gemini-3.8-flash` default, OpenAI, OpenRouter, Groq) with masked API key storage and extension theme switcher.

---

## 🏛️ Architecture & State Management

DOMLens is architected around a strict **Single Source of Truth (`ComponentState`)** to ensure predictable state propagation across all tabs:

```text
USER SELECTS ELEMENT (Capture Phase Click Intercept)
                     │
                     ▼
       EXTRACT HTML + CSS + ASSETS
                     │
                     ▼
          CENTRAL COMPONENT STATE
    ├── original (Immutable: HTML + CSS + Assets + Telemetry)
    └── current  (Authoritative: HTML + CSS)
         │
         ├───► LIVE PREVIEW CANVAS (Isolated Iframe)
         │
         ├───► CODE SYNTHESIZER    (React Tailwind JSX & HTML+CSS)
         │
         ▼
USER PROMPT EDIT (Tab 7: "Edit with AI")
         │
         ▼
LLM ENGINE (Structured JSON: { html, css, changes })
         │
         ▼
UPDATE CURRENT STATE ──► Live Preview re-renders & React code regenerates
```

- **Shadow DOM Encapsulation**: Injected UI lives inside `<website-inspector-root>` using Shadow DOM to prevent host page CSS pollution.
- **Normalized Root Positioning**: Component root positioning (`top/left/right/bottom` and external margins) is normalized to `position: relative !important; margin: 0 auto !important;` so extracted elements render cleanly inside the preview canvas.
- **Pointer-Capture Dragging**: Panel header uses `setPointerCapture` and dynamic iframe shielding during movement, preventing cursor drag sticking.

---

## 🤖 Multi-Provider AI Engine

DOMLens connects to leading AI providers directly from your browser with client-side key storage (`chrome.storage.local`):

| Provider | Default Model | Capabilities |
| :--- | :--- | :--- |
| **Google Gemini** *(Recommended)* | `gemini-3.8-flash` | Ultra-fast structured JSON output, native schema enforcement (`responseSchema`), zero latency |
| **OpenAI** | `gpt-4o` / `gpt-4o-mini` | Precise layout transformations, semantic code generation |
| **Groq** | `llama-3.3-70b-versatile` | Blazing-fast inference speeds for rapid component experimentation |
| **OpenRouter** | Any OpenRouter Model | Support for Claude 3.5 Sonnet, DeepSeek-V3, and open weights via Server-Sent Events (SSE) |

---

## 🛠️ Technology Stack

| Layer | Technology | Specification / Standard |
| :--- | :--- | :--- |
| **Platform** | Chrome Extension Manifest V3 | Service Worker Architecture (`manifest_version: 3`) |
| **Language & Subsystems** | Vanilla ES2023 JavaScript | Native ES Modules (`import`/`export`) |
| **UI & Styling** | HTML5 & CSS Custom Properties | Modular Design Tokens (`utils/theme.js`) |
| **DOM Isolation** | Shadow DOM API | Encapsulated root (`<website-inspector-root>`) |
| **Build System** | Zero External Bundlers | Raw JavaScript / Native Web APIs |

---

## 🚫 Zero Dependencies

DOMLens requires **no bundlers**, **no build steps**, and **no third-party dependencies**:
- ❌ No Node.js runtime required to run the extension
- ❌ No Webpack, Vite, or Rollup compilation step
- ❌ No React, Vue, jQuery, TailwindCSS, or Bootstrap packages bundled
- 🟢 **Pure Vanilla JavaScript** executing directly in the browser engine

---

## 📥 Installation & Quick Start

### 1. Clone or Download the Repository
```bash
git clone https://github.com/<YOUR-USERNAME>/DOMLens.git
cd DOMLens
```

### 2. Load the Unpacked Extension in Chrome
1. Open Google Chrome (or Edge / Brave / Opera).
2. Navigate to `chrome://extensions/` in the address bar.
3. In the top-right corner, toggle **Developer mode** to **ON**.
4. Click the **Load unpacked** button in the top-left toolbar.
5. Select the **`DOMLens`** folder (the folder containing `manifest.json`).

### 3. Start Inspecting
1. Open any website (e.g., `https://github.com` or `https://news.ycombinator.com`).
2. Press **`Ctrl + Shift + I`** (Mac: **`Cmd + Shift + I`**) or click the DOMLens extension icon in the toolbar and hit **Enable Inspector**.
3. Hover and click on any element to open the inspector panel!

---

## ⌨️ Keyboard Shortcuts

| Shortcut (Windows / Linux) | Shortcut (macOS) | Action |
| :--- | :--- | :--- |
| `Ctrl + Shift + I` | `Cmd + Shift + I` | Toggle Element Inspect Mode on active tab |
| `Escape` | `Escape` | Cancel element selection / Close panel |
| `Click` | `Click` | Select element and lock inspector panel |

---

## 📁 Project Directory Structure

```text
DOMLens/
├── manifest.json                  # Manifest V3 extension configuration
├── background/
│   └── background.js              # Service worker handling commands & tab messaging
├── content/
│   ├── loader.js                  # Dynamic ES module content script injector
│   ├── inspector.js               # Hover highlight, event capture & element selection
│   ├── overlay.js                 # Visual bounding box & dimension badge overlay
│   ├── extractor.js               # Computed style extractor & structural CSS builder
│   └── panel.js                   # 9-tab inspector panel controller & UI coordinator
├── popup/
│   ├── popup.html                 # Extension toolbar action popup UI
│   ├── popup.css                  # Action popup styles
│   └── popup.js                   # Popup toggle logic & status indicators
├── utils/
│   ├── component_state.js         # Central Single Source of Truth state manager
│   ├── preview_renderer.js        # Sandboxed iframe canvas & zoom controller
│   ├── component_generator.js     # React (Tailwind JSX) & HTML+CSS synthesizer
│   ├── tailwind_mapper.js         # CSS to Tailwind utility class converter
│   ├── asset_extractor.js         # Deep DOM asset & YouTube thumbnail scanner
│   ├── llm_service.js             # Multi-provider AI client (Gemini, OpenAI, Groq, OpenRouter)
│   ├── prompt_generator.js        # Few-shot prompts for structured AI code editing
│   ├── theme.js                   # Design tokens, CSS variables & color themes
│   ├── selector.js                # Unique CSS selector path generator
│   ├── style.js                   # Computed style categorization & parsing
│   ├── dom.js                     # DOM traversal & coordinate math helpers
│   ├── clipboard.js               # Clipboard helper with toast feedback
│   └── toast.js                   # Floating notification toasts
├── assets/
│   ├── logo.svg                   # Vector brand identity
│   └── icons/                     # 16px, 48px, 128px extension icons
└── MD_Files/                      # Exhaustive 24-file technical documentation suite
```

---

## 📚 Technical Documentation Matrix

Comprehensive technical documentation is maintained in the [`MD_Files/`](MD_Files/) directory:

| Document | Focus Area |
| :--- | :--- |
| [01_PROJECT_OVERVIEW.md](MD_Files/01_PROJECT_OVERVIEW.md) | Vision, problem statement, core value proposition |
| [02_TECH_STACK.md](MD_Files/02_TECH_STACK.md) | Architectural standards & zero-dependency guarantee |
| [03_REQUIREMENTS.md](MD_Files/03_REQUIREMENTS.md) | Functional & non-functional requirements matrix |
| [04_FEATURES.md](MD_Files/04_FEATURES.md) | Deep dive into the 9 core feature subsystems |
| [05_SYSTEM_ARCHITECTURE.md](MD_Files/05_SYSTEM_ARCHITECTURE.md) | Complete system diagrams & state data flows |
| [06_EXTENSION_ARCHITECTURE.md](MD_Files/06_EXTENSION_ARCHITECTURE.md) | MV3 Service worker, lifecycle & loader strategy |
| [07_FOLDER_STRUCTURE.md](MD_Files/07_FOLDER_STRUCTURE.md) | Directory structure specifications |
| [08_FILE_RESPONSIBILITIES.md](MD_Files/08_FILE_RESPONSIBILITIES.md) | Granular responsibilities for every file |
| [09_DATA_FLOW.md](MD_Files/09_DATA_FLOW.md) | User interaction pipelines & event flow sequences |
| [10_DOM_INSPECTION.md](MD_Files/10_DOM_INSPECTION.md) | Event capture phase intercept & pointer math |
| [11_ELEMENT_INFORMATION_SCHEMA.md](MD_Files/11_ELEMENT_INFORMATION_SCHEMA.md) | JSON schemas for element telemetry & style payloads |
| [12_CHROME_APIS.md](MD_Files/12_CHROME_APIS.md) | Manifest V3 permissions justification |
| [13_UI_UX.md](MD_Files/13_UI_UX.md) | Shadow DOM design tokens & interaction design |
| [14_SECURITY.md](MD_Files/14_SECURITY.md) | CSP adherence, API key protection & iframe sandboxing |
| [15_TESTING.md](MD_Files/15_TESTING.md) | Manual test suites & cross-site verification matrix |
| [16_SETUP.md](MD_Files/16_SETUP.md) | Step-by-step developer setup & installation guide |
| [17_DEVELOPMENT_WORKFLOW.md](MD_Files/17_DEVELOPMENT_WORKFLOW.md) | Development lifecycle & debugging tips |
| [18_CODING_STANDARDS.md](MD_Files/18_CODING_STANDARDS.md) | ES2023 standards & modular design principles |
| [19_ERROR_HANDLING.md](MD_Files/19_ERROR_HANDLING.md) | Resilient error boundaries & fallback strategies |
| [20_CURRENT_IMPLEMENTATION.md](MD_Files/20_CURRENT_IMPLEMENTATION.md) | Implementation audit & feature readiness |
| [21_ROADMAP.md](MD_Files/21_ROADMAP.md) | Future enhancements & planned capabilities |
| [22_FUTURE_AI_INTEGRATION.md](MD_Files/22_FUTURE_AI_INTEGRATION.md) | Prompt engineering & structured schema design |
| [23_KNOWN_LIMITATIONS.md](MD_Files/23_KNOWN_LIMITATIONS.md) | Browser constraints (cross-origin iframes, chrome:// pages) |
| [24_CHANGELOG.md](MD_Files/24_CHANGELOG.md) | Release history & version notes |

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

1. Fork the repository.
2. Create your feature branch (`git checkout -b feature/AmazingFeature`).
3. Commit your changes (`git commit -m 'feat: Add some AmazingFeature'`).
4. Push to the branch (`git push origin feature/AmazingFeature`).
5. Open a Pull Request.

---

## 📄 License & Academic Credits

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

Developed with ❤️ by **Brijesh Sevra** and team at **Maharaja Sayajirao University of Baroda (Department of Computer Science & Engineering)** under the guidance of **Jhal Shah**.
