# AGENTS.md — Spinner Evolve

Spinner Evolve is a Vite + React app that uses **Gemini 3 Flash** to generate **p5.js** loading-spinner mutations. Users **A/B select** between two parallel streams; the winner seeds the next generation. Export logic for Windows frames, Linux Plymouth, and Chrome cursors exists in `utils/exportService.ts` but is **not wired to the UI** yet.

## Key files

| File | Responsibility |
|------|----------------|
| `App.tsx` | History, phantom slot, candidates A/B, selection, evolve |
| `services/geminiService.ts` | `generateNextSpinner`, schema, streaming |
| `components/P5Canvas.tsx` | p5 sandbox, scale, PNG/GIF ref API |
| `components/Terminal.tsx` | Partial JSON, metrics, code download |
| `utils/exportService.ts` | `generateExportPack` ZIP export |
| `types.ts` | `SpinnerData`, `CandidateState` |

## Verification

```bash
npm install
npm run build
npm run dev   # http://localhost:3000 — requires GEMINI_API_KEY in .env.local
```

Manual smoke: START → both variants render → select one → history grows → prev/next works.

## Copy-paste prompts

| Task | Prompt |
|------|--------|
| **Default session** | `Follow AGENTS.md for Spinner Evolve. Read .cursor/rules/spinner-evolve-core.mdc.` |
| **export-hub** | `Use AGENTS.md task "export-hub". Wire generateExportPack + P5CanvasRef downloads in App/Terminal. Run verification-before-completion.` |
| **Gemini / streaming** | `Use gemini-js-genai skill. Only change services/geminiService.ts and types. Preserve structured JSON schema and dual-stream contract in App.tsx.` |
| **UI / UX polish** | `Use frontend-design skill. Match existing dark terminal aesthetic in App.tsx and Terminal.tsx. No generic AI slop.` |
| **a11y pass** | `Use web-accessibility skill on App.tsx, Terminal.tsx, P5Canvas. Keep keyboard select A/B.` |
| **Playwright E2E** | `Use webapp-testing + E2E rule. Mock @google/genai stream. 3–5 tests: start, select winner, history nav.` |
| **Extract npm core** | `Use writing-plans first, then extract generateNextSpinner + types to packages/core. No any.` |
| **Supabase gallery** | `Use supabase skill + MCP plugin-supabase-supabase. Schema for mutations table; do not break local-only mode.` |
| **Next.js consumer** | `Use nextjs skill. Embed exported PNG/SVG as loading.tsx fallback; document in README integration section.` |
| **Pre-PR review** | `Use code-reviewer subagent. Check P5 sandbox safety, env key handling, and dual-stream state machine.` |
| **Explore codebase** | `Use explore subagent (medium). Map App state machine, dead export code, and integration seams.` |

## Task router

| Work | Skills | Rule(s) | Subagent | MCP |
|------|--------|---------|----------|-----|
| export-hub | frontend-design, verification-before-completion | ui-terminal, p5-sandbox | generalPurpose | — |
| Gemini changes | gemini-js-genai | gemini-streaming | — | — |
| UI features | frontend-design, brainstorming | ui-terminal | generalPurpose | — |
| a11y | web-accessibility | ui-terminal | — | — |
| persistence / gallery | supabase | integrations | generalPurpose | Supabase |
| Next embed | nextjs | integrations | ai-architect | Vercel |
| npm package | writing-plans, typescript-advanced-types | gemini-streaming, integrations | ai-architect | — |
| Playwright | webapp-testing | spinner-evolve-core | shell | — |
| CI branding bot | cursor-sdk | integrations | ai-architect | — |
| Pre-merge review | — | spinner-evolve-core | code-reviewer | — |
| Unfamiliar code | — | spinner-evolve-core | explore | — |

## Recommended skills

| Skill | Use for |
|-------|---------|
| gemini-js-genai | `geminiService.ts` |
| frontend-design | App / Terminal UI |
| web-accessibility | Roadmap #9 |
| verification-before-completion | Before claiming done |
| check-compiler-errors | After TS edits |
| brainstorming | New UX before coding |
| writing-plans / executing-plans | Multi-repo integrations |
| webapp-testing | Local :3000 smoke |
| supabase | Gallery (#4) |
| nextjs, vercel-react-best-practices | Next consumer (#1) |
| ai-sdk | Dual-stream port (#5) |
| cursor-sdk | CI bot (#8) |

**Usually skip:** firebase-*, pinecone-*, marketing-*, deployment-expert (unless deploy requested).

## Subagents

| Subagent | When |
|----------|------|
| explore | Map codebase, find dead export wiring |
| generalPurpose | Cross-file features (export UI) |
| ai-architect | Package boundaries, AI SDK port |
| code-reviewer | Before merge |
| shell | `npm run build`, install |

## MCP (Cursor Settings)

| Server | Use |
|--------|-----|
| plugin-supabase-supabase | Gallery schema, migrations |
| plugin-vercel-vercel / user-vercel | Deploy app or consumer |
| plugin-firebase-firebase | Not used for this repo |

## Cursor rules

| File | Scope |
|------|--------|
| `.cursor/rules/spinner-evolve-core.mdc` | Always on |
| `.cursor/rules/gemini-streaming.mdc` | `services/**`, `types.ts` |
| `.cursor/rules/p5-sandbox.mdc` | `P5Canvas`, `exportService` |
| `.cursor/rules/ui-terminal.mdc` | `App`, `Terminal`, `StatsChart` |
| `.cursor/rules/integrations.mdc` | `README`, `packages/**` |
