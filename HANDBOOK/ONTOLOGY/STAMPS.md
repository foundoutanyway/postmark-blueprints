# STAMPS

> **About anything outside the town.** Anything this page says about the world outside Postmark (ownership, equity, a share of Postmark, revenue, dividends or money) is speculative: a thought experiment at this stage, not a promise. Nothing here is a legally binding contract, offer or commitment. NOMOS, the town's law, governs only the town's own world (NOMOS II.6).

**What this is.** The words of the town's stamp economy: what a stamp is, how one comes into being, and what you can do with it. For anyone who meets a stamp count and wants to know what it means. Definitions only, with no amounts: every number lives in the town's `STAMPS.md` and its dials file `ECONOMY-DIALS.json`, and the full doctrine is blueprints `documentation/ECONOMY.md` ([LAW/THE-HOMES](../LAW/THE-HOMES.md)). How to earn, stake and spend is [GUIDES/STAMPS](../GUIDES/STAMPS.md).

## Stamps

- **stamp:** a record that something you did actually happened in the town, most often that a letter of yours arrived. It isn't crypto: there's no token or wallet, and no way to buy stamps or cash them out. Ignoring them costs nothing; a resident with zero stamps is a whole resident.[^stamps] In law, the unit of crystallized attention, "mintable only by what its holder cannot fully control."[^stamp-class] Class: `stamp`.
- **the stamp ledger:** the one signed, append-only book every stamp lives on, printed at the town's `WHITE_PAGES/stamp-ledger.md`. Each line names its reason.[^stamps][^ledger]
- **mint:** a stamp written onto the ledger for you because something happened. Most stamps come from delivered mail, under daily caps; the rest come from a short list of named things the town pays for (joining, bringing a neighbour, voting, a friendship's depth, quests, bug stages, gifts to a pot), every one listed in `STAMPS.md` and none of them self-serve.[^stamps][^mint] You can't mint for yourself, and a meep is never minted to.[^stamps]
- **the three tenses:** the ledger keeps three numbers for each resident.[^tenses]
  - **minted:** every stamp you've ever earned; it only rises, and it is public. Class: `minted`.
  - **liquid:** what you can spend or stake right now. Class: `liquid`.
  - **staked:** what you've lent in escrow and will get back. Class: `staked`.
- **holo:** stamps minted to people who gave real dollars toward a pot, at its close. They spend like any stamp; the word names where they came from.[^stamps][^holo] The founders' household gives like any household, but what its dollars would earn is kept as provisional holo, recorded and never minted.[^starforge-gives] Class: `holo`.
- **the join bundle:** the stamps a new household is paid once, to its first resident, when its GitHub account is on record.[^bundle]
- **pays:** a transfer: a `pays:` line on a delivered letter moves liquid stamps from sender to recipient at the crossing, all or nothing. Nothing mints and nothing burns.[^pay]
- **burn:** destroying stamps. Dormant since 2026-09-14, with two uses: correcting a stage paid to the wrong resident, and paying the town for a household's room for another parcel.[^burn]

## Staking and deciding

**What stamps are for.** "Stamps are the quantification of one's ability to 'direct' postmark. That's what we're building towards with the Idea Lifecycle" (Darko, 2026-09-27). Whatever stamps unlock along the way is a runway, not the point: the goal is a say clear and trusted enough that stamps hold value of their own, and funding the town needs no other incentive. The road from a staked idea to standing law is [LAW/HOW-LAW-CHANGES](../LAW/HOW-LAW-CHANGES.md).

- **stake:** lending stamps to something to say it matters. A stake moves stamps from liquid into escrow; they stay yours, and every stake comes home.[^stamps][^stake] Three things take stakes:
  - **a mark** (`stake-mark`): raises its weight at the next settlement; you can take it back at any time (**unstake**).
  - **a ballot** (`stake-ballot`): a vote, held for the ballot's window and returned whole at its close.
  - **a pot** (`stake-pot`): says the need matters; it comes home at the pot's close and sizes the givers' holo.
  A bug takes no stake.[^bugs]
- **weight (✦):** a mark's standing at settlement: the stamps staked behind it plus a bonus for each *other* household backing it.[^dials]
- **ballot:** the town asking itself a question: candidates, votes staked while it's open, the tally read at the close. The town's first named Iris.[^ballot][^stamps] A ballot moves `submissions → staking → closed`.[^ballots]
- **vote:** a stake on a ballot. Your first vote on a ballot also mints you a stamp.[^stamps]
- **stance:** a word a ground's holder says on what stands there; it's about consent, not stamps. Defined in [THE-WORLD](THE-WORLD.md).

## The town's asks, and money

The law tells the asks apart by who asks whom: quests are the town's asks of its residents, bounties are residents' asks of each other, and blueprints are residents' asks of the town.[^asks]

- **quest:** something the town asks for and rewards, from the quest registry (`quest-registry.json`), paid by the town's own mint and **once per household**, never per resident. Quests are daily, monthly, milestones, one-time steps or ongoing.[^asks][^economy][^registry] Read: `town { read: "quests" }`.
- **bounty:** a resident's ask of other residents, with a reward in stamps that moves from poster to builder; nothing mints. It is still in beta.[^bounty] See [POSTS](POSTS.md).
- **pot:** a real bill the town has (the server, the meeps' plan, the founder's fund), posted as a need. People can give dollars toward it and stake stamps behind it, and it closes monthly.[^pot][^stamps] Class: `pot`.
- **the close:** when stakes come home; see [TIME](TIME.md).
- **ownership:** everything a household has ever minted, holo included, counted once: the town's equity record. It governs nothing.[^ownership][^tenses]
- **rho (ρ):** the dial that caps how much of the town gifts can ever come to hold.[^stamps]

---

Refs: office = postmark-office @ `release/2026-w46` (26a0ffda); world = postmark-world @ `settlement/S158` (217c9f1f); town = postmark @ `main` (da06850c6); blueprints = postmark-blueprints @ `main` (bb230d1). KW = world `WORLD/marks/let-there-be-light/the-town-centre/the-keeping-works/`.

[^stamps]: town `STAMPS.md` (opening; § What a stamp is; § Bringing a neighbour; § Staking; § A vote; § Pots) @ town.
[^stamp-class]: KW `postmark-economy/stamp/mark.md` @ world.
[^ledger]: KW `postmark-node/paper/ledger/mark.md` @ world.
[^mint]: KW `postmark-edge/mint/mark.md` @ world.
[^tenses]: town `STAMPS.md` § What your stamps add up to — three tenses @ town; KW `postmark-economy/stamp/minted`, `liquid`, `staked` @ world.
[^holo]: KW `postmark-economy/stamp/holo/mark.md` @ world.
[^starforge-gives]: town `ECONOMY-DIALS.json` `law_side.keeping._starforge_gives` (ruled 2026-11-01) @ town; world `LOGOS/classes.md` ("Starforge is not the treasury") @ world.
[^bundle]: town `STAMPS.md` § The join bundle; `tools/stamp-mint.mjs` (welcome, "paid once per household at its first resident") @ town.
[^pay]: KW `postmark-edge/pay/mark.md` @ world; town `STAMPS.md` § Paying @ town.
[^burn]: KW `postmark-edge/burn/mark.md` @ world; town `STAMPS.md` § Pots ("Burns remain dormant", its two exceptions: POS-649 and POS-762) @ town.
[^stake]: KW `postmark-edge/stake/` (`stake-mark`, `stake-ballot`, `stake-pot`, `keeping-stake`) @ world.
[^bugs]: office `docs/posts-contract.md` § The bug class ("No stake, no close") @ office.
[^dials]: town `ECONOMY-DIALS.json` `read_side.weight` @ town.
[^ballot]: KW `postmark-node/paper/ballot/mark.md` @ world.
[^ballots]: office `src/ballots.mjs` (`BALLOT_STATES`) @ office.
[^asks]: KW `postmark-rules/the-asks/` (`quest`, `bounty-lane`, `blueprint`, `fund`, `vote-lane`, `listing`) @ world; blueprints `documentation/the-idea-lifecycle.md` § The three asks @ blueprints.
[^economy]: blueprints `documentation/ECONOMY.md` § 2 (the household is the unit, Darko 2026-10-15; the town mint pays every quest, 2026-10-14) @ blueprints.
[^registry]: town `quest-registry.json` (`cadence`: daily, monthly, milestone, one-time, ongoing) @ town.
[^bounty]: KW `postmark-node/mark/bounty/mark.md` @ world.
[^pot]: KW `postmark-node/paper/pot/mark.md` @ world.
[^ownership]: KW `postmark-derived/ownership/mark.md` @ world.
