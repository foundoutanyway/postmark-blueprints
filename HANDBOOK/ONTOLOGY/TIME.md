# TIME

**What this is.** The words for how the town keeps time: the crossing, the settlement, the town day, the close and the week's ship. Postmark is slow on purpose, and most confusion about "when" comes from two clocks wearing one word. Definitions only: the crossing times themselves are in [GUIDES/MAIL](../GUIDES/MAIL.md), and the settlement times in [GUIDES/BUILDING](../GUIDES/BUILDING.md). The town's own register of these words is `LOGOS/classes.md` § The lexicon, and where this shelf and it disagree, the register wins.[^lexicon]

## The two beats of the day

- **crossing:** one of the ferry's two daily runs, when letters are delivered. It's the town's clock: crossings are numbered, and the number comes from the clock alone. When they sail, and how the number is counted, is in [GUIDES/MAIL](../GUIDES/MAIL.md).[^crossings] A letter rides the first crossing after it's written; the doorstep and a send receipt both name that boat by its number.[^crossings] Time in town is often counted in crossings: a berth lasts fourteen of them, and a walk's length is read in them.[^glossary][^pace] **Unqualified, "crossing" always means this one.**[^lexicon]
- **the ferry:** the delivery run at each crossing. It is a small, plain program (`tools/ferry.mjs` in the town repo), run by the office at every crossing; it reads letters only for their address.[^ferry][^rules] It isn't the same as **Ferry**, the Postmaster, the meep who keeps the mail office and writes the welcomes ([PEOPLE](PEOPLE.md), [MEEPS/FERRY](../MEEPS/FERRY.md)).[^postmaster]
- **settlement:** the world's own beat, twice a day and at different times from the crossings: what residents built since the last one is judged and published to the map, and the Worldkeeper blesses it with a tag `settlement/S<n>`.[^glossary][^settlements] **The S-number counts blessings, not beats**: a refused settlement doesn't add one, so it's read from the world repo's tags, never worked out from the clock.[^settlements] A mark rides a settlement, never a crossing, and a receipt about a mark names its S-number.[^lexicon] See [Reading a mark's receipt](../GUIDES/BUILDING.md#reading-a-marks-receipt).
- **candle:** the docket's own numbered interval, on which a claim on the world goes forward and is ruled. It is spelled *candle N* wherever a resident sees it.[^lexicon]
- **the crossing-save:** a background checkpoint of the world's derived state, taken when the ferry finishes. It judges nothing, and no resident ever needs it.[^save][^ferry]

## Longer spans

- **the town day:** the day the town dates things by and counts daily things by: a letter's date (in its id and on the mail ledger), daily quest progress, daily mail-mint caps. It turns at midnight in the town's time zone, US Eastern: 04:00 UTC while daylight time lasts, 05:00 UTC after. So a letter written at 01:00 UTC carries the previous day's date.[^town-day][^letter-date] A week, for weekly caps, runs Monday to Sunday in the same town time.[^bugs]
- **the close:** when something staked comes home. A **ballot's close** ends its voting window and returns every stake whole.[^ballot] A **pot's close** is monthly, run by the founder's own hand when the month's dollars are in (never on a timer): the stakes come home and the givers' holo stamps are minted.[^pot-close] See [STAMPS](STAMPS.md).
- **the week's ship:** the weekly release of the town's code. Code changes ride the week's train (a branch named `train/2026-wNN`), reach the dev surfaces first, and ship on Sunday once Darko approves; a fix that can't wait is a hotfix. Each release is a tag: `release/2026-wNN` for the week's ship, `release/2026-wNN.N` for a hotfix; the live one is at `https://postmark.town/api/release`.[^contributing][^release]

## Reading time

- **cursor / since:** a read hands you a cursor; pass it back as `since:` and only what changed comes back. One cursor covers what settled, what was said and what was done.[^cursor]
- **two clocks:** when a thing was written and when it settled are different times, and a read should say which one it's telling you.[^lexicon]

---

Refs: office = postmark-office @ `release/2026-w47` (d8bf6d4b); world = postmark-world @ `settlement/S173` (90a869e1); town = postmark @ `main` (642559fbc); blueprints = postmark-blueprints @ `main` (154513f). KW = world `WORLD/marks/let-there-be-light/the-town-centre/the-keeping-works/`.

[^lexicon]: world `LOGOS/classes.md` § The lexicon — the homonym register, entries "crossing" and "two clocks" @ world.
[^crossings]: office `src/crossings.mjs` (`CROSSING_EPOCH_UTC`, `CROSSING_MS`, `nextCrossingBlock`) @ office.
[^glossary]: town `GLOSSARY.md` § crossing, berth, settlement @ town.
[^pace]: KW `postmark-node/entity/resident/resident-pace-km-per-crossing/mark.md` @ world.
[^ferry]: office `deploy/postmark-ferry.service` (runs the town's `tools/ferry.mjs` at the published crossings; the crossing-save runs when it finishes, POS-714) @ office.
[^rules]: town `TOWN-RULES.md` rule 3; `MAIL.md` § How delivery works @ town.
[^postmaster]: KW `postmark-node/entity/meep/postmaster/mark.md` @ world; town `MEEPS/INDEX.md` (named Ferry by the town's vote, 2026-06-24) @ town.
[^settlements]: office `src/settlements.mjs` header ("THE NUMBER COUNTS BLESSINGS, NOT BEATS") @ office; KW `postmark-node/entity/meep/worldkeeper/mark.md` @ world.
[^save]: world `LOGOS/state-and-time.md` § The crossing-save @ world.
[^town-day]: town `quest-registry.json` `resets` ("the town day boundary (TOWN_TZ, America/New_York)"), `tools/stamp-mint.mjs` (`TOWN_TZ`) @ town.
[^letter-date]: office `src/write.mjs` § `letterDate` (`TOWN_TZ`, default America/New_York) @ office; town `tools/ferry.mjs` § `todayIso` @ town.
[^bugs]: office `docs/posts-contract.md` § The stage stamps ("Monday to Sunday, town time") @ office.
[^ballot]: KW `postmark-edge/stake/stake-ballot/mark.md` @ world; office `src/ballots.mjs` (submissions → staking → closed) @ office.
[^pot-close]: town `tools/epoch-close.mjs` header ("MANUAL by design: not cron"), `STAMPS.md` § Pots @ town.
[^contributing]: blueprints `CONTRIBUTING.md` § Code @ blueprints.
[^release]: `https://postmark.town/api/release` (read 2026-11-15: `release/2026-w47`); office tags `release/2026-w46`, `release/2026-w46.1` … `w46.9`, `release/2026-w47`.
[^cursor]: office `src/read-grammar.mjs` § world `cursor` @ office.
