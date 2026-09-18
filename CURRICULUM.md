# A/B Testing Playbook — Learning Program

**Repo:** `github.com/omri-sabag83/A-B-Testing-Playbook` (private for now; the
git remote will be wired up when you're ready to push).

**Goal:** build real working knowledge of experimentation as practiced at B2C
product companies, deep enough to (a) design, run, and analyze A/B tests
credibly on the job, and (b) hold up in product-analyst interviews that probe
A/B testing / experimentation.

**Starting point:** solid general analytics skill (SQL, Python, stats
fundamentals) and real B2B product-analyst experience — but B2B experimentation
rarely looks like B2C. Low traffic volumes, long sales cycles, and
account-level (not user-level) decisions usually make classic A/B testing
impractical, so the gap isn't analytical ability, it's exposure to
B2C-specific machinery: high-volume randomized experiments, funnel/engagement
metrics, and the statistical rigor that comes with running hundreds of
concurrent tests. Where that B2B vs. B2C contrast is genuinely useful, it's
called out inline (see "Why it matters" per module); where it isn't, the
module just covers the concept.

---

## Progress

`🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩🟨` **92% complete (11/12 modules) — original 10 done, Module 12 built, pending review**

⬜ Not started · 🟨 In progress · 🟩 Completed

| # | Module | Status | Completed On |
|---|--------|--------|---------------|
| 1 | Foundations: Causal Inference & When to Test | 🟩 Completed | 2026-09-15 |
| 2 | B2C Product Metrics | 🟩 Completed | 2026-09-15 |
| 3 | Statistical Foundations: Significance, CIs, Power & Sample Size | 🟩 Completed | 2026-09-15 |
| 4 | Experiment Design: Hypotheses, Randomization & Guardrail Metrics | 🟩 Completed | 2026-09-16 |
| 5 | Multiple Looks: Peeking, Multiple Testing & Stopping Rules | 🟩 Completed | 2026-09-16 |
| 6 | Analyzing & Interpreting Results | 🟩 Completed | 2026-09-16 |
| 7 | Common Experimentation Pitfalls | 🟩 Completed | 2026-09-17 |
| 8 | Beyond Simple A/B: Quasi-Experiments | 🟩 Completed | 2026-09-17 |
| 9 | Communicating Results to a Product/Business Audience | 🟩 Completed | 2026-09-17 |
| 10 | Interview Prep & Portfolio Application | 🟩 Completed | 2026-09-18 |
| 11 | Bayesian A/B Testing | 🟩 Completed | 2026-09-18 |
| 12 | Multi-Armed Bandits | 🟨 In progress | — |

*This table and the bar above get updated as we complete modules — I'll keep
both in sync.*

---

## How this program works

Each module has:
- **Concepts** — what you need to understand, not just recite
- **Why it matters** — the real-world consequence of not knowing this
  (flagged with the B2B→B2C angle where relevant)
- **Worked example** — a concrete illustration of the concept in action, to
  learn from — not necessarily something you do yourself
- **Resources** — optional external reading/viewing, time-boxed and kept
  tight on purpose; a "must-read before this module" is flagged explicitly
  when it's worth it, and skipped when the in-module content is enough
- **Exercise type** — the kind of exercise that anchors the module. Resolved
  2026-09-15: exercises (case-based *and* computational alike) are delivered
  fully worked and explained by Claude, for Omri to read/review — not blank
  cells for Omri to write and run himself
- **Interview angle** — how this tends to get probed in interviews

Modules are ordered so each one only depends on what came before it —
statistics before design (design needs sizing math), design before
monitoring/analysis, all of it before pitfalls (which assumes you already
know what "doing it right" looks like), and interview prep last as the
capstone. Still a starting map, not a contract — order and scope will flex as
we go.

---

## <u>Module 1 — Foundations: Causal Inference & When to Test</u>

**Concepts**
- Correlation vs. causation, and why RCTs (Randomized Controlled Trials —
  the formal name for what a product team calls an "A/B test") are the
  gold standard for causal claims
- The counterfactual framing: what would have happened without the change
- When A/B testing is the right tool vs. when it isn't (low traffic, ethical
  constraints, network effects, one-time launches, infrastructure changes)
- The B2C experimentation "culture" — why companies run hundreds of
  concurrent tests, and what that implies about tooling and rigor

**Why it matters**
This is the mental model everything else hangs off. B2B analysts often reason
causally from small-n, pre/post comparisons — B2C interviewers will probe
whether you reach for a proper experiment instead of a correlation.

