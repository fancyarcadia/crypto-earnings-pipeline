# RaptorX Research Challenge — Submission Dossier
## Ask Better Questions. Find Better Alpha.

> Superteam Earn bounty: RaptorX Research Challenge ($125 USDC, deadline 2026-11-08).
> Format: research dossier (GitHub-hosted). This document answers *why most crypto research fails* and demonstrates a better question-stack applied to a live Solana ecosystem case study. All facts cited are structural (public architecture/tokenomics), verified against primary docs; no price predictions.

---

## 1. The core thesis: alpha comes from better questions, not better data

Everyone has the same price feeds. Alpha decays to zero the moment it's published. The only durable edge is a repeatable **question stack** that filters noise before you ever look at a chart. This dossier formalizes that stack and applies it to one case study.

### The question stack (5 levels)

1. **Who owns the cash flows?** — where does revenue actually accrue (protocol, tokenholders, validators, MEV searchers)?
2. **What is the token's job?** — is it a claim on cash flows, a work token, a governance token, or a meme with extra steps?
3. **Is supply aligned with usage?** — do emissions/sinks match real demand (fees, burns, staking drains)?
4. **What breaks the flywheel?** — single points of failure: one dominant LP, one whale, one team-controlled oracle?
5. **What does the market misprice?** — the question that turns research into a trade.

## 2. Case study: Jito (JTO) — the cleanest "fee-switch" story on Solana

Why Jito? Because it is one of the few Solana protocols where **revenue accrues to the protocol itself**, not just to searchers. That makes it a clean laboratory for the question stack.

### Q1 — Who owns the cash flows?
- Jito is Solana's largest MEV/validator ecosystem. Its core products: the JitoSOL liquid-staking pool and the MEV (block-engine) marketplace.
- MEV tips flow to validators; **JitoSOL staking rewards flow to pool depositors**. Protocol revenue historically comes from a portion of MEV tips (the protocol fee) and from the staking spread.

### Q2 — What is JTO's job?
- JTO is a governance token, but with a twist: Jito introduced a **protocol treasury fee switch** — governance can direct a share of MEV revenue toward the protocol treasury, i.e., JTO holders vote on whether value is captured at the protocol level.
- This makes JTO a *conditional claim* on cash flows: it has optionality, not a fixed yield.

### Q3 — Is supply aligned with usage?
- JitoSOL supply grows with SOL deposits: more deposits → more staking rewards in the pool. The token's utility (governance over the fee switch) is tied to the protocol's own revenue, not to speculative emissions.

### Q4 — What breaks the flywheel?
- **Concentration risk:** Jito's MEV market share is a function of its validator network; a competing block engine could commoditize tips, reducing protocol fee potential.
- **Staking competition:** Liquid staking is contested (Marinade, Sanctum, etc.); JitoSOL's yield premium is the moat, and it can compress.

### Q5 — What might the market misprice?
- The question that matters here: **if the fee switch channels a meaningful share of MEV revenue to the treasury, what is JTO worth as a claim on that stream?** The market has historically priced JTO as pure governance; a fee-switch activation would repricing the asset as a revenue claim — a classic "stale narrative" mispricing candidate.

## 3. The repeatable output: a 1-page research template

Use this for any token:

| Question | Answer template | Where to look |
|---|---|---|
| Who owns cash flows? | Protocol / tokenholders / validators / none | fee docs, protocol revenue trackers |
| Token's job? | claim / work / governance / meme | token docs, tokenomics |
| Supply aligned with usage? | emissions vs burn vs sinks | inflation schedule, burn logs |
| What breaks it? | concentration, dependency, team risk | holder distribution, code dependencies |
| What's mispriced? | one-sentence divergence thesis | TVL vs revenue vs FDV |

## 4. Why this wins (for the sponsor)

- It is **model-free**: works on any token without exotic data feeds.
- It is **testable**: each answer can be falsified with a primary source.
- It identifies **structural, not narrative, alpha** — the kind that survives.

---

*Prepared for the RaptorX Research Challenge. Sources: Jito docs (jito.network), Solana ecosystem dashboards (public). All claims are structural; nothing here is financial advice.*
