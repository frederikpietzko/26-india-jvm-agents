---
layout: intro
class: section-intro
---

# The demo

## one boring business process, reviewed by three agents

<!--
- One demo, three frameworks
- Who owns the control flow?
  - Spring AI - my own loop in Kotlin
  - Embabel - declared steps, planner finds the path
  - Koog - my graph, Koog walks it
- Domain stays the same, only the reviewer changes
- Travel expense approval - everyone has it, nobody likes it
-->

---
class: code-slide
---

<div class="pillar-tag"><ph-books />context</div>

# The rules are sentences written for people, not for code

```kotlin no-compile
Policy("Hotel budget",
    "Hotels may cost at most 180 EUR per night. " +
    "In London, Paris and Zurich the limit is 250 EUR per night.")

Policy("Rail first",
    "If the train journey takes 4 hours or less, the train must be used " +
    "instead of a flight. Second class only.")
```

<!--
- Five policies: hotel budget, flight class, rail first, lead time, weekend stays
- Why a model
  - prose by HR, full of exceptions
  - if-else for all = boring work
- Policies = table rows
  - POST /api/policies at runtime, e.g. car rental
  - next review picks it up, no redeploy
- Confluence rules checked by hand
-->

---
class: code-slide
---

<div class="pillar-tag"><ph-brackets-curly />type safety</div>

# Three agents, one contract

```kotlin no-compile
@Serializable
data class ReviewOutcome(
    val verdict: Verdict,
    val summary: String,
    val violations: List<String> = emptyList(),
)

interface TripReviewer {
    val agent: ReviewAgent
    fun review(tripId: Long): ReviewOutcome
}
```

<!--
- Same ReviewOutcome for every framework
  - one implementation each
  - verdict, summary, violations - a type, not a chat message
- Verdict enum: APPROVED, NEEDS_CHANGES, REJECTED
  - maps to trip status
- Same facts for all - TravelFacts service
  - read-only, snapshots not entities
  - costs computed in code - models bad at arithmetic
  - each framework exposes it as tools
- Red line: how does each framework get to this type?
-->

---
class: graphic-slide
---

# Every reviewer drafts, a judge audits, a rejection goes back

<div class="flow flow-5">
  <div class="flow-node"><ph-robot /><span>reviewer with tools</span></div>
  <div class="flow-arrow"><span>draft</span></div>
  <div class="flow-node"><ph-gavel /><span>judge without tools</span></div>
  <div class="flow-arrow"><span>accepted</span></div>
  <div class="flow-node"><ph-seal-check /><span>ReviewOutcome</span></div>
  <div v-click class="flow-loop reveal"><span>feedback</span></div>
</div>

<!--
- Judge pattern - all three use it
  - reviewer with tools writes a draft
  - independent call audits it against the facts
- Why
  - LLMs confident when wrong
  - fresh context catches many mistakes
  - cheap insurance - reliability tile
- [click] Rejection - feedback back to the reviewer
  - max two revisions
- Same pattern, three different expressions
-->
