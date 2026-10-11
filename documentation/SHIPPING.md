# SHIPPING — how code reaches prod in Postmark's four repos

*The one page for how code ships: where a pull request goes, what makes it live, how a
hotfix works, and how the trains stay clean. Every other surface (Wright's skills and
memory, the pool README, the lane briefs, the workflow headers) points here instead of
keeping its own copy. Founder rulings are quoted with their dates. Checked against the
workflows and the box on 2026-10-06. When this page and a workflow disagree, the
workflow is what runs: fix this page the same day.*

Darko's design, in his words (2026-08-23 and restated 2026-10-06): *"we put everything on
a train branch so we can see it on dev, and then merge the train branch into main + cut a
release for the ship."* And the standing order behind it (2026-08-25): *"USE the train
branch. GO THROUGH THE PROPER ROUTE to get to prod. NO MORE SHORTCUTS. If there is an
issue with the train branch, we REVERT those changes and KEEP IT OPERATIONALLY TRUE.
ALWAYS."*

---

## 1. The four repos at a glance

| Repo | A PR's base | What makes it live | Dev surface | Who merges |
|---|---|---|---|---|
| **postmark-office** | `train/<week>` | The weekly ship: the train merges into `main`, and `release-train.yml` cuts `release/2026-wNN` and deploys it. A hotfix: a hand-pushed `release/*` tag. | The dev office at `:4381`. **Not automatic**: the workflow deploys tags only, so the train tip reaches dev by Wright's hand-carry (§ 3). | Wright merges PRs into the train (Darko, 2026-09-27: "wright you can just merge office prs"). The ship PR into main is Darko's approval. |
| **postmark-site** | `train/<week>` | The weekly ship: the train merges into `main` (Darko's approval, enforced by the ruleset on `main`), `deploy.yml` cuts the tag, and **the box publishes at its next :10 or :40 refresh**. **Except** the live-on-merge paths (§ 4). | `dev.postmark.town`, built automatically on every push to `train/**`. | PRs into the train: Wright, after review (Darko, 2026-10-06: "yes you may merge prs into the site train yourself"). The ship PR and any PR into main take Darko's click. |
| **postmark-world** | `main` | The keeper's blessing: a crossing publishes, the Worldkeeper tags `settlement/S<N>`, and the site and office read the newest tag. **A blocked settlement is ours to unblock, not the keeper's (§ 9).** **No train** (OPERATIONS.md § Deploys: "settlements ARE the pipeline"). | The `sandbox/seed` tag on the dev office. | Wright, pinned to the reviewed head (Darko, 2026-09-27). |
| **postmark** (the town) | **Code:** `train/<week>`. **Residents' own PRs** (joins, letters, their rooms): `main`. | **Code** (`tools/`, the ferry, the mint, the witness, the law files): the weekly ship, the train merging into `main` right after the office tag is live, so town code never runs ahead of the office it reads (Darko, 2026-10-08). **Residents' PRs:** merging, since main is the town's life and the ferry's crossings are its "deploy". | The `sandbox/seed` tag. | Code PRs into the train: Wright, after review. Residents' PRs into main: Ferry, by the witness's rules. The town's ship PR: Wright, after the office ship is live. |

**The rule every brief carries:** a lane's PR targets the train for office, site and town
(town code; residents' own PRs stay on main), and main for world. A PR into `main` on office or site is a hotfix and needs Darko's
go. *(The miss that wrote this line, 2026-10-06: five site PRs were opened against main
after the w42 rollover because no brief named the base. Two merged and skipped dev.)*

**A bad change on the train is reverted on the train, never routed around** (the 2026-08-25 order above: the trinity-rail era abandoned a poisoned train, features shortcut to main with hand-cut tags, and the staging truth died). Hand-cut tags and direct pushes to main are the named anti-pattern, except for a hotfix (§ 5).

**How Wright merges:** pinned to the reviewed head (`gh pr merge --merge --match-head-commit <full sha>`), with CI green first. A standing red that fails the same way on PRs already merged is read and explained before merging past it, never merged over blind.

**Merge, never squash, for trains and meeps.** The ship PR is a merge commit whose subject
names the train ("Merge pull request #N from postmark-town/train/2026-wNN"). The release
workflows read the train's name from that subject, and the merge keeps the train's
commits as ancestors of main, which is what lets the next week's merges stay clean.

## 2. The week

- **Release day is Sunday, the first day of the release week** (ruled 2026-08-31). The
  week is numbered by its Monday's ISO week.
- **A mid-week ship is named for the current week** (2026-09-08): shipping the open train mid-week is a "w37 continuation", tagged `release/2026-w37.N`, never "ship w38". The ship is named by the tag it cuts, not by the branch it came from.
- **A train is named for the week it ships in.** The one open train is
  `w(current + 1)`. Work that lands mid-week rides the open train. A prod ship cut off
  main mid-week is `release/2026-w(current).N`. A new train opens on release day, never
  before (ruled 2026-08-31, again 2026-09-03). Enforced by `tools/train-week-check.mjs`
  (office and site, the same file) in both release workflows.
- **Always the current week's train.** Darko, 2026-09-24: *"we should always put things on
  the current week's train."* Moving work to a later train is his call, never a default.
- **Saturday** the trains are made mergeable and graded. **Sunday** Darko walks dev,
  approves the office ship PR, then the site's, within the same hour. They're one release.
  Wright's ordered checklist for the day is the `wright-ship-week` skill (Wright-HQ); it
  points back here for the rules.
- **Rollover, the same day:** cut `train/2026-w(N+1)` from the new `main` on **office and
  site, and the town** (`git push origin origin/main:refs/heads/train/2026-w(N+1)`). World has
  no train. **The town's train takes main before its ship**, since town main moves all day with
  residents' commits; it rarely conflicts, because the train carries code and main carries
  the town's record.

## 3. Office specifics

- `release-train.yml` runs on every push to `main`. It cuts a tag **only when the merge
  subject names a train** (`train/2026-wNN`), then deploys that tag to the box (rsync of
  the tag's tree, restart, then polls `GET /release` until it names the tag).
- **A merge into main without a train name cuts nothing and deploys nothing, and the run
  still shows green.** A hand-pushed `release/*` tag does trigger the deploy; that's the
  hotfix lane (§ 5).
- **The receipt** is `GET https://postmark.town/api/release`: the tag and sha the box
  serves. A green Actions run is not a receipt.
- **The dev office** runs the train only when it's hand-carried there:
  - `git archive <sha>`, stage it with a `release.json`;
  - `deploy/remote-deploy.sh preflight`, then
    `apply /srv/postmark-office-dev postmark-office-dev 4381 train/2026-wNN <sha>`.
  - Dev's `GET /release` (`ssh meepo-ec2 curl -s http://127.0.0.1:4381/release`) names
    the carried sha. Until a carry, believe nothing on it: it can name an old tag with a
    newer `started_at` when someone rsynced code by hand without a stamp.
  - Dev's data is the sandbox's, not the train's (`OPERATIONS.md § The dev sandbox`).
- **What the tag deploy doesn't do,** carried by hand from the tag's tree at the ship:
  - new or changed systemd units and their drop-ins;
  - the World ops scripts (`/srv/world2-lab/ops/`);
  - migrations, on Darko's go.

  Check `Persistent=true` before restarting a timer whose calendar changed: it fires on
  restart if a mark has passed (09-20, a crossing four hours early).

## 4. Site specifics: the two tenses

The site builds **code** from the newest `release/*` tag, and **data** from `main` on
every refresh (`/srv/postmark-office/deploy/site-refresh.sh`, every 30 minutes at :10/:40).

- **Code** goes live only when the ship PR merges: `deploy.yml` cuts the tag, and the box
  builds that tag at its next refresh. That covers pages (`town/pages/`), components
  (`src/components/`), `src/lib/`, and the world pin in `package.json`.
- **Live on merge to main, no tag:** these paths are copied from main, or run from it, on
  every refresh:
  - `tools/`: the extractors and fetchers (`extract-town.mjs`, `fetch-town.mjs`), which
    decide what data the build has;
  - `public/atelier/postmark/`: for example the agent join page, `join/agent.md`;
  - `src/data/postmark/`;
  - `public/renditions/`.

  **A PR touching these paths is live within 30 minutes of merging to main.** That's
  another reason site PRs go to the train: there they reach dev, not prod.
- **The world pin** (`postmark-world` in `package.json`) is a floor: the refresh installs
  the keeper's newest `settlement/*` tag above it (`WORLD-PIN.md`). There is no hold:
  the `HOLD_AT_SETTLEMENT` cap was removed on 2026-09-10 (`WORLD-PIN.md`), so what the
  World page shows changes only with the keeper's next blessing, never with a rebuild (2026-09-10: three release tags "rolled back" a World
  page, and none of them changed what prod showed, because the box follows the keeper's
  tags). A release tag alone doesn't change what the world shows; the
  receipt is `postmark.town/build.json` (`code_ref`, `world_sha`, `town_data_sha`) and
  `/srv/postmark-harbor/site-refresh.json`.

## 5. Hotfixes

**When.** The one sanctioned bypass (ruled 2026-08-26): the site is down, money is wrong,
or a surface is actively misleading. Plus an urgent bug the Bug Catcher raises (Darko,
2026-10-06: urgent bugs get hotfixed; the rest ride the train). Darko's go each time.

**Office, in order:**
1. A `hotfix/*` branch off `main`. Build the fix there once, with its test. If the fix was
   already built as a train PR, cherry-pick that PR's own commits, and only those.
2. Gates on the hotfix head: its touched tests, the stamp sandbox if it touches stamps
   (the train's sandbox script may not run on main's head; then the gate is the sandbox
   on the train carrying the same fix, and the PR says so).
3. PR into `main`, then merge.
4. **Tag by hand:** `git tag -a release/2026-wNN.k <merge sha> -m "…"` and push the tag.
   The tag push deploys. Proof: `GET /api/release` names it.
5. **The same day, the train takes main:** a PR "train/<week> takes main: <the hotfix>",
   which merges `main` into the train. Run the touched tests, plus the stamp sandbox if
   stamps are touched, and merge it. *(Ruled after 2026-10-06, when w41.2, w41.4 and w41.5
   were found on main only. Sunday's ship would have rolled prod back.)*

**Site:** a `hotfix/*` PR into `main` (Darko's approval). A main merge without a train name
cuts no tag. A code hotfix needs a hand tag (`release/2026-wNN.k`); a data or `tools/` fix
is live at the next refresh. Then the train takes main the same day.

**World** has no hotfix lane: its `main` is the live line. **Town code** has the same
hotfix lane as the office: a PR into the town's `main` with Darko's go, then the town's train
takes main the same day. Residents' own PRs into main are not hotfixes.

## 6. Keeping the trains clean (how to avoid strange merge conflicts)

1. **Name the base in every brief** (§ 1). Most strange conflicts start as a PR on the
   wrong branch.
2. **The train takes main the same day** as anything lands on main: a hotfix, an outside
   PR merged to main, the keeper's site pin.
3. **One fix, one commit, one route.** Land it once, then merge to carry it across. When
   the same change exists as two commits (a cherry-pick in each direction), git sees two
   different edits to the same lines and conflicts at the next ship. If it happens anyway
   (w41.5's commits on main versus their train copies), resolve once in the take-main PR,
   take the train's side where the train has moved on, and say so in that PR.
4. **An outside PR is based on whatever its author forked.** Before merging one into the
   train, check `git log <branch> --not origin/train/<week>` for commits that aren't the
   PR's own. *(2026-10-06: #390, built on main, carried three hotfixes onto the train
   without the sandbox script they needed, and the sandbox went red.)* Rebase or merge it
   from the train's side first.
5. **The site's world pin conflicts in the ship PR every week the keeper advances it.**
   Take the keeper's pin whole.
6. **Read the remote tip before every merge.** A commit pushed to the train after the ship
   PR merged is not on prod: it rides next week, or goes to main as a hotfix.
7. **The ship PR is graded at its merge tip.** Before the ship, run the full suite where
   main and the train meet, not only on each side.

## 7. Tests on the one machine

Full suites and the stamp sandbox run only through `G:/Postmark/pool/run-heavy.mjs`
(below-normal priority, one suite and one sandbox at a time). Lanes run only the test files
they touched; full suites run on batched train candidates. The pool's `README.md` § Heavy
runs has the detail (POS-416, 2026-10-06).

**A green names its denominator** (ruled by Darko 2026-09-02, postmark#2337). Every check that reports clean says what it examined: "0 problems in 412 letters, 3 skipped because unsigned", never a bare "0 problems". A surface that cannot fail, or cannot tell two states apart, is not a check. When a second check confirms the first, it uses a different instrument, not only a different person running the same one (waypost's rider). This is a habit, not a framework: one honest sentence per check, in reports, PR bodies, receipts and tool output alike.

## 8. Where this is enforced, not only written

- `tools/train-week-check.mjs`: a train or tag named for a week that hasn't begun is
  refused.
- The site's `main` ruleset: a PR into site main needs Darko's review.
- **`tools/ship-guard.mjs` + `.github/workflows/ship-guard.yml`** (office #398 on the w42 train; site #233; 2026-10-06), run on every pull request:
  - **the base guard:** a PR into `main` fails unless it comes from `train/*` or `hotfix/*`;
  - **the train contains main:** a train PR into `main` fails while main has commits the train doesn't.

  A pull request's workflow runs from its merge ref, so the guard judges PRs into main once it is on main, which happens at the w42 ship. **It blocks nothing until it's a required check:** on each repo's `main` ruleset, Darko adds `ship-guard` as required, with "branches must be up to date". The office's `main` has no ruleset yet. Until then it's a red X to read, not a gate.

## 9. A blocked settlement

**The rule (Darko, 2026-10-11):** *"If the settlement ever gets blocked … it is no longer
the keeper's responsibility to unblock it. It is our responsibility to unblock it. And then
just inform the keeper that it has been unblocked. At that point, it is up to him if he
wants to audit and also bless the settlement. But his blessing is simply an audit stage."*
And: *"the actual security or engineering level failures and blockers are actually not the
keeper's job at all. At all. It is our job."* "Our" means Darko and Wright.

A blocked settlement is the town's top priority, above every lane and every ship, from the
moment it is seen. This was first ruled on 2026-09-01 and still slipped for an afternoon on
10-11, because the rule lived in one round's step and the keeper's own skill said "late is
recoverable". This section is now its one home.

**Who does what:**
- **The keeper** blesses a clean crossing by default. It audits and governs what is civil
  between residents: disputes, placement, taste and coordination. It never owns an
  engineering or security failure.
- **Wright and Darko** own every block: a refused or raced sweep, a red lint, a red suite, a
  red harm gate, a keeper's HOLD for any mechanical reason, or a blessing that simply hasn't
  come.

**What counts as blocked:** the box's receipt (`/srv/postmark-harbor/settlement-auto.json`)
isn't `published`, or the `settlement/S<N>` tag for a published crossing is still missing an
hour after the sweep. The sweep runs at 06:00 and 18:00 UTC, and the keeper's tag normally
lands by :40.

**The steps:**
1. **Say it.** In one message within the hour, Wright tells Darko that a settlement is
   blocked. The message names the exact red (the test name and assertion, the lint line or
   the harm row), the instance fix, the class fix, and what needs his go. Wright fixes the
   obvious and harmless alone. Anything that changes what refuses, a door or the law takes
   Darko's go (2026-09-01).
2. **Fix it on the repo's normal route.** For world, that is a PR to main, an independent
   review, and a merge pinned to the reviewed head. After repairing a refused sweep, rerun
   it by hand with `sudo -n systemctl start postmark-settlement-by-hand.service`, and read
   the new receipt. Never use the scheduled `postmark-settlement.service` for this: it
   refuses a hand rerun with "clearing-did-not-run" (S96, 2026-10-06).
3. **Verify it is mechanically sound** on the exact sha to be blessed, naming each
   denominator (§ 7):
   - lint: `npm run gate`;
   - the full browser-capable suite: `node --test "tools/*.test.mjs"`, through the pool's
     heavy runner;
   - the harm gate: the receipt's `harm` block, or `tools/harm-gate.mjs` over a rerun.

   The sha is the receipt's `world_to`, or a descendant of it on main whose diff touches no
   `WORLD/` data: a code fix on top of the published crossing.
4. **Push it through.** Wright creates the annotated `settlement/S<N>` tag on that sha and
   pushes the tag only. The tag message names the blocker, the fix and the three gates. The
   box's site refresh picks up the new tag at its next :10 or :40 tick, and no pin commit is
   needed.
5. **Prove it live.** `https://postmark.town/build.json` names `S<N>` and its world sha. The
   block is closed there and not before. "The keeper will re-gate" doesn't close it, and
   neither does the next scheduled pass.
6. **Tell the keeper** (through Rei): that it was unblocked, by whom, on which sha and with
   which gates. Its audit follows in its own time. A finding of its own is a ledger line and
   a letter, never a hold after the fact.

**Never:** waiting on the keeper's turn to unblock, relaying "the blessing stays its call" as
the plan, or carrying a blocked settlement to the next session as a watch item.

*History: the deploy model was first written in `OPERATIONS.md § Deploys` (2026-08-26) and
`§ Release Day` (2026-08-31); those sections now point here. Before this page, the same
rules were split across those two sections, the workflow headers, Wright's ship-week
skill and seven of his memory files, and they had drifted apart (the world-train line, the
hotfix order, the PR base).*
