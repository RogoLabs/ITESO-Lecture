# Speaker Notes — Vulnerability Management and the AI Crossroads

**ITESO · June 3, 2026 · Jerry Gamblin (@jgamblin), Founder, RogoLabs**
53 slides, slide-by-slide, in deck order. Talking points only — beats to hit, not a script to read.

**Legend**
- `↪ Next:` one-line bridge into the following slide.
- `⟲ Callback:` ties back to an earlier thread (opening question, rain/flood, MTTR/WoE, "more CVEs ≠ more risk").
- `❓ Likely Q:` a question students tend to ask → a crisp answer.
- `⚠ Guardrail:` an accuracy landmine — say it this way, not that way.

**The five running threads:** (1) the opening question *"what does secure even mean?"* (set slides 2/5 → paid off slide 44); (2) **Rain vs. Flood** (14/16 → 19/23); (3) **MTTR race / Window of Exposure** (28 → 30 → 32 → 33); (4) **more CVEs ≠ more risk / human capacity** (9 → 45/46); (5) **CVE Decaf + Consensus Engine** (2 → 10).

---

## Section 0 — Opening (Slides 1–5)

## Slide 1 — Title: Vulnerability Management and the AI Crossroads
- Open with who you are in one breath: years spent on one question — *how do we make vulnerability data actually usable?*
- Plant the @jgamblin handle: invite them to push back and ask hard questions in real time.
- Set expectations: 4 hours, one break, lots of room for discussion — this is not a lecture-at-you.
- ↪ **Next:** that one question has a name and a frame — let's start there.

## Slide 2 — The Question That Drives This Session ("CVE Decaf")
- Explain CVE Decaf in your own words: decaffeinated CVEs — strip the noise, keep the signal. Same protection, none of the jitters from chasing 40,000 CVEs a year.
- Everything today ladders up to one obsession: **actionable data quality over raw volume**.
- Walk the three cards: The Problem (AI outpaces fixers *and* the NVD) → The Frame (data quality over volume) → The Goal (day-one frameworks, not theory).
- ⟲ **Thread:** CVE Decaf is your project — you'll cash it out at slide 10 with the Consensus Engine.
- ↪ **Next:** here's exactly what you'll walk out able to do.

## Slide 3 — By the End of Today You Will Be Able To… (Objectives)
- Don't read all five — name them fast, then point at #2 and #3 as the practical core ("if you remember two things, KEV+EPSS and the MTTR race").
- Frame these as *abilities*, not topics — they'll apply them in their first security job.
- ↪ **Next:** here's the clock and the spine of the day.

## Slide 4 — What We'll Cover Today (Agenda + Central Question)
- Quick orientation: 4 sections, break at 10:50, back at 11:05.
- Read "The Central Question" box aloud and let it sit — this is the hook for the whole session.
- Tell them plainly: we return to this with a *precise, usable* answer at the end. Ask them to hold it.
- ⟲ **Thread:** this is the question paid off on slide 44.
- ↪ **Next:** let's frame the question properly before we start.

## Slide 5 — Opening Question: …what does "secure" even mean?
- Slow down. This is the intellectual frame for the next four hours.
- Read it as a real question, not rhetorical: AI finds bugs faster than humans fix them, faster than the NVD catalogs them — so what's "secure"?
- Resist answering now. The whole point is that the answer is earned, not asserted.
- ❓ **Likely Q:** "Is the answer just 'patch faster'?" → Tease: "Partly — but speed has a floor you can't automate away. Hold that thought." (sets up Section 3.)
- ↪ **Next:** to answer it, start with how the ground shifted — automation → autonomous AI.

---

## Section 1 — From Automation to Autonomous AI (Slides 6–13)

## Slide 6 — Section 1 Title: From Automation to Autonomous AI
- Section thesis in one line: this is a shift of **kind, not degree**.
- Preview the three icons: where we started (rule-based), where we are (AI end-to-end), why it matters (operating model changed).
- ↪ **Next:** we've always automated — so what's actually new?

## Slide 7 — We've Always Automated Security
- Concede the obvious: Nessus, Qualys, SIEM rules — automation isn't new. Don't oversell novelty.
- The real change: the **human in the middle** is moving out of the triage loop and into the *approval* loop.
- Land the callout: a faster scanner is the same model; an agent that discovers, drafts, and deploys is a different model entirely.
- The bottleneck doesn't disappear — it **relocates** to organizational process. (Foreshadows Section 3.)
- ❓ **Likely Q:** "Isn't a fancy scanner already 'AI'?" → No — pattern-matching at speed ≠ an agent that acts end-to-end. The distinction is autonomy, not throughput.
- ↪ **Next:** that shift broke something we relied on — CVE forecasting.

