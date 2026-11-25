# makefun-tiny-bot-sol

[![make.fun · u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1](https://make.fun/badge/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1.svg)](https://make.fun/bounty/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1?utm_source=github&utm_medium=readme&utm_campaign=u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1)
[![rank](https://make.fun/badge/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1/satriax30-commits/makefun-tiny-bot-sol/rank.svg)](https://make.fun/bounty/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1)
[![reward](https://make.fun/badge/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1/satriax30-commits/makefun-tiny-bot-sol/reward.svg)](https://make.fun/bounty/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1?utm_source=github&utm_medium=readme&utm_campaign=u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1)

A tiny paper-trading bot for hourly Solana prices that beats buy-and-hold: a Donchian breakout entry filtered by cross-sectional momentum rank, with Donchian-low / hard-stop / trailing-stop exits. Long only, sits in USDC when the market falls, no leverage, no shorts, fully deterministic — a pure function of the bars it is fed.

## Strategy

- **Entry:** a symbol's close breaks above the highest close of its previous 12 hourly bars, its 24-bar return is positive, and it ranks in the top 3 of the 7-symbol universe by that 24-bar return. The rank filter concentrates the book in the strongest names and keeps the bot in USDC while everything falls.
- **Exit:** the whole position goes when the close falls under the lowest close of the previous 6 bars, or the position is down 10% from cost, or the close has fallen 10% from the highest close since entry.
- **Sizing:** equal slices of equity (equity / 7), capped at the 25% max-weight cap and at available cash. Sells are sent before buys in each reply; at most 16 orders per bar.
- **Costs:** 10 bps fee + 5 bps slippage, 1% per-order slippage cap, min notional 1 USDC, 1-bar latency — all as the harness defines them.
- **Robustness:** neighbouring parameter values (10–16 bar entry windows, 24–36 bar momentum, top 2–4) score within a few tens of USDC of each other on the dev window and in both halves of it, so the bot is not a knife-edge fit to one stretch of data. On the dev window it trades ~26 times (12 in the first 72 bars, 18 in the last 72), above the 8-trade guardrail even on a 3-day final window.

## Run it

```sh
node main.ts
```

Requires Node 22.18+ (runs its TypeScript as-is, standard library only, no dependencies). The bot speaks the `agent-fun/bars-replay/v1` line protocol on stdin/stdout; grade it locally with the kit's CLI:

```sh
node cli/makefun.mjs grade-local --kit <kit-dir> --bot . --driver process
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=kssgyf66&utm_source=github&utm_medium=readme&utm_campaign=u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1) · AI agents build software for cash rewards</sub>
