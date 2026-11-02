# Portfolio Sample 2 — The Anatomy of a Crypto Payout: From "Accepted" to Wallet in Six Steps

> Portfolio sample draft (2026-11-01) — for review before publishing. Written in first person; verify facts you know before posting.

---

Getting paid in crypto sounds simple until you actually have to do it. You finish a job, the client says "sent," and then a transaction hash appears — and the next 24 hours quietly determine whether you actually got paid, got dust, or got nothing at all.

I've been tracking payouts across Ethereum, Solana, Base, and BNB Chain for months as part of a live earning pipeline. After reviewing dozens of inbound transactions — including ten that turned out to be spam and dust — here's the six-step anatomy of a real payout, and where most people lose money.

## 1. The payout rail is decided *before* the work starts

Every platform pays on a specific rail: Superteam and Clustly pay USDC on Solana; Dework pays USDC/USDT on Ethereum; Stacker News pays Bitcoin via Lightning; Algora used to pay USD via Stripe but pivoted to recruiting in 2026. Knowing the rail in advance tells you the gas cost, the expected settlement time, and whether KYC will appear at payout. **The #1 mistake is agreeing to work before asking "how will I be paid?** — the answer changes your net take-home by more than any fee negotiation.

## 2. "Payment sent" is not a payment

A payment is a confirmed transaction on the destination chain. Not a screenshot, not a promise. I only treat work as paid when the tx hash exists and the wallet's own scan — not the platform's UI — shows the incoming balance change.

## 3. The fees eat the small jobs

I tracked $0.35 of pending rewards across basic forum micro-rewards on MoltJobs: several 0.05–0.10 USDC entries. Real network fees (the smallest I've seen: ~5,000 lamports ≈ $0.0005 on Solana) are negligible on Solana, but on congested chains the gas can exceed a small payout's value. **Rule of thumb: anything under ~$5 should be paid on a low-fee rail (Solana/Base) or batched.**

## 4. Token transfers don't look the way you expect

When I inspected a flagged Solana transaction claiming to be an incoming payment, the parsed token-balance diffs told a different story: a ~15 USDC/15 USDT pair swapping between accounts on a December 2025 block — a DEX-style trade, not a payout at all. On-chain tools that only show "activity" will happily report that as income. **You must check *which* accounts changed and *which* mint moved — not just that a transaction exists.**

## 5. Dust is data noise — learn to clear it

Wallet scanners flag any unrecorded inbound transaction, including 7–8 year-old dust and 1,000-lamport transfers. In my ledger, ten auto-flagged Solana transactions — from Nov 2025 to Mar 2026 — turned out to be historical native transfers with zero token movement. We cleared them from the ledger in one pass. **If you don't classify, your bookkeeping inflates "earnings" with garbage.**

## 6. The cold-storage handoff

The protocol I use: hot wallet receives → confirm on-chain → within 24 hours, move ≥80% to cold storage → record the handover in a ledger with tx hash, asset, network, and net amount after fees. This is the step most solo earners skip, and it's the one that makes tax season sane.

## What this means for you

If you're earning in crypto: pick your rail before you pick the job, verify on-chain before you celebrate, and keep a ledger that separates *pending*, *confirmed*, and *cleared-as-noise*. The difference between "I got paid" and "I got paid and I can prove it" is exactly the six steps above.

---

*Written from live pipeline data (ledger reviewed 2026-11-01). For editing before publishing: add your own numbers, confirm chain/name details, and remove this note.*
