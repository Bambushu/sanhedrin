---
name: sanhedrin
description: Sanhedrin — multi-persona strategic advisory council. Dispatches 5 agents (Sun Tzu, Machiavelli, Maimonides + 2 dynamic) for career, business, and life strategy. Protocol: blind memos, strategy clustering, cross-exam, Codex audit, final counsel, HTML report. Invoke with /sanhedrin.
---

# Sanhedrin

Multi-persona strategic advisory council. Seven distinct strategic thinkers debate your situation and produce actionable counsel.

Motto: *"Accept the truth from whatever source it comes"* — Maimonides

## How to Invoke

```
/sanhedrin
[describe your situation]
```

Or with roster override:
```
/sanhedrin
Roster: akiva, talleyrand
[describe your situation]
```

Or wargame mode:
```
/sanhedrin wargame
[describe your situation + opponent details]
```

## The Council

### Permanent (always present)
| Persona | File | Lens |
|---|---|---|
| Sun Tzu | `agents/sun-tzu.md` | Terrain, timing, indirect strategy |
| Machiavelli | `agents/machiavelli.md` | Power, appearances, pragmatism |
| Maimonides | `agents/maimonides.md` | Systems, ethics, golden mean |

### Dynamic Roster (2 chosen per session)
| Persona | File | Best for |
|---|---|---|
| Schelling | `agents/schelling.md` | Negotiations, signaling, BATNA |
| Kahneman | `agents/kahneman.md` | Bias correction, uncertainty |
| Talleyrand | `agents/talleyrand.md` | Politics, restructurings, alliances |
| Rabbi Akiva | `agents/akiva.md` | Persistence, reinvention, doubt |

## Protocol

Follow these steps in order. The user sees everything in their terminal — this is a conversational experience, not a background process.

### Step 1: Briefing + Intake

Read the user's situation description. Extract these fields:

- **Objective** — what does success look like?
- **Counterpart(s)** — who are the other actors?
- **Deadline** — time pressure?
- **Stakes** — what's at risk?
- **BATNA** — what's the alternative if this fails?
- **Constraints** — what can't be done?

Score briefing adequacy:

- **High (4+ fields clear):** Select 2 dynamic members, proceed.
- **Medium (2-3 fields clear):** Default to Schelling + Kahneman. Ask 1-2 targeted questions for the most critical gaps before spawning.
- **Low (0-1 fields clear):** Ask up to 3 targeted questions before proceeding. Priority: objective first, then counterpart, then stakes.

**User can override roster at any time:** "bring in Akiva" or "drop Talleyrand."

**Dynamic roster selection by situation type:**

| Situation keywords | Dynamic picks |
|---|---|
| Negotiation, salary, contract, offer, counter-offer | Schelling + Kahneman |
| Org politics, restructuring, alliances, layoffs, reorg | Talleyrand + Kahneman |
| Stuck, pivot, doubt, starting over, rebuilding, giving up | Akiva + Kahneman |
| Competitive, market, growth, competitor, launch | Schelling + Talleyrand |
| Mixed / unclear | Schelling + Kahneman (safe default) |

**Announce the council:**
> "Sanhedrin convened. **Sun Tzu, Machiavelli, Maimonides, [Dynamic 1], [Dynamic 2]** are present. Situation received. Dispatching blind memos."

### Step 2: Blind Memos

Read each agent's prompt from `agents/<name>.md` in this skill's directory. Replace `{SITUATION_BRIEFING}` with the user's situation description plus the extracted intake fields.

Dispatch all 5 agents in parallel using the Agent tool:
- `mode: bypassPermissions`
- `run_in_background: true`
- Each agent's prompt is the full content of its `.md` file with `{SITUATION_BRIEFING}` filled in

When all 5 return, display each memo to the user with the persona's name as a header:

> **Sun Tzu:**
> [memo content]
>
> **Machiavelli:**
> [memo content]
>
> [etc.]

### Step 3: Strategy Clustering

Analyze the 5 memos. Group them into 2-3 genuinely distinct strategies. Each cluster gets:
- A short label (e.g., "Direct confrontation", "Patient positioning", "Do nothing and build alternatives")
- A 1-2 sentence summary of the core approach
- Which personas aligned with this cluster

**Edge cases:**
- **All memos converge to 1 strategy:** Skip Step 4 (cross-exam). Flag to the user: "Unanimous council — strong consensus or groupthink? Proceeding to Codex audit for independent check."
- **4+ distinct strategies:** Force-merge the two most similar. Max 3 clusters.
- **One outlier vs 4 aligned:** Preserve the outlier as its own cluster — outliers are often the most valuable signal.

Display the clusters to the user:
> **Strategy Clusters:**
>
> **A: [Label]** (Sun Tzu, Maimonides)
> [summary]
>
> **B: [Label]** (Machiavelli, Schelling)
> [summary]
>
> **C: [Label]** (Kahneman — outlier)
> [summary]

### Step 4: Cross-Exam

Send all 5 agents the clustered strategies (NOT the raw memos) via SendMessage. **Important:** When using SendMessage to reach named agents, always include a `summary` parameter (e.g., "Cross-exam: steelman and attack clustered strategies"). Each agent must:

1. **Steelman** one competing strategy — restate it in terms its author would accept
2. **Attack** one competing strategy with structural criticism only:
   - Faulty incentive assumptions
   - Ignored second-order effects
   - Weak or untested assumptions
   - Poor timing
   - Reputational blowback

