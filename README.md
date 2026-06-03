# Vulnerability Management and the AI Crossroads

**ITESO | June 3, 2026 | 9:00 AM – 1:00 PM**
**Presenter: Jerry Gamblin**

---

## What's In This Repo

| Path | Contents |
|------|----------|
| [`outline.md`](outline.md) | Full lecture outline with speaker notes |
| [`slides/slide_outline.md`](slides/slide_outline.md) | Full slide-by-slide content outline (43 slides + appendix) |
| [`slides/speaker_notes.md`](slides/speaker_notes.md) | Presenter talking points for all 53 slides — transitions, anticipated Q&A, and accuracy guardrails |
| [`code/`](code/) | Python data-visualization demos for each section |

## Running the Notebooks

```bash
cd code
pip install -r requirements.txt
jupyter lab          # then open any .ipynb file
```

On first run, notebooks that query live APIs will fetch and cache data to `code/output/`. Subsequent runs load from cache and are instant. **Without a key, NVD throttles to ~5 requests / 30s, so notebooks 1–3 are slow on a cold start.** Setting `NVD_API_KEY` in the first cell of notebooks 1–3 cuts this by ~10× (free key at nvd.nist.gov/developers/request-an-api-key).

| Notebook | Live APIs | First run (no key) | First run (with `NVD_API_KEY`) |
|----------|-----------|--------------------|--------------------------------|
| `01_cve_discovery_analysis.ipynb` | NVD | ~4–5 min | ~30s |
| `02_exploitability_overlay.ipynb` | CISA KEV, EPSS, NVD | ~20–25 min | ~2–3 min |
| `03_exploitation_timeline.ipynb` | CISA KEV, NVD | ~25–30 min | ~3–4 min |
| `04_ephemeral_software.ipynb` | EPSS | ~5s | ~5s |

All outputs (charts + JSON) are saved to `code/output/`.

## Lecture Sections

1. **Introduction: From Automation to Autonomous AI** (09:00 – 09:45)
2. **The Exploitability Overlay: Rain vs. Floods** (09:45 – 10:50)
3. *Break* (10:50 – 11:05)
4. **Defensive AI and the MTTR Race** (11:05 – 11:55)
5. **Ephemeral Software and Micro-Vulnerabilities** (11:55 – 12:40)
6. **Conclusion and Q&A** (12:40 – 1:00)
