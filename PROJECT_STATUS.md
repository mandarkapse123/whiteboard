# Studio — Master Platform Project Status, Architecture & Roadmap

**Current Version**: `v5.8 (shadcn/ui Edition)`  
**Repository**: [https://github.com/mandarkapse123/whiteboard](https://github.com/mandarkapse123/whiteboard)  
**Live Deployments**:
- ⚡ **Vercel**: `https://whiteboard-one-eta.vercel.app`
- 🔥 **Firebase Hosting**: `https://whiteboard-c6f38.web.app`

---

## 📋 1. What We Have Implemented Till Now

### 🏠 Master Workspace & Navigation
- **Top Navigation Bar**: Seamless tab routing between `[ 🏠 Home ] [ 📖 Books ] [ 📰 Articles ] [ 🔬 Research Whiteboard ] [ 📊 Dashboard ]`.
- **Omni-Search (`⌘K`)**: Unified cross-platform search across books, articles, whiteboard notes, and mindmap concepts.
- **Night / Dark Mode**: System-preference auto-detection (`prefers-color-scheme: dark`) with 1-click `🌓` manual toggle in the top navbar.
- **shadcn/ui Design System**: Complete Zinc palette integration with clean primary, secondary, and outline button variants, segmented tabs, and focus rings.

---

### 📖 Books & EPUB/PDF E-Reader
- **E-Reader**: Fullscreen reading for both `.epub` (native client-side JSZip parsing) and `.pdf` documents.
- **In-Book Text Highlighting**: Floating highlight toolbar with 5 colors (🟡 Yellow, 🟢 Green, 🔵 Blue, 🌸 Pink, 🟣 Purple).
- **Inline Annotation Notes**: Attach sticky commentary notes to highlighted passages.
- **Bookmarks Drawer & Chapter Jumps**: Dedicated drawer showing page bookmarks and highlight excerpts with 1-click jump-to-page navigation.
- **Genre Filter Shelf**: Organize books by genre with progress indicators.

---

### 📰 Articles & Papers Library
- **Article Manager**: Add web articles with external URL redirection, reading time, and status tags.
- **Direct Canvas Quotes**: 1-click "Send to Whiteboard" to convert article highlights and excerpts into research canvas cards.
- **Topic Categorization**: Filter essays and papers by research themes.

---

### 🔬 Research Whiteboard & Dual Mindmap System
- **60fps Direct-DOM Whiteboard**: Hardware-accelerated canvas transforms eliminating rendering lag.
- **Zero Iframe Reloads**: In-canvas browser windows preserve active browsing sessions without refreshing on node movement.
- **Visual Connection Ports**: 4 interactive anchor dots (Top, Bottom, Left, Right) on cards/shapes for intuitive drag-to-connect relations.
- **Dedicated Connector Tool (`↗`)**: Bottom dock tool to click Card A then Card B to establish connector arrows.
- **Tool Behavior Separation**:
  - **Pointer (`V`)**: Dedicated purely to selecting, dragging cards, and pulling connection ports. Does NOT drag/pan the canvas.
  - **Hand (`H`)**: Smooth canvas panning. (Also supports `Spacebar` hold or middle-click).
- **Pure-Symbol FigJam Dock**: 8 geometric shapes (Rectangle, Rounded, Circle, Diamond, Cylinder, Cloud, Star, Section Frame), Sticky Notes, Cards, Browser Tabs, and Zoom controls.
- **NotebookLM-Style Hierarchical Mindmap**:
  - Structured tree view with curved SVG cubic bezier branches.
  - Multi-level color coding (Root: Slate, Level 1: Charcoal, Level 2: Forest Green, Level 3: Mint).
  - Expandable/collapsible branch count pills (`<` / `>`).
  - Inline double-click node text editing.
  - `⚡ Auto-Branch`: 1-click extraction of your books, bookmarks, and articles into a structured concept tree.
  - `💡 Export to Canvas`: 1-click projection of the mindmap tree onto the freeform whiteboard as interconnected cards.

---

### ☁️ Cloud Backend & Deployment Architecture
- **Firebase Firestore Backend**: Real-time snapshot sync and restore for books, bookmarks, articles, genres, whiteboards, and mindmaps.
- **Hardcoded Firebase Project**: Configured for project `whiteboard-c6f38` with active status indicators (`🔥 Synced`).
- **1-Click Multi-Cloud**: Dual support for Firebase Firestore and Supabase.
- **Multi-Platform Hosting**: Automated continuous deployment on Vercel and pre-configured Firebase Hosting CLI.

---

## 🔍 2. Issues Identified & Fixed in This Build

| Issue | Root Cause | Fix Applied |
| :--- | :--- | :--- |
| **Mindmap showing over Dashboard** | `#mm-viewport` HTML was unclosed and placed outside the main view container, causing it to float globally. | Encapsulated `#mindmap-container` and `#mm-viewport` strictly inside `#canvas-container` and isolated visibility per tab in `setMode()`. |
| **Whiteboard and Mindmap visible at the same time** | Both `#viewport` and `#mindmap-container` were set to `display: flex` simultaneously without mutual exclusion. | Strictly toggled `style.display = 'none'` / `'flex'` between `#viewport` and `#mindmap-container` in `setResearchView()`. |
| **Canvas moving with Pointer tool** | Pointerdown event on canvas background was triggering `panState` regardless of active tool. | Panning is now strictly restricted to `activeTool === 'pan'`, `Spacebar` hold, or middle-click. Pointer tool only selects/drags cards. |
| **Mindmap text collapsing vertically** | CSS `word-break: break-word` inside an inline-flex element without min-width basis caused 1-char columns. | Applied `white-space: nowrap`, `overflow: hidden`, `text-overflow: ellipsis`, and expanded `320px` horizontal clearance. |
| **New topic showing old topic data** | `switchTopic()` was not resetting `state.mindmapTree` or initializing clean boards for newly created topics. | Ensured new topics initialize with 100% clean blank boards and isolated root mindmap trees. |

---

## 🚀 3. What We Are Planning Next (Roadmap)

1. **Multi-User Collaboration**: Real-time multi-cursor collaboration on the whiteboard and mindmap using Firebase Realtime Presence.
2. **AI Synthesis & Auto-Summarization**: Built-in Gemini/LLM assistant to ask questions across all saved books, articles, and canvas cards.
3. **PDF Annotation Highlighting**: Extend highlight selection overlay to embedded PDF viewers.
4. **Rich Export Formats**: Export whiteboards and mindmaps as high-resolution PNG, SVG, or Markdown reports.
5. **Offline PWA Support**: Service Worker caching for complete offline functionality on tablets and mobile devices.
