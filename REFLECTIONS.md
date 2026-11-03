> If Le Mans Métropole had limited resources to improve its public transport network,
> where and when should it prioritize interventions?

They should priorize the most popular public transport and expand it.


> **Reflection 1.** Before reading the workflow steps later in this document: the
> static feed will run on a monthly schedule, the real-time feed every 15 minutes.
> Both *could* technically run every 15 minutes. Write your own guess now for why the
> static one doesn't — you'll check it against Stage 5's reasoning.

The static feed does not need to run every 15 minutes because scheduled routes, stops,
and trips do not change that frequently

> **Reflection 2.** Before running anything: if a tool infers each file's column
> types automatically and independently (so one file's `stop_id` column becomes
> numbers, another file's `stop_id` column stays text, just because of what values
> happened to appear in each file), what goes wrong when you try to join those two
> files on `stop_id`? Write your prediction, then compare it to the comment in the
> script below explaining why every GTFS file is read with `dtype=str`.

There will be a conflict of type between str and int resulting in errors or missing 
data, we should use only str because it can read both

> **Reflection 3.** Before reading further: roughly how many individual stop-time
> update rows would a 12-week semester of 15-minute snapshots produce, if you logged
> every one of them? (Order of magnitude is the point, not exact math.) Now compare
> that to logging one row **per route** per snapshot instead (there are only 27-40
> routes). What real analytical question — something a transport planner might
> actually ask — becomes harder to answer with the per-route version? Keep your
> answer; you'll need it again in the final section.

Tens of millions and a couple hundred thousand of rows

> **Reflection 4.** Is that the right way to encode this KPI? A bus arriving very
> early can strand a waiting passenger just as effectively as a very late one — from
> a passenger's perspective, both extremes might be "bad." Propose an alternative
> color/encoding rule for this same chart, and explain what decision or conclusion a
> viewer would reach *differently* under your rule versus the one below.

I would use green for buses arriving close to schedule and orange or red for buses 
arriving too early or too late.

> **Reflection 5.** Now check your Reflection 2 guess against the real answer: why
> would a 15-minute schedule for the *static* GTFS reference be wasteful, and why
> would a monthly schedule for the *real-time* delay snapshots make the whole
> real-time picture useless? Answer both halves — the mismatch runs in both
> directions.

I was right on the reflection 3 but also running real-time snapshots monthly would 
miss delays and disruptions as they happen, making it impossible to analyze daily
patterns reliably.

> **Reflection 6.** Pick one route currently showing a large delay on your
> dashboard. Compare it against
> [SETRAM's own live departure information](https://www.setram.fr/) or plain common
> sense (is there a known local event, roadwork, rush hour?). Did the number match
> what you'd expect? If not, what's your best guess for the discrepancy — a real
> disruption, a data artifact, or something else? This is the same "does this number
> make sense" habit this whole course keeps returning to; write what you actually
> found, not what you expected to find.

On 3 November at 07:32, C4 had an average delay of 124.7 seconds. 
Two minutes seems possible during the morning commute.

> **Reflection 7.** Before writing any code: you're about to use commune
> **population** as a stand-in for real transport **demand**, because real ridership
> data doesn't exist as open data. Name one concrete, realistic way a commune could
> have *low* population but genuinely *high* transport demand (or the reverse) — so
> you go into this exercise already knowing the proxy's failure mode, not just its
> formula.

A commune with few residents can still have a lot of workers or a shopping area. 
These visitors need transport too, but they are not counted in its population.

> **Reflection 8.** State your chosen supply metric in one sentence. Then, using
> your own actual output (not a hypothetical), name one commune where the ranking
> looks wrong to you, and explain why — using what you actually know about that
> commune, not just the formula.

My metric is scheduled stop-visits per 1,000 residents. Arnage is ranked fifth with
295.2, but the data gives it 193 visits from Oasis-Centre des Expositions, which 
is actually in Le Mans. Using the nearest commune center makes Arnage look better
served than it really is.

> **Reflection 9.** Go back to Reflection 4's answer about per-route vs. per-stop
> logging. If you wanted "which *stops* are affected by repeated delays" answered
> from real-time data (not the static schedule), you'd hit that same data-volume
> problem again. You don't have to build this — but describe, in a few sentences, one
> concrete approach you'd take to aggregate stop-level events so a semester of
> 15-minute snapshots stays a reasonably-sized, git-tracked file.

I would save one summary per stop per day, with the number of updates, average 
delay and number of big delays. I would keep monthly files instead of saving 
every individual update. This would still help identify stops with repeated problems.

> **Reflection 10 (closing).** Return to Reflection 1. Which of the business
> question's sub-questions turned out to be answerable today, which needed real
> judgment calls from you, and which genuinely need weeks of accumulated history
> before they're answerable at all? Compare this to what you originally guessed.

Choosing the metric and deciding if a commune is underserved needed judgment. 
Repeated delay patterns and predictions need several weeks of history with good
coverage. Comparing routes was easier, like I expected, but population alone was
not enough to show real demand.