## Slide 8 — CVE Growth Was Once Predictable
- Tell the old story sympathetically: for years CVE growth was a tidy time-series. Last year × 1.12 was good enough to budget headcount and tooling.
- The curve was smooth, human-bounded, and legible to finance — that *mattered* operationally.
- ⚠ **Guardrail:** the CAGR is **~12% (2017–2023)**. Do **not** say 25% — the code's own data doesn't support it.
- ↪ **Next:** then the line stopped being a line.

## Slide 9 — Then the Trendline Broke (Capability-Triggered Model)
- Old model: growth bounded by the number of human researchers — smooth, linear.
- New model: growth bounded by **AI capability** — discontinuous jumps each time a better model ships.
- Use a Moore's-Law-style analogy if it helps: predictable until the constraint changed.
- ⚠ **Guardrail:** "capability-triggered model" is **your own framing**, not an established industry term. Say so out loud — it's descriptively accurate, not a citation.
- ❓ **Likely Q:** "Has a model release actually caused a measurable CVE spike?" → Be honest: the mechanism is clear; clean attribution per-release is still messy because CNA expansion and backlog clearing confound it. That's why raw volume is a bad signal.
- ↪ **Next:** and the cataloging infrastructure cracked *before* the surge even peaked.

## Slide 10 — The Infrastructure Cracked Before the Surge (NVD Crisis + Consensus Engine)
- Tell the 2024 NVD story: May 2024, NIST suspends enrichment for most new CVEs; tens of thousands assigned-but-unanalyzed, invisible to downstream scanners that rely on CVSS/CPE enrichment.
- Root cause is the thesis in miniature: **insufficient human staffing at a single critical node**.
- Pivot to the Consensus Engine — your $0, one-developer response that cross-checks CVE feeds vs. NVD enrichment vs. EPSS vs. KEV for gaps. This is **accountability, not replacement**.
- Make the structural point on screen: the durable fix isn't *more NIST staffing* — it's holding **CNAs** accountable for record quality at creation, pushing enrichment upstream to the issuer instead of concentrating it at one agency.
- ⟲ **Callback:** this is CVE Decaf's worldview (slide 2) made concrete.
- ⟲ **Thread:** "single human org bottlenecks the whole ecosystem" returns as the closing argument (slides 45/46).
- ❓ **Likely Q:** "Isn't this just NIST's funding problem?" → No — it's an architecture problem. Centralized enrichment is the single point of failure; funding only papers over it.
- ↪ **Next:** so what's actually driving the discovery surge? Real, shipping tools.

