# Remove Two Index Links Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Remove the Overhead Variances and Segment Margin cards from the animation hub while retaining their standalone HTML files.

**Architecture:** The hub cards are rendered from item objects in the Unit 3 JavaScript data array in `index.html`. Delete only the two targeted objects and leave the standalone pages and all other hub data unchanged.

**Tech Stack:** Static HTML, CSS, and browser JavaScript; PowerShell verification; Git.

## Global Constraints

- Keep `Ch10_OverheadVariances.html` and `Ch11_SegmentMargin.html` in the repository.
- Do not alter any other chapter links, page content, styling, or behavior.

---

### Task 1: Remove the Two Hub Entries

**Files:**
- Modify: `index.html:159-162`
- Test: shell assertions against `index.html` and the two standalone HTML files

**Interfaces:**
- Consumes: the existing Unit 3 `items` array used by the hub renderer
- Produces: the same array without the Overhead Variances and Segment Margin item objects

- [ ] **Step 1: Run the desired-state assertion and verify it fails**

```powershell
$index = Get-Content -Raw 'index.html'
if ($index -match 'Ch10_OverheadVariances\.html|Ch11_SegmentMargin\.html') {
    throw 'The two unwanted index links are still present.'
}
```

Expected: FAIL with `The two unwanted index links are still present.`

- [ ] **Step 2: Delete only the two targeted item objects**

Remove these exact lines from `index.html`:

```javascript
    { ch:"Ch 10", title:"Overhead Variances", desc:"Variable and fixed overhead variance signals.", time:"8–10 min", file:"Ch10_OverheadVariances.html" },
    { ch:"Ch 11", title:"Segment Margin", desc:"Traceable vs. common fixed costs in segment reports.", time:"7–9 min", file:"Ch11_SegmentMargin.html" },
```

- [ ] **Step 3: Run the desired-state and file-retention assertions**

```powershell
$index = Get-Content -Raw 'index.html'
if ($index -match 'Ch10_OverheadVariances\.html|Ch11_SegmentMargin\.html') {
    throw 'The two unwanted index links are still present.'
}
if (-not (Test-Path 'Ch10_OverheadVariances.html') -or -not (Test-Path 'Ch11_SegmentMargin.html')) {
    throw 'A standalone animation file is missing.'
}
```

Expected: PASS with exit code 0.

- [ ] **Step 4: Check the exact diff and whitespace**

Run:

```powershell
git diff --check
git diff -- index.html
```

Expected: no whitespace errors and exactly two deleted item lines.

- [ ] **Step 5: Commit and push**

```powershell
git add -- index.html docs/superpowers/plans/2026-07-30-remove-two-index-links.md
git commit -m "Remove two animation links from hub"
git push origin main
```

Expected: commit succeeds and `origin/main` advances.
