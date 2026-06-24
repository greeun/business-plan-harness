# Source Article — Full Inventory + Operational Mapping

> The methodology of `business-plan-harness` is a direct port of the article below. This file preserves the exhaustive source inventory so the harness's operation stays faithful to the original, and maps every code-harness concept to its business-plan equivalent. Read this when applying or stress-testing the harness.

---

## Source Reference — Full Article Inventory

The following is the exhaustive inventory of the source article. It is preserved in full so the harness's operation remains faithful to the original methodology. Do not drop any item when adapting.

### Publication
- Title: "Harness design for long-running application development"
- Author: Prithvi Rajasekaran (Member of Anthropic Labs team)
- Published: Mar 24, 2026
- Publisher: Anthropic (engineering blog)

### Core problem the article addresses
- Getting Claude to produce high-quality frontend designs.
- Getting Claude to build complete applications without human intervention.
- Earlier efforts on a frontend design skill and a long-running coding agent harness hit performance ceilings.

### Key inspiration and approach
- Inspired by Generative Adversarial Networks (GANs).
- Core design: multi-agent structure with generator and evaluator agents.
- Central challenge: converting subjective judgments ("is this design good?") into concrete, gradable criteria.

### Identified failure modes

**Context-related issues**
- Models lose coherence on lengthy tasks as the context window fills.
- "Context anxiety" behavior: models wrap up work prematurely because they believe they are approaching the context limit.
- Solution: context resets — clearing the context window entirely and starting a fresh agent with a structured handoff.
- Distinction from compaction: compaction preserves continuity but does not provide a clean slate; context anxiety persists through compaction.
- Finding: Claude Sonnet 4.5 exhibited context anxiety strongly enough that compaction alone was insufficient.
- Context resets add orchestration complexity, token overhead, and latency.

**Self-evaluation problem**
- Agents confidently praise their own work even when quality is mediocre.
- Particularly pronounced for subjective tasks like design (no binary verification equivalent).
- Solution: separate the agent doing the work from the agent judging it.
- Tuning a standalone evaluator to be skeptical is more tractable than making the generator self-critical.

### Frontend design harness — four grading criteria
1. **Design quality.** Whether the design reads as a coherent whole — colors, typography, layout, and imagery combining into a distinct mood and identity — rather than a disconnected collection of parts.
2. **Originality.** Whether the work shows deliberate, custom creative decisions rather than template layouts, library defaults, and recognizable AI-generated patterns (e.g. stock gradient-over-card motifs). A human designer should see intent behind the choices.
3. **Craft.** Technical execution — typography hierarchy, spacing consistency, color harmony, contrast — assessed as competence rather than creativity.
4. **Functionality.** Usability independent of aesthetics: whether users can understand the interface, find primary actions, and complete tasks without guessing.

**Weighting strategy**
- Emphasized design quality and originality over craft and functionality.
- Claude scored well on craft and functionality by default.
- Criteria explicitly penalized generic "AI slop" patterns.
- Language like "the best designs are museum quality" steered the generator toward a particular visual convergence.

**Implementation details**
- Built on Claude Agent SDK.
- Generator created HTML/CSS/JS frontend based on user prompt.
- Evaluator given Playwright MCP for live page interaction.
- Evaluator navigated pages independently, taking screenshots before scoring.
- Iteration count: 5 to 15 iterations per generation.
- Wall-clock time: full runs stretched up to four hours.
- After each evaluation the generator made a strategic decision: refine direction if scores were trending well, or pivot to a different aesthetic if the approach was not working.
- Evaluator calibrated with few-shot examples containing detailed score breakdowns.

**Key findings**
- Evaluator assessments improved over iterations before plateauing.
- The pattern was not always cleanly linear — middle iterations were sometimes preferred over the final ones.
- Implementation complexity tended to increase across rounds.

**Dutch Art Museum example**
- By iteration 9: clean, dark-themed landing page.
- Iteration 10: scrapped the approach entirely and reimagined it as a spatial experience with a 3D room (CSS perspective checkered floor), free-form artwork positioning, and doorway-based navigation between gallery rooms.
- Described as a "creative leap" not seen before from single-pass generation.

### Full-stack coding harness — three-agent architecture

