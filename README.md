# Sanhedrin

Structured disagreement for hard decisions.

> "Accept the truth from whatever source it comes" - Maimonides

## What it does

Sanhedrin runs 7 independent agents that examine a decision through distinct cognitive frameworks. It uses blind memos, strategy clustering, cross-examination, and an optional Codex audit to pressure-test judgment. Consensus is not guaranteed. Useful disagreement is the output.

## Install

```bash
git clone ~/sanhedrin ~/.claude/skills/sanhedrin
```

## Quick start

```text
/sanhedrin
I have two acquisition offers, 45 days of runway, and a cofounder who wants to wait. What should we do?
```

## Demo

Open [demo/replay.html](demo/replay.html) for the replay. The recorded walkthrough is included as `demo/sanhedrin-demo.mp4`.

## How it works

- Briefing: extract the objective, counterpart, deadline, stakes, BATNA, and constraints.
- Blind memos: each framework writes an independent memo before seeing other views.
- Clustering: overlapping recommendations are grouped into distinct strategic options.
- Cross-exam: each framework steelmans one rival path and attacks another on structure.
- Audit: an optional Codex pass surfaces assumptions, probabilities, and missing evidence.
- Final counsel: return a recommended move, fallback, tripwires, first action, and preserved dissent.

## The Council

These are cognitive frameworks, not identity simulations. Three are always present and four are available as dynamic additions depending on the decision.

| Framework | Lens | Status |
|---|---|---|
| Sun Tzu | Terrain, timing, indirect strategy, and information advantage | Permanent |
| Machiavelli | Power dynamics, incentives, appearances, and decisive pragmatism | Permanent |
| Maimonides | Systems thinking, sustainability, ethics, and principled compromise | Permanent |
| Thomas Schelling | Bargaining, signaling, commitment, BATNA, and deterrence | Dynamic |
| Daniel Kahneman | Cognitive bias audit, base rates, framing, and calibration | Dynamic |
| Talleyrand | Coalition dynamics, political mapping, optionality, and regime-change survival | Dynamic |
| Rabbi Akiva | Persistence, reinvention, hidden opportunity, and long-term human trust | Dynamic |

## Example output

From the included demo transcript:

> "The unlicensed casino thinks it is offering you money. It is actually showing you a map. Read the map."

That is the kind of counsel Sanhedrin is built to produce: not a generic recommendation, but a reframing that changes what the decision actually is.

## Wargame mode

Wargame mode simulates adversarial moves once the opponent model is specific enough to be credible. It is designed for negotiations, competitive responses, and conflict-heavy decisions where the next move depends on how the other side is likely to react.

## Requirements

- Claude Code
- Codex integration (optional, for audit step)

## License

MIT
