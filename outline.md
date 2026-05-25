# Lecture Outline: Vulnerability Management and the AI Crossroads

**ITESO | June 3, 2026**
**Time: 9:00 AM – 1:00 PM**
**Presenter: Jerry Gamblin**

---

## Learning Objectives

By the end of this session, students will be able to:

1. Explain why AI-driven vulnerability discovery has changed CVE forecasting from a predictable time-series problem to a capability-triggered one.
2. Apply an exploitability overlay (CISA KEV + EPSS) to reduce a raw CVE feed to an actionable patching target.
3. Describe the MTTR race between AI-accelerated exploit development and AI-accelerated patch generation, including its organizational limits.
4. Articulate the risk posed by ephemeral AI-generated code and explain why CVE scanners cannot detect it.
5. Identify the human capacity constraints that define the CVD ecosystem regardless of AI automation.

---

## 09:00 AM – 09:45 AM | Introduction: From Automation to Autonomous AI

### Presenter Introduction

Two days ago I joined **Empirical Security** as Principal Security Engineer. Before that, several years focused on one question: *how do we make vulnerability data actually usable?* That question is what **CVE Decaf** is about — decaffeinated CVEs. Strip the noise, keep the signal. Same amount of protection, none of the jitters that come from chasing 40,000 CVEs a year. Everything in this lecture is anchored to that same obsession: actionable data quality over raw volume.

The **Consensus Engine** — an open-source, single-person lab initiative to audit global vulnerability infrastructure and surface data discrepancies — comes from the same place. When centralized infrastructure becomes a bottleneck, individual practitioners with the right tooling can run independent accountability audits. We'll come back to that.

