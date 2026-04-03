# Sanhedrin

Structured disagreement for hard decisions.

> *"Accept the truth from whatever source it comes"* -- Maimonides

<div align="center">

https://github.com/Bambushu/sanhedrin/raw/main/demo/sanhedrin-demo.mp4

</div>

## The problem

You ask an AI for advice and it gives you one perspective. A confident one. It sounds right. You act on it.

That is the failure mode.

Hard decisions -- career moves, negotiations, competitive strategy, whether to walk away -- have multiple valid framings. The same situation looks completely different through the lens of power dynamics, game theory, cognitive bias, or long-term reputation. One model, one perspective, one answer is not enough.

Sanhedrin dispatches seven independent strategic thinkers against your situation. They write blind memos before seeing each other's work. Then they cross-examine each other's strategies. The output is not consensus. It is structured disagreement that shows you what the decision actually is.

From the demo:

> *"The unlicensed casino thinks it is offering you money. It is actually showing you a map. Read the map."*

That is the kind of counsel Sanhedrin produces. Not a generic recommendation, but a reframing that changes what you are deciding.

## Install

```bash
git clone https://github.com/Bambushu/sanhedrin ~/.claude/skills/sanhedrin
```

## Quick start

```
/sanhedrin
I have two acquisition offers, 45 days of runway, and a cofounder who wants to wait.
What should we do?
```

## How it works

```
Briefing        You describe the situation. Sanhedrin extracts objective,
                counterpart, deadline, stakes, BATNA, constraints.

Blind memos     5 advisors write independent analyses in parallel.
                No advisor sees another's work.

Clustering      Overlapping recommendations are grouped into 2-3
                distinct strategic options.

Cross-exam      Each advisor steelmans one rival strategy and attacks
                another. No hedging. No "it depends."

Intervention    You challenge advisors, add information, swap roster
                members, or trigger wargame mode.

Audit           An independent auditor pressure-tests everything:
                probability bands, assumption tables, pre-mortems,
                decision trees, and a mandatory "do nothing" row.

Final counsel   Recommended move, fallback, tripwires, first action
                today, and any preserved dissent.
```

## The council

Three permanent members. Four available as dynamic picks based on what you are facing.

### Always present

**Sun Tzu** -- Terrain, timing, indirect strategy
*"Where is the enemy weak? What battle should you avoid entirely?"*
Calm, indirect, metaphorical. Prefers asymmetric positions where downside is limited. Deeply skeptical of urgency.

**Machiavelli** -- Power dynamics, incentives, appearances
*"What do they actually want? What would happen if you did nothing?"*
Pragmatic, unsentimental, direct. Separates stated reasons from actual incentives. Cuts through self-deception.

**Maimonides** -- Systems thinking, ethics, sustainability
*"What is the sustainable path? Where is the balance between principle and pragmatism?"*
The ethical anchor. If the recommended move is effective but destructive to reputation, his dissent is prominently displayed even if he is outvoted.

### Dynamic roster

Chosen automatically by situation type, or override with `Roster: akiva, talleyrand`.

**Thomas Schelling** -- Bargaining, signaling, BATNA
*"What signals are you sending? What commitment would change the game?"*
Best for: negotiations, contracts, salary, counter-offers.

**Daniel Kahneman** -- Cognitive bias, base rates, calibration
*"What's the base rate? How confident should you really be?"*
Best for: any situation where overconfidence or loss aversion might be distorting the analysis.

**Talleyrand** -- Coalition dynamics, political survival
*"Who actually has power right now? How do you make yourself indispensable to them?"*
Best for: org politics, restructurings, layoffs, alliance-building.

**Rabbi Akiva** -- Persistence, reinvention, hidden opportunity
*"What opportunity is hidden in this difficulty? What could this become if you persist?"*
Best for: moments of doubt, pivots, rebuilding, starting over.

## Wargame mode

For decisions with an active opponent. Sanhedrin builds an opponent model (their incentives, constraints, BATNA, public vs private posture), validates it with the council, then simulates moves and counter-moves in a structured table.

```
/sanhedrin wargame
[your situation + what you know about the opponent]
```

Three or more advisors flagging the same gap in the opponent model will halt the simulation and ask you to fill it. No wargame runs on a weak model.

## What makes this different

**Blind memos.** Advisors write independently before seeing each other. This prevents anchoring and groupthink. When five frameworks converge on the same answer without coordination, that signal is worth something.

**Cross-examination.** Every strategy gets steelmanned by a rival and attacked on structure: faulty incentive assumptions, ignored second-order effects, poor timing, reputational blowback. Generic objections are not allowed.

**Mandatory "do nothing" row.** The audit always evaluates inaction as an explicit option. This prevents action bias -- the default tendency to do *something* even when waiting is correct.

**Preserved dissent.** If Maimonides says a strategy is effective but corrosive, or Akiva sees a hidden opportunity the majority dismissed, their position is preserved in the final counsel. Minority views are often the most valuable signal.

## Demo

Open [demo/replay.html](demo/replay.html) in a browser for the interactive terminal replay, or watch `demo/sanhedrin-demo.mp4`.

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code)
- Codex integration (optional, for the audit step)

## License

MIT
