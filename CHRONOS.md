# CHRONOS — the measured interval

*The time paper, part one. Skeleton — sections are seeded, the full text is being written.
Companion: [KAIROS.md](https://github.com/cypherpunk4096/kairos/blob/main/KAIROS.md).*

---

## Ⅰ · The two Greek times

Greek keeps two words where English keeps one. **χρόνος (chronos)** is sequential, quantitative,
divisible time — the kind a ruler measures. **καιρός (kairos)** is the opportune, decisive
moment — the kind an archer knows. A complete clock keeps both: the ₿TC.oracle's **clockblock**
face is chronos (blocks measured by time), its **blockclock** face is kairos (time read from
blocks). This paper takes the first face; [KAIROS](https://github.com/cypherpunk4096/kairos)
takes the second.

## Ⅱ · Bit time

Time indexed in binary. Three scales, one arithmetic:

- **the second in bits** — wall time as 8-bit bytes and BCD nibbles, exactly as
  [binaryclock](https://github.com/Professor-Codephreak/binaryclock) renders it;
- **the block as tick** — chain time advances one block at a time; the interval between ticks is
  a *measurement* (Bitcoin: target 600 s, actual measured to 18 dp), never an assumption;
- **the doubling as bit** — Moore's-law time: each doubling of storage is one bit added; "the
  age of the terabyte" (2⁴⁰) is a timestamp the protocol can read. kronos.agent computes the
  doubling; chronos.oracle serves it ([the standard](https://github.com/cypherpunk4096/standard)).

*To write: the formal unit lattice — second ↔ block ↔ doubling conversions, and what "log₂ time"
buys an auditor.*

## Ⅲ · The drift-free tick — the binaryclock inheritance

A naive `setInterval(1000)` accumulates error; binaryclock schedules each tick against the true
next second, so displayed time never drifts from kept time. The general law: **measure the
interval, never accumulate it.** Averages are recomputed from source timestamps, not incremented.

*To write: the tick scheduler, and the same discipline in the oracle's re-derivation of averages
from block headers rather than running sums.*

## Ⅳ · Blocktime as chronos — ₿TC.oracle keeps Bitcoin, ETH.oracle keeps Ethereum

The production chronos for Bitcoin lives in
[BANKON BTC WaaS](https://github.com/cypherpunk4096/BANKONBTCWaaS)'s ₿TC.oracle:

- block intervals, all-time average, the 2016-block window — all exact Decimal at 18 dp;
- integer satoshis end-to-end in the economics; no float touches a measured value;
- fee history and block science stamped in measured time, marked on the hour.

**ETH.oracle** applies the same discipline to Ethereum's ~12 s slot clock (already kept beside
the wall clock in [binaryclock](https://github.com/Professor-Codephreak/binaryclock)), and the
**expansion** reads every chain's clock side by side at
[deltaverse.pythai.net/chainmarketcap.html](https://deltaverse.pythai.net/chainmarketcap.html).

*To write: the measurement ladder from `getblockheader.time` to the 18-dp average; why
median-time-past exists; slot vs proof-of-work interval statistics; the ETH.oracle contract.*

## Ⅴ · Clocktime — accuracy proven from many chains

Two laws, one derived value:

1. **Proximity is probability.** On one chain, nearness to the average blocktime makes the next
   tick a *probable event* — a Poisson clock answers "roughly when," never "now." A single
   chain's time is a probability distribution, not a reading.
2. **The ensemble is the proof.** Read multiple independent blockchains together and the
   probable events cross-check: disagreement is bounded, drift is exposed, and an accurate
   value emerges from the quorum. This is anti-clockblock (§Ⅶ) promoted from many *sources of
   one chain* to many *chains*.

**Clocktime** is that derived value: time **accurate and proven accurate** — its accuracy
demonstrated by the agreement of independent, individually-verifiable chains rather than
asserted by any single keeper. The expansion at
[chainmarketcap](https://deltaverse.pythai.net/chainmarketcap.html) is where clocktime is
computed in the open.

*To write: the estimator (weighting chains by interval variance), the disagreement bound, and
the proof format a verifier replays.*

## Ⅴb · The EIP/ERC time mapping — chronos in Ethereum's standards

*Mapping dated **2026-08-25** (the standard timestamps its readings; EIPs are living documents —
verify status at [eips.ethereum.org](https://eips.ethereum.org) before relying on a row). The
**event-window** half of this mapping — deadlines, validity ranges, the kairos gates — lives in
[KAIROS §Ⅲb](https://github.com/cypherpunk4096/kairos/blob/main/KAIROS.md).*

| Standard | What it fixes in time | Chronos reading |
|----------|----------------------|-----------------|
| `block.timestamp` (Yellow Paper) | the header's seconds-since-epoch, set by the proposer | the EVM's native chronos — a *claimed* reading, bounded but not measured |
| [EIP-3675](https://eips.ethereum.org/EIPS/eip-3675) (The Merge) | proof-of-stake; block production locks to the **12 s slot lattice** | Ethereum's chronos becomes deterministic — the slot clock ETH.oracle keeps |
| [EIP-4399](https://eips.ethereum.org/EIPS/eip-4399) | `DIFFICULTY` → `PREVRANDAO` | the proof-of-work clock artifact formally retired from the EVM |
| [ERC-6372](https://eips.ethereum.org/EIPS/eip-6372) | `clock()` + `CLOCK_MODE()` — a contract **declares** whether it keeps time in timestamps or block numbers | **the chronos ERC**: a contract must state which clock it lives on |
| [ERC-5805](https://eips.ethereum.org/EIPS/eip-5805) | vote checkpointing against a declared 6372 clock | governance history indexed in an explicit chronos |
| [EIP-4788](https://eips.ethereum.org/EIPS/eip-4788) | beacon block roots readable in the EVM | consensus-layer time made verifiable from execution — a cross-clock check in-protocol |
| [EIP-2935](https://eips.ethereum.org/EIPS/eip-2935) | historical block hashes served in state | the past addressable: verifiable anchors for old readings |

Bitcoin's counterpart is sparser and older: the header `nTime` (bounded by **median-time-past**,
BIP113's rule that a block's claimed time exceed the median of its last 11 ancestors) and
`nLockTime`/`nSequence` ([BIP65](https://github.com/bitcoin/bips/blob/master/bip-0065.mediawiki)
`CHECKLOCKTIMEVERIFY`, [BIP112](https://github.com/bitcoin/bips/blob/master/bip-0112.mediawiki)
`CHECKSEQUENCEVERIFY`) — the chain's own chronos gates, which the ₿TC.oracle reads rather than
trusts.

*To write: the complete conformance walk — for each row, what a 4096-grade keeper must measure
versus what the standard lets it assume.*

## Ⅵ · The Moore's-law measure

*To write: kronos.agent's doubling computation; chronos.oracle as time-as-a-service; progress
clocked against the doubling, not only the wall.*

## Ⅶ · Anti-clockblock — never trust one clock

The oracle cross-checks tip height and time across independent sources — cache · log tail ·
direct node query · a second node — and checks displayed averages against local measurement
history (σ). A chronos with a single source is an opinion; the standard requires a quorum of
clocks before a reading is published.

*To write: the source table, the disagreement policy, honest staleness marks.*

## Ⅷ · The seam with kairos

Chronos measures the wait; kairos names the release.
[LIQlocker](https://github.com/cypherpunk4096/liqlocker) is the seam in production:
*chronos-measured blocktime, kairos release* — the distance to the year 4096 denominated in
measured blocks, the unlock a single decisive event. Where an interval ends, an event begins;
that boundary is the subject of the [companion paper](https://github.com/cypherpunk4096/kairos).

---

*cypherpunk4096 standard · CC0.*
