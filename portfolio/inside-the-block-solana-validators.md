# Inside the Block: How Solana Validators Order Transactions

> FINAL submission draft v2 — 2026-11-02. Superteam Earn bounty "Inside the Block: How Solana Validators Order Transactions" ($2,500 USDC, deadline 2026-11-05).
> Sources: verified-live links are marked ✅ (checked 2026-11-02). Items marked 🔎 are pre-verified historically but should be re-checked before you send.

---

Everyone knows Solana is fast. Far fewer people can explain *how a block actually gets built and why transactions end up in the order they do*. This is the story inside the block: how validators decide what goes in, in what order, and why ordering on Solana is fundamentally different from Ethereum.

## 1. There is no mempool — transactions go to a leader

On Ethereum, transactions sit in a public mempool where validators and searchers can see, reorder, and front-run them. Solana deliberately removed this. Instead, the network runs a **leader schedule**: at any moment, exactly one validator (the current "leader") is designated to produce blocks for a short window of slots, and the schedule is computed in advance from the validator set ✅ (Solana docs — Terminology: *leader/leader schedule, slot, epoch*: https://solana.com/docs/terminology).

Clients send transactions directly to the current leader — via RPC, then through the network's QUIC transport layer, with Turbine used to propagate completed blocks to the rest of the network. The leader is the first entity that sees and orders transactions for that slot.

## 2. The leader orders by arrival, chunked into entries

The leader receives a stream of transactions. As they arrive, the leader packages them into **entries** — sequential chunks of transactions — and timestamps each entry with a **Proof-of-History (PoH)** sequence. PoH is a verifiable delay function: the leader repeatedly hashes a value, producing a continuous, verifiable timeline. Every entry is cryptographically chained to the previous one, so the order of entries is provable, not just claimed.

This is the crux: **on Solana, block order is the order the leader recorded transactions, frozen into the PoH sequence.** There is no "proposer reordering" after the fact, because the order is part of the chain's clock itself. (Architecture overview ✅: https://solana.com/docs/architecture)

## 3. How the leader actually picks transactions

The leader doesn't run a gas auction per position. Instead:

- **Inclusion vs. order:** priority fees (additional lamports per compute unit) primarily buy *inclusion* under load — they don't reorder already-included transactions ✅ (Transaction fees & priority fees: https://solana.com/docs/transaction_fees).
- **Compute budget:** each transaction declares a compute-unit budget ✅ (Core concepts — transactions & fees: https://solana.com/docs/core/transactions, https://solana.com/docs/core/fees); the leader executes transactions one by one in entry order, subject to the slot's overall compute limits. A transaction that exceeds its budget or is invalid is rejected or rolled back.
- **Duplicate/conflict handling:** if two transactions touch the same state, the first to commit wins; the later one fails atomically. This is why sending the same order twice costs you the second one.

So the practical rule for builders: **on Solana, you cannot rely on "later = higher priority" within a block. If you need to be first, you must reach the leader first.**

## 4. Non-leader validators: vote, don't order

While the leader builds, every other validator does two jobs:

1. **Verification:** independently replay the block (same runtime, same BPF execution) to confirm the state changes are valid.
2. **Voting:** cast a vote referencing the PoH hash of the block they consider canonical, via **Tower BFT** — a stake-weighted consensus protocol. A block becomes *confirmed* once it accumulates votes from ≥ 2/3 of the staked supply, and *finalized* after the fork is locked in by Tower BFT's locking rules. 🔎 (Consensus details — Tower BFT and Proof of History are covered in Solana's docs under "Architecture"; search "Tower BFT" at https://solana.com/docs)

Because ordering is baked into PoH, a vote for a block is simultaneously a vote for its transaction order. There is no separate "order consensus" phase — that's the architectural trick that lets Solana settle ordering in parallel with execution.

## 5. Leader selection: stake-weighted quality

Who gets to be leader? **Stake-weighted quality (SWQ)**: a validator's chance to be scheduled as leader is proportional (with small corrections for liveness/quality metrics) to its share of total stake. The schedule is derived deterministically from the validator set, so everyone knows in advance who leads when — which makes it possible to direct traffic straight to the right leader. 🔎 (See "Leader selection" in Solana docs; search "stake-weighted quality" at https://solana.com/docs)

## 6. Where MEV actually happens: off-chain, before the leader

Because there's no public mempool, classic on-chain front-running is mostly closed on Solana. But extractable value hasn't disappeared — it moved upstream:

- **Jito (and similar) MEV marketplaces** let searchers send **bundles** — ordered sets of transactions — directly to leaders, with a **tip auction** paid to the leader for inclusion. The leader can order bundles before ordinary traffic in a slot. 🔎 (Jito docs: https://jito.network — search "bundles" / "tip")
- The result: ordering *is* influenced by economics, but it happens at the leader's discretion via the bundle auction, not through a transparent global order book.

For users, the practical takeaway is the same: if your transaction's value depends on ordering (arbitrage, liquidations, mints), you must compete at the leader level — with priority fees, or via a bundle/relay — not by hoping for mempool luck.

## 7. Why this matters for dApp builders

- **User experience:** "confirmed" ≠ "ordered." Explain to users that a rejected tx is usually a conflict (state changed first), not a network failure.
- **Latency chasing:** the race is to the leader. Use low-latency RPCs and consider sending through multiple entrypoints.
- **Fairness assumptions:** if your dApp promises fair ordering, you can't — the leader controls inclusion order, and bundles can jump the queue. Design accordingly (e.g., commit-reveal, or off-chain matching with on-chain settlement).

## Conclusion

Solana's block is a leader-built, PoH-stamped, vote-finalized sequence. Order is fixed at the leader the moment entries are chained — and that single design choice is why Solana can process thousands of transactions per second without a global mempool, and why builders must think about ordering differently than they do on Ethereum. The block isn't just a list of transactions; it's a proof of time.

---

## Sources

**Verified live (checked 2026-11-02):**
- ✅ Solana docs — home: https://solana.com/docs
- ✅ Architecture overview: https://solana.com/docs/architecture
- ✅ Terminology (leader, slot, epoch, confirmation/finality): https://solana.com/docs/terminology
- ✅ Transaction fees & priority fees: https://solana.com/docs/transaction_fees
- ✅ Core concepts — transactions: https://solana.com/docs/core/transactions
- ✅ Core concepts — fees: https://solana.com/docs/core/fees

**Re-check before sending (links moved in 2026 restructure):**
- 🔎 Proof of History / Tower BFT / leader selection / Turbine / QUIC: search these terms on https://solana.com/docs (they moved out of the old /docs/consensus/ path)
- 🔎 Jito MEV bundles & tips: https://jito.network (docs section)
- 🔎 Original Solana whitepaper: https://solana.com/solana-whitepaper.pdf
