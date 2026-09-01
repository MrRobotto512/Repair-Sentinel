# Sentinel: Why We're Giving Away Our Repair Data (And Why You Should Too)

If you've been doing board-level repair for any amount of time, you know the drill. A console lands on your bench with no video, you pull the UART log, and you get an error code that means... something. So you start digging. A forum thread from 2022. A YouTube video where the guy mumbles through the good part. A Discord message from someone who *thinks* it was the RAM rail but never followed up to say if the fix actually worked.

We've all lost hours to this. Sometimes days. And here's the thing that drives us crazy: **somebody already fixed this exact fault.** Probably dozens of somebodies. They just had no good way to write it down where the rest of us could find it.

That's the problem Sentinel exists to solve.

## What Sentinel actually is

Sentinel is a shared, community-verified knowledge base of real electronics repairs. Not repair *guides*. Not "have you tried reflowing it" forum advice.

It started as one thing: bench records, where every entry pairs what the machine said — UART/syscon error codes, POST codes, boot behavior, battery stats, temperatures, the raw telemetry pulled straight off the device — with what a human technician actually found and fixed: the component they probed, the measurement they got, and whether the repair worked.

That pairing is the whole point. An error code on its own is a riddle. An error code sitting next to "probed the 2R2 inductor, measured 0.4 ohms, replaced the shorted cap on the RAM rail, console lives" is an answer.

Since then the pool has grown a second kind of entry, and honestly it's taken over by volume: the **knowledge card**. A card is a single claim about a fault — what goes wrong, the evidence that proves it, the readings, the part numbers — pulled out of reference material rather than out of a repair. Where a bench record says *this console had this fault and here's what fixed it*, a card says *here is how this fault behaves, and here's where that's established*. They complement each other. The record tells you what happened; the card tells you what to expect.

## A real example from our bench

Earlier this summer a PS5 came in that refused to update. Every method failed with SU-101337-5. We pulled the UART codes and they pointed toward a USB Type-C error — which sounds unrelated to an update failure until you know the front USB-C ports matter more than they should.

Sure enough, only one of the front ports would charge a phone. We hooked up a tester, confirmed bad pin readings on the dead port, replaced the USB-C controller IC, and the console updated on the first try.

That whole diagnostic chain — the SU error, the UART codes, the "check if both ports charge" trick, the specific IC — is now a single entry in the Sentinel pool. The next tech who sees SU-101337-5 doesn't have to rediscover any of it. They can go from symptom to suspect component in the time it takes to search a folder.

Multiply that by every repair, on every bench, in every shop that joins in. That's the idea.

## Where the cards come from — and what we won't take

The cards get made the same way the bench records do: as a by-product of work someone was doing anyway. Point a bench at a service manual and it reads the *whole* document in the background — every section surveyed and mined, and the figure pages that matter (diode charts, voltage maps, pinout tables) read by vision, so the numbers printed on a diagram become searchable text instead of staying locked in a picture. Drop in an annotated boardview or a reading chart and the same thing happens to it. A tech can point it at a repair video, too.

That last one deserves a straight answer, because "we mined YouTube" is the kind of sentence that should make you suspicious. Here's the rule we hold ourselves to. When a card comes from someone else's video, the shared copy carries our own paraphrase of the claim, the evidence for it, and a **link back to the source** — and the verbatim quote and its timestamp are stripped out before the card ever leaves the bench. The shop that made it keeps the full card locally, quotes included, because that's their own working note. The pool gets the fact and the credit, not the transcript. If a card's value depended on reproducing someone's video, it doesn't belong in the pool.

Everything read by vision is capped at medium confidence until a person confirms it, which matters more than it sounds: a model misreading "0.4" as "6.4" off a photographed chart is exactly the kind of error that would poison a dataset quietly.

## How the data stays trustworthy

Anyone who's used crowd-sourced repair info knows the failure mode: it fills up with guesses, half-remembered fixes, and confident wrong answers. Sentinel is built to keep that out.

Every submission lands in a **pending** queue first. Nothing gets consumed from there. A human maintainer reviews each entry, and only approved, verified repairs make it into the **pool** — the actual dataset. Rejected entries, duplicates, and anything missing information go to an archive so there's an audit trail, but they never pollute the data you'd actually rely on.

The house rules are simple and non-negotiable: real, bench-verified repairs only. One repair per file. And absolutely no customer-identifying information — no names, no serials, no IMEIs, no phone numbers. We care about the fault and the fix, not whose console it was.

That last rule is now enforced by code rather than by remembering. Identifying fields are stripped structurally on the way out, wherever they appear in a record, and free text gets scrubbed on top of that. It's the kind of promise that's worth checking rather than trusting, so we check it: as of this writing there is not a single customer name, serial, IMEI or phone number anywhere in the published pool.

Reviewing at volume needed work too. When submissions arrived in ones and twos, approving them one at a time was fine; a backlog of thirty meant thirty clicks and thirty page reloads. A maintainer can now select a whole batch and approve or reject it in one pass, while the judgements that genuinely are per-entry — duplicate, needs more information — stay per-entry, because those aren't decisions you should be able to make thirty at a time by accident.

