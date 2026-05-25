# Slide Outline: Vulnerability Management and the AI Crossroads

**ITESO | June 3, 2026**
Total slides: ~43 | Total time: 4 hours (with break)

---

## SECTION 0: OPENING (Slides 1–4)

### Slide 1 — Title Slide
- **Title:** Vulnerability Management and the AI Crossroads
- **Subtitle:** How AI Is Reshaping What We Find, What We Fix, and How Fast We Have to Move
- **Presenter:** Jerry Gamblin (@jgamblin) | Principal Security Engineer, Empirical Security
- **Date:** June 3, 2026
- **Visual suggestion:** Abstract network/code background, single accent color
- **Opening speaker notes:**
  - Two days ago I joined Empirical Security. Before that, several years focused on one question: *how do we make vulnerability data actually usable?*
  - That question is what "CVE Decaf" is about — decaffeinated CVEs. Strip the noise, keep the signal. Same amount of caffeine (protection), none of the jitters (alert fatigue from chasing 40,000 CVEs a year).
  - Follow along, push back, and ask hard questions: [@jgamblin](https://twitter.com/jgamblin)
  - Everything we cover today is anchored to that same obsession: actionable data quality over raw volume.

### Slide 2 — Learning Objectives
- **Header:** By the End of Today You Will Be Able To…
- **Content (numbered list):**
  1. Explain why CVE forecasting shifted from a time-series to a capability-triggered problem
  2. Apply a KEV + EPSS exploitability overlay to reduce a raw CVE feed to an actionable target
  3. Describe the MTTR race and its organizational limits
  4. Articulate the risk of ephemeral AI-generated code and why CVE scanners cannot see it
  5. Identify the human capacity constraints that define the CVD ecosystem

### Slide 3 — About This Session
- **Header:** What We'll Cover Today
- **Content (4 bullets):**
  - The AI-driven surge in vulnerability discovery — and why it's mostly noise
  - How to filter signal from noise using exploitability overlays
  - The MTTR race: AI on both offense and defense (with real limits)
  - Ephemeral software: the attack surface CVEs can't capture
- **Time map:** Simple horizontal timeline showing 09:00–13:00 with section labels

### Slide 4 — A Question to Start
- **Header:** If AI Can Find Bugs Faster Than Humans Can Fix Them…
- **Sub-header:** …and faster than the NVD can even catalog them…
- **Full-bleed text:** "…what does 'secure' even mean?"
- **Note:** We'll return to this question at the end
- **Purpose:** Set the intellectual frame; prime students to listen for the answer

---

## SECTION 1: FROM AUTOMATION TO AUTONOMOUS AI (Slides 5–12)

### Slide 5 — Section Title
- **Header:** Introduction
- **Sub-header:** From Automation to Autonomous AI
- **Time:** 09:00 – 09:45

### Slide 6 — The Security Automation Arc
- **Header:** We've Always Automated Security
- **Content (two-column comparison):**
  - *Left — "Automation (2010s)"*
    - Rule-based scanners (Nessus, Qualys)
    - SIEM correlation rules
    - Humans triage → humans patch
  - *Right — "Agentic AI (2025–2026)"*
    - AI discovers vulnerabilities end-to-end
    - Agents draft patches and open PRs
    - Humans approve → systems deploy
- **Key point (callout box):** "The shift is not one of degree — it's one of kind"

### Slide 7 — How We Used to Forecast CVEs
- **Header:** CVE Growth Was Predictable
- **Content:**
  - Annual CVE disclosures grew at roughly 12% CAGR from 2017–2023 (NVD published totals)
  - Time-series models worked: last year × 1.12 = good enough for budgeting
  - Security teams could plan headcount and tooling accordingly
- **Visual:** Simple line chart showing 2017–2023 steady growth
- **Data ref:** `code/01_cve_volume_trends.py`
- **Speaker note:** The compound growth rate is ~12% from NVD data; avoid stating "25%" — the code's own embedded data does not support it.

### Slide 8 — The Capability-Triggered Model
- **Header:** Then the Trendline Broke
- **Content:**
  - Old model: growth bounded by number of human researchers
  - New model: growth bounded by AI capability — discontinuous jumps when better models ship
  - Each major AI capability release is a potential step-change in disclosure volume
- **Visual:** Same chart extended to 2026, curve bending sharply upward after 2023
- **Key term:** "Capability-triggered model" (presenter's framing, not an established industry term)
- **Data ref:** `code/01_cve_volume_trends.py`

### Slide 9 — The 2024 Structural Shock: NVD Backlog
- **Header:** The Infrastructure Cracked Before the Surge
- **Content:**
  - May 2024: NIST announced suspension of CVE enrichment for most new entries in the NVD
  - Tens of thousands of CVE IDs assigned but unanalyzed — invisible to downstream scanners
  - Root cause: insufficient human staffing at a single critical infrastructure node
- **Key insight:** The disclosure ecosystem is bottlenecked by human organizations with limited capacity — AI makes this more exposed, not less
- **Source:** NIST NVD announcement, May 2024
- **Counterpoint — The Consensus Engine (callout box):**
  - One response to brittle centralized infrastructure is grassroots data-quality auditing
  - The **Consensus Engine** is an open-source, single-person initiative that continuously cross-checks the global vulnerability infrastructure — comparing CVE feeds, NVD enrichment status, EPSS scores, and KEV entries for gaps and discrepancies
  - Cost: $0. Stack: one developer's tooling
  - **Teaching point:** When a government agency is the single point of failure for global security data, individual practitioners with the right tooling can run independent audits. This is not a replacement — it is accountability. AI discovery volume makes this kind of independent data-quality work more important, not less.

### Slide 10 — The Tools Driving the Surge
- **Header:** Real AI-Assisted Vulnerability Research (Documented)
- **Content (tool cards):**
  - **Google OSS-Fuzz + LLM analysis** — automated fuzzing with AI-assisted crash triage; has found thousands of real bugs in critical open-source software
  - **GitHub Copilot Autofix** — AI-suggested security fixes inline in code review
  - **Microsoft Security Copilot** — AI assistant for threat intelligence, alert triage, and vulnerability context
  - **Commercial AI fuzzers (Mayhem, Jazzer)** — production tools for automated vulnerability discovery
- **Key point:** These are documented, shipping tools — not future projections

### Slide 11 — Real-World Signal: NVD 2023 → 2024
- **Header:** ~38% More CVEs in a Single Year
- **Content:**
  - NVD 2023: ~28,900 published CVEs
  - CVE IDs assigned 2024: ~40,000 — a ~38% single-year jump
  - Drivers: CNA expansion (100 → 400+ authorized issuers since 2017), AI-assisted research, retroactive disclosure
  - Similar increases reported across major bug bounty programs
- **Visual suggestion:** Two large numbers side-by-side: "28,900" (2023) → "40,000" (2024) with arrow
- **Source:** NVD/MITRE published annual totals
- **Discussion prompt:** "Is this good news or bad news?"

### Slide 12 — What This Means for Forecasting
- **Header:** You Can No Longer Budget Off the Raw CVE Feed
- **Content (3 points):**
  - Raw CVE volume is now a function of AI capability releases, not software complexity alone
  - Year-over-year comparisons are misleading without normalizing for AI tooling adoption
  - The signal you care about (exploitable risk) is decoupled from the noise (total disclosures)
- **Transition:** "So what IS the right signal? That's next."

---

## SECTION 2: THE EXPLOITABILITY OVERLAY (Slides 13–21)

### Slide 13 — Section Title
- **Header:** The Exploitability Overlay
- **Sub-header:** Rain vs. Floods
- **Time:** 09:45 – 10:50

### Slide 14 — The Real Bottleneck Is Not Discovery
- **Header:** We've Always Found More Than We Can Fix
- **Content:**
  - Discovery has never been the constraint in security operations
  - Human capacity — to verify, coordinate, prioritize, test, and deploy — is always the limit
  - AI expanding discovery widens the gap; it amplifies an existing problem, not a new one
- **Important nuance (callout box):** "Patching burden = CVEs × affected assets × environments. 50 KEV entries × 10,000 servers = 500,000 remediation instances."
- **Key quote:** "The constraint is not what we know. The constraint is what we can act on."

### Slide 15 — The Rain/Flood Framework
- **Header:** Not All Rain Becomes Floods
- **Content:**
  - **Rain** = total CVE volume: every disclosed vulnerability, exploitable or not
  - **Flood** = actionable exploitable risk: the subset that can hurt you right now
  - Heavy rainfall does not necessarily cause flooding; terrain, drainage, and elevation matter
- **Visual suggestion:** Rain labeled "~40,000 CVEs/year (2024)" vs. flood zone labeled "~200 new KEV entries/year"
- **Purpose:** Introduce the metaphor that frames the section
- **Caveat (small text):** "KEV is backward-looking — it records confirmed exploitation with a lag. Watch for that lag shrinking."

### Slide 16 — Heavy Rainfall: What's Driving Total Volume Up
- **Header:** Three Reasons the Rain Is Getting Heavier
- **Content (3 items):**
  1. **AI-assisted bug hunting** — automated pipelines finding issues at scale (OSS-Fuzz, Copilot Autofix, commercial fuzzers)
  2. **CNA expansion** — 100 → 400+ authorized CVE issuers since 2017; more things get CVE numbers
  3. **Retroactive disclosure + NVD backlog clearing** — old issues being documented and cataloged
- **Key takeaway:** Volume is up for structural and tooling reasons; it does not mean software is suddenly much worse

### Slide 17 — Stable Flood Lines: The Exploitability Overlay
- **Header:** The Actionable Subset Grows Much More Slowly
- **Content:**
  - Two filters that transform the raw feed into an actionable signal:
    - **CISA KEV catalog** — ~1,200 cumulative entries of confirmed in-the-wild exploits
    - **EPSS > 10%** — practitioner-convention threshold that filters ~85–90% of CVE volume
  - Annual new KEV entries: ~150–300/year (recent years; higher 2022 when historical entries were added)
  - Ratio: ~40,000 new CVEs in 2024 against ~200 new KEV entries ≈ 200:1 noise-to-signal
- **Visual:** Bar chart — total CVEs per year (tall) vs. KEV cumulative catalog size (small), 2020–2026
- **Data ref:** `code/02_exploitability_overlay.py`

### Slide 18 — CISA KEV: Your Minimum Viable Patch List
- **Header:** KEV: The Patch List That's Not Optional
- **Content:**
  - Maintained by CISA; contains only CVEs with confirmed, active exploitation in the wild
  - Federal agencies legally required to patch KEV entries (Binding Operational Directive 22-01); best practice for everyone
  - ~1,200 total entries as of early 2026; ~150–300 new entries per year in recent years
  - Live URL: `cisa.gov/known-exploited-vulnerabilities-catalog`
- **Caveat (callout):** KEV is a lagging indicator. By the time a CVE appears in KEV, active exploitation has already been confirmed. It is a floor, not a ceiling.
- **Visual suggestion:** Screenshot or mockup of the KEV catalog page

### Slide 19 — EPSS: Predicting What Gets Exploited
- **Header:** EPSS: The Probability Score That Changes Prioritization
- **Content:**
  - Exploit Prediction Scoring System (FIRST.org) — ML model trained on exploitation telemetry
  - Outputs probability (0–100%) that a CVE will be observed in exploitation activity within 30 days
  - EPSS > 10%: a widely-used practitioner convention, not an official standard — tune to your risk appetite
  - Updated daily; responds to new threat intelligence
- **Key insight:** CVSS tells you how bad a vuln *could* be; EPSS tells you how likely it is to *actually* appear in attacks
- **Visual suggestion:** Scatter plot (CVSS score vs. EPSS score) showing high CVSS ≠ high EPSS
- **Source:** first.org/epss

### Slide 20 — The Four-Tier Patching Framework
- **Header:** Calibrate to Your Risk Appetite
- **Content (table):**

| Tier | Filter | ~Annual CVE Count (2024 baseline) |
|------|--------|----------------------------------|
| Minimum viable | KEV only | ~200 new entries/year |
| Recommended | KEV + EPSS > 10% | ~2,000–4,000/year |
| Comprehensive | EPSS > 1% | ~8,000–15,000/year |
| Exhaustive | All CVEs | ~40,000/year (2024); ~82k projected 2026 |

- **Important note (below table):** These are raw CVE counts. Your actual remediation burden = CVE count × affected assets × environments. A 10-server shop and a 100,000-server enterprise have very different workloads from the same CVE count.
- **Discussion prompt:** "Which tier does your organization target? Which tier does it actually achieve?"

### Slide 21 — Section Takeaway
- **Header:** More Rain, Slower-Growing Flood Lines
- **Content (3 bullets):**
  - AI discovery is increasing CVE volume — structural and will continue
  - The exploitable subset is not growing at the same rate — exploitability filters remain effective
  - But: KEV's value depends on the lag between exploitation and cataloging; watch for that lag to shrink as AI accelerates exploitation
- **Transition:** "What happens when AI accelerates the exploit side? That's next."

---

## BREAK (Slide 22)

### Slide 22 — Break
- **Header:** 15-Minute Break
- **Content:**
  - Back at 11:05
  - *Try this:* Go to `api.first.org/data/v1/epss?cve=CVE-2021-44228` in a browser — what's the score? Is it what you'd expect?

---

## SECTION 3: DEFENSIVE AI AND THE MTTR RACE (Slides 23–31)

### Slide 23 — Section Title
- **Header:** Defensive AI and the MTTR Race
- **Sub-header:** Poachers Turning Gamekeepers
- **Time:** 11:05 – 11:55

### Slide 24 — The Symmetry of Offense and Defense
- **Header:** The Same Capability Works Both Ways
- **Content:**
  - Understanding a code path well enough to find a bug is nearly identical to understanding it well enough to fix it
  - Every offensive AI advance becomes a defensive capability within months
  - Historical pattern: fuzzing → defensive fuzzing; static analysis → defensive SAST; now: AI exploit generation → AI patch generation
- **Visual suggestion:** Two-sided arrow or mirror image graphic

### Slide 25 — Defensive AI Tooling (Real, Shipping Products)
- **Header:** The Defensive Stack Is Catching Up
- **Content (documented tools):**
  - **Microsoft Security Copilot** — AI assistant for threat intelligence, incident investigation, and alert triage; integrated with Sentinel and Defender
  - **CrowdStrike Charlotte AI** — conversational AI for threat hunting and alert explanation in Falcon
  - **GitHub Copilot Autofix / Advanced Security** — AI-suggested security fixes inline in the development workflow
  - **Snyk / Semgrep AI features** — AI-assisted vulnerability triage and fix suggestion in CI/CD pipelines
  - **AI-assisted WAF signature generation** — real-time detection rule generation from observed attack patterns
- **Key point:** These are shipping and in use — the defensive AI ecosystem is real, not speculative

### Slide 26 — Defining MTTR
- **Header:** Mean Time to Remediate: The Metric That Matters
- **Content:**
  - MTTR = time from CVE disclosure (or detection) to patched deployment in production
  - Industry median MTTR for critical vulnerabilities: 60–80 days pre-AI era
  - *Source: Tenable "Life of a Vulnerability" report; Edgescan annual statistics*
  - MTTR = time-to-know + time-to-prioritize + time-to-patch + time-to-test + time-to-deploy
  - AI can compress the middle three steps — but only for in-house code

### Slide 27 — The Critical Caveat: Vendor-Dependent Patching
- **Header:** AI Can't Patch What It Doesn't Own
- **Content:**
  - Most enterprise attack surface is third-party software: Oracle, SAP, Cisco, VMware, Microsoft
  - For vendor software, you wait for the vendor's release schedule — AI drafting a patch is irrelevant
  - Emergency patches for legacy ERP systems can take 6–18 months to validate
  - This is not a temporary limitation — it is structural to how enterprise software works
- **Key point:** AI-accelerated MTTR is real for greenfield, in-house, cloud-native code. For the rest — which is most of an enterprise — the old timeline applies.
- **Mini-case study — "Patch in 4 hours, deployed in 47 days" (callout box):**
  - *Setting:* Mid-size regulated bank. Java web portal using a vulnerable Apache Commons component. CVE published Monday morning, EPSS 0.58, KEV-listed within 48 hours.
  - *Day 0 (4 hrs):* GitHub Copilot Autofix drafts a patch, PR opened. Developer approves after review. Technically correct.
  - *Day 1–2:* AppSec review finds the fix requires a transitive dependency upgrade incompatible with an internal audit-logging library on Java 8. Technical hold.
  - *Day 4:* Java 8→11 compatibility work done. New PR opened.
  - *Day 5:* CAB submission. Next meeting: Thursday. Standard notice period: 5 days.
  - *Day 8:* CAB approval — full regression suite required; deploy Saturday maintenance window only.
  - *Day 15:* Regression environment is shared; Monday-morning slot only. Test run starts.
  - *Day 18:* Two test failures — unrelated to patch; require sign-off from two application owners.
  - *Day 24:* Sign-offs collected. Saturday deploy approved.
  - *Day 26:* Patch deployed to staging.
  - *Day 33:* Mandatory 5-business-day staging soak (regulatory requirement) completes.
  - *Day 40:* Deployed to production.
  - *Day 47:* Verification complete. Ticket closed.
  - **AI solved the 4-hour problem. The organization solved the 43-day problem. These are different problems.**

### Slide 28 — The Exploit Side of the Race
- **Header:** AI-Accelerated Exploit Development
- **Content:**
  - Pre-AI: working proof-of-concept for a published CVE took days to weeks
  - Post-AI: working PoC can be generated within hours for well-understood vulnerability classes
  - This compresses the "safe window" between public disclosure and active exploitation
  - The old operational assumption — "we have a week to patch critical CVEs" — no longer holds for high-profile vulnerabilities in well-understood software classes

### Slide 29 — The Race Condition Visualization
- **Header:** What Matters Is the Delta
- **Visual (primary content):** Timeline chart showing:
  - Pre-AI: disclosure → PoC (days) → in-the-wild (weeks) → patch available (weeks) → deployed (months). WoE = ~76 days
  - Post-AI (in-house code): disclosure → PoC (hours) → in-the-wild (days) → patch drafted (days) → deployed (weeks). WoE = ~24 days
- **Data ref:** `code/03_mttr_race.py`
- **Key insight:** Both timelines compress; the WoE can still shrink overall — but only if organizational deployment processes keep pace
- **Note on chart:** Timelines are illustrative medians for in-house code. Third-party software follows vendor release cycles on either timeline.

### Slide 30 — Organizational Constraints Aren't Going Away
- **Header:** The Bottleneck AI Can't Fix
- **Content (expanded):**
  - Change approval boards and change windows — floor of 5–10 days for standard changes at most enterprises
  - Regression testing requirements — the organizations with slow MTTR often lack the test coverage to deploy faster
  - Vendor-dependent patching — Oracle, SAP, Cisco patch on their schedule
  - Regulatory freeze windows — banks and healthcare systems have periods (quarter-end, audit periods) where no production changes are allowed
  - Contractual liability — patching vendor software without their approval can void support contracts
  - Human accountability — someone must sign off, own the decision, and own any resulting outage
- **Point:** AI solves the technical drafting problem. Leadership must solve the process, vendor, and regulatory problems.
- **Mini-case mapping (speaker note / discussion anchor):** Return to the Slide 27 case and map each delay to this list:
  - 5-day CAB notice → *change approval board*
  - Monday-only regression environment → *regression testing requirements (resource-constrained)*
  - Java 8/11 incompatibility → *vendor/dependency coupling*
  - 5-day staging soak → *regulatory freeze/soak requirement*
  - Application owner sign-offs → *human accountability*
  - Each item was a real control with a real reason — none was bureaucratic waste
  - **Discussion prompt:** "Which of these controls would you eliminate? Which would you keep if it was your name on the change record?"

### Slide 31 — Section Takeaway
- **Header:** The MTTR Race Is Already Underway
- **Content (3 bullets):**
  - Exploit generation has crossed into the hours timescale for known vulnerability classes in well-understood software
  - Defensive AI tooling (Security Copilot, Charlotte AI, Copilot Autofix) is real and in deployment
  - The organizations that will suffer are those that automate discovery but cannot accelerate deployment — for both process and vendor-dependency reasons
- **Transition:** "There's one more attack surface we haven't discussed — and it's the one no CVE will ever cover."

---

## SECTION 4: EPHEMERAL SOFTWARE AND MICRO-VULNERABILITIES (Slides 32–37)

### Slide 32 — Section Title
- **Header:** Ephemeral Software and Micro-Vulnerabilities
- **Sub-header:** The Attack Surface CVEs Can't See
- **Time:** 11:55 – 12:40

### Slide 33 — The Assumption Vulnerability Management Was Built On
- **Header:** Traditional VM Assumes a Stable Inventory
- **Content:**
  - You have a software asset register
  - Libraries have known CVEs filed against them
  - Vendors issue patches; you apply them
  - The system works because software changes slowly and is cataloged
- **Transition:** "AI assistants are breaking every one of these assumptions"

### Slide 34 — Ephemeral Instant Software
- **Header:** The Shadow Registry
- **Content:**
  - When a developer asks an AI assistant to "write a parser for this log format," novel code is generated, deployed, and running in production
  - This code has: no CVE history, no dependency manifest entry, no formal security review, no patch channel
  - Next week, a slightly different version might be regenerated
  - **This is happening at scale today**
- **Visual suggestion:** Developer → AI assistant → code → production (simple flow diagram), with a question mark over "security review"

### Slide 35 — Why Micro-Vulnerabilities Are Different
- **Header:** Not Like Log4j
- **Content (comparison):**
  - **Log4j (traditional):** One library, millions of deployments, one CVE, one patch, distributed through existing channels
  - **AI-generated code vulnerability:** Unique per instance, bespoke, no CVE filed, no universal patch, invisible to dependency scanners
- **Key properties:**
  - *Bespoke:* No universal patch exists
  - *Invisible:* Will not appear in NVD; no CNA will file a CVE
  - *Persistent:* May run for years before anyone audits it

### Slide 36 — The Research Signal (Real, Citable Studies)
- **Header:** This Is Already Showing Up in Research
- **Content:**
  - Pearce et al. 2022, "Asleep at the Keyboard?" — ~40% of Copilot-generated code in security-relevant scenarios contained vulnerabilities
  - Perry et al. 2023 — AI code assistants can lead developers to write less secure code
  - Multiple 2024–2025 follow-up studies: consistent rates of input validation, integer handling, and injection vulnerabilities in AI-generated code
- **Key point:** These vulnerabilities pass syntactic code review because they look correct. The flaw is behavioral, not structural.
- **Sources:** arxiv.org/abs/2108.09293 and arxiv.org/abs/2211.03622

### Slide 37 — What Can We Do? (Honest Answers)
- **Header:** The Tooling Gap Is Real — Here's What We Have
- **Content (table format):**

| Approach | What It Catches | Limitation |
|----------|----------------|------------|
| **Static analysis** (bandit, semgrep) | Known bad patterns: injection, weak crypto, shell=True | Misses logic-level flaws; false positive fatigue |
| **Mandatory human review** for security-sensitive AI code | Anything a reviewer can spot | Doesn't scale; "sensitive" is hard to define |
| **Runtime monitoring** (Falco, eBPF, behavioral EDR) | Anomalous behavior in production | Reactive, not preventive; hard to deploy universally |
| **Restrict AI codegen on critical paths** | Prevents the problem | Hard to enforce consistently |

- **Bottom line:** Add static analysis to your CI/CD pipeline today. Require human review on auth, data access, and external input handling. Be honest that this attack surface does not yet have a complete answer.
- **Data ref:** `code/04_ephemeral_software.ipynb` — live bandit scan of real vulnerability patterns
- **Where this is heading (forward-looking bullets — research directions for students):**
  - **eBPF runtime enforcement** — Cilium Tetragon and similar tools are moving beyond alerting toward active policy enforcement: blocking unexpected syscalls and file-access patterns at the kernel level *before* they complete. This could catch AI-generated code behaving anomalously even before any CVE exists. Current limitation: requires Linux kernel ≥ 5.8, deep expertise, and works cleanly only in cloud-native environments.
  - **AI-agent code reviewers** — Dedicated AI agents trained to review AI-generated code for security anti-patterns are in early commercial development (Snyk DeepCode AI, CodeAnt AI, Semgrep AI). The open research question: can AI reliably detect its own failure modes at a false-negative rate low enough to be useful? Early results are mixed.
  - **AI-provenance tagging** — Some organizations are beginning to annotate git commits and build artifacts with AI-generation metadata, enabling downstream policy gates ("no AI-authored code in payment flow without AppSec sign-off"). No standard yet; watch for SLSA or SBOM extensions to pick this up.
  - *Caveat for students:* None of these are production-ready universal answers. eBPF enforcement at scale requires expertise most enterprises don't have. AI reviewers have their own blind spots. The security engineering research agenda for 2025–2030 is substantially about closing this gap — which means this is where careers get built.

### Slide 38 — Discussion Pause
- **Header:** Scenario Question
- **Question:** "You find AI-generated code in a production service during a security review. No tests, no review record, and bandit flags two medium-severity findings. What do you do — and who do you involve?"
- **Purpose:** Active learning; connects to students' future roles as developers and security engineers; no single right answer

---

## SECTION 5: CONCLUSION AND Q&A (Slides 39–43)

### Slide 41 — Section Title
- **Header:** Conclusion
- **Sub-header:** Returning to the Question We Started With

### Slide 42 — Answering the Opening Question
- **Header:** What Does "Secure" Mean in the AI Era?
- **Large callout text:** "Secure means maintaining a manageable, risk-calibrated Window of Exposure — not zero vulnerabilities."
- **Supporting content:**
  - The goal is not to eliminate all flaws (impossible at any scale)
  - The goal is to ensure the exploitable, unpatched subset in your environment remains small and decreasing
  - This requires exploitability-filtered prioritization, MTTR discipline, and coverage of AI-generated code

### Slide 43 — Analysts Are Humans
- **Header:** The Binding Constraint Is Human Capacity
- **Content:**
  - The CVD ecosystem — researcher → CNA → vendor → patch → deployment — runs on human decisions at every node
  - AI accelerates inputs; it does not remove humans from the loop
  - The 2024 NVD backlog crisis demonstrated this: a single understaffed human organization created a gap affecting the entire global security ecosystem
  - More CVEs ≠ more risk. More unpatched exploitable vulnerabilities = more risk.

### Slide 44 — The Five Takeaways
- **Header:** What to Remember
- **Content (numbered list):**
  1. AI discovery is expanding CVE volume discontinuously — mostly noise; apply an exploitability overlay
  2. KEV + EPSS > 10% is the actionable target; it grows much more slowly than total disclosures
  3. MTTR is the critical metric — the race matters, but vendor-dependent patching sets an organizational floor
  4. Ephemeral AI-generated code is invisible to CVE scanners; static analysis and mandatory review are the current best mitigations — neither is complete
  5. Human capacity — to coordinate, decide, and deploy — is the binding constraint in the CVD ecosystem

### Slide 45 — Budgeting Recommendation
- **Header:** How to Resource Your Vulnerability Program
- **Key message (large callout):**
  > "Resource your vulnerability program to the size and growth rate of your software asset register — not to the raw CVE feed."
- **Practical note for the workforce:**
  - Intellectually correct framing: asset register growth
  - Politically effective framing (what gets budget approved): breach likelihood, compliance exposure (SOC 2, PCI, HIPAA), insurance cost
  - Knowing both is a professional skill

### Slide 46 — Q&A
- **Header:** Open Floor
- **Suggested discussion starters:**
  - "What is your organization's patching SLA, and is it based on CVSS, KEV, EPSS, or something else?"
  - "Has anyone encountered AI-generated code in a security review? What did you find?"
  - "NIST suspended NVD enrichment in 2024. What does it mean for the ecosystem if a single agency is a critical bottleneck?"
- **Visual suggestion:** Clean, minimal — mostly whitespace to invite conversation

---

## APPENDIX SLIDES (Optional Reference)

### Slide A1 — Key Resources
- CISA KEV Catalog: `cisa.gov/known-exploited-vulnerabilities-catalog`
- EPSS API + docs: `first.org/epss`
- NVD API: `nvd.nist.gov/developers/vulnerabilities`
- Bandit (Python security linter): `bandit.readthedocs.io`
- Semgrep (multi-language static analysis): `semgrep.dev`

### Slide A2 — Code Sample Index
- `01_cve_discovery_analysis.ipynb` — Real NVD API queries, CAGR analysis, structural break visualisation
- `02_exploitability_overlay.ipynb` — Live KEV download, EPSS fetch, CVSS vs. EPSS scatter, triage query function
- `03_exploitation_timeline.ipynb` — Real exploitation lag from KEV+NVD dates, SLA adequacy analysis
- `04_ephemeral_software.ipynb` — Live bandit scan of AI-generated code patterns, risk surface visualisation

### Slide A3 — What This Lecture Didn't Cover
*(For students who ask what comes next)*
- Asset discovery — finding all the assets before you can patch them
- Ownership chains — who actually owns each patch across teams
- Exception management — risk acceptance, compensating controls, open exceptions
- SLA definition — from what date does the clock start?
- Vendor-dependent patching — the majority of most enterprises' real patching workload
- Regulatory and change freeze windows
- VM metrics and board-level reporting

### Slide A4 — Glossary
- **CVE** — Common Vulnerabilities and Exposures: unique identifier for a publicly disclosed vulnerability
- **CVSS** — Common Vulnerability Scoring System: severity score (0–10) based on technical characteristics
- **EPSS** — Exploit Prediction Scoring System: probability of observed exploitation activity in the next 30 days (FIRST.org)
- **KEV** — Known Exploited Vulnerabilities: CISA's catalog of confirmed in-the-wild exploits
- **CNA** — CVE Numbering Authority: organizations authorized to assign CVE IDs
- **MTTR** — Mean Time to Remediate: average time from discovery to deployed patch
- **WoE** — Window of Exposure: time between "exploit available" and "patch deployed"
- **CVD** — Coordinated Vulnerability Disclosure: the process by which researchers report findings to vendors before public disclosure
- **SBOM** — Software Bill of Materials: formal record of a software component's dependencies and provenance
- **Static analysis** — automated inspection of source code for known vulnerability patterns without executing it (bandit, semgrep, CodeQL)
