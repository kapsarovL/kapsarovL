# 👋  Hey, I'm Lazar

**Frontend Engineer** | Next.js | React 19 | TypeScript

[portfolio](https://www.lazarkapsarov.com) | [LinkedIn](https://www.linkedin.com/in/lazar-kapsarov) | [Twitter](https://x.com/kapsarovlazar)


I spent five years in IT support keeping office systems, networks, and hardware alive the kind of job where you learn what "production is down" feels like before you ever write a line of application code. In 2022 I retrained as a JavaScript developer, and since then I've been building web apps the way I used to keep systems running: measure first, type everything, assume things will fail.

In 2025 I founded PrismaFlux Media, my own studio, where I build and ship client projects end to end: schema design, API routes, auth, payments, deployment.



## Numbers I can actually back up
 
I don't list metrics I can't reproduce. Run the Lighthouse reports yourself:
 
| What | Score | Where |
| --- | --- | --- |
| My portfolio (desktop) | 100 / 100 / 100 / 100 | [lazarkapsarov.com](https://www.lazarkapsarov.com) |
| StoreFront (mobile) | 100 Perf · 96 A11y · 100 BP · 99 SEO | [live site](https://e-commerce-nextjs16.vercel.app/) — LCP 1.4s, CLS 0 |



## 🛠️ What I've built
 
### [StoreFront — full-stack e-commerce](https://e-commerce-nextjs16.vercel.app/)
 
`Next.js (App Router)` `React 19` `TypeScript` `Drizzle ORM` `Neon` `Clerk` `Stripe`
 
The full e-commerce loop, built independently: catalog, cart, Stripe Checkout.
I designed the database schema, wrote the API routes for cart logic and payment
webhooks, and wired up Clerk auth. The parts I'm most proud of are the boring
ones idempotent webhook handlers and integer-cent pricing, because payment
code is where float bugs go to ruin your week.
 
### Kalchev Family Winery
 
`Next.js` `React` `TypeScript` `Drizzle ORM` `Neon PostgreSQL`
 
A brand website for a local family winery, built and launched end to end
through PrismaFlux Media and deployed on Vercel in July 2026. Real client,
real deadline, real content.
 
### [PrismaFlux Media](https://prismaflux-media.com/)
 
My studio's own site. I migrated it off WordPress/Hostinger onto Next.js and
Vercel, including the full DNS move across Cloudflare and Hostinger the
[blog](https://prismaflux-media.com/blog) has write-ups on how I work.



## 🏗️ Stack
 
**Daily drivers:** Next.js (App Router) · React 19 · TypeScript · Tailwind CSS v4 · Shadcn/UI
**Data & payments:** Drizzle ORM · Neon PostgreSQL · Clerk · Stripe
**Tooling:** Vercel · GitHub Actions · Playwright · Vitest · Biome · Neovim (LazyVim)
**Currently learning:** Node.js and SQL fundamentals heading toward full-stack, honestly not there yet



## 🧠 How I think about code
 
1. **100ms or explain yourself** — interactions slower than that need a transition or an optimization.
2. **Types are documentation** — if it isn't typed, it's a liability someone inherits.
3. **Ship lean** — Server Components over client JS, Biome over ESLint, measure before optimizing.



## 📡 Right now
 
Open to full-time frontend roles — remote or Skopje. The fastest way to reach
me is through [lazarkapsarov.com](https://www.lazarkapsarov.com) or LinkedIn.
