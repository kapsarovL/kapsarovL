# 👋 Hey, I'm Lazar

**Frontend Design Engineer** | Next.js | React | TypeScript

[Portfolio](https://www.lazarkapsarov.vercel.app) | [LinkedIn](https://www.linkedin.com/in/lazar-kapsarov) | [Twitter](https://x.com/kapsarovlazar)

> Bridging design token pipelines with production codebases.

> **Provenance:** the incident ticket and the debugging rulebook below are worked examples of a bug class, not records from a named employer. Every number and tool named in this file is verifiable in the repositories above and below.

* **Target Role:** Design Engineer / Frontend Systems Engineer
* **Core Disciplines:** Design Systems, Component Primitives, Micro-Interactions, A11y Architecture
* **Execution Stack:** React 19, Next.js 16, TypeScript (strict), SCSS Modules, Node

---

## 1. Technical Capabilities Matrix

| Domain | Systems & Infrastructure |
| :--- | :--- |
| **Interface Systems** | Design Tokens (CSS custom properties), Component Primitives, Polymorphic Types (`asChild`), SCSS Modules |
| **Accessibility** | WAI-ARIA Authoring Practices — roving tabindex, focus restoration, landmark fallbacks — applied across 30 primitive files |
| **Quality** | Strict TypeScript, Biome (lint + format), Vitest, supply-chain scanning (gitleaks / osv-scanner / trivy) as a blocking pre-commit gate |
| **Tooling & Automation** | Yarn 4 workspaces, CI workflows, enforced git hooks |

---

## 2. Prismaflux-Design-System

**Private repo** — ask and I'll grant access. Token-driven React component library, Next.js 16 / React 19 / SCSS Modules.

| | |
| :--- | :--- |
| **Primitives** | 22 components, each a single `.tsx` with a SCSS Module, barrel-exported from `components/primitives/index.ts` |
| **Tokens** | 460 custom properties in `styles/tokens.scss` across color, input, spacing, radius, typography, motion — mirrored by a `DESIGN.md` manifest |
| **Theming** | Dark-first scale on a Vercel-neutral single-blue accent, driven by CSS custom properties rather than a runtime provider |
| **Workbench** | Live theme playground, command palette, and per-component docs rendered from the same token source |
| **Supply chain** | gitleaks + osv-scanner + trivy as blocking pre-commit and CI gates |

`next.config.ts` documents its `remotePatterns: hostname: "**"` choice in-file as a security tradeoff, with the tightened allow-list as the stated upgrade path.

---

## 3. Incident Ticket #INC-8042: Focus Loss on Dynamic Overlay Unmount

* **Status:** Resolved | **Severity:** P2 (Accessibility Regression)
* **Component:** `Dialog` (overlay primitive, `components/primitives/dialog/`)

### 1. Timeline

* **10:14 UTC** — Incident flagged: Focus trapped on `document.body` following nested overlay closure on dynamic routes.
* **10:45 UTC** — Issue reproduced in staging; SC 2.4.3 Focus Order violation confirmed.
* **11:20 UTC** — Memory trace identified a race condition between unmounting trigger elements and focus locks.
* **12:00 UTC** — Fallback focus restoration algorithm implemented; validated via VoiceOver and NVDA.

### 2. Forensic Evidence

When the overlay unmounted, it tried to return focus to `document.activeElement`, captured once in the effect body. Because the parent state change unmounted the trigger element in the same commit, that node was already detached from the document by the time the cleanup ran. `previousFocus?.focus()` is a no-op on a detached node, so focus fell through to `document.body`.

```tsx
// BEFORE (Buggy Cleanup)
useEffect(() => {
  const previousFocus = document.activeElement as HTMLElement;
  return () => {
    previousFocus?.focus(); // Fails silently if the node is already detached
  };
}, []);
```

```tsx
// AFTER (Refined Fallback Resolution)
useEffect(() => {
  const previousFocus = document.activeElement as HTMLElement | null;
  return () => {
    if (previousFocus && document.body.contains(previousFocus)) {
      previousFocus.focus();
      return;
    }
    // Fallback: focus the main content landmark if the trigger node is dead.
    // The landmark must carry tabindex="-1" in markup to be focusable here.
    document.querySelector<HTMLElement>("[data-main-content]")?.focus();
  };
}, []);
```

---

## 4. The Debugging Rulebook (Problem → Solution)

Repeatable patterns, symptom to root cause to fix.

| Observed Symptom | Identified Root Cause | Corrective Engineering Action |
| :--- | :--- | :--- |
| **Focus dropped to `body` after closing overlay** | Trigger element was unmounted from DOM while overlay unmounted. | Check `document.body.contains(target)`; fallback to `[data-main-content]` if target is detached. |
| **UI stutter during canvas drag operations** | Pointer events triggering top-level React context re-renders on every pixel move. | Decouple dragging state from React tree; mutate ref coordinates and schedule visual updates via `requestAnimationFrame`. |
| **Flash of Unstyled Content (FOUC) on load** | Reading theme preference from `localStorage` inside client-side `useEffect`. | Inject blocking, zero-dependency inline script inside `<head>` to resolve theme class prior to initial paint. |
| **Secret API keys exposed in commit history** | Hardcoded credentials or accidentally committing un-ignored `.env` files. | Add `.env` to `.gitignore`; enforce secret scanning as a blocking pre-commit gate, plus the same scan in CI. |

---

## 5. Security & Secret Isolation Rules

Three rules keep credentials out of the repository:

1. **Environment Variable Separation:** Store all keys in a `.env` file that is strictly listed in `.gitignore`.
2. **Blocking Scanner, Not a Warning:** gitleaks and a dependency scan run as a pre-commit hook that fails the commit on a finding, so a secret never reaches history in the first place.
3. **CI Is the Second Gate:** the same gitleaks / osv-scanner / trivy pass runs on every push and pull request, catching anything that arrives by another route.
