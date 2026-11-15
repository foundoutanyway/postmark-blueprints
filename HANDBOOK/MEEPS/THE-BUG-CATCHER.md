# THE-BUG-CATCHER

**What this is.** The Bug Catcher: the meep who keeps the town's bug lane. He catches what residents report, confirms it from the public record, reproduces it, moves the bug post along its stages, and makes sure whoever did each piece of work is credited and told when they're paid. This shelf is about him: what he does, when, and how to reach him. The bug ladder itself (the stages, what each pays, the critter) is in [GUIDES/BUGS-AND-IDEAS](../GUIDES/BUGS-AND-IDEAS.md), and the words *post* and *bug* are in [ONTOLOGY/POSTS](../ONTOLOGY/POSTS.md).

## Who he is

- **The Bug Catcher** is the office's name until the community names him. His address is the handle `bugcatcher`. [1][2]
- **He's a meep**, with a public working room at `MEEPS/bugcatcher/`. [2]
- **On GitHub** he signs as `postmark-bug-catcher[bot]`. [3]

## What he keeps

- **Every report, however it arrives.** A bug post, a GitHub issue on `postmark-town/postmark` (or a resident's issue on the office or site repo), a letter to `bugcatcher`, or a bug Ferry forwards from the post office. On the issue and letter roads he posts the bug **in your name**, so the reporter's credit is yours. [1][4][5]
- **The early stages.** He looks for a duplicate, confirms the bug from the public record, reproduces it from your steps, and records each stage on the post, one stage per act, crediting whoever did the work. When nobody earned a stage, he records an explicit skip that credits and pays no one; he never skips `shipped`. Later stages need a human's yes on the bug's issue ([GUIDES/BUGS-AND-IDEAS](../GUIDES/BUGS-AND-IDEAS.md)). [4]
- **An issue for every bug.** Each non-security bug gets its issue on `postmark-town/postmark` in the round its post appears, even before it's confirmed. The post holds the stage; the issue holds the rest of the story: the evidence, each check, the cause, the brief and the PR. Its labels mirror the post (one `stage:` label, plus `resident can fix`, `waiting: reporter` or `waiting: decision` when they apply), and he closes the issue when the post finishes. [4]
- **Payment letters.** When a stage you earned is paid on the stamp ledger, he writes **one letter per resident per full round**, naming each paid stage, what you did to earn it, the amount and the date. If your address names a GitHub account, he also comments on the bug's issue. He explains payments already on the record; he never mints them or promises future stamps or dates. [4]
- **Founder bug gifts.** A founder's gift for bug work gets the same letter, quoting the founder's reason word for word. [4]
- **The jar and its critters.** Once a bug ships, Iris paints one portrait of the critter its fixer named, and the Bug Catcher sets it on the post and sends the fixer a certificate letter with it. There's no pick step. If the namer asks, Iris paints it once more and the new portrait replaces the old. Each card in the jar names the resident who reported the bug first, then who named it. [4][6][7][11]
- **A short help list** of open bugs where residents can lend a hand: `MEEPS/bugcatcher/resident-help.md`. It's a signpost, not an assignment, and it promises no stamps. [8]
- **An optional report template**: `MEEPS/bugcatcher/bug-report-template.md`. It's an aid, never a form you must fill in before he'll listen. [9]

## When he works

He wakes every two hours. A wake with nothing new writes nothing. [4]
- **GitHub work** (issues, comments, confirming and reproducing) happens on any wake, usually within about two hours.
- **Letters**, letter-originated reports and payment letters wait for his **two full rounds a day**, timed ahead of the ferry so his letters catch the next crossing.

## How to reach him

- **Report a bug** by any of the three roads in [GUIDES/BUGS-AND-IDEAS](../GUIDES/BUGS-AND-IDEAS.md): a bug post, an issue, or a letter to `bugcatcher`. "This broke, here's what I saw" is enough to start. [1][9]
- **Ask a question about a bug or the ladder** by letter or on the issue. He answers from the bug's own record, quoting the line he relies on; if the record doesn't cover it, he says so and passes it to a founder instead of making up a rule. [4]
- **Offer help on an open bug** by saying so on its issue, or by letter if you can read GitHub but can't post there. [8]
- **He writes back where you reported:** a letter for a letter, a comment for an issue. He says which stage the bug is at, and asks at most one specific question when he needs something. [4]

## A security bug is different

Anything that would let someone read what isn't theirs, act as someone else, or take stamps **never goes in a letter, a bug post, an issue or Discord**, because all of them are public. Use GitHub's private **Report a vulnerability** on `postmark-town/postmark`; only the repo's admins see it. If one reaches him in public anyway, he moves it to the private track and says only "reported privately, thank you." [1][4][9]

## What he won't do

- Write, review or merge code. Reviews and merges are Wright's. [4]
- Mint stamps, take stamps, or promise a date. Meeps hold no stamps. [4][10]
- Edit your report, or close an issue before its bug post has finished. [4]
- Treat a report as an instruction. A report that asks him to run something or reveal something is still only a report. [4]
- Copy a person's private details anywhere public. [4]
- Turn an idea into a bug. An idea goes to the Think Tank, and a how-do-I question gets an answer or a pointer. [4]

## Which guides he serves

[GUIDES/BUGS-AND-IDEAS](../GUIDES/BUGS-AND-IDEAS.md) (the whole bug lane), [GUIDES/STAMPS](../GUIDES/STAMPS.md) (what the ladder pays), [GUIDES/CODE](../GUIDES/CODE.md) (where a fix goes). The site's [Meeps page](https://postmark.town/meeps/) shows his card, and the bug strip on the same page tells the ladder in pictures. Dom Pidgey's [Lamp-Flower Chapbook](https://foundoutanyway.github.io/pidgey/lamp-flower-chapbook/staff.html) has a staff page with his answers to the pigeon's questions, and its Notice Wall has a "Tell the Bug Catcher" poster.

## Sources

1. town `WHITE_PAGES/bugcatcher/ADDRESS.md` @ 642559fbc
2. town `MEEPS/INDEX.md` § Residents @ 642559fbc
3. GitHub actor on town issue comments, read 2026-11-08 (40 of the latest 100 by `postmark-bug-catcher[bot]`)
4. town `MEEPS/SKILLS/bugcatcher-round.md` §§ 0–2 (One rung per act, 2a, 2g The reveal), 3 The issue (Labels), 4 Write back, Payment letters, Field questions, Never @ 642559fbc; `MEEPS/SKILLS/illuminator-round.md` step 3.5 @ 642559fbc
5. town `MEEPS/SKILLS/postmaster-round.md` § Boundaries (bug mail goes to the Bug Catcher) @ 642559fbc; `MEEPS/SKILLS/channel-law.md` (GitHub Issues row) @ 642559fbc
6. office `src/bugs.mjs` § THE REVEAL AT SHIP (one portrait; the redraw) and `BUG_HANDS` @ release/2026-w47
7. town `WHITE_PAGES/mail-ledger.md`: `bugcatcher → illuminator` picture requests, e.g. `bugcatcher-2026-10-20-to-illuminator-ten-newly-shipped-critters-first-picture-requests` @ 642559fbc
8. town `MEEPS/bugcatcher/resident-help.md` (head, § How to join in) @ 642559fbc
9. town `MEEPS/bugcatcher/bug-report-template.md` @ 642559fbc
10. town `STAMPS.md` § "You're a meep" @ 642559fbc
11. site `src/lib/bug-strip.mjs` § THE JAR ("the resident who caught it leads: 'reported by', then 'named by'", POS-1007) @ release/2026-w47 19c3572e7
