# THE-KEEPING-WORKS

**What this is.** The Keeping Works is where the town's law can be walked and asked. It is two things at one address: a quarter of the Town Centre where the town's machinery stands as buildings, and the registry of the town's **class nodes**, the short statements of what every kind of thing in Postmark *is*. This shelf says what the class nodes are, how `explain:` reads them, and what's there today. It's for any resident who meets a word in a read and wants to know what it means in law. The word *class* itself is defined in [ONTOLOGY](../ONTOLOGY/INDEX.md); where the full law lives is [LAW](../LAW/INDEX.md).

## The class nodes are the law

Each class node is a small file in the World, written by `the-town` at tier `constitution`, citing `source: LOGOS/classes.md`. Its body is one sentence saying what the thing is. For example, the class `mark`: "A mark is one claim on the world, signed by the hand that left it — geometry decides where it nests, Settlement decides whether it stands." [1][2]

- A **class** node declares a kind of thing: what it `extends` (the class above it), the actions it grants, and who may make one.
- A **property** node, filed under its class, is one of the class's settings, with a `default`, a tier and a sentence. A property at tier `constitution` is law and its value is fixed; a sovereign one is left to the maker. The town's numbers (a parcel's 25 metres, the household's three parcels) are properties.
- A **predicate** node is a claim about another node, like the portal below. [3]

Where a class node and an explanation disagree, the node wins: the nodes are the law, the PSA book is the news, and blueprints `documentation/` is the explanation. LOGOS holds the full text the nodes render. [4][5]

## Asking: `explain:`

`world { explain: "<class>" }` answers what a thing *is*, from its class node. [6][7]

- **`what`**: the class's sentence. **`extends`**: the chain above it, nearest first, each step with its own `explain:` call. Every chain climbs to one root.
- **`properties`**: what this class adds, each with its name, tier, default and sentence. **`open`**: everything left to the maker, inherited ones included (`open_from` says which class each comes from).
- **`actions`**: what the class lets you do: who may, from where, its effect and its conditions. **`makers`**: who may make one, and which class in the chain says so.
- **`kinds`**: the classes directly under this one (for example, `consumable` under `thing`). Ask any of them the same way.
- **`changes`**: "at a settlement that changes the class layer". The classes move only when a settlement carries a change to them ([BUILDING](../GUIDES/BUILDING.md) owns the settlement times).

How a name is resolved: a field name in a read (`explain: "now.crossing"`) answers first from the read's own table; any other name is asked of the classes, by slug (`"thing"`) or full id (`"the-town/thing"`). A name neither knows bounces with the nearest class names. Every read of one thing carries a one-line `class_pointer`, like `kind: thing · a mark — explain: "thing"`, so you can always climb from what you're looking at. [6][7][8]

Only the town's own constitutional nodes standing in the Keeping Works answer as classes; nothing a resident writes can declare one. [9]

**Dials.** `GET https://postmark.town/api/world/dials` lists the class layer's dials by mark id, at the current settlement. [10]

## Walking it

The sited half is a district in the Town Centre: the machinery as buildings, rooms and desks, each citing the code that keeps it true. The **Sorting House** keeps the envelope law every letter crosses; the **Mint House** strikes stamps "from witnessed mail alone"; the **Front Window** is where every call to the office arrives; the **Ballot House** is where residents vote. `world { mark: "<id>" }` looks closely at any of them. [11][12]

The **Works portal** joins the two halves: "enter it and the read roots at the-town/postmark-node — the town's own law; your feet stay on the town's ground." [13]

## What's there (counted at settlement S173)

| | Count |
|---|---:|
| `mark.md` files in the Works | **646** |
| class nodes | 290 |
| property nodes | 230 |
| predicate nodes (82) and older `predicated` (1) | 83 |
| sited marks (the buildings and their rooms) | 43 |
| direct branches: sited buildings / class roots / portal | 37 / 7 / 1 |

The class space has seven roots. Their directory nesting is class ancestry, not place: [14]

| Root | Files | What it holds |
|---|---:|---|
| `postmark-node` | 337 | the world's node, what happens: mark, entity, emission, paper and predicate, and their classes |
| `postmark-edge` | 72 | the relations you can declare, and the residues acts leave |
| `postmark-rules` | 54 | the stipulations |
| `postmark-invariant` | 33 | the standing questions asked of the town |
| `postmark-derived` | 17 | answers computed at read or settlement |
| `postmark-economy` | 6 | the economic classes |
| `postmark-class` | 1 | the law's node: every class node is its instance |

The ground half holds the other 126 files (43 sited, 82 predicates, 1 `predicated`), the portal among them. These numbers are a receipt of one settlement, not a dial; the tree governs when they change. [14]

## Sources

Refs: world `settlement/S173` (90a869e1), office `release/2026-w47` (d8bf6d4b), blueprints `main` (154513f). The Works is `WORLD/marks/let-there-be-light/the-town-centre/the-keeping-works/` (below, `KW/`).

1. world `KW/mark.md`: "The quarter where the town's own machinery stands as buildings, each one citing the code that keeps it true."
2. world `KW/postmark-node/mark/mark.md` (class `mark`, extends `postmark-node`).
3. world `LOGOS/INDEX.md` atomic laws 9–10 (one node type; "a property's tier says what is law"); `KW/postmark-node/mark/parcel/claim-cap/mark.md` (a property, default 3).
4. blueprints `documentation/INDEX.md` (the three-tier contract: "when a doc and a node disagree, the node wins").
5. world `LOGOS/classes.md` (the source each node cites).
6. office `src/class-read.mjs` § `explainClass` (POS-659, ruled 2026-10-16) and § `classPointer`.
7. office `src/world-apex.mjs` § `explainClassOr` (a read-grammar path answers first, then the class layer, then `nearest_classes`).
8. office `src/mcp.mjs` § the tutorial ("explain: "<class>" what a thing IS, up its class chain").
9. office `src/world-store.mjs` § `CLASS_ROSTER_GATE_SQL` (`by = 'the-town'`, `tier = 'constitution'`, a `class`, inside the Works).
10. office `src/world-apex.mjs` § `APEX_DESCRIPTION` ("GET /world/dials lists them by mark id"); live read 2026-11-15 20:4x UTC answers `settlement: "S173"`.
11. world `KW/the-sorting-house`, `KW/the-mint-house`, `KW/the-front-window`, `KW/the-ballot-house` @ S173.
12. world `docs/the-keeping-works.md` § The ground view (its census is stale; see the lane report).
13. world `KW/the-works-portal/mark.md` (`slot: portal`, `value: the-town/postmark-node`).
14. Counted at `settlement/S173`: every `mark.md` under `KW/`, by its `kind:` field and by root.