Follow along, push back, ask hard questions: [@jgamblin](https://twitter.com/jgamblin)

### Context Setting: Comparing Traditional Security Automation with AI-Driven Processes

Traditional vulnerability management has always relied on automation — scanners, SIEM rules, patch orchestration pipelines. The shift happening now is not one of degree but of kind: automation that follows rules is being replaced by agents that generate rules, write patches, and triage findings independently.

Key contrast:
- **Automation (2010s):** Nessus runs scans, Jira tickets open, humans triage, humans patch.
- **Agentic AI (2025–2026):** AI agents discover vulnerabilities end-to-end, draft patches and open PRs, with humans approving and systems deploying.

### The 'Epochal' Shift: From Time-Series Models to Capability-Triggered Models

For years, CVE forecasting was a time-series problem. Analysts looked at year-over-year disclosure rates (a compound annual growth rate of roughly 12% from 2017–2023, based on NVD published totals) and extrapolated. That model broke starting around 2023.

The new model is **capability-triggered**: the rate of discovery is bounded by what AI tools can do, not by how many human researchers are working. When a new AI model ships with significantly better code-reasoning ability, disclosure volume jumps discontinuously, not gradually.

*Note for speaker:* "Capability-triggered model" is the presenter's own framing, not an established industry term. Present it as such.

Speaker note: Draw the analogy to Moore's Law — predictable for decades, then disrupted by physics. CVE forecasting is in a similar moment.

### The AI Discovery Era: The Surge in Identified Software Flaws

The category of AI-assisted vulnerability research is real and growing. Examples of documented, real tools in this space:
- **Google OSS-Fuzz with LLM-assisted analysis** — Google's open-source fuzzing platform has integrated LLM-assisted crash triage and bug report generation, substantially increasing throughput. OSS-Fuzz has found thousands of vulnerabilities in critical open-source projects.
- **GitHub Copilot Autofix** — GitHub's AI-assisted code scanning feature that suggests security fixes inline, documented as reducing the time developers spend fixing security alerts.
- **Commercial AI fuzzing tools (Mayhem, Jazzer)** — production tools used by security teams to automate vulnerability discovery at scale.
- **Microsoft Security Copilot** — documented AI security assistant for threat intelligence synthesis and vulnerability triage.

Real-world structural signal:
- NVD published approximately 28,900 CVEs in 2023 and approximately 40,000 CVE IDs were assigned in 2024 — a ~38% single-year jump. This is partly driven by CNA expansion and partly by increased AI-assisted research activity.
- Critically, 2024 also brought a major structural disruption: **NIST announced in May 2024 that it was suspending enrichment of most new CVEs in the NVD** due to resource and funding constraints, creating a backlog of tens of thousands of unanalyzed entries. This is directly relevant to the lecture's thesis: the discovery and cataloging infrastructure depends on human organizations with limited capacity.

**The Consensus Engine — grassroots accountability for global vulnerability infrastructure:**
One response to this brittleness is independent data-quality auditing. The **Consensus Engine** is an open-source, single-person initiative that continuously cross-checks the global vulnerability infrastructure: comparing CVE feed assignments against NVD enrichment status, EPSS score coverage, KEV catalog entries, and publication timing for gaps and inconsistencies. Cost: $0. Stack: one developer's tooling. Purpose: demonstrate that data quality at global scale can be independently audited by motivated practitioners — not only by government agencies. When the NVD backlog crisis made tens of thousands of CVEs invisible to downstream scanners, projects like this provided visibility that official infrastructure did not. **Teaching point for students:** AI is dramatically increasing discovery volume. The infrastructure for cataloging, enriching, and operationalizing that volume is fragile and human-staffed. Individual practitioners who build accountability tooling counterbalance that fragility in a way that enterprise SIEMs and compliance frameworks cannot.

Visualization: See `code/01_cve_volume_trends.py` for a chart of annual CVE disclosures 2015–2026.

**Discussion question for students:** If AI can find bugs faster than humans can fix them — and faster than the NVD can even catalog them — what does that mean for the concept of a "secure" codebase?

---

## 09:45 AM – 10:50 AM | The Exploitability Overlay (Rain vs. Floods)

### The Real Bottleneck

The narrative in the press focuses on CVE volume. The operational reality is different: **the constraint has never been discovery — it has always been human capacity to act.**

A security team with 5 analysts cannot triage 5,000 new CVEs per week regardless of how sophisticated their tooling is. The question is not "how many vulnerabilities exist?" but "which ones require my team's attention today?"

Important note: the "patching burden" is not simply a count of CVEs. It is the number of **asset-CVE pairs** — a KEV entry affecting 10,000 servers in your environment is 10,000 remediation instances to track. The numbers below describe the raw CVE signal; actual analyst burden scales with your asset footprint.

### Heavy Rainfall (Total Volume)

Factors driving aggregate disclosure volume up:
1. **AI-assisted bug hunting** — automated pipelines finding issues at superhuman scale (see tools above).
2. **CNA expansion** — the number of CVE Numbering Authorities has grown from roughly 100 in 2017 to 400+ by 2024, pulling more previously-unreported issues into the formal registry.
3. **Retroactive disclosure** — researchers using new tools to surface and document long-standing issues.
4. **NVD backlog processing** — when NIST resumed enrichment and cleared backlogs, previously-invisible CVEs appeared in dashboards all at once.

Visualization: See `code/01_cve_volume_trends.py`.

### Stable Flood Lines (Actionable Risk)

The counterintuitive finding: when you apply an **exploitability overlay**, the actionable subset grows much more slowly than total volume.

Two primary filters:
- **CISA KEV (Known Exploited Vulnerabilities) catalog** — approximately 1,200 cumulative entries as of early 2026; these are confirmed, in-the-wild exploits. CISA adds roughly 150–300 new entries per year in recent years (the addition rate was higher in 2022 when many historical entries were retroactively cataloged). If you patch nothing else, patch these.
- **EPSS > 10% threshold** — the Exploit Prediction Scoring System (FIRST.org) gives each CVE a probability-of-exploitation score. The 10% threshold is a practitioner convention, not an official standard — organizations commonly use 1%, 5%, or 10% depending on team capacity. At 10%, you capture most real-world risk while filtering out ~85–90% of raw volume.

Comparing signal to noise: The cumulative NVD database contains approximately 250,000 total CVEs. The KEV catalog contains approximately 1,200. That is roughly a 208:1 ratio of catalog-to-exploited. For the 2024 annual cohort alone: ~40,000 new CVEs assigned against ~200 new KEV entries — approximately 200:1. AI discovery is increasing the numerator; it is not meaningfully increasing the denominator.

Visualization: See `code/02_exploitability_overlay.py` for a live query of the KEV catalog and EPSS data.

### Separating Signal from Noise: Risk Appetite Alignment

Different organizations have different risk tolerances and different asset profiles. Framework for calibrating (burden figures are raw CVE counts for a 2024 annual cohort; actual remediation instances scale with your asset footprint):

| Tier | Filter | Approx. Annual CVE Count (2024 baseline) |
|------|--------|----------------------------------------|
| Minimum viable | KEV only | ~200 new entries/year |
| Recommended | KEV + EPSS > 10% | ~2,000–4,000/year |
| Comprehensive | EPSS > 1% | ~8,000–15,000/year |
| Exhaustive | All CVEs | ~40,000/year (2024); ~82,000 projected 2026 |

Speaker note: Ask students which tier their employer or internship operates at. The answer is usually "exhaustive in policy, minimum viable in practice."

**Important caveat on the KEV filter:** KEV is backward-looking — it records confirmed exploitation, with a lag between actual exploitation and catalog entry. If AI-accelerated exploit development shrinks the gap between CVE disclosure and active exploitation, KEV will increasingly lag behind the threat. It remains an excellent minimum viable filter; it should not be treated as a real-time threat signal.

---

## 10:50 AM – 11:05 AM | Break

15-minute break. Encourage students to visit `https://api.first.org/data/v1/epss?cve=CVE-2021-44228` in a browser and look up a CVE they know.

---

## 11:05 AM – 11:55 AM | Defensive AI and the MTTR Race

### Poachers Turning Gamekeepers

The same capabilities that enable AI-assisted vulnerability discovery also enable AI-assisted defense. This is not a coincidence — the techniques are symmetric. Understanding a code path well enough to find a bug in it is very close to understanding it well enough to write a fix.

Historical pattern: Offensive techniques (fuzzing, static analysis, symbolic execution) all eventually become defensive tools. AI follows the same arc, just faster.

### Tooling Counterbalance

Key defensive AI systems in production or active development as of 2025–2026:
- **Microsoft Security Copilot** — documented AI assistant for security operations; integrates with Sentinel, Defender, and Intune for threat intelligence synthesis and incident summarization.
- **CrowdStrike Charlotte AI** — AI assistant in the Falcon platform for threat hunting and alert triage.
- **GitHub Advanced Security / Copilot Autofix** — AI-assisted code scanning that suggests security fixes in-line during the development process.
- **Google Mandiant AI** — AI-assisted threat intelligence and incident response tooling.
- **AI-assisted WAF signature generation** — models that observe attack patterns and generate detection rules in near-real-time, compressing the window between exploit publication and detection.
- **Automated patch drafting** — LLM agents that ingest a CVE advisory, locate the vulnerable code path in a codebase, and submit a draft PR for human review. This exists in prototype form at several large organizations and in commercial products like Snyk and Semgrep.

### Mean Time To Remediate (MTTR): The Defining Dynamic

MTTR has historically been measured in weeks to months. Industry benchmark reports (Tenable's "The Life of a Vulnerability," Edgescan's annual statistics report) consistently show median MTTRs of 60–80 days for critical vulnerabilities. The AI era is compressing timelines on both sides.

**Critical caveat — vendor-dependent patching:** The MTTR discussion below applies primarily to **in-house code** where AI can draft a patch. For third-party and vendor software — which constitutes the majority of most enterprises' attack surface — AI cannot help. You wait for Oracle, SAP, Cisco, VMware, or Microsoft to release a patch on their schedule. This is a fundamental limit on the AI-driven MTTR improvement and is often glossed over.

- **Exploit side:** AI can generate working proof-of-concept exploits for published CVEs within hours of disclosure for well-understood vulnerability classes, compared to days-to-weeks previously.
- **Defense side:** For in-house code with good test coverage and CI/CD, AI patch drafting can reduce time-to-patch significantly. However, deploying to production still requires organizational approval chains, change windows, regression testing, and rollback planning. These process constraints are not solved by AI.

**The race condition:** Both timelines are compressing. What matters is the **Window of Exposure (WoE)** — the gap between "exploit available" and "patch deployed." If exploit generation outpaces organizational deployment capacity, the WoE grows even if individual MTTR improves.

Visualization: See `code/03_mttr_race.py` for a timeline chart modeling the pre-AI and post-AI scenarios.

**Mini-case study — "Patch in 4 hours, deployed in 47 days":**
This is not a horror story. Every delay below was a real control with a real reason. Walk students through it as a map of the organizational terrain they will actually work in.

*Setting:* Mid-size regulated bank (~2,000 employees). Java web portal using a vulnerable Apache Commons component. CVE published Monday morning, EPSS 0.58, added to CISA KEV within 48 hours of publication.

- **Day 0, 4 hours:** GitHub Copilot Autofix identifies the vulnerable call site, drafts a patch, opens a PR. The developer reviews and approves. The patch is technically correct.
- **Day 1–2:** Application security team reviews the PR. Discovers the fix requires upgrading a transitive dependency that is incompatible with an internal audit-logging library locked to Java 8. Technical hold placed.
- **Day 4:** Java 8→11 compatibility work completed by a different team. New PR opened and approved.
- **Day 5:** CAB (Change Advisory Board) submission. Next CAB meeting is Thursday. Standard notice period: 5 business days. Cannot expedite without a P1 incident declaration (which requires active exploitation evidence in *this* environment).
- **Day 8:** CAB approves — with conditions: full regression test suite required; production deploy must occur during Saturday night maintenance window only.
- **Day 15:** Shared regression environment is booked Monday mornings only. Test run begins.
- **Day 18:** Two test failures. Unrelated to the patch itself, but bank policy requires acknowledgment and sign-off from the two application owners whose services regressed.
- **Day 24:** Both sign-offs collected after scheduling and calendar delays.
- **Day 26:** Patch deployed to staging (Saturday window).
- **Day 33:** Mandatory 5-business-day staging soak period completes (regulatory requirement for changes affecting the core banking tier).
- **Day 40:** Deployed to production (Saturday window).
- **Day 47:** Verification complete, ITSM ticket closed.

**AI solved the 4-hour problem. The organization solved the 43-day problem. These are different problems.**

Mapping to the constraint categories: CAB notice period → *change approval boards*; Monday-only regression environment → *resource-constrained testing infrastructure*; Java 8/11 incompatibility → *vendor/dependency coupling*; 5-day staging soak → *regulatory requirement*; application owner sign-offs → *human accountability chain*.

None of these delays are unique to this bank. Every enterprise in a regulated industry has equivalents. The students sitting in this room will encounter all of them within their first two years in the workforce.

**Discussion question:** What organizational constraints (approval chains, change management, vendor-dependent systems, regulatory freeze periods) would prevent your organization from deploying a patch 4 hours after it was written?

---

## 11:55 AM – 12:40 PM | Ephemeral Software and Micro-Vulnerabilities

### The Shadow Registry

Traditional vulnerability management assumes a relatively stable, auditable software inventory. You know what libraries you depend on, those libraries have CVEs filed against them, and you patch when notified.

AI assistants break this model. When a developer asks Claude, Copilot, or Cursor to "write me a parser for this custom log format," a novel piece of code is generated, reviewed cursorily, and deployed. This code:
- Has no CVE history (it was just written).
- May not be in any dependency manifest.
- May never be reviewed by a security engineer.
- May be regenerated slightly differently next week.

This class of software — **ephemeral instant software** — constitutes a growing but largely invisible attack surface.

### Systemic Localized Risk

Unlike a vulnerability in `log4j` (one flaw, millions of deployments, one CVE), micro-vulnerabilities in AI-generated code are:
- **Bespoke:** Each instance is unique. There is no "patch" to distribute.
- **Invisible:** They will not appear in the NVD. No CNA will file a CVE for a custom parser written by an intern's AI assistant.
- **Persistent:** The code may run in production for years before anyone audits it.

Published research on AI-generated code security (all real and citable):
- Pearce et al. 2022, "Asleep at the Keyboard? Assessing the Security of GitHub Copilot's Code Contributions" — found that approximately 40% of Copilot-generated code in security-relevant scenarios contained vulnerabilities.
- Perry et al. 2023, follow-up study on AI code assistants and developer security behavior.
- Multiple 2024–2025 follow-up studies showing continued rates of security-relevant issues in AI-generated code, particularly for input validation and integer handling.

### What Can We Do About It?

The honest answer is that no mature, universal solution exists. The current practical options all have significant limitations:

**Static analysis (bandit, semgrep, CodeQL):**
Tools like bandit can catch known bad patterns — SQL injection via string formatting, `shell=True` with user input, weak hash functions. This is the most immediately deployable response and should be part of every CI/CD pipeline that deploys AI-assisted code. Limitation: static analysis misses logic-level flaws that look syntactically correct and produces false positives that erode developer trust.

**Mandatory human review for AI-generated code touching security-sensitive paths:**
Require security engineer sign-off on any AI-generated code that handles authentication, authorization, data access, or external input. Limitation: expensive, doesn't scale, and "security-sensitive" is hard to define consistently across an organization.

**Runtime monitoring:**
eBPF-based tools (Falco, Cilium Tetragon) can flag anomalous behavior in production — code paths that access resources unexpectedly. Effective in Kubernetes/cloud-native environments; much harder to deploy universally across a heterogeneous enterprise estate. Reactive, not preventive.

**Restricting AI codegen to non-sensitive paths:**
Hard to enforce and the boundary is rarely clear.

The practical takeaway for students: **Add static analysis to your CI/CD pipeline, require human review on security-sensitive AI-generated code, and be honest with your organization that this attack surface does not yet have a complete tooling answer.**

**Where this is heading — research directions and forward-looking signals:**

The tooling gap is real today. The following are active research and early-deployment directions worth following — and worth building careers on:

- **eBPF runtime enforcement (beyond monitoring):** Tools like Cilium Tetragon are moving from alerting toward active policy enforcement — blocking unexpected syscalls, file-access patterns, and network connections at the Linux kernel level *before* they complete. Unlike signature-based detection, eBPF enforcement requires no prior knowledge of what a vulnerability looks like; it enforces what *should* happen. This could catch AI-generated code behaving anomalously even when no CVE exists for it. Current hard limit: requires Linux kernel ≥ 5.8, significant operational expertise, and works cleanly only in cloud-native Kubernetes environments. Heterogeneous enterprise estates present a much harder deployment problem.

- **AI-agent code reviewers:** Dedicated AI agents trained specifically to review AI-generated code for security anti-patterns are in early commercial development. Snyk DeepCode AI, CodeAnt AI, and Semgrep's AI features all address parts of this problem. The open research question: *can AI reliably detect its own failure modes?* Early results show these tools catch some vulnerability classes (injection patterns, weak crypto) at useful rates, but logic-level authorization flaws and race conditions remain hard. The false-negative rate at which these tools miss real vulnerabilities is not yet publicly characterized for real-world corpora.

- **AI-provenance tagging in build pipelines:** Some organizations are beginning to annotate git commits, pull request metadata, and SBOM entries with AI-generation provenance — enabling downstream policy gates ("no AI-authored code touching the payment authorization flow without AppSec sign-off"). No standard exists yet; watch for SLSA (Supply-chain Levels for Software Artifacts) or SBOM extensions to pick this up in 2025–2026.

*Honest framing for students:* None of these are universal production-ready answers. eBPF enforcement at enterprise scale requires expertise and infrastructure most organizations don't have. AI code reviewers have their own blind spots and are evaluated on largely synthetic benchmarks. The security engineering research agenda for 2025–2030 is substantially about closing this gap. That means it is a genuinely open field — which is where early-career researchers can do work that matters.

Visualization: See `code/04_ephemeral_software.ipynb` for a live bandit scan of vulnerable AI-generated code patterns.

**Discussion question:** You find AI-generated code in a production service during a security review. It has no tests, no review record, and bandit flags two medium-severity findings. What do you do, and who do you involve?

---

## 12:40 PM – 1:00 PM | Conclusion and Q&A

### Analysts Are Humans: Returning to the Opening Question

We opened with: *"If AI can find bugs faster than humans can fix them — and faster than the NVD can catalog them — what does 'secure' mean?"*

The answer the lecture proposes: **"Secure" in the AI era means maintaining a manageable, risk-calibrated Window of Exposure — not zero vulnerabilities.** The goal is not to eliminate all flaws (impossible) but to ensure that the exploitable, unpatched subset in your environment remains small and decreasing.

The entire coordinated vulnerability disclosure (CVD) ecosystem — from researcher to CNA to vendor to patch to deployment — runs on human capacity. AI accelerates every node in that graph, but humans remain the decision points and the bottleneck.

Key takeaway: More CVEs does not mean more risk. More unpatched exploitable vulnerabilities means more risk. These are different quantities, and conflating them leads to both over-investment in noise and under-investment in signal.

### Budgeting for Software Growth

Practical recommendation for security teams:

> **Resource your vulnerability program to the size and growth rate of your software asset register, not to the raw CVE feed.**

If your software estate grew 30% this year, your patching and triage capacity should grow proportionally.

**Practical note for students entering the workforce:** Enterprise security budgets are typically set through annual cycles benchmarked against peers as a percentage of IT spend or revenue. The argument that gets budget approved usually speaks to breach likelihood, compliance exposure (SOC 2 findings, PCI non-compliance, HIPAA audit risk), and insurance cost. Software asset growth is the intellectually correct framing; compliance and breach risk are usually the politically effective framing. Knowing both is valuable.

### Topics This Lecture Didn't Cover (But You'll Encounter in the Job)

A four-hour lecture can't cover everything. Things you'll hit on day one that this session didn't prepare you for:
- **Asset discovery** — finding all the assets before you can patch them. Shadow IT, cloud sprawl, M&A integrations, and dev environments that became production are where VM programs fail first.
- **Ownership chains** — who actually owns the patch? A CVE in an Apache component used by 40 application teams doesn't have one owner.
- **Exception management** — most mature VM programs have more open exceptions than closed vulnerabilities. The workflow for risk acceptance and compensating controls is a major operational component.
- **SLA definition** — "patch critical CVEs in 30 days" sounds simple. From what date? Detection? Disclosure? Scan? These choices matter.
- **Vendor-dependent patching** — Oracle, SAP, Cisco, legacy ERP — you wait for the vendor, full stop.
- **Regulatory and change freeze windows** — banks, healthcare, and utilities have periods where no production changes are allowed, regardless of how fast AI can draft patches.

### Key Takeaways

1. AI-driven discovery is expanding CVE volume discontinuously. This is mostly noise.
2. The exploitable subset (KEV + EPSS > 10%) is the actionable target and is growing much more slowly.
3. MTTR is becoming the critical metric. The race between AI-generated exploits and AI-generated patches defines your exposure window — but vendor-dependent and legacy patching remain outside AI's reach.
4. Ephemeral AI-generated code is a new attack surface that CVE scanners cannot see. Static analysis (bandit, semgrep) and mandatory human review are the current best mitigations — neither is complete.
5. Human capacity — to verify, coordinate, decide, and deploy — remains the binding constraint in the CVD ecosystem.

### Open Floor: Q&A

Suggested prompts if conversation stalls:
- "What is your organization's patching SLA, and is it based on CVSS score, KEV membership, EPSS, or something else?"
- "Has anyone here encountered AI-generated code in a security review? What did you find?"
- "NIST suspended NVD enrichment in 2024. What does it mean for the security ecosystem if a single government agency is a critical bottleneck in vulnerability disclosure?"

---

## Key References

- CISA KEV Catalog: https://www.cisa.gov/known-exploited-vulnerabilities-catalog
- EPSS documentation: https://www.first.org/epss/
- NVD API: https://nvd.nist.gov/developers/vulnerabilities
- Pearce et al. 2022 "Asleep at the Keyboard?": https://arxiv.org/abs/2108.09293
- Perry et al. 2023 "Do Users Write More Insecure Code with AI Assistants?": https://arxiv.org/abs/2211.03622
- NIST NVD backlog announcement (May 2024): https://www.nist.gov/system/files/documents/2024/05/14/NVD_Announcement.pdf
- Tenable "Life of a Vulnerability" report: https://www.tenable.com/cyber-exposure/research
- Bandit (Python security linter): https://bandit.readthedocs.io/
- Semgrep (multi-language static analysis): https://semgrep.dev/