No "it depends" objections. No hedging.

Display the cross-exam responses as they arrive. This creates the debate flow the user sees.

### Step 5: User Intervention

After displaying cross-exam results, pause and ask the user:

> "The council has debated. You can:
> - Challenge a specific advisor (e.g., 'Schelling, what if my BATNA is weaker?')
> - Add information the council doesn't have
> - Override the roster ('bring in Akiva')
> - Say **'audit'** to proceed to Codex audit
> - Say **'wargame'** to simulate opponent moves"

Route user messages to the relevant agent(s) via SendMessage (always include `summary` parameter). Continue the conversation until the user says "audit" or "wargame."

### Step 6: Codex Audit

Read `references/codex-audit-template.md` in this skill's directory for the full audit specification.

Dispatch a Codex agent (subagent_type: codex:codex-rescue) with:
- The full audit template
- All blind memos
- The strategy clusters
- The cross-exam transcript
- Any user interventions and responses
- The structured intake fields from Step 1

Display the Codex audit results to the user.

### Step 7: Final Counsel

Synthesize the full session into:

- **Recommended move** — the council's consensus or majority position
- **Fallback** — if the recommended move fails
- **Tripwires** — "if X happens, switch to Y" (specific, observable signals)
- **First action today** — one concrete thing to do right now
- **Dissenting view** — if any persona strongly disagrees, preserve their position

Present to the user. Ask: "Does this land? Want to dig deeper on anything, or is this actionable?"

### Step 8: HTML Report

After final counsel lands, render a single self-contained HTML report capturing the full session. Terminal output is ephemeral — HTML is the durable artifact you can re-read in a month or share with a trusted advisor.

**Save to:** `~/sanhedrin-reports/YYYY-MM-DD-<slug>.html` where `<slug>` is 3-5 lowercase hyphenated words from the user's objective (e.g., `2026-05-13-laid-off-pivot.html`). Create `~/sanhedrin-reports/` if missing.

**Content (in this order):**

1. **Header** — situation summary, council roster (with permanent vs dynamic distinction), date, deadline if any.
2. **Persona cards** — one card per persona showing blind memo + cross-exam stance. Permanent personas styled distinctly from dynamic picks. If a persona was an outlier, flag visually.
3. **Strategy clusters** — visual grid (2-3 columns), each cluster with label, summary, aligned personas as chips. Outliers visually distinct (border, badge).
4. **Cross-exam matrix** — table or grid: which persona steelmanned/attacked which strategy, with their key argument inline.
5. **Codex audit** — verbatim, including the mandatory "do nothing / delay" row.
6. **Wargame transcript** (if Wargame Mode ran) — move table with rounds, opponent model, council commentary.
7. **Final counsel hero** — recommended move (large, prominent), fallback, tripwires as a checkbox list (so the user can tick them later), first action today, dissenting view preserved prominently if any.
8. **Copy-as-prompt button** — JS button copying `"First action: [action]. Tripwires: [list]. Context: [one-line situation]."` to clipboard for paste-back into Claude Code.

**Style requirements:**
- Single self-contained file. Inline CSS. No external CDN, no remote fonts — system font stack only.
- Apply `frontend-design` / `impeccable` quality bar. This is a strategic artifact, not a generic AI page. Avoid default-ish layouts.
- Mobile responsive + print-friendly (Mike may share with advisors).
- If Maimonides dissented, surface his dissent with visual weight (border, badge) — never buried in a footer.

**Final terminal message:**
> "Report saved: `~/sanhedrin-reports/YYYY-MM-DD-<slug>.html` — `open` it to view."

**Skip the report if:** user said "no report" / "skip report" during the session, or session aborted before Step 7. Otherwise default to writing it.

## Wargame Mode

Activated when the user says "wargame" during Step 5, or invokes `/sanhedrin wargame`.

### Before simulation:
1. Ask the user to specify the opponent model:
   - What are their incentives?
   - What are their constraints?
   - What's their BATNA?
   - What's their public vs private posture?

2. Send the opponent model to all 5 council agents. Each flags what's missing or assumed.

3. **Abort gate:** If 3+ personas flag the same dimension as under-specified, halt and ask the user to fill the gap. No simulation runs on a weak opponent model.

### Simulation:
- Dispatch 2 agents: one plays the user's position, one plays the opponent
- Use move tables (structured: "Move -> Expected response -> Counter"):

| Round | User's move | Opponent's likely response | User's counter |
|---|---|---|---|
| 1 | [specific action] | [based on opponent model] | [adaptation] |
| 2 | [next move] | [response] | [counter] |

- Max 2 critical decision branches
- Remaining council members observe and comment after each round
- After simulation, proceed to Step 6 (Codex audit) with the wargame transcript included

## Guardrails

- **Legal/medical/crisis:** If the situation involves legal disputes, medical decisions, or active crisis, add a prominent disclaimer: "This is strategic thinking, not professional advice. Consult a [lawyer/doctor/crisis counselor] for decisions in this domain."
- **"Do nothing" always evaluated:** The Codex audit table always includes a "do nothing / delay" row. This prevents action bias.
- **Maimonides as ethical anchor:** If the recommended move is effective but potentially destructive to reputation or relationships, Maimonides' dissent is prominently displayed even if he's outvoted.
