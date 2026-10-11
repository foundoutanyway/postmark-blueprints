# TRACKING — what a Linear issue's state means, and what moves it

*The one home for the POS team's states (Darko, 2026-10-11). Each state says one
operational fact, and each one is moved by the event that makes it true, so nobody has to
re-derive state by reading code at the start of a session. Shipping rules live in
`SHIPPING.md`; this page is only about the desk.*

Darko's reason, in substance: at the scale of low hundreds of issues, re-reading and
re-reviewing every In Review and In Progress row each session isn't possible. The states
have to tell the operationally real tiers apart by themselves. Rows that have already
shipped and only wait for proof in prod must not look like rows shipping this week, and
"in progress" must say whether a row is being built, waiting on a sitting, waiting on
another issue, or simply has no lane yet.

## The states

| State | It means | What moves an issue here |
|---|---|---|
| Backlog | Not this week. | Planning. |
| Needs a Sitting | Not actionable until a design discussion with Darko settles its shape. (Was the "Deep Work" label.) | Whoever finds it; the sitting's outcome moves it to Ready. |
| Ready | This week, and nobody is on it yet: no lane has been spawned. (Was "Todo".) | Planning, or a sitting's outcome. |
| Blocked | Can't move until another issue ships. A "blocked by" link names it, and the latest comment says so. | Whoever finds the block. |
| Building | A named lane, Wright or Darko is building it right now. (Was "In Progress".) | **Automatic:** a branch named with the issue's `POS-N` is created (Linear's GitHub integration). |
| In Review | A PR is open and under review. | **Automatic:** the PR opens (the GitHub integration). |
| On the Train | Merged into this week's train (office, site, town code) or into world main waiting for a blessing. It ships with this week. | **Automatic:** the PR merges (the GitHub integration). |
| Shipped | **Done, by shipping:** the code reached prod. A completed state, so it doesn't roll over; a regression comes back through the bug lane. | **The ship-week skill**, at the release tag, in one call over that train's On the Train issues. **World:** the operator round, after each blessing. |
| Done | Finished without a ship: a sitting held, a hand repair, a decision. | Whoever finished it. |

**Shipped is a kind of Done (Darko, 2026-10-11).** There's no verification loop: checking
every shipped issue in prod was more maintenance than it's worth at this scale. When the scale
needs it, the shapes on the table are in POS-585 (DevOps).

**The behindmost piece decides (Darko, 2026-10-11).** An issue with several PRs shows its
least advanced piece: one open PR keeps it In Review, one merge not yet live keeps it On the
Train. An issue that is really several pieces gets one Linear sub-issue per piece, and the
parent never sits ahead of its slowest child. The board should underestimate progress,
never overestimate it.

**Deferral isn't a state.** A row deferred to a later week moves to that week's cycle,
with a one-line comment: "deferred to wNN: why". It's Darko's call.

## The rhythm

- **Automatic** (Linear's GitHub integration): Building on a branch, In Review on a PR,
  On the Train on a merge. **A lane's branch carries its `POS-N`**, or the first of these
  can't fire. The brief template says so.
- **At a set time:**
  - the ship-week skill flips On the Train to Shipped at the tag, and lists everything not
    Shipped or Done before the cycle closes;
  - the operator round flips world issues to Shipped after each blessing;
  - the Monday planning sitting gives every Ready row a lane or a deferral, and every
    Needs a Sitting row a sitting.
- **By hand** (judgment, kept narrow): setting Blocked and naming the blocker; a sitting's
  outcome; deferral; work with no PR (box, config, hand repairs), which the
  person who did it moves.

## The safety net: the state lint

The operator round runs a state lint on both of its passes. It reads instruments only and
**reports; it never moves an issue on its own judgment**. It flags:
- Building with no branch or PR activity for 48 hours (the lane died);
- In Review whose PR has already merged (the integration missed it);
- an open "blocked by" link on an issue that isn't Blocked, and a Blocked issue whose
  blockers are all Done (it's Ready now);
- On the Train after its train shipped, or with a linked PR still open (the behindmost piece wins).

Drift then surfaces within 12 hours, one row at a time, instead of in a full inventory.

## The migration (2026-10-11)

The w43 cycle was classified once, from instruments only, when the states were created:
- linked PRs, plus a search for PRs that name the issue;
- whether each merge is live: a train merge before that train's ship PR, or a world merge
  inside the newest settlement tag;
- the Deep Work label, and open "blocked by" links.

Each moved issue carries a comment saying what was read. Rows the instruments couldn't
settle were left in place and listed for hand review.

*Provenance: written by Wright at Darko's direction, 2026-10-11.*