## Slide 11 — Real AI-Assisted Vulnerability Research
- Emphasize the subtitle hard: these are **documented, shipping tools** — not projections, not vendor marketing.
- One line each: OSS-Fuzz + LLM triage (thousands of real bugs in OpenSSL/libpng/SQLite); Copilot Autofix (fixes inline in the PR diff); Security Copilot (triage/context, GA); Mayhem/Jazzer (production fuzzers in regulated industries).
- The point isn't any single tool — it's that the *cost of finding bugs* has collapsed.
- ❓ **Likely Q:** "Which should I learn first?" → Free + immediate: OSS-Fuzz to understand fuzzing, Semgrep/bandit for static (you'll run bandit live in Section 4).
- ↪ **Next:** here's what that collapse did to the numbers in a single year.

## Slide 12 — ~38% More CVEs in a Single Year
- Anchor on three numbers: 28,900 (2023) → ~40K assigned (2024) → 400+ CNAs.
- The jump is real but it's coverage + tooling, not "software got 38% worse" — say that explicitly.
- Land the discussion line yourself: "good news or bad news? Both." More coverage is good, more noise is bad — what matters is what you do with the signal.
- ❓ **Likely Q:** "Is AI the main driver?" → It's one of three; CNA expansion and retroactive/backlog disclosure matter as much. Resist single-cause stories.
- ⚠ **Guardrail:** ~40K is CVE IDs *assigned* (MITRE), not NVD-*published* — different denominators. 28,900 is the NVD-published 2023 figure.
- ↪ **Next:** so if raw volume is the wrong number to budget on — what is?

## Slide 13 — You Can No Longer Budget Off the Raw CVE Feed
- Three beats: volume is now a function of AI capability releases; YoY comparisons mislead without normalizing for tooling adoption; the signal you care about (exploitable risk) is **decoupled** from the noise (total disclosures).
- The fix is not "hire a bigger team to chase every CVE" — it's a filter.
- ⟲ **Thread:** "decoupled signal" is the bridge into the entire rain/flood section.
- ↪ **Next:** that filter is the Exploitability Overlay — Section 2.

---

## Section 2 — The Exploitability Overlay: Rain vs. Floods (Slides 14–23)

## Slide 14 — Section 2 Title: The Exploitability Overlay (Rain vs. Floods)
- Set the metaphor now: **Rain** = ~40,000 total disclosures; **Flood** = ~200 new KEV entries that can actually hurt you.
- Define the two filters on screen in plain language: KEV = confirmed in-the-wild exploitation; EPSS = ML probability of exploitation within 30 days.
- ⟲ **Thread:** rain/flood carries the whole section and pays off at the 200:1 ratio (slide 19).
- ↪ **Next:** but first — why filtering matters at all. The bottleneck was never discovery.

## Slide 15 — We've Always Found More Than We Can Fix
- Core claim: discovery has *never* been the constraint — human capacity to verify, coordinate, test, deploy is.
- Do the math on screen out loud: **50 KEV entries × 10,000 servers = 500,000 remediation instances.** The CVE count is the *least* important variable.
- The quote to land: "The constraint is not what we know. The constraint is what we can act on."
- ⚠ **Guardrail:** this slide is where you establish that all later tier counts are **raw CVEs**, not asset-instances. Reuse this math at slides 20 and 22.
- ↪ **Next:** the metaphor that makes this intuitive — not all rain becomes floods.

## Slide 16 — Not All Rain Becomes Floods
- Rain card: total CVE volume includes theoretical issues, legacy software nobody runs, unmaintained systems with no network exposure, chained-only edge cases.
- Flood card: ~200 new KEV/year — working exploit code already in attacks against real targets.
- The drainage line: terrain, drainage, elevation = your asset inventory, network exposure, update velocity. Heavy rain ≠ flood if your drainage is good.
- ⚠ **Guardrail:** read the bottom callout — **KEV is backward-looking**; a lagging indicator with a shorter lag is still lagging. Plant this now; you reinforce it at slides 20 and 23.
- ↪ **Next:** so why is the rain getting heavier? Three structural reasons.

## Slide 17 — Three Reasons the Rain Is Getting Heavier
- One line each: (1) AI-assisted bug hunting — a week of researcher work now an overnight run; (2) CNA expansion 100 → 400+; (3) retroactive disclosure + NVD backlog clearing.
- The takeaway box is the thesis: volume is up for **structural and tooling** reasons — software isn't suddenly catastrophically worse; the documentation system is catching up and AI lowered the cost of finding bugs.
- ↪ **Next:** meanwhile the *actionable* subset barely moves — here's the data.

## Slide 18 — The Actionable Subset Grows Much More Slowly
- Point at the bars: total CVEs tower; new KEV entries are a sliver — and the sliver grows slowly.
- Two filters in plain terms: KEV (~1,200 cumulative, ~150–300/yr) and EPSS > 10% (filters ~85–90% of volume).
- Pre-load the number: ~40K new CVEs vs ~200 new KEV ≈ 200:1.
- Mention the notebook (`02_exploitability_overlay.ipynb`) reproduces this live — outputs will differ from the slide, on purpose.
- ↪ **Next:** that ratio deserves its own slide.

## Slide 19 — The Ratio That Changes Everything (200:1)
- This is a "let the number breathe" slide — say it, pause, repeat it: **200 to 1.**
- The instruction is the whole slide: *apply the filter.* That's the actionable habit you want them to leave with.
- ⟲ **Callback:** this is the payoff of the rain/flood metaphor (slides 14/16).
- ❓ **Likely Q:** "Doesn't filtering risk missing the next big one?" → KEV+EPSS is a floor you *always* do, plus asset-context on top — not instead of judgment. Filtering frees capacity for the judgment calls.
- ↪ **Next:** let's make each filter concrete — KEV first.

## Slide 20 — KEV: The Update List That's Not Optional
- Three cards: what it is (CISA, confirmed in-the-wild only, ~1,200); who must comply (federal agencies under BOD 22-01, best practice for everyone); the critical caveat.
- The line to deliver: if you update nothing else, update these. It's your **minimum acceptable target**, a floor not a ceiling.
- ⚠ **Guardrail:** reinforce the lag — by the time a CVE hits KEV, exploitation is already confirmed. Don't sell it as a real-time threat feed.
- ❓ **Likely Q:** "Should non-US orgs care about a US gov list?" → Yes — exploitation is global; KEV is the best free confirmed-exploitation signal regardless of jurisdiction.
- ↪ **Next:** KEV tells you what's *already* exploited; EPSS predicts what's *about to* be.

## Slide 21 — EPSS: The Probability Score That Changes Prioritization
- What it is: FIRST.org ML model on exploitation telemetry (honeypots, threat feeds, incident reports); outputs probability-of-exploitation within 30 days; updated daily.
- The mental model: **CVSS = how bad it *could* be; EPSS = how likely it is to *actually* happen.** Different axes.
- EPSS > 10% is a *practitioner convention*, not an official standard — tune to risk appetite (1/5/10%).
- ⚠ **Guardrail:** read the yellow box — **high CVSS ≠ high EPSS.** Many CVSS 9.x sit under 1% EPSS; many CVSS 5.x are actively exploited. Never prioritize on CVSS alone.
- ❓ **Likely Q:** "So is CVSS useless?" → No — it's a severity input, not a prioritization output. Use it to break ties *after* exploitability, not before.
- ↪ **Next:** put KEV and EPSS together into a calibration framework.

## Slide 22 — Calibrate to Your Risk Appetite (Four-Tier Framework)
- Walk the tiers: Minimum Viable (KEV only, ~200/yr) → Recommended (KEV + EPSS>10%, ~2–4K) → Comprehensive (EPSS>1%, ~8–15K) → Exhaustive (all CVEs, ~40K, ~82K projected 2026).
- The honest aside (from outline): most orgs are "exhaustive in policy, minimum-viable in practice." Ask the room which tier their employer/internship *targets* vs. *achieves*.
- Land the Exhaustive row's verdict: "Nobody. This isn't a program, it's a treadmill."
- ⚠ **Guardrail:** read the red box — these are **raw CVE counts**; real burden = count × assets × environments (slide-15 math). A 10-server shop and a 100k-server enterprise differ wildly on the same count.
- ↪ **Next:** zoom back out — what does the whole section add up to?

## Slide 23 — More Rain, Slower-Growing Flood Lines (Section 2 Takeaway)
- Three beats: AI is increasing volume (structural, will continue); the exploitable subset is *not* growing at the same rate (filters work); but watch KEV's lag — it's shrinking as AI accelerates exploitation.
- The third beat is the cliffhanger: a lagging indicator with a shorter lag demands faster response cycles.
- ⟲ **Callback:** closes the rain/flood loop (14/16/19).
- ↪ **Next:** what happens when AI accelerates the *exploit* side? That's after the break.

---

## Break (Slide 24)

## Slide 24 — 15-Minute Break (Back at 11:05)
- Set the hard return time out loud: 11:05.
- Pitch the live exercise: open `api.first.org/data/v1/epss?cve=CVE-2021-44228` (Log4Shell). What's its EPSS *today*? Is it what you'd expect for a famous 3-year-old vuln?
- Tease the discussion: we'll unpack *why* the score is what it is when we're back — it's a great intuition primer for the MTTR race.
- ❓ **Likely Q (on return):** "Why isn't Log4Shell ~100%?" → EPSS predicts *near-term observed activity*, not historical fame or severity; mitigations and patch saturation pull mature CVEs down. Perfect lead-in to "exploitability is a moving target."

---

## Section 3 — Defensive AI and the MTTR Race (Slides 25–34)

## Slide 25 — Section 3 Title: Defensive AI and the MTTR Race ("Poachers Turning Gamekeepers")
- Frame the three icons: Offense (PoCs in hours), Defense (real shipping tools), The Floor (process limits AI can't compress).
- The section's promise: AI is on *both* sides — and there's a hard organizational floor neither side automates away.
- ↪ **Next:** start with why offense and defense are the *same* capability.

## Slide 26 — The Same Capability Works Both Ways
- Core idea: understanding a code path well enough to *exploit* it ≈ understanding it well enough to *fix* it. The asymmetry doesn't persist.
- Historical arc on screen: fuzzing (offense → defensive pre-release standard), static analysis (→ SAST in every CI/CD), now AI exploit-gen → AI fix-gen.
- The reassuring framing: every offensive AI advance becomes a defensive capability within months.
- ↪ **Next:** and the defensive tools are already shipping — not roadmap items.

## Slide 27 — The Defensive Stack Is Catching Up
- Subtitle is the point: **shipping and in active enterprise use**, not prototypes.
- One line each: Security Copilot (triage/investigation, GA, Sentinel/Defender); Charlotte AI (conversational threat hunting in Falcon); Copilot Autofix (fix at the moment of introduction, not weeks later); Snyk/Semgrep AI (triage + ranks by exploitability/fix-confidence, cuts false-positive fatigue).
- Tie back: these compress the *middle* of MTTR — which we're about to define.
- ↪ **Next:** so let's define the metric this whole race is about.

## Slide 28 — Mean Time to Remediate: The Metric That Matters
- Define it precisely: disclosure (or detection) → **deployed updated software in production.** Not "version available," not "PR merged." Production.
- Pre-AI median for criticals: **60–80 days** (Tenable "Life of a Vulnerability," Edgescan). Cite the source — it earns credibility.
- Walk the five components; flag that AI compresses **steps 2–4 for in-house code only**. Steps 1 and 5 are largely unchanged; vendor releases bypass AI entirely.
- ⟲ **Thread:** this defines the Window of Exposure paid off at slide 32.
- ↪ **Next:** the giant caveat — AI can't patch what it doesn't own.

## Slide 29 — AI Can't Fix What It Doesn't Own (Vendor-Dependent)
- The structural reality: most enterprise attack surface is third-party — Oracle, SAP, Cisco, VMware, Microsoft. For vendor software you wait for *their* schedule; AI drafting a fix is irrelevant.
- Two columns: What AI can accelerate (in-house, OSS you maintain, cloud-native, IaC) vs. what it can't touch (vendor updates, signed firmware, legacy ERP, embedded software).
- Legacy ERP emergency updates: 6–18 months to validate. This is **structural, not temporary**.
- The red box is the thesis: AI-accelerated MTTR is real for greenfield/in-house; for *most* of an enterprise, the old timeline applies.
- ❓ **Likely Q:** "Doesn't this make AI patching kind of useless at enterprises?" → No — it's decisive for the code you own, which is also where you have the most exposure you can actually control. Just don't over-claim it for the vendor estate.
- ↪ **Next:** let me make that 'old timeline' painfully concrete.

## Slide 30 — Case Study: Patch in 4 Hours, Deployed in 47 Days
- This is the emotional center of Section 3 — slow down, walk the timeline beat by beat.
- Frame up front: **not a horror story.** Every delay was a real control with a real reason.
- Walk it: Hour 0 published / Hour 4 Autofix patch → Day 2 KEV-listed → Day 12 CAB approval → Day 31 staging/regression → Day 47 production.
- Hit the punchline hard: **"AI solved the 4-hour problem. The organization solved the 43-day problem. These are different problems."**
- ⟲ **Callback:** this is the Window of Exposure (slide 28) made concrete.
- ❓ **Likely Q:** "Couldn't they have rushed it?" → Yes, via a P1 incident declaration — but that needs active-exploitation evidence *in this environment*. Foreshadow slide 33: every delay maps to a real control.
- ↪ **Next:** meanwhile the exploit side compressed to *hours* — that's the squeeze.

## Slide 31 — AI-Accelerated Exploit Development
- Pre-AI: working PoC took days-to-weeks and a skilled developer — that lag *was* the defender's safety window.
- Post-AI: working PoC in hours for well-understood classes (buffer overflows, injection, deserialization). The window is compressing.
- The operational kill-line (red box): the old assumption "we have a week to deploy for critical CVEs" no longer holds for high-profile vulns in well-understood software. For a Log4Shell-class event in 2026: assume **hours, not days**.
- ⚠ **Guardrail:** stay specific — this is for *well-understood vulnerability classes*, not every novel bug. Don't overstate it as "all exploits now take hours."
- ↪ **Next:** so the race isn't about either timeline alone — it's the delta between them.

## Slide 32 — What Matters Is the Delta (Window of Exposure chart)
- Read the chart: every phase compresses under AI — but the exploit side compresses *faster* than the deployment side.
- The deployment bar (patch-drafted → prod) stays stubbornly long because **process sets a floor tooling can't move**. The race is won or lost in process improvement, not tool selection.
- ⚠ **Guardrail:** these are **illustrative medians for in-house code**, not measured data. Third-party software follows vendor cycles on either timeline. Say so.
- ⟲ **Callback:** this quantifies the 47-day case (slide 30) and the WoE definition (slide 28).
- ↪ **Next:** so what exactly is that floor AI can't fix?

## Slide 33 — The Bottleneck AI Can't Fix
- Four cards, one line each: Change Approval Boards (5–10 day floor); Regression Testing (test debt = MTTR debt); Vendor-Dependent Updating (their schedule; bypassing voids support); Regulatory Freeze Windows (quarter-end, audit periods).
- Map them back to the case study out loud: CAB notice, Monday-only regression env, Java 8/11 coupling, 5-day staging soak, owner sign-offs — each a real control, none bureaucratic waste.
- Run the discussion prompt as a real question: *"Which would you eliminate? Which would you keep if it was YOUR name on the change record?"* Let students argue — there's no clean answer, and that's the lesson.
- ❓ **Likely Q:** "Aren't these just bureaucracy that good orgs skip?" → No — they're accountability and safety controls. The orgs with slow MTTR usually *lack the test coverage* to safely go faster, not the will.
- ↪ **Next:** pull the section together.

## Slide 34 — The MTTR Race Is Already Underway (Section 3 Takeaway)
- Three cards: Offense is already there (hours); Defense is real and deployed (but needs investment/integration to capture); The Dangerous Middle — orgs that automate *discovery* but can't accelerate *deployment* are in the worst spot: more findings, same MTTR, bigger visible gap.
- The "dangerous middle" is the line to land — it's the trap most enterprises are walking into.
- ⟲ **Thread:** "automate discovery but can't deploy" echoes the human-capacity thesis (slide 9 → 45).
- ↪ **Next:** one attack surface left — the one no CVE will ever cover.

---

## Section 4 — Ephemeral Software and Micro-Vulnerabilities (Slides 35–42)

## Slide 35 — Section 4 Title: Ephemeral Software and Micro-Vulnerabilities
- Frame: this defines the security-research agenda for 2025–2030. Read the flow diagram: AI assistant → novel code → production → no CVE history / no dependency manifest / no security review / no patch channel → regenerated differently next week → repeats at scale.
- The hook: everything in Sections 1–3 assumed software you can *catalog*. This section breaks that assumption.
- ↪ **Next:** start with the assumption traditional VM was built on.

## Slide 36 — Traditional VM Assumes a Stable Inventory
- Walk the four assumptions: you have an asset register; libraries have CVEs filed; vendors issue updates; you deploy them. The system works because software changes slowly and is cataloged.
- Land the red box: AI assistants break **every one of these simultaneously.**
- ↪ **Next:** here's the break, side by side — the shadow registry.

## Slide 37 — The Shadow Registry
- Contrast the two columns: traditional flow ends in "every component has a CVE history"; AI-generated flow ends in "no CVE catalog entry exists; no updated version will ever come."
- The example that lands: "write me a parser for this custom log format" → novel code, reviewed for correctness only, shipped. No CVE, no manifest, no patch channel.
- The term to plant: **shadow registry** — bespoke vulnerabilities invisible to every scanner that relies on CVE matching.
- ↪ **Next:** why this is categorically unlike the vuln you all know — Log4j.

## Slide 38 — Not Like Log4j
- Walk the table row by row: Scope, CVE filed, release exists, scanner visibility, update channel, persistence.
- The thesis (red box): the very properties that made Log4j *manageable* — universality, one CVE, one fix, existing distribution — are exactly what AI-generated vulns **lack**. The response playbook does not transfer.
- Three properties to name: **bespoke** (no universal patch), **invisible** (no CNA files it), **persistent** (may run for years before anyone audits).
- ❓ **Likely Q:** "Isn't this just 'write secure code'?" → Yes and no — the *scale and invisibility* are new. You can't scan for what has no CVE and no manifest entry. That's the gap.
- ↪ **Next:** this isn't speculation — the research already shows it.

## Slide 39 — This Is Already Showing Up in Research
- Three citable beats: Pearce et al. 2022 "Asleep at the Keyboard?" (~40% of Copilot code in security-relevant scenarios had vulns); Perry et al. 2023 (AI assistants → developers write *less* secure code while *feeling* more secure — the confidence effect); 2024–25 follow-ups (consistent input-validation/integer/injection rates across models).
- The killer line (yellow box): these pass syntactic review because they *look correct* — the flaw is **behavioral, not structural**. A reviewer checking for clean code misses them.
- ⚠ **Guardrail:** these are real, citable papers (arxiv 2108.09293, 2211.03622) — name them, don't hand-wave "studies show."
- ↪ **Next:** so what can we actually do? Honest answers, real limits.

## Slide 40 — The Tooling Gap Is Real: Here's What We Have
- Walk the table honestly — each row has a real limitation: static analysis (misses logic flaws, FP fatigue); mandatory human review (doesn't scale, "sensitive" is fuzzy); runtime monitoring (reactive, expertise-heavy); restrict AI codegen (hard to enforce).
- The actionable green box: **add static analysis to CI/CD today; require human review on auth/data-access/external-input.** Incomplete answers — but available now.
- Be honest with the room: this surface does **not** yet have a complete tooling answer. That honesty is the point.
- ❓ **Likely Q:** "So we just accept the risk?" → No — you *bound* it with the available controls and you *know* it's incomplete. Naming the gap is step one; pretending it's solved is the failure mode.
- ↪ **Next:** and the gap is exactly where careers get built.

## Slide 41 — Where This Is Heading: Research Directions
- Three frontiers, one line each: eBPF runtime *enforcement* (Tetragon blocking syscalls before they complete — needs kernel ≥5.8, cloud-native, deep expertise); AI-agent code reviewers (Snyk DeepCode, CodeAnt, Semgrep AI — open question: can AI detect its *own* failure modes? results mixed); AI-provenance tagging (annotate commits/SBOM with AI-gen metadata for policy gates — no standard yet, watch SLSA/SBOM extensions).
- The framing for students: none are production-ready universal answers — that's *why* this is where early-career researchers do work that matters. Make it a recruiting pitch for the field.
- ↪ **Next:** let's make it personal — a scenario you'll actually face.

## Slide 42 — Scenario Question (AI code found in production)
- Read the scenario: AI-generated code in a prod service, no tests, no review record, bandit flags two mediums (SQLi + shell=True).
- Run it as active learning — three lenses on screen: What do you do (triage/escalate)? Who do you involve (dev/manager/AppSec/legal)? What policy is missing (minimum viable governance)?
- The pivot that sharpens it: *does the answer change if it's a payment flow vs. an internal reporting tool?* Push them to that distinction.
- Bottom box is the takeaway: no single right answer — the friction is real; get comfortable with it now.
- ❓ **Facilitation tip:** call on a "developer" answer and a "security engineer" answer separately — the tension between them *is* the lesson.
- ↪ **Next:** time to close the loop and answer the opening question.

---

## Section 5 — Conclusion and Q&A (Slides 43–48)

## Slide 43 — Section 5 Title: Conclusion (the pyramid)
- Use the pyramid to recap the whole arc bottom-to-top: CVE volume (40K/yr) → actionable target (KEV+EPSS, 2–4K) → MTTR race / Window of Exposure → AI-generated ephemeral code (the unmeasured surface at the apex).
- One sentence: "Each layer up is smaller, more actionable, and harder to see — and the top layer has no CVE at all."
- ↪ **Next:** now answer the question we opened with.

## Slide 44 — What Does "Secure" Mean in the AI Era?
- This is the payoff — deliver the big line deliberately: **secure = maintaining a manageable, risk-calibrated Window of Exposure, not zero vulnerabilities.**
- Walk the four supporting points: AI expands attack surface faster than it shrinks MTTR (for now); prioritize by exploitability (KEV+EPSS), not raw volume; the binding constraint is human capacity, not AI capability; AI isn't creating new vuln *classes* — it's revealing accumulated technical debt.
- Land the fourth point with weight: the same errors from the 2007 "Unforgivable Vulnerabilities" paper still dominate the 2025 CWE Top 25. Real risk reduction means **secure-by-design at the maker level**, not just better deployment at the operator level.
- ⟲ **Callback:** this directly answers slides 4/5. Say so — "remember the question we held? Here's the answer."
- ❓ **Likely Q:** "Isn't 'not zero vulns' just lowering the bar?" → No — it's setting an *achievable, measurable* bar. Zero is a fantasy that wastes capacity; a shrinking exposure window is a program you can actually run.
- ↪ **Next:** why human capacity is the real constraint.

## Slide 45 — The Binding Constraint Is Human Capacity
- The CVD ecosystem runs on human decisions at every node: researcher → CNA → vendor → patch → deployment. AI accelerates inputs; it doesn't remove humans from the loop.
- Use the 2024 NVD crisis as the proof point: one understaffed org created a gap affecting the *entire global ecosystem*. Not a tooling failure — a human-capacity failure at a critical node.
- The precise distinction (right column): **more CVEs ≠ more risk; more unpatched exploitable vulns = more risk.** The whole discipline is the work of keeping those two quantities separate.
- ⟲ **Callback:** closes the thread opened at slide 9.
- ↪ **Next:** the five things to actually remember.

## Slide 46 — What to Remember (Five Takeaways)
- Don't re-explain — *name* them as memory hooks: (1) volume is discontinuous + mostly noise → apply the overlay; (2) KEV+EPSS>10% is the actionable target, grows slowly; (3) MTTR is the metric, but vendor releases set the floor; (4) ephemeral AI code is invisible to scanners — static analysis + review, neither complete; (5) human capacity is the binding constraint.
- Tell them: if you remember one, make it #2 — it's the day-one habit.
- ↪ **Next:** how to turn this into a budget argument.

## Slide 47 — How to Resource Your Vulnerability Program
- The big line: **resource to the size and growth rate of your software asset register — not the raw CVE feed.** A CVE that touches no asset you own costs you nothing.
- The professional-skill point: there are two framings — intellectually correct (asset-register growth) vs. politically effective (breach likelihood, SOC 2 / PCI / HIPAA / ISO 27001 exposure, cyber-insurance cost, board risk appetite). Knowing *both, and when to use each*, is what gets budget approved.
- Be candid: the technically correct argument loses the budget conversation more often than it should — so learn to translate.
- ⟲ **Callback:** this operationalizes the "actionable over volume" thesis from slide 2.
- ↪ **Next:** open the floor.

## Slide 48 — Open Floor (Q&A)
- Keep it minimal — this slide is an invitation, not content.
- If the room is quiet, prime with the three starters: (1) your org's update SLA — CVSS, KEV, EPSS, or something else? Do you even know? (2) Has anyone hit AI-generated code in a review — what did you find? (3) NIST suspended NVD enrichment in 2024 — what does it mean if one agency is a critical bottleneck, and what should replace it?
- Have the appendix slides ready to jump to if a question maps to one (resources, notebooks, glossary).

---

## Appendix (Slides 49–53) — Reference only

> Don't present these in sequence — skip unless asked, and point students to the GitHub repo (`github.com/RogoLabs/ITESO-Lecture`). Jump to the relevant one if a Q&A question lands on it.

## Slide 49 — Key Resources
- The four free, use-today links: CISA KEV catalog (your non-negotiable floor), EPSS API + docs (no auth, use it in your tooling), NVD API (register for a key — rate-limited), static analysis (bandit `pip install` + 30-sec run; Semgrep free tier).
- If asked "where do I start tonight?" — this is the slide.

## Slide 50 — Code Sample Index
- Point at the four notebooks and what each does. Emphasize: they run against **live APIs**, so outputs differ from the slides — that's intentional, the data is real, not canned.

## Slide 51 — Live Jupyter Notebooks: Download and Run
- The repo URL is the takeaway: `github.com/RogoLabs/ITESO-Lecture`. Clone, `pip install -r requirements.txt`, no API keys needed for KEV/EPSS.
- Encourage them to run `02` and `04` themselves — those are the most viscerally useful (the overlay and the live bandit scan).

## Slide 52 — What This Lecture Didn't Cover
- Use this to manage scope honestly: asset discovery, ownership chains, exception management, SLA definition (from *what date* does the clock start?), VM metrics / board reporting.
- The line: "a 4-hour lecture can't cover everything — these are what you'll hit on day one in the job."
- ❓ **Likely Q:** "What's the hardest one in practice?" → Asset discovery — you can't patch what you don't know you own; it's where VM programs fail first.

## Slide 53 — Glossary
- Pure reference — don't read it. Mention it exists for students reviewing later: CVE, CVSS, EPSS, KEV, CNA, MTTR, WoE, CVD, SBOM, static analysis.