**Planner agent**
- Takes a simple 1–4 sentence prompt.
- Expands it into a full product spec.
- Instructed to be ambitious about scope.
- Stays focused on product context and high-level technical design rather than detailed implementation.
- Avoids granular technical details to prevent cascading errors.
- Asked to weave AI features into the product spec.
- Had access to the frontend design skill.

**Generator agent**
- One-feature-at-a-time approach carried over from the earlier harness.
- Works in sprints, picking up one feature at a time from the spec.
- Stack: React, Vite, FastAPI, SQLite (later PostgreSQL).
- Self-evaluates its work at the end of each sprint before handing off to QA.
- Uses git for version control.

**Evaluator agent**
- Uses Playwright MCP to click through the running application the way a user would.
- Tests UI features, API endpoints, and database states.
- Grades each sprint against bugs found and the evaluation criteria.
- Criteria adapted from the frontend experiment: product depth, functionality, visual design, code quality.
- Each criterion has a hard threshold; failure on any means the sprint fails.
- Provides detailed feedback on what went wrong.
- Sprint contract negotiation: generator and evaluator agree on what "done" means before any code is written. The generator proposes what will be built and how success will be verified; the evaluator reviews to ensure the right thing is being built. The two iterate until agreement.
- Communicates via files: agents read and write files to pass information between themselves.

### Opus 4.5 implementation

**Model context handling**
- Opus 4.5 largely removed context anxiety behavior.
- Agents run as one continuous session across the whole build.
- Claude Agent SDK's automatic compaction handles context growth.

**Retro video game maker example — RetroForge**

*Solo run (baseline)*
- Duration: 20 minutes.
- Cost: $9.
- Output issues:
  - Layout wasted space with fixed-height panels.
  - Workflow rigid, did not guide the user toward necessary sequences.
  - Game broken: entities appeared on screen but nothing responded to input.
  - Entity wiring between definitions and runtime broken with no surface indication.

*Full harness run*
- Duration: 6 hours.
- Cost: $200 (over 20× more expensive than the solo run).
- Planner expanded a one-sentence prompt into a 16-feature spec across ten sprints.
- Additional features beyond the solo run: sprite animation system, behavior templates, sound effects, music, AI-assisted sprite generator, level designer, game export with shareable links.
- Advantages over the solo run:
  - More polish and smoothness.
  - Canvas used the full viewport.
  - Sensibly sized panels.
  - Consistent visual identity tracking the design direction.
  - Richer, more featured sprite editor.
  - Cleaner tool palettes.
  - Better color picker.
  - More usable zoom controls.
  - Built-in Claude integration for AI-generated game parts.
  - Playable game with working entity movement.
  - Physics had rough edges but the core feature worked (unlike the solo run).

**Evaluator performance issues and tuning**
- Out of the box, Claude was a poor QA agent.
- Identified legitimate issues, then talked itself into deciding they were not a big deal.
- Tested superficially rather than probing edge cases.
- Tuning loop: read evaluator logs, found examples of judgment divergence, updated the QA prompt.
- It took several rounds of the development loop before evaluator grading was reasonable.
- Remaining limitations: small layout issues, unintuitive interactions, undiscovered bugs in nested features.
- More verification headroom available with further tuning.
- Compared to the solo run, there was an obvious lift.

**Example contract issues caught by the evaluator**

| Criterion | Finding |
|---|---|
| Rectangle fill tool allows click-drag to fill a rectangular area with the selected tile | FAIL — tool only places tiles at drag start/end points. `fillRectangle` function exists but is not triggered properly on mouseUp. |
| User can select and delete placed entity spawn points | FAIL — delete key handler requires both `selection` and `selectedEntityId` set, but clicking an entity only sets `selectedEntityId`. |
| User can reorder animation frames via API | FAIL — `PUT /frames/reorder` route is defined after the `/{frame_id}` routes; FastAPI matches "reorder" as integer `frame_id` and returns 422. |

### Harness iteration and simplification

**Core principle**
- Every component of a harness encodes an assumption about what the model cannot do on its own. Those assumptions are worth stress-testing because they may be wrong to begin with and because they go stale as models improve.

**Referenced principle**
- Start with the simplest solution that works and only add complexity when it is actually needed.

