# Visual Learning Path Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a fixed floating button to both site pages that opens a full-screen overlay showing all 14 math sections organized into 4 learning phases.

**Architecture:** Pure HTML/CSS/JS additions inside the two existing page files. No new files, no build step. CSS goes inside the existing `<style>` block, HTML just before `</body>`, JS inside the existing `<script>` block (or a new inline `<script>` before `</body>`). Both pages share identical structure; only text labels differ.

**Tech Stack:** Vanilla HTML/CSS/JS, no dependencies.

---

## Files

- Modify: `docs/index.html` — ES page (Spanish labels)
- Modify: `docs/en/index.html` — EN page (English labels)

---

## Task 1: Add path overlay CSS and HTML+JS to the ES page

**Files:**
- Modify: `docs/index.html`

- [ ] **Step 1: Add CSS inside the existing `<style>` block**

Find the closing `</style>` tag in `docs/index.html` and insert the following immediately before it:

```css
  /* ── Path overlay ── */
  .path-btn {
    position: fixed;
    right: 32px;
    bottom: 32px;
    font-family: 'DM Mono', monospace;
    font-size: 11px;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    padding: 8px 16px;
    border: 1px solid var(--accent);
    background: transparent;
    color: var(--accent);
    cursor: pointer;
    border-radius: 2px;
    transition: all 0.2s;
    z-index: 50;
  }
  .path-btn:hover { background: var(--accent); color: var(--bg); }

  .path-overlay {
    position: fixed;
    inset: 0;
    background: rgba(13,15,20,0.92);
    z-index: 100;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 24px;
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.2s ease;
  }
  .path-overlay.open { opacity: 1; pointer-events: auto; }

  .path-panel {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 40px 48px;
    max-width: 680px;
    width: 100%;
    max-height: 80vh;
    overflow-y: auto;
    position: relative;
  }

  .path-close {
    position: absolute;
    top: 16px;
    right: 20px;
    font-family: 'DM Mono', monospace;
    font-size: 14px;
    color: var(--muted);
    background: none;
    border: none;
    cursor: pointer;
    transition: color 0.2s;
    line-height: 1;
  }
  .path-close:hover { color: var(--accent); }

  .path-title {
    font-family: 'DM Mono', monospace;
    font-size: 11px;
    letter-spacing: 0.2em;
    color: var(--accent);
    text-transform: uppercase;
    margin-bottom: 28px;
  }

  .path-phase-header {
    display: flex;
    align-items: center;
    gap: 12px;
    margin-bottom: 10px;
  }
  .path-phase-pill {
    font-family: 'DM Mono', monospace;
    font-size: 10px;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    padding: 4px 12px;
    border-radius: 2px;
    white-space: nowrap;
  }
  .path-phase-line { flex: 1; height: 1px; background: var(--border); }

  .path-chips {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
    padding-left: 8px;
    margin-bottom: 4px;
  }
  .path-chip {
    font-family: 'DM Mono', monospace;
    font-size: 9px;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--muted);
    background: var(--tag-bg);
    border: 1px solid var(--border);
    padding: 3px 9px;
    border-radius: 2px;
  }

  .path-connector {
    width: 1px;
    height: 20px;
    background: var(--border);
    margin-left: 16px;
  }

  @media (max-width: 700px) {
    .path-btn { right: 20px; bottom: 20px; }
    .path-panel { padding: 28px 20px; }
  }
```

- [ ] **Step 2: Add button + overlay HTML just before `</body>`**

Find `</body>` in `docs/index.html` and insert the following immediately before it:

```html
<button class="path-btn" onclick="openPath()">RUTA →</button>

<div class="path-overlay" id="pathOverlay" onclick="handleOverlayClick(event)">
  <div class="path-panel">
    <button class="path-close" onclick="closePath()">✕</button>
    <div class="path-title">// Ruta de aprendizaje</div>

    <div class="path-phase">
      <div class="path-phase-header">
        <span class="path-phase-pill" style="background:#2a1f0a;border:1px solid #c8a96e;color:#c8a96e">① Fundamentos</span>
        <div class="path-phase-line"></div>
      </div>
      <div class="path-chips">
        <span class="path-chip">00 · Álgebra Lineal</span>
        <span class="path-chip">01 · Geometría</span>
        <span class="path-chip">02 · Cálculo</span>
        <span class="path-chip">03 · Optimización</span>
      </div>
    </div>

    <div class="path-connector"></div>

    <div class="path-phase">
      <div class="path-phase-header">
        <span class="path-phase-pill" style="background:#0a1f2a;border:1px solid #7eb8c9;color:#7eb8c9">② Core ML</span>
        <div class="path-phase-line"></div>
      </div>
      <div class="path-chips">
        <span class="path-chip">04 · Probabilidad</span>
        <span class="path-chip">05 · Estadística</span>
        <span class="path-chip">06 · Teoría Información</span>
      </div>
    </div>

    <div class="path-connector"></div>

    <div class="path-phase">
      <div class="path-phase-header">
        <span class="path-phase-pill" style="background:#1a0a2a;border:1px solid #a87ec9;color:#a87ec9">③ Avanzado</span>
        <div class="path-phase-line"></div>
      </div>
      <div class="path-chips">
        <span class="path-chip">07 · Medidas</span>
        <span class="path-chip">08 · Señales</span>
        <span class="path-chip">09 · Grafos</span>
        <span class="path-chip">10 · Geom. Diferencial</span>
      </div>
    </div>

    <div class="path-connector"></div>

    <div class="path-phase">
      <div class="path-phase-header">
        <span class="path-phase-pill" style="background:#2a0a0a;border:1px solid #c97e7e;color:#c97e7e">④ IA Moderna</span>
        <div class="path-phase-line"></div>
      </div>
      <div class="path-chips">
        <span class="path-chip">11 · Transformers</span>
        <span class="path-chip">12 · Difusión</span>
        <span class="path-chip">13 · RL</span>
      </div>
    </div>
  </div>
</div>
```

- [ ] **Step 3: Add JS inside the existing `<script>` block**

Find `render();` near the bottom of the `<script>` block in `docs/index.html`. Add the following after it (before `</script>`):

```js
function openPath() {
  document.getElementById('pathOverlay').classList.add('open');
  document.body.style.overflow = 'hidden';
}
function closePath() {
  document.getElementById('pathOverlay').classList.remove('open');
  document.body.style.overflow = '';
}
function handleOverlayClick(e) {
  if (e.target === document.getElementById('pathOverlay')) closePath();
}
document.addEventListener('keydown', function(e) {
  if (e.key === 'Escape') closePath();
});
```

- [ ] **Step 4: Verify in browser**

Open `docs/index.html` in a browser (or the live site). Check:
- Gold "RUTA →" button is visible at bottom-right
- Clicking it opens the overlay with a dark backdrop
- All 4 phases show with correct colored pills and topic chips
- ✕ button closes it
- Clicking the dark backdrop outside the panel closes it
- Pressing Esc closes it
- Page does not scroll while overlay is open

- [ ] **Step 5: Commit**

```bash
git add docs/index.html
git commit -m "Add visual learning path overlay to ES page"
```

---

## Task 2: Add path overlay CSS and HTML+JS to the EN page

**Files:**
- Modify: `docs/en/index.html`

- [ ] **Step 1: Add CSS inside the existing `<style>` block**

Find the closing `</style>` tag in `docs/en/index.html` and insert the same CSS block as Task 1 Step 1 (identical — copy it exactly).

```css
  /* ── Path overlay ── */
  .path-btn {
    position: fixed;
    right: 32px;
    bottom: 32px;
    font-family: 'DM Mono', monospace;
    font-size: 11px;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    padding: 8px 16px;
    border: 1px solid var(--accent);
    background: transparent;
    color: var(--accent);
    cursor: pointer;
    border-radius: 2px;
    transition: all 0.2s;
    z-index: 50;
  }
  .path-btn:hover { background: var(--accent); color: var(--bg); }

  .path-overlay {
    position: fixed;
    inset: 0;
    background: rgba(13,15,20,0.92);
    z-index: 100;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 24px;
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.2s ease;
  }
  .path-overlay.open { opacity: 1; pointer-events: auto; }

  .path-panel {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 40px 48px;
    max-width: 680px;
    width: 100%;
    max-height: 80vh;
    overflow-y: auto;
    position: relative;
  }

  .path-close {
    position: absolute;
    top: 16px;
    right: 20px;
    font-family: 'DM Mono', monospace;
    font-size: 14px;
    color: var(--muted);
    background: none;
    border: none;
    cursor: pointer;
    transition: color 0.2s;
    line-height: 1;
  }
  .path-close:hover { color: var(--accent); }

  .path-title {
    font-family: 'DM Mono', monospace;
    font-size: 11px;
    letter-spacing: 0.2em;
    color: var(--accent);
    text-transform: uppercase;
    margin-bottom: 28px;
  }

  .path-phase-header {
    display: flex;
    align-items: center;
    gap: 12px;
    margin-bottom: 10px;
  }
  .path-phase-pill {
    font-family: 'DM Mono', monospace;
    font-size: 10px;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    padding: 4px 12px;
    border-radius: 2px;
    white-space: nowrap;
  }
  .path-phase-line { flex: 1; height: 1px; background: var(--border); }

  .path-chips {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
    padding-left: 8px;
    margin-bottom: 4px;
  }
  .path-chip {
    font-family: 'DM Mono', monospace;
    font-size: 9px;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--muted);
    background: var(--tag-bg);
    border: 1px solid var(--border);
    padding: 3px 9px;
    border-radius: 2px;
  }

  .path-connector {
    width: 1px;
    height: 20px;
    background: var(--border);
    margin-left: 16px;
  }

  @media (max-width: 700px) {
    .path-btn { right: 20px; bottom: 20px; }
    .path-panel { padding: 28px 20px; }
  }
```