**Worked example**
A product team notices users who enable push notifications have 40% higher
30-day retention than users who don't, and wants to credit push notifications
for the lift. The catch: users who *choose* to enable notifications are
probably already more engaged — the comparison is confounded by self-selection,
not causal. The fix is an A/B test that *randomly assigns* users to see a
notification opt-in prompt (or not) — now the two groups differ only in what
they were offered, so any retention gap is attributable to the prompt itself.

**Resources**
- *Trustworthy Online Controlled Experiments* — Kohavi, Tang, Xu (book).
  Chapter 1 only (~20 min) — the clearest short framing of why controlled
  experiments beat observational analysis. Free official PDF:
  [Chapter 1](https://experimentguide.com/wp-content/uploads/TrustworthyOnlineControlledExperiments_PracticalGuideToABTesting_Chapter1.pdf)
  via [experimentguide.com](https://experimentguide.com/), the book's
  official companion site (verified — also has translated versions of Ch. 1
  and Kohavi's own FAQ/bibliography). This book is the spine resource for
  this whole program; later modules cite specific chapters, which need the
  full book (Kindle/paperback links also on the companion site).

**Exercise type:** short case-based judgment calls ("would you A/B test
this, and why/why not") rather than computation.

**Interview angle:** "How would you test X?" / "When would you *not* run an
A/B test?" — extremely common opener.

---

## <u>Module 2 — B2C Product Metrics</u>

**Concepts**
- Funnel metrics (conversion, activation, drop-off) and how to decompose them
- Engagement and retention metrics (DAU/MAU — Daily/Monthly Active Users,
  retention curves, cohort analysis)
- Revenue/monetization metrics and their relationship to engagement
- Leading (proxy) vs. lagging (true north-star) metrics, and the risk of
  optimizing a proxy that diverges from what actually matters

**Why it matters**
This is largely new territory relative to B2B, where the metric set is
usually narrower (pipeline, deal size, account-level churn). B2C product
analytics has a much richer, standardized metric vocabulary, and Module 4
(experiment design) leans on it directly to define primary/guardrail metrics.

**Worked example**
A signup funnel: Visit → Signup → Activation → Day-7 Retention, with a new
onboarding flow that lifts Signup→Activation from 40% to 48%. Looks like a
clear win — until Day-7 Retention among those *newly* activated users drops
from 60% to 50%. The flow is pulling in more people, but lower-intent ones
who don't stick. A single top-line "conversion" number would have hidden
this entirely — you need the funnel decomposed to see it.

**Resources**
- [What Is a Retention Curve: Complete Definition & Examples](https://amplitude.com/explore/analytics/retention-curve)
  (Amplitude, ~10 min) — covers the four common curve shapes (declining,
  flat-ish, flat, "smile"), which this module's demo reproduces.
- [Every Product Needs a North Star Metric](https://amplitude.com/blog/product-north-star-metric)
  (Amplitude, ~10 min) — the leading- vs. lagging-indicator framing directly
  extends this module's proxy-metric risk point.

**Exercise type:** applied — build funnel and retention analyses on a public
or simulated B2C dataset.

**Interview angle:** "what metrics would you look at for X feature" —
product-sense questions that blend into experimentation questions.

---

## <u>Module 3 — Statistical Foundations: Significance, CIs, Power & Sample Size</u>

**Concepts**
- Hypothesis testing mechanics: null/alternative, p-values, significance
  level, confidence intervals (CI)
- Type I vs. Type II error, and the tradeoff between them
- Statistical power, and what drives it (effect size, sample size, variance,
  alpha)
- Choosing a test: two-proportion z-test (conversion rate), t-test
  (continuous metrics like revenue or session length), chi-square for
  categorical outcomes, and where normality assumptions break down (e.g.
  heavy-tailed revenue data)
- Sample size / Minimum Detectable Effect (MDE) calculations, and the
  practical tension between "how long can we run this" and "what effect can
  we actually detect"

**Why it matters**
This is the technical core interviewers screen for, and the toolkit every
later module depends on — you can't design a properly sized test (Module 4)
or reason about peeking (Module 5) without it.

**Worked example**
Baseline checkout conversion is 5%, and the team wants to detect a lift to
5.5% (a 10% relative lift) with 80% power at a 5% significance level. Plugging
these into the standard two-proportion sample-size formula gives a required
sample per arm in the tens of thousands — small relative lifts on already-low
base rates need surprisingly large samples. We'll implement this formula in
Python as this module's exercise so you get the exact number, not just the
intuition.

**Resources**
- [Evan Miller's Sample Size Calculator](https://www.evanmiller.org/ab-testing/sample-size.html)
  (verified, interactive, ~10 min) — play with it to build intuition for how
  effect size and baseline rate drive required sample size, before this
  module builds the same calculation from scratch.
- *Trustworthy Online Controlled Experiments* — Kohavi, Tang, Xu, Ch. 3
  ("Statistical Fundamentals for A/B testing") (~30 min).

**Exercise type:** computational — implement power/sample-size calculators
from scratch in Python, then simulate experiments (known true effect) to
verify your test correctly detects it at the expected rate.

**Interview angle:** live stats questions, sample-size case questions ("we
get 10k signups/day, 5% baseline conversion, want to detect a 2pp lift — how
long do we run it?").

---

## <u>Module 4 — Experiment Design: Hypotheses, Randomization & Guardrail Metrics</u>

**Concepts**
- Turning a product question into a testable hypothesis
- Randomization unit: user vs. session vs. device vs. cluster, and why the
  choice changes both the analysis and the risk of contamination
- Sample Ratio Mismatch (SRM) — what it is, why it invalidates a test, how to
  detect it
- Primary metric, secondary metrics, guardrail metrics, and the Overall
  Evaluation Criterion (OEC) — designing what "success" means *before*
  looking at results
- Pre-registration / analysis plans, and why deciding your test after seeing
  data is a trap
- Practical randomization mechanics (hashing users into buckets, consistent
  bucketing across sessions)

**Why it matters**
Design mistakes here silently poison everything downstream — no amount of
correct statistics rescues a badly randomized or badly scoped test. This is
also where B2C experimentation platforms (Optimizely, GrowthBook, in-house
systems) encode a lot of institutional best practice you won't have seen in
B2B, where a "test" is often just a phased rollout to a handful of accounts.

**Worked example**
Designing a test for a new one-click checkout button: hypothesis is "reducing
checkout friction increases completion rate without hurting order value."
Randomization unit is the *user* (not session) so the same person doesn't
see both variants across visits. Primary metric: checkout completion rate.
Guardrails: revenue per user (make sure completion isn't coming from smaller
carts) and refund rate (make sure faster checkout isn't causing mis-orders).
The OEC combines these so a win on the primary metric that tanks a guardrail
doesn't get shipped by accident.

**Resources**
- [Experiments at Airbnb](https://medium.com/airbnb-engineering/experiments-at-airbnb-e2db3abf39e7)
  — Jan Overgoor, Airbnb Tech Blog (~15 min). Verified real and canonical;
  Medium blocks automated fetches with a 403, so it couldn't be read
  directly — may prompt for a free Medium sign-in. Covers stopping tests
  too early, marketplace-level bias, and A/A tests — directly relevant to
  this module's SRM section.

**Exercise type:** design critique exercises — given a proposed test, find
the design flaws (wrong randomization unit, missing guardrails, SRM risk)
before ever touching data.

**Interview angle:** "design an experiment for X" case questions — scored
heavily on design rigor, not just stats.

---

## <u>Module 5 — Multiple Looks: Peeking, Multiple Testing & Stopping Rules</u>

**Concepts**
- The "peeking" problem — why checking significance repeatedly over time
  inflates the false-positive rate
- The same phenomenon along a different axis: multiple testing across many
  metrics or segments (Bonferroni, FDR — False Discovery Rate —
  correction) — it's the same "repeated looks" problem, just spatial
  instead of temporal
- Sequential testing and always-valid p-values as a principled fix
- Stopping rules and minimum runtime, and why "stop as soon as it's
  significant" is wrong
- Practical monitoring: SRM checks and guardrail regressions surfacing
  mid-test

**Why it matters**
At B2C scale, tests are monitored live and there's constant organizational
pressure to call results early or highlight whichever metric/segment turned
green. Knowing why both are the same statistical trap — and how modern
platforms handle it — is a strong credibility signal.

**Worked example**
Checking a test's p-value every day for two weeks and stopping the moment it
crosses 0.05 sounds harmless but isn't: under the null hypothesis (no real
effect), that repeated-checking procedure triggers a "significant" result far
more than 5% of the time — this is the well-documented finding behind
Optimizely's and other platforms' move to sequential/always-valid testing
(see Johari, Pekelis & Walsh's peeking paper below). The fix isn't
"don't look" — it's using a testing procedure that stays valid *even if* you
look every day.

**Resources**
- [How Not to Run an A/B Test](https://www.evanmiller.org/how-not-to-run-an-ab-test.html)
  — Evan Miller (verified, ~15 min). Cites a specific number this module's
  simulation reproduces: continuously monitoring a test and stopping the
  moment it crosses 5% significance inflates the true false-positive rate to
  **26.1%** — over 5x the nominal rate.
- *Peeking at A/B Tests: Why It Matters, and What to Do About It* — Johari,
  Koomen, Pekelis & Walsh (KDD 2017, verified real via search; ACM's page
  blocks automated fetches the same way Medium did, so no direct link —
  search the title, it's the paper behind Optimizely's "always valid
  p-values," and turns up freely via Google Scholar). The rigorous version;
  worth skimming even if you don't read every proof (~20 min skim).

**Exercise type:** simulation — show empirically (via repeated simulation)
how peeking inflates false-positive rate, then implement a simple
sequential-testing correction.

**Interview angle:** less common as a direct question, but a strong
differentiator when it comes up ("someone wants to end the test early
because it's significant — what do you say?").

---

## <u>Module 6 — Analyzing & Interpreting Results</u>

**Concepts**
- Statistical significance vs. practical/business significance — p < 0.05
  with a trivial effect size is not a reason to ship
- Heterogeneous treatment effects — does the effect differ by segment, and
  when is segment-cutting legitimate vs. p-hacking (ties directly back to
  Module 5's multiple-comparisons problem)
- Interpreting ambiguous or flat/failed experiments — a non-significant
  result is information, not a failure of the analysis
- Interference/network effects — when treatment and control users affect
  each other (marketplaces, social features), breaking the independence
  assumption the stats in Module 3 rely on

**Why it matters**
This is where most real-world analytical mistakes happen — not in running
the test, but in over- or under-interpreting the result afterward. It's also
a frequent interview "gotcha" because it's easy to sound rigorous and still
get it wrong.

**Worked example**
A test shows no significant overall effect, but slicing by "new users" shows
a significant lift. Is that real? On its own, no — if you didn't pre-specify
that segment, you're implicitly running many tests (Module 5's problem again)
and some will look significant by chance. What would make it credible: the
segment was hypothesized *before* the test, the effect direction is
consistent with a plausible mechanism, and ideally it replicates in a
follow-up test scoped to that segment.

**Resources**
- *Trustworthy Online Controlled Experiments* — Kohavi, Tang, Xu, Ch. 4 and
  Ch. 20/21 on segmentation and interpreting results (~30 min combined).

**Exercise type:** given a results table with some real pattern and some
traps baked in, diagnose what's actually going on.

**Interview angle:** "the test showed a significant result in one segment
but not overall — what do you conclude?" — classic trap question.

---

## <u>Module 7 — Common Experimentation Pitfalls</u>

**Concepts**
- Simpson's paradox and other aggregation traps
- Novelty and primacy effects — why a metric's short-term movement can
  mislead about its long-term effect
- SRM revisited as a pitfall (not just a design-time check)
- A gallery of real, well-documented experimentation failures at known
  companies, and what specifically went wrong in each

**Why it matters**
Module 6 taught you how to analyze *your* results correctly; this module is
a catalog so you recognize known failure patterns fast, in your own work and
when critiquing someone else's — a very common interview format.

**Worked example**
Simpson's paradox: overall conversion looks *worse* in treatment than
control, but within both mobile and desktop segments, treatment is actually
better. The explanation: randomization (or traffic allocation) skewed more
low-converting mobile users into treatment, so the segment mix — not the
treatment — drove the aggregate number. This is exactly why Module 4's SRM
check and segment-consistency checks matter: the aggregate number alone would
have led to killing a feature that was actually working.

**Resources**
- [Seven Rules of Thumb for Web Site Experimenters](https://exp-platform.com/rules-of-thumb/)
  — Kohavi, Deng, Longbotham, Xu (KDD 2014; verified, free PDF on Kohavi's
  own site, ~20 min) — a compact list of real pitfalls from real companies
  (Amazon, Booking.com, LinkedIn, Microsoft). Author list corrected here —
  an earlier draft of this curriculum had it wrong (mixed up with a
  different, related 2013 paper).

**Exercise type:** case-study critique — given a described (real or
realistic) experimentation failure, diagnose which pitfall it is and how it
should have been caught.

**Interview angle:** "tell me about a time an A/B test result was
misleading" / pitfall-recognition questions.

---

## <u>Module 8 — Beyond Simple A/B: Quasi-Experiments</u>

**Concepts**
- Cluster randomization, properly this time — a worked example and its real
  cost (a shrunken effective sample size from within-cluster correlation),
  closing the loop on Module 4/6's brief, example-free mentions
- Difference-in-differences
- Switchback tests (for marketplace/network settings)
- Geo experiments / holdout regions
- Regression discontinuity, briefly, for completeness

**Why it matters**
Not every question can be A/B tested (pricing changes with legal/PR risk,
infrastructure changes, marketplace-wide features). Knowing the fallback
toolkit — and its weaker causal guarantees — signals maturity beyond "just
run an A/B test," and is lower-depth than Modules 3–4 since this is breadth,
not the core skill being screened for.

**Worked example**
A company changes its shipping-fee policy nationally — it can't randomize by
user since everyone in a given region sees the same policy. Instead, it rolls
the change out in a handful of states first (treatment geos) and picks
demographically similar states that didn't get the change (control geos),
then uses difference-in-differences to estimate the effect: (post-change
trend in treatment geos) minus (post-change trend in control geos), netting
out any nationwide trend that would have happened anyway.

**Resources**
- [Difference-in-Difference Estimation](https://www.publichealth.columbia.edu/research/population-health-methods/difference-difference-estimation)
  — Columbia Mailman School of Public Health (verified, ~15-25 min,
  includes parallel-trends diagrams).

**Exercise type:** conceptual + light computation (diff-in-diff on a small
dataset).

**Interview angle:** occasionally shows up as "how would you measure this if
you can't randomize."

---

## <u>Module 9 — Communicating Results to a Product/Business Audience</u>

**Concepts**
- Translating statistical results into a ship/no-ship recommendation
- Framing uncertainty and effect size for non-technical stakeholders
- Handling an inconclusive or flat result without it reading as failure
- Pushing back on stakeholders who want to peek early or ignore guardrails
  (directly using Modules 5 and 6)

**Why it matters**
This is the one area where your existing B2B background transfers most
directly — stakeholder communication is stakeholder communication. This
module mainly adapts it to experimentation-specific framing.

**Worked example**
Bad summary: *"p = 0.03, we're good to ship!"* — no effect size, no
business context, implies false certainty. Better summary: *"Checkout
completion rose from 22% to 23.4% (a 1.4pp / ~6% relative lift, 95% CI:
0.5pp–2.3pp). Guardrails (revenue per user, refund rate) are flat. We
recommend shipping — the effect is both statistically and practically
meaningful, and downside risk looks low."* Same underlying result, one
version a PM can actually act on.

**Resources** — none needed; this module leans on your existing strengths,
just retargeted.

**Exercise type:** written recommendation memos based on Module 6 results,
evaluated for clarity and honesty about uncertainty.

**Interview angle:** "walk me through how you'd present this result to a PM
who wants to ship anyway."

---

## <u>Module 10 — Interview Prep & Portfolio Application</u>

**Concepts**
- Common product-analyst A/B testing interview formats: stats questions,
  experiment design cases, results-interpretation cases, product-sense
  hybrids
- Building 1–2 portfolio-worthy artifacts that demonstrate this skill set
  end-to-end (design → simulate/analyze → recommend)
- Mapping your B2B experience into interview answers where it's genuinely
  relevant (stakeholder management, hypothesis-driven thinking) instead of
  downplaying it

**Why it matters**
This is the direct job-hunt payoff — everything else is preparation for this
module.

**Worked example**
Sample question: *"Walk me through how you'd A/B test a new onboarding
flow."* A strong answer skeleton, pulling directly from Modules 1–9:
hypothesis (Module 1) → primary/guardrail metrics (Modules 2, 4) →
randomization unit and design (Module 4) → sample size (Module 3) →
monitoring plan and stopping rule (Module 5) → how you'd interpret an
ambiguous result (Module 6) → recommendation framing (Module 9). Interviewers
are largely scoring whether you hit these beats in order, not just whether
each individual answer is correct.

**Resources** — none prescribed; this module's "reading" is largely mock
practice using everything built in Modules 1–9.

**Exercise type:** mock case interviews (timed or self-paced) plus a
portfolio piece — likely the meatiest deliverable, and a strong candidate for
the E##-style exercise format already used elsewhere.

**Interview angle:** this module *is* the interview angle.

---

## Extension: Modules 11-12 (added 2026-09-18)

The original 10-module program (above) is complete. These two were added
afterward, from an honest self-critique Omri asked for once the program
was done — real gaps, not originally planned, added deliberately rather
than folded into the "finished" 10.

## <u>Module 11 — Bayesian A/B Testing</u>

**Concepts**
- Prior, likelihood, posterior — the Bayesian update, in plain terms
- The Beta-Binomial conjugate model for conversion-rate tests — closed-form,
  no heavy machinery needed
- Probability that B beats A, and credible intervals — the Bayesian answers
  to "is it significant" and "what's the CI"
- Contrasting the same result under frequentist vs. Bayesian framing —
  where they agree, where the interpretation genuinely differs

**Why it matters**
Frequentist and Bayesian are two different, both legitimate lenses on the
same data. Knowing why "there's a 95% probability B is better" (Bayesian)
is a different claim from "p = 0.05" (frequentist) — even though the
numbers invite reading them as complements of each other, they're not — is
a common interview differentiator, and Bayesian methods are used in real
experimentation platforms.

**Worked example**
The same checkout-test numbers from Module 3, analyzed both ways: the
frequentist z-test/CI (already built), and a Beta-Binomial Bayesian
analysis on the identical data — probability B beats A, and a 95% credible
interval — compared side by side.

**Resources**
- [Bayesian Statistics for A/B Testing Explained](https://www.growthbook.io/insights/bayesian-statistics)
  — GrowthBook (verified, ~10-12 min, accessible).
- [Formulas for Bayesian A/B Testing](https://www.evanmiller.org/bayesian-ab-testing.html)
  — Evan Miller (verified, ~15-20 min, calculus-heavy derivations; the
  author's own advice is to skip to the implementation section if that's
  not your thing).

**Exercise type:** computational — implement the Beta-Binomial update from
scratch, compute probability-to-beat and a credible interval, contrast with
the frequentist read on the same data.

**Interview angle:** "explain Bayesian vs. frequentist A/B testing" / "what
does '95% probability B is better' actually mean."

---

## <u>Module 12 — Multi-Armed Bandits</u>

**Concepts**
- The explore-exploit tradeoff
- Epsilon-greedy, the simplest bandit algorithm
- Thompson Sampling — using Module 11's Bayesian posterior to decide live
  traffic allocation
- When a bandit is the right call vs. a fixed-split A/B test, and the
  causal-inference cost of choosing one

**Why it matters**
A real, commonly-asked alternative paradigm to fixed A/B testing, especially
relevant for ranking/recommendation-heavy B2C contexts — and a natural
showcase of Module 11's machinery actually being used for something.

**Worked example**
Three variants with different true conversion rates: a fixed 33/33/33
split vs. a Thompson Sampling bandit, run over the same total traffic.
Compare total conversions captured (the bandit should win) against how
confidently each design lets you state the true ranking at the end (the
fixed split should win) — the actual real tradeoff, not a one-sided pitch
for bandits.

**Resources**
- [Multi-arm bandit testing: when to use it (and when not to)](https://www.growthbook.io/blog/guide-to-multi-arm-bandits-what-is-it-and-why-you-probably-shouldnt-use-it)
  — Graham McNicoll, GrowthBook Blog (verified, ~10 min).
- [Thompson Sampling](https://towardsdatascience.com/thompson-sampling-fc28817eacb8/)
  — Steve Roberts, Towards Data Science (verified, ~8 min).

**Exercise type:** computational — implement epsilon-greedy and Thompson
Sampling from scratch, compare cumulative reward vs. a fixed split.

**Interview angle:** "when would you use a bandit instead of an A/B test."

---

## Open questions / decisions for as we go

- ~~**Public vs. private:**~~ resolved — repo is private for now
  (`A-B-Testing-Playbook`), can revisit going public later.
- ~~**Exercise write style:**~~ resolved 2026-09-15 — exercises (including
  computational ones in Modules 3/5/8) are fully worked and explained by
  Claude, for review, not blank cells for Omri to write/run himself.
- ~~**Format of exercises:**~~ resolved by practice, 2026-09-18 — module-by-module
  short drills, not larger cross-module case studies (Module 10's capstone
  was the one deliberate exception, by design, as the closing synthesis).
- ~~**Pacing:**~~ resolved by practice, 2026-09-18 — strictly in order,
  Module 1 through Module 10, no jumping around.

All open questions resolved as of Module 10 (2026-09-18).