**First simplification attempt**
- Cut the harness back radically.
- Tried creative new ideas.
- Could not replicate the original performance.
- Difficult to determine which pieces were load-bearing.
- Moved to a methodical approach: remove one component at a time.

### Opus 4.6 release impact
- Further motivation to reduce harness complexity.
- Expected 4.6 would need less scaffolding than 4.5.
- 4.6 is described as planning more carefully, sustaining agentic work over longer horizons, operating more reliably in large codebases, and catching more of its own mistakes through stronger code review and debugging behavior.
- 4.6 also improved substantially on long-context retrieval.
- All capabilities the harness was built to supplement.

### Removing the sprint construct
- Sprint construct removed entirely from the updated harness.
- Sprint structure had helped decompose work for coherent model execution.
- Opus 4.6 improvements suggested the model could handle the work without decomposition.
- Planner and evaluator kept because each continued to add obvious value.
- Without a planner: the generator under-scoped and created less feature-rich applications.
- Evaluator moved to a single pass at the end rather than per-sprint grading.
- Key insight: evaluator usefulness depends on task position relative to model baseline.
- On 4.5 the evaluator caught meaningful issues across the build (the boundary was at the edge of capability).
- On 4.6 raw capability increased and the boundary moved outward.
- Tasks that once needed the evaluator's check are now within native generator capability.
- For tasks at the edge of capability, the evaluator continues to give real lift.
- Implication: the evaluator is not a fixed yes/no decision — it is worth its cost when the task is beyond reliable solo performance.

### Prompting improvements
- Prompting added to improve how the harness builds AI features into apps.
- Focused on getting the generator to build a proper agent that drives app functionality through tools.
- Required real iteration because recent training data coverage was thin.
- With enough tuning the generator built agents correctly.

### Updated harness results — DAW (Digital Audio Workstation)

Prompt: "Build a fully featured DAW in the browser using the Web Audio API."

| Agent & phase | Duration | Cost |
|---|---|---|
| Planner | 4.7 min | $0.46 |
| Build (Round 1) | 2 hr 7 min | $71.08 |
| QA (Round 1) | 8.8 min | $3.24 |
| Build (Round 2) | 1 hr 2 min | $36.89 |
| QA (Round 2) | 6.8 min | $3.09 |
| Build (Round 3) | 10.9 min | $5.88 |
| QA (Round 3) | 9.6 min | $4.06 |
| **Total V2 harness** | **3 hr 50 min** | **$124.70** |

**Output quality**
- Builder ran coherently for over two hours without sprint decomposition.
- Planner expanded a one-line prompt into a full spec from logs.
- Generator planned the app and agent design well, wired the agent, and tested before handing to QA.
- QA still caught real gaps in Round 1 feedback: the app had strong design fidelity, a solid AI agent, and good backend, but several core DAW features were display-only without interactive depth — clips could not be dragged or moved on the timeline, there were no instrument UI panels (synth knobs, drum pads), and no visual effect editors (EQ curves, compressor meters).
- Round 2 QA feedback: audio recording still stub-only (button toggled but no mic capture); clip resize by edge drag and clip split not implemented; effect visualizations were numeric sliders, not graphical (no EQ curve).

**Actual functionality achieved**
- All core pieces of a functional music production program present.
- Working arrangement view, mixer, and transport running in the browser.
- User could put together a short song snippet entirely through prompting.
- The agent set tempo and key, laid down a melody, built a drum track, adjusted mixer levels, and added reverb.
- Core primitives for song composition present; the agent could drive the app autonomously using tools.
- Created a simple production end-to-end.
- The app was far from a professional program; the agent's song composition skills need work.
- Claude cannot actually hear, which made the QA feedback loop less effective regarding musical taste.

### Key learnings and future direction

**General principles**
- Always good practice to experiment with the model you are building against.
- Read traces on realistic problems.
- Tune performance to achieve desired outcomes.
- For complex tasks: decompose the task and apply specialized agents to each aspect.
- When a new model lands: re-examine the harness, strip away non-load-bearing pieces, and add new pieces for greater capability.

**On model improvement**
- As models improve, they can handle longer horizons and more complex tasks. In some cases the scaffold around the model matters less and problems resolve themselves with a new model release. At the same time, stronger models open more room for harnesses that push beyond what the raw model can do on its own.

