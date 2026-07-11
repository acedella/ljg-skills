---
name: ljg-invest
description: "Investment analysis (投资分析). Given a project (company name, pitch deck/BP, founder conversation records), writes an in-depth investment analysis report. Doesn't follow the traditional investment-analysis path — there's only one core judgment: is this project an 'order-creating machine' (秩序创造机器)? Use when user says '投资报告' (investment report), '投资分析' (investment analysis), '分析这个项目' (analyze this project), '写投资报告' (write an investment report), 'investment report', 'invest analysis', or provides entrepreneur conversation records wanting investment evaluation. Also trigger when user pastes or references meeting notes, pitch decks, or founder interviews and asks for analysis."
---

# ljg-invest: Investment Report

Given a project, write an investment analysis. The whole report answers exactly one question: is this thing creating new order, or just shuffling around old order?

## Foundation

One definition holds up the entire report: wealth isn't money — it's order illuminated by desire, and money is merely order's unit of measure. Investing is trading the order in your hand for a better order-generating machine.

Starting from this definition, the report asks different questions than market convention:

| The usual question | This report asks |
|----------|------------|
| What's this company worth | Does this machine actually turn |
| How big is the market | What outdated label is the market using to see it |
| How much can it grow | What am I trading for what, and who's smarter afterward |

## Input

Company name, pitch deck, written description, conversation records — any material describing the project works. For a well-known company, the name alone is enough — use the Research skill or a subagent to pull the latest financials and industry data, don't write from stale impressions.

## Report structure

The five sections are the skeleton, not a fill-in-the-blank form. Whichever section has the most material for this particular project, write more there; sections with nothing to say get a sentence or two, or get skipped entirely. The report serves the judgment — completeness for its own sake means nothing.

### 1. What this is

One table, plus a track/category definition in your own words.

| Dimension | Content |
|----------|----------------------------------------------|
| Project name |                                              |
| Track definition | In our own words, not copied from market labels |
| Stage     |                                              |
| Funding status | Amount / valuation / terms (fill in if available, note if not) |
| Data snapshot | Key operating metrics |

The track definition needs to say what this company is really doing — the layer market labels can't say. Calling it "a search engine company" says nothing; saying it's "the monopoly operator of humanity's cognitive infrastructure" — how it makes money, what it fears — is all in that one sentence.

### 2. Order-creating machine (秩序创造机器) verdict

The weight of the whole report rests on this section. No item-by-item scoring — just answer one question: **does this machine actually turn?** From three angles.

**Is the flywheel spinning?**
First find the loop in the system that gets better the more it's used: more users means more data, more data means a better product, a better product means more users. Once found, see how far along it is — stalled, just starting, already spinning; if spinning, how long has it been spinning, and is it accelerating or flattening out; if stalled, write out exactly which link is jammed.

**Does it get stronger or weaker after a shock?**
Competition breaks in, technology shifts, the market crashes — does this machine shatter, hold up, or turn the shock into its own fuel? Go through its history: has it been hit before, and did it come out weaker or stronger.

**Are resources being pushed toward it, or arriving on their own?**
If expansion means negotiating deal by deal, buying piece by piece, that's pushing. If others rush toward it on their own — losing out if they don't — that's gravitational pull. Look for signs of "gathering without being pushed."

**Overall verdict**, pick one of three:

- Order-creating machine — flywheel spinning, gets stronger after shocks, resources arrive on their own
- Has potential — the flywheel's structure exists, not yet proven to actually spin
- Order shuffling — rearranges what already exists, without generating new order

### 3. Genesis formula

Every order-creating machine has a core algorithm — write it in one sentence. Reference examples:

- Amazon = profit → reinvest → cut costs → cut prices → more users → more profit
- Tesla = hardware collects data → data trains the algorithm → the algorithm redefines the hardware
- Google = every time humanity's way of finding answers migrates, become the default infrastructure of the new way

After writing it, ask two follow-ups: how many times has this formula been validated, and to what degree; is anyone else running a similar formula, and where's the difference.

### 4. What the market sees vs. what we see

The investment timing gets decided in this section.

**Where is it on the S-curve?**
Accumulation phase, inflection point, acceleration phase, plateau — which segment does it fall in. For anything before the inflection point, spell out what conditions would trigger it.

**What outdated lens is the market viewing it through?**
What label has the market stuck on it, what does that label obscure, and what does our framework see that it doesn't. How big is this perception gap — that's where the excess return comes from. To gauge the perception discount, look for three signals: it takes real effort to explain before others get it; pricing has been anomalous for a long time, the parts don't add up to the whole; none of the ready-made analogies quite fit — like X but not quite X.

**What does it control that others can't take away?**
What it's holding onto — data, distribution, standards, or network effects. Is this control static (brand, patents), or does it compound over time. Look one step further ahead: will this scarcity shift elsewhere, and can the project keep up.

**What wave is it riding?**
Three kinds of cost are currently collapsing: the cost of understanding, the cost of coordinating, the cost of acting. Which one is this project riding on, and how much of the energy released by that collapse has it captured.

### 5. Trade or not

- **Trade recommendation**: recommend investing / recommend watching / recommend passing
- **If investing**: recommended amount range, key terms
- **Core assumptions**: which assumptions is this decision betting on. Pair each assumption with an exit signal — what data appearing would mean the assumption is wrong and it's time to leave.
- **Open questions**: 3-5 questions that matter for the decision but don't have answers yet, ranked by importance.

### Final line

Answer in one sentence: what is the essence of this project — is it creating new order, or shuffling old order?

## Output

- Format: Markdown
- Directory: `"/Users/jjin/Documents/Obsidian Vault/Investment Notes/"` (quote the path — it has spaces; create the folder if it doesn't exist)
- Naming per denote: `YYYYMMDDTHHMMSS==z--investment-analysis-PROJECT_NAME.md`, e.g. `20260326153000==z--investment-analysis-example-ai.md`
- Write using the Write tool, and report the full path back to the user when done

## Generation rules

1. Use only real information. If something can't be found, mark it as such — don't force it, don't make it up.
2. Commit to a judgment. Statements like "could be good, could be bad" are not allowed at all.
3. Every judgment carries evidence: data, citation, specific fact.
4. These phrases are banned: "huge market," "great team," "broad prospects," "blue ocean market."
5. Length should match what it takes to say it fully. If 2,000 words says it, use 2,000; if it needs 7,000, use 7,000.
6. Write in the same language as the user's input/request.
</content>
