<div align="center">

# CHRONOS

**χρόνος — the measured interval. Time as quantity, kept exact.**

The time paper, part one of two. Chronos is sequential, measured, divisible time — the ruler,
not the moment. Its companion **[KAIROS](https://github.com/cypherpunk4096/kairos)** names the
moment; chronos measures the distance between moments. **Bit time** is its native unit: time
indexed in binary — bits, blocks, and doublings — so the clock and the machine share one
arithmetic.

**Built to the [cypherpunk4096 standard](https://github.com/cypherpunk4096/standard)** (2¹²) —
determinism · zero dependencies · green-checkmark verification · precision · quantum compliance.

[![License: CC0](https://img.shields.io/badge/license-CC0-000000?style=for-the-badge)](LICENSE)
[![Standard: cypherpunk4096](https://img.shields.io/badge/standard-cypherpunk4096-000000?style=for-the-badge)](https://github.com/cypherpunk4096/standard)
[![Inherits: binaryclock](https://img.shields.io/badge/inherits-binaryclock-2ea44f?style=for-the-badge)](https://github.com/Professor-Codephreak/binaryclock)
[![Companion: kairos](https://img.shields.io/badge/companion-kairos-627EEA?style=for-the-badge)](https://github.com/cypherpunk4096/kairos)
[![Precision: 18 decimals](https://img.shields.io/badge/blocktime-18%20decimals-F7931A?style=for-the-badge)](KAIROS-CHRONOS.md)

</div>

---

> **STATUS: SKELETON.** The structure and commitments are laid; the full paper is being written.
> Everything stated here is already implemented somewhere named below — this repository is the
> place where those implementations are read as one idea.

## Lineage — chronos inherits from cypherpunk2048, and from binaryclock

Like [the standard](https://github.com/cypherpunk4096/standard) itself, this paper stands on
**[cypherpunk2048](https://github.com/cypherpunk2048)** (2¹¹) — write code · sovereignty over
custody · power-of-two discipline · verification over trust. Its clock discipline evolved in
the open: cypherpunk2048's BTC Standard begat BANKON, and BANKON's **₿TC.oracle is the
evolution** of that line — the point where "verification over trust" was applied to *time
itself* (measured blocktime, 18-dp exactness, anti-clockblock quorums). What the 2048 mark
demanded of money, the oracle demands of the clock; chronos writes that demand down.

## Lineage — chronos inherits from binaryclock

Chronos stands on **[Professor-Codephreak/binaryclock](https://github.com/Professor-Codephreak/binaryclock)**
— the working proof that time belongs in bits: a dependency-free 3D binary clock (live at
[binaryclock.pythai.net](https://binaryclock.pythai.net/)) that reads wall time as 8-bit bytes and
BCD nibbles, ticks **drift-free** by scheduling against the true next second instead of a naive
interval, and already keeps a chain clock (Ethereum blocktime) beside the wall clock. Every
commitment below is that clock's discipline, generalized:

- **time rendered in binary** → bit time as the native unit, not a novelty display;
- **drift-free ticking** → the interval is measured, never accumulated;
- **no dependencies, no build** → the clock must be auditable in one sitting;
- **the chain beside the wall** → blocktime is a first-class clock, not an add-on.

The same clock's alarm/countdown side — the timesignal becoming an **event** — is inherited by
the companion paper: [KAIROS](https://github.com/cypherpunk4096/kairos). One ancestor, two
inheritances: binaryclock's *display* begets chronos; its *alarm* begets kairos.

## Bit time

Time indexed in binary, at three scales:

| Scale | The tick | Kept by |
|-------|----------|---------|
| the second | wall time as bytes/nibbles — the binaryclock display | [binaryclock](https://github.com/Professor-Codephreak/binaryclock) |
| the block (₿) | one block = one tick of chain time; ~600 s on Bitcoin, measured never assumed | **₿TC.oracle** ([BANKONBTCWaaS](https://github.com/cypherpunk4096/BANKONBTCWaaS)) |
| the block (Ξ) | Ethereum's ~12 s slot tick — already kept beside the wall clock in binaryclock | **ETH.oracle** |
| every chain | the expansion: one clock per chain, read together | [deltaverse.pythai.net/chainmarketcap.html](https://deltaverse.pythai.net/chainmarketcap.html) |
| the doubling | one doubling of storage = one bit of Moore's-law time — "the age of the terabyte" as a readable timestamp | chronos.oracle, measure supplied by kronos.agent (per [the standard](https://github.com/cypherpunk4096/standard)) |

## The keepers — who holds chronos where

**₿TC.oracle keeps Bitcoin.** In [BANKON BTC WaaS](https://github.com/cypherpunk4096/BANKONBTCWaaS),
the ₿TC.oracle's **clockblock** face is chronos in production, and this paper adopts its concepts
whole:

- **clockblock** — wall-time as the ruler: block intervals, the all-time average, the 2016-block
  window. Chronos is the *measuring* face of the clock.
- **exactness** — every blocktime figure is exact Decimal arithmetic at 18 dp
  (`0.000000000000000001`); no float drift, ever. An interval that could be exact must be.
- **anti-clockblock** — *never trust one clock*: tip height and time are cross-checked across
  independent sources (cache · log tail · direct node query · a second node), and displayed
  averages are checked against the local measurement history (σ). A chronos with one source is
  an opinion.
- **measured, not estimated** — the oracle publishes what it measured, marks what it inferred,
  and refuses to dress an estimate as a reading.

**ETH.oracle keeps Ethereum — and the expansion keeps them all.** The same discipline applied to
Ethereum's slot clock, then widened: at
[deltaverse.pythai.net/chainmarketcap.html](https://deltaverse.pythai.net/chainmarketcap.html)
every chain's clock is read side by side — the many-clocks view that makes **clocktime** possible.

## Clocktime — accuracy proven from many chains

Two laws, one derived value:

1. **Proximity is probability.** On a single chain, nearness to the average blocktime makes the
   next tick a **probable event** — a Poisson clock tells you *roughly when*, never exactly. One
   chain's time is a probability, not a reading.
2. **The ensemble is the proof.** Read **multiple** independent blockchains together and the
   probable events cross-check each other: disagreement is bounded, drift is exposed, and an
   accurate time value emerges from the quorum — the anti-clockblock rule promoted from many
   *sources* of one chain to many *chains*.

The derived value is **clocktime**: time that is **accurate and proven accurate** — its accuracy
is not asserted by any keeper but demonstrated by the agreement of independent chains, each of
which anyone can verify. Clocktime is what the expansion at
[chainmarketcap](https://deltaverse.pythai.net/chainmarketcap.html) exists to compute.

**LIQlocker spends chronos.** The ownerless timelock
([cypherpunk4096/liqlocker](https://github.com/cypherpunk4096/liqlocker)) is *chronos-measured*:
its distance to the year 4096 is denominated in measured blocktime — and its release is a kairos.
That handoff — chronos measures the wait, kairos names the release — is the seam between the two
papers.

## The paper

**[CHRONOS.md](CHRONOS.md)** — skeleton laid, sections being written:
Ⅰ the two Greek times · Ⅱ bit time · Ⅲ the drift-free tick (binaryclock inheritance) ·
Ⅳ blocktime as chronos (₿TC.oracle · ETH.oracle) · Ⅴ clocktime — accuracy proven from many
chains · Ⅴb the EIP/ERC time mapping (dated) · Ⅵ the Moore's-law measure · Ⅶ anti-clockblock ·
Ⅷ the seam with kairos.

---

*χρόνος measures the flow · καιρός names the strike — [the other half](https://github.com/cypherpunk4096/kairos).*
*cypherpunk4096 standard.*