**Author's conviction**
- The space of interesting harness combinations does not shrink as models improve — it shifts. The ongoing work for AI engineers is to keep finding the next useful combination.

### Acknowledgements (listed in the article)
Mike Krieger, Michael Agaby, Justin Young, Jeremy Hadfield, David Hershey, Julius Tarng, Xiaoyi Zhang, Barry Zhang, Orowa Sidker, Michael Tingley, Ibrahim Madha, Martina Long, Canyon Robbins, Jake Eaton, Alyssa Leonard, Stef Sequeira.

### Appendix — RetroForge spec example
- Project: RetroForge — 2D Retro Game Maker.
- Overview: web-based creative studio for designing and building 2D retro-style video games combining nostalgic 8-bit and 16-bit aesthetics with modern intuitive editing tools.
- Integrated modules: tile-based level editor, pixel-art sprite editor, visual entity behavior system, playable test mode.
- AI integration: Claude-powered assistance for sprite generation, level design, and behavior configuration through natural language.
- Target users: hobbyist creators to indie developers; anyone who loves retro gaming aesthetics and wants modern conveniences.
- Features include project dashboard and management (create, view, open, delete projects with confirmation, duplicate), and project data model components (metadata, canvas settings, tile size, color palette, sprites/tilesets/levels/entity definitions).

---

## Operational Mapping — From Code Harness to Business Plan Harness

Every concept above maps to an equivalent in the business-plan domain. Apply the mapping below when running the harness.

| Source concept | Business-plan equivalent |
|---|---|
| GAN-inspired generator/evaluator split | Draft-writer role separated from critic role |
| Context anxiety | Writer wrapping a plan prematurely because it "feels long enough" |
| Context resets with structured handoff | `handoff.md` at each sprint boundary, fresh context for next sprint |
| Distinction from compaction | Do not silently compact — always write an explicit handoff |
| Sprint contract negotiation | Generator and Evaluator agree in writing on acceptance criteria before drafting each sprint |
| Design quality | Strategic coherence — problem, customer, value, business model, GTM reinforce each other (C1/coherence) |
| Originality | Differentiation & insight (C2) — non-obvious, defensible edge vs named competitors, not boilerplate |
| Craft | Craft & rigor — numbers internally consistent (G-h), claims sourced or labeled `ASSUMPTION:` (G-d) |
| Functionality | Feasibility & unit-economics soundness (C3) — a founding team could act next week; the model reconciles |
| 5–15 iteration count | Expect 5–15 evaluator cycles per sprint |
| Playwright MCP probing | Evaluator actively stress-tests assumptions, runs the numbers, checks comparables (no live app to click) |
| Visual-craft human checkpoint (wireframes) | Financial-assumption human checkpoint (STEP 7) — the founder approves the input assumptions the model rests on |
| Communicates via files | All artifacts are Markdown files in a `business-plan/` directory |
| Planner ambition + AI-features weave-in | Planner must inject the differentiation / moat hook unique to the venture |
| Per-sprint grading vs single pass | Default: per-sprint grading. If the topic is narrow and the user opts in, collapse to a single final review |
| Evaluator tuning loop | Re-read critiques across sprints; tighten criteria if the evaluator is being lenient (esp. on financials) |
| "Every harness component encodes an assumption" | The sprint structure assumes "research/financial quality must be gated before synthesis" — stress-test on upgrade |
| "Simplest solution possible" | Do not add sprints or criteria the topic does not require |
| Code stack (React/FastAPI/SQLite) | Out of scope — the business plan never prescribes a tech stack; that is `service-planning-harness` |
| Product depth / code quality criteria | Re-mapped to differentiation/moat (C2) + feasibility/unit-economics (C3) — the axes Claude is weak on for a plan |

### Domain swap note (S3)
In the source code-harness the per-sprint axis Claude was weakest on was UI/feature completeness. For a **business plan** the weak axis is instead **business-model realism + financial coherence**. So this harness's S3 is "business-model·unit-economics·financial hypotheses" (feeding C3/C4 + gates G-d/G-h), not UX/screens. The conditional human checkpoint likewise moves from "approve the wireframe visual craft" to "approve the financial input assumptions."
