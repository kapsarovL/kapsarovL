# Hey, I'm Lazar

**Frontend Design Engineer** | Next.js | React | TypeScript

[![link](https://www.readmecodegen.com/api/social-icon?name=link&size=24&bg=%23f3f4f6&link=https%3A%2F%2Fwww.lazarkapsarov.vercel.app&color=%23000000)](https://www.lazarkapsarov.vercel.app) | [![linkedin](https://www.readmecodegen.com/api/social-icon?name=linkedin&size=24&theme=dark&textAlignment=horizontal&color=%23ffffff&showText=true&link=https%3A%2F%2Fwww.linkedin.com%2Fin%2Flazar-kapsarov)](https://www.linkedin.com/in/lazar-kapsarov) | [![X Badge](https://img.shields.io/badge/X-000?logo=x&logoColor=fff&style=flat)](https://x.com/kapsarovlazar)

>Bridging design token pipelines with production codebases.

>**Provenance:** the incident ticket and the debugging rulebook below are worked examples of a bug class, not records from a named employer. Every number and tool named in this file is verifiable in the repositories above and below.

---

* **Target Role:** Design Engineer / Frontend Systems Engineer
* **Core Disciplines:** Design Systems, Component Primitives, Micro-Interactions, A11y Architecture
* **Execution Stack:**  ![JavaScript Badge](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=000&style=flat) ![React Badge](https://img.shields.io/badge/React-61DAFB?logo=react&logoColor=000&style=flat)  ![Next.js Badge](https://img.shields.io/badge/Next.js-000?logo=nextdotjs&logoColor=fff&style=flat) ![TypeScript Badge](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=fff&style=flat)  ![Sass Badge](https://img.shields.io/badge/Sass-C69?logo=sass&logoColor=fff&style=flat)  ![Node.js Badge](https://img.shields.io/badge/Node.js-5FA04E?logo=nodedotjs&logoColor=fff&style=flat)  ![Tailwind CSS Badge](https://img.shields.io/badge/Tailwind%20CSS-06B6D4?logo=tailwindcss&logoColor=fff&style=flat)  ![PostgreSQL Badge](https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=fff&style=flat)  ![Neon Badge](https://img.shields.io/badge/Neon-34D59A?logo=neon&logoColor=fff&style=flat)  ![Git Badge](https://img.shields.io/badge/Git-F03C2E?logo=git&logoColor=fff&style=flat)  ![GitHub Actions Badge](https://img.shields.io/badge/GitHub%20Actions-2088FF?logo=githubactions&logoColor=fff&style=flat)  ![Vercel Badge](https://img.shields.io/badge/Vercel-000?logo=vercel&logoColor=fff&style=flat)  ![Docker Badge](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=fff&style=flat)  ![Vitest Badge](https://img.shields.io/badge/Vitest-00FF74?logo=vitest&logoColor=000&style=flat)  ![Babel Badge](https://img.shields.io/badge/Babel-F9DC3E?logo=babel&logoColor=000&style=flat)  ![Biome Badge](https://img.shields.io/badge/Biome-60A5FA?logo=biome&logoColor=fff&style=flat)  ![Jest Badge](https://img.shields.io/badge/Jest-C21325?logo=jest&logoColor=fff&style=flat)  ![Linux Badge](https://img.shields.io/badge/Linux-FCC624?logo=linux&logoColor=000&style=flat)

---

## Highlights

- Built a Next.js 16 / React 19 component library from scratch: **22 TS primitives** backed by **460 design tokens**, strict typing, barrel exports, SCSS Modules.
- Security & quality first: **Biome + Vitest**; **gitleaks / osv-scanner / trivy run as blocking** pre-commit + CI gates so secrets never enter history no findings to date.
- Accessibility is systemic: WAI-ARIA roving focus, focus restoration, landmark fallbacks across primitives; fixed an SC 2.4.3 focus-loss regression on overlay unmount (see Ticket #INC-8042).
- Infrastructure-minded: hardened devcontainer (**read-only rootfs, `cap-drop=ALL`, no-new-privileges**) with pinned + sha256-verified tool installs (Node, cosign, sops, trivy, gitleaks).
- Provenance note: incident ticket & debugging examples are **worked artifacts**, not named-employer records every number/tool cited is verifiable.

---

## 1. Technical Capabilities Matrix

| Domain | Systems & Infrastructure |
| :--- | :--- |
| **Interface Systems** | Design Tokens (CSS custom properties), Component Primitives, Polymorphic Types (`asChild`), SCSS Modules |
| **Accessibility** | WAI-ARIA Authoring Practices roving tabindex, focus restoration, landmark fallbacks applied across 30 primitive files |
| **Quality** | Strict TypeScript, Biome (lint + format), Vitest, supply-chain scanning (gitleaks / osv-scanner / trivy) as a blocking pre-commit gate |
| **Tooling & Automation** | Yarn 4 workspaces, CI workflows, enforced git hooks |

---

## 2. Prismaflux-Design-System

**Private repo** — ask and I'll grant access. Token-driven React component library, Next.js 16 / React 19 / SCSS Modules.

| | |
| :--- | :--- |
| **Primitives** | 22 components, each a single `.tsx` with a SCSS Module, barrel-exported from `components/primitives/index.ts` |
| **Tokens** | 460 custom properties in `styles/tokens.scss` across color, input, spacing, radius, typography, motion mirrored by a `DESIGN.md` manifest |
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