## Where the bench records come from

At our shop, Sentinel entries are generated automatically by our BenchTool diagnostic stations. When a tech finishes a repair, the station already has the telemetry — the error codes, the readings, the boot behavior. The tech adds what they verified and what they fixed, hits SHARE, and the entry gets submitted for review. No extra paperwork, no writing up a forum post at 9pm when you'd rather be home.

That's deliberate. The reason repair knowledge doesn't get shared isn't that techs don't want to share it — it's that documentation is a chore, and chores lose to the next device in the queue. If sharing takes one button press at the end of a job you already did, it actually happens.

But you don't need our tooling to contribute. The format is plain JSON, one file per repair, and anyone can open a pull request. If you fixed something on your bench and you can describe the symptoms, the telemetry, what you probed, and what you replaced — you can add to the pool.

## Where it stands today

Numbers, because a project like this should be willing to show them rather than talk in the abstract.

The pool holds about 150 approved entries — 141 knowledge cards and 9 bench-verified repair records — alongside 88 reference images. The cards trace back to 135 service documents, 3 repair videos, 2 annotated images and 1 web source, every one of them linked. Four different benches have contributed, which is the part we care most about: it stopped being our dataset somewhere around the point the second shop's entries outnumbered a third of ours. The archive holds a handful of rejected and duplicate submissions, which is the audit trail working as intended, and the pending queue is currently empty — everything submitted has been looked at.

It's still small. A hundred and fifty entries is a rounding error against the number of repairs done in a week across the trade. But it's real, it's verified, and it's growing from more than one bench, which is the only shape this could ever have taken if it was going to work at all.

## Sharing tools, not just data

One thing we didn't anticipate: shops started wanting to share the *tools* they built, not only what they found.

BenchTool lets a shop write its own bench tool — describe what you want in plain words and the AI drafts it against the app contract, or write it yourself and drop in a zip. Once a shop has something that works, publishing it to the shared pool is one action from the dashboard. It queues immediately and uploads on the next sync, so a dead connection at the wrong moment never costs you the submission.

Apps go through the same human review gate as everything else, and each approved app is published with a SHA-256 hash so a bench can verify it got what the maintainer signed off on. Approved apps then reach every participating bench through the same sync that keeps the rest of the unit current. They live outside the BenchTool install, so a firmware update can't clobber them and they can't break an update, and each one writes its own log like any built-in tool.

There are five apps in the pool today, from two different shops — an IMEI lookup, a review-QR generator, a batch iOS activation tool for testing, a Windows intake tester, and a fleet-maintenance tool. None of those are things we would have built. That's rather the point.

## Why give this away?

Fair question. Repair data is hard-won. Every entry in that pool represents real bench time, real probing, and sometimes real money spent going down the wrong path first. Why publish it for free, including to shops that compete with us?

Because the math works out in everyone's favor. Any single shop only sees a slice of what's out there. We might see fifty PS5s a month; another shop sees fifty phones we'd rarely touch. Pool the verified fixes and every participating shop suddenly has diagnostic reach far beyond its own bench history. The stuff you contribute is worth less to you than the stuff everyone else contributes is worth to you. That's the whole trade, and it's a good one.

And zooming out: the real competition isn't the shop across town. It's the landfill. Every device that gets fixed instead of tossed is a win for the customer, a win for the trade, and one less slab of electronics in the ground. Manufacturers aren't going to publish this data for us. Diagnostic knowledge kept in silos dies in silos — techs retire, shops close, hard drives fail, and the community starts over from zero. Written down and shared, it compounds.

## Machine data plus human judgment

One more thing worth calling out. Some of our entries include an ML-generated fault prediction alongside the telemetry. We keep those in the record — but they never stand alone. Every entry that reaches the pool includes actual technician verification: what was probed, what was measured, whether the predicted fault was real.

That makes the pool useful in both directions. Techs get verified fixes they can trust today. And over time, the dataset becomes exactly what's needed to make diagnostic predictions *better* — because every prediction is graded against what a skilled human actually found. The machine helps the tech, the tech corrects the machine, and the record of that exchange helps the next bench down the line.

## Get involved

If you run a repair bench — whether you're a full shop or a solo operator fixing consoles in your garage — Sentinel gets more useful with every entry you add.

Shops running BenchTool can point their config at the Sentinel repo and start submitting with the SHARE button — repairs, distilled reference knowledge, or an app you built and think someone else could use. Everyone else can contribute via pull request; the format is plain JSON and the repo documents it. And even if you never submit a thing, the pool is there for you to search the next time a cryptic error code lands on your bench. Consume `pool/` and `apps/` — everything in them has been through a human.

The knowledge already exists. It's sitting in the heads and notebooks of thousands of technicians. Sentinel is just the place to put it where it can't be lost — and where it does the most good.

See something on your bench worth sharing? Send it in. The next tech will thank you.
