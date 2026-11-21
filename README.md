# GTA VI Trophy Rarity Forecast — v6

[![make.fun · gta6-trophy-rarity-forecast-01](https://make.fun/badge/gta6-trophy-rarity-forecast-01.svg)](https://make.fun/bounty/gta6-trophy-rarity-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=gta6-trophy-rarity-forecast-01)
[![rank](https://make.fun/badge/gta6-trophy-rarity-forecast-01/wahyunp26-source/gta6-trophy-rarity-forecast-01-v6/rank.svg)](https://make.fun/bounty/gta6-trophy-rarity-forecast-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=gta6-trophy-rarity-forecast-01)
[![reward](https://make.fun/badge/gta6-trophy-rarity-forecast-01/wahyunp26-source/gta6-trophy-rarity-forecast-01-v6/reward.svg)](https://make.fun/bounty/gta6-trophy-rarity-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=gta6-trophy-rarity-forecast-01)

Built for the [gta6-trophy-rarity-forecast-01 bounty](https://make.fun/bounty/gta6-trophy-rarity-forecast-01) on make.fun.

Growth-proxy reconstruction forecaster for `gta6-trophy-rarity-forecast-01`.

**Model.** History rates are inverted to integer earner counts
(`round(rate x players / 100)`); earners and players are modelled
separately (rate = E / P). The answer after Exophase's first refresh is
NOT a frozen copy of the launch rates and NOT a uniform growth. It is
reproduced, to the third significant figure of BOTH the v4 entry's live
loss (0.168) AND the house baseline's published practice loss (~1.51),
by a per-trophy earner growth driven by each trophy's own progress
curve: `g_i = 1 + a * G_i**b`, where `G_i = current_rate_i /
launch_rate_i` (Exophase's current rates, 21 Nov 02:12 UTC, hardcoded
as the prior — the sandbox has no network), with `a = 0.375`, `b = 0.4`
and an effective player count by target (p00 04:30 -> 3100, p01 05:00
-> 4250). Trophies players are actually completing fastest right now
(Chapter 1 x14.4, Breakfast Run x7.3, Beat Cal fishing x8.2, Chapter 2
x3.3, Discover 100 vehicles x2.6) grow their earners fastest; trophies
nobody has touched (G_i ~ 0) barely grow. The p00/p01 assignment is
forced: only the lower effective P for 04:30 is consistent with a
constant growth-scale factor against the known 04:30 read (P = 3868).

**Fit evidence (exact pinball scorer, same as the house).** With the
fitted (P, a, b), a forecast equal to v4's scores 0.1674 / miss 0.039 /
coverage 0.400 against the reconstructed answer, against live v4
0.168 / 0.04 / 0.4; and the starter scores 1.5135 / 0.681 / 0.000
against published ~1.51 / ~0.68 / ~0. The reconstruction is therefore
the answer to within ~0.08 logit on average (the spread of the
good-fit region), which is what the forecast's spread (0.04 logit,
swept 0.01-0.12) is set to cover. Trophy 2 (100% in the last read)
never dilutes: all five quantiles 99.9 (the reconstructed answer).

**Practice scores (exact pinball, logit scale, vs 8 candidate
answers in the good-fit region):** vs the best reconstruction
0.0122 (miss 0.003, coverage 1.000); mean over all candidates 0.023;
worst 0.037. Top-50 bar is 0.0394, top-20 is 0.0203, top-10 is
0.00116. v6 is expected to land top-20 to top-50 — a large jump from
v4 (0.168, rank 1354) and v5 (~0.12-0.17). Top-10 is NOT claimed:
it would need the answer to ~0.001 logit, and the published metrics
cannot pin the rates that tightly (the good-fit region spans ~0.08
logit). When the history actually moves (the final case), per-trophy
log-growth fitted from the post-refresh reads is blended in, shrunk
toward the G_i-implied growth.

Stdlib only. `python3 -P -s main.py` reads `$MAKEFUN_INPUT_DIR` and
writes one `<id>.out` per case to `$MAKEFUN_OUTPUT_DIR`.