- [ ] **Step 2: Add button + overlay HTML just before `</body>` — English labels**

Find `</body>` in `docs/en/index.html` and insert:

```html
<button class="path-btn" onclick="openPath()">PATH →</button>

<div class="path-overlay" id="pathOverlay" onclick="handleOverlayClick(event)">
  <div class="path-panel">
    <button class="path-close" onclick="closePath()">✕</button>
    <div class="path-title">// Learning path</div>

    <div class="path-phase">
      <div class="path-phase-header">
        <span class="path-phase-pill" style="background:#2a1f0a;border:1px solid #c8a96e;color:#c8a96e">① Foundations</span>
        <div class="path-phase-line"></div>
      </div>
      <div class="path-chips">
        <span class="path-chip">00 · Linear Algebra</span>
        <span class="path-chip">01 · Geometry</span>
        <span class="path-chip">02 · Calculus</span>
        <span class="path-chip">03 · Optimization</span>
      </div>
    </div>

    <div class="path-connector"></div>

    <div class="path-phase">
      <div class="path-phase-header">
        <span class="path-phase-pill" style="background:#0a1f2a;border:1px solid #7eb8c9;color:#7eb8c9">② Core ML</span>
        <div class="path-phase-line"></div>
      </div>
      <div class="path-chips">
        <span class="path-chip">04 · Probability</span>
        <span class="path-chip">05 · Statistics</span>
        <span class="path-chip">06 · Info Theory</span>
      </div>
    </div>

    <div class="path-connector"></div>

    <div class="path-phase">
      <div class="path-phase-header">
        <span class="path-phase-pill" style="background:#1a0a2a;border:1px solid #a87ec9;color:#a87ec9">③ Advanced</span>
        <div class="path-phase-line"></div>
      </div>
      <div class="path-chips">
        <span class="path-chip">07 · Measure Theory</span>
        <span class="path-chip">08 · Signals</span>
        <span class="path-chip">09 · Graphs</span>
        <span class="path-chip">10 · Diff. Geometry</span>
      </div>
    </div>

    <div class="path-connector"></div>

    <div class="path-phase">
      <div class="path-phase-header">
        <span class="path-phase-pill" style="background:#2a0a0a;border:1px solid #c97e7e;color:#c97e7e">④ Modern AI</span>
        <div class="path-phase-line"></div>
      </div>
      <div class="path-chips">
        <span class="path-chip">11 · Transformers</span>
        <span class="path-chip">12 · Diffusion</span>
        <span class="path-chip">13 · RL</span>
      </div>
    </div>
  </div>
</div>
```

- [ ] **Step 3: Add JS inside the existing `<script>` block**

Find `render();` near the bottom of the `<script>` block in `docs/en/index.html`. Add the following after it (before `</script>`):

```js
function openPath() {
  document.getElementById('pathOverlay').classList.add('open');
  document.body.style.overflow = 'hidden';
}
function closePath() {
  document.getElementById('pathOverlay').classList.remove('open');
  document.body.style.overflow = '';
}
function handleOverlayClick(e) {
  if (e.target === document.getElementById('pathOverlay')) closePath();
}
document.addEventListener('keydown', function(e) {
  if (e.key === 'Escape') closePath();
});
```

- [ ] **Step 4: Verify in browser**

Open `docs/en/index.html`. Check:
- Gold "PATH →" button visible at bottom-right
- Overlay opens with English labels (Foundations, Core ML, Advanced, Modern AI)
- All 4 phases render with correct chips and colors
- ✕, backdrop click, and Esc all close it

- [ ] **Step 5: Commit**

```bash
git add docs/en/index.html
git commit -m "Add visual learning path overlay to EN page"
```

---

## Task 3: Housekeeping and push

**Files:**
- Modify: `.gitignore` (create if absent)

- [ ] **Step 1: Add `.superpowers/` to `.gitignore`**

Check if `.gitignore` exists at the repo root. If not, create it. Add this line:

```
.superpowers/
```

- [ ] **Step 2: Commit and push**

```bash
git add .gitignore
git commit -m "Ignore .superpowers/ brainstorm artifacts"
git push origin main
```
