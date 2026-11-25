---
layout: intro
class: section-intro
---

# Embabel

## declare the steps, let the planner find the path

<!--
- Rod Johnson, creator of Spring
- Kotlin, works from Java, on top of Spring AI
- Spring AI : Embabel = Servlet API : Spring MVC
- Control flow: nobody writes it down
  - typed actions + goals, planner derives the order
- Same TravelFacts, ReviewOutcome, judge
- Changes how the 2nd and 3rd agent get built
-->

---
class: graphic-slide
---

# Embabel plans like a game AI: actions, conditions, goals

<div class="flow flow-5">
  <div class="flow-node"><ph-gear /><span>@Action methods</span></div>
  <div class="flow-arrow"><span>types</span></div>
  <div class="flow-node"><ph-path /><span>GOAP planner</span></div>
  <div class="flow-arrow"><span>plan</span></div>
  <div class="flow-node"><ph-flag-checkered /><span>@AchievesGoal</span></div>
  <div v-click class="flow-loop flow-loop-end reveal"><span>replan</span></div>
</div>

<!--
- GOAP - goal oriented action planning
  - from games - F.E.A.R. enemies
  - preconditions, effects, search to the goal
- Preconditions / effects mostly types
  - TripDossier action needs a TripDossier first
  - named conditions where types are not enough
- Planner = algorithm, not LLM
  - deterministic, explainable, logged
- [click] Replan after every action
  - unexpected effect - another route
- Cost and value 0 - 1 per action
  - cheaper route preferred
- Newer: Utility, Supervisor planners, @State machines
-->

---
class: graphic-slide
clicks: 3
---

# Actions never call each other, they meet on a typed blackboard

<svg class="blackboard-graph" viewBox="0 90 1400 470" role="img" aria-label="An action reads its inputs from the blackboard by type and writes its result back, the planner reads the blackboard">
  <defs>
    <linearGradient id="bb-gradient" gradientUnits="userSpaceOnUse" x1="0" y1="0" x2="1400" y2="0">
      <stop offset="0" stop-color="var(--fundamentals-purple)" />
      <stop offset="1" stop-color="var(--fundamentals-pink)" />
    </linearGradient>
    <marker id="bb-head" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path class="bb-head" d="M0,0 L10,5 L0,10 z" />
    </marker>
    <marker id="bb-head-on" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path class="bb-head bb-head-on" d="M0,0 L10,5 L0,10 z" />
    </marker>
  </defs>
  <!-- the board: every object of one agent run, by type -->
  <g class="bb-board">
    <rect x="40" y="270" width="1320" height="150" rx="18" />
    <text x="62" y="300">BLACKBOARD · ONE PER AGENT RUN</text>
  </g>
  <g class="bb-object">
    <rect x="60" y="320" width="240" height="64" rx="14" />
    <text x="180" y="360" text-anchor="middle">TripReviewRequest</text>
  </g>
  <g class="bb-object">
    <rect x="320" y="320" width="240" height="64" rx="14" />
    <text x="440" y="360" text-anchor="middle">TripDossier</text>
  </g>
  <g class="bb-object">
    <rect x="580" y="320" width="240" height="64" rx="14" />
    <text x="700" y="360" text-anchor="middle">ReviewOutcome</text>
  </g>
  <g class="bb-object" :class="{ 'is-hidden': $clicks < 2 }">
    <rect x="840" y="320" width="240" height="64" rx="14" />
    <text x="960" y="360" text-anchor="middle">Judgement</text>
  </g>
  <g class="bb-object bb-goal">
    <rect x="1100" y="320" width="240" height="64" rx="14" />
    <text x="1220" y="360" text-anchor="middle">EmbabelReview</text>
  </g>
  <!-- the action only touches the board: parameters in by type, result out -->
  <g class="bb-edge" :class="{ 'is-on': $clicks >= 1 }">
    <path class="bb-base" d="M440,318 C440,230 465,146 566,146" :marker-end="$clicks >= 1 ? 'url(#bb-head-on)' : 'url(#bb-head)'" />
    <path class="bb-hl" stroke="url(#bb-gradient)" pathLength="1" d="M440,318 C440,230 465,146 566,146" />
    <text x="425" y="215" text-anchor="end">reads by type</text>
  </g>
  <g class="bb-edge" :class="{ 'is-on': $clicks >= 1 }">
    <path class="bb-base" d="M700,318 L700,186" :marker-end="$clicks >= 1 ? 'url(#bb-head-on)' : 'url(#bb-head)'" />
    <path class="bb-hl" stroke="url(#bb-gradient)" pathLength="1" d="M700,318 L700,186" />
  </g>
  <g class="bb-edge" :class="{ 'is-on': $clicks >= 2 }">
    <path class="bb-base" d="M834,146 C935,146 960,230 960,316" :marker-end="$clicks >= 2 ? 'url(#bb-head-on)' : 'url(#bb-head)'" />
    <path class="bb-hl" stroke="url(#bb-gradient)" pathLength="1" d="M834,146 C935,146 960,230 960,316" />
    <text x="980" y="215">writes result</text>
  </g>
  <g class="bb-actor" :class="{ 'is-on': $clicks >= 1 }">
    <rect x="570" y="110" width="260" height="72" rx="36" />
    <text x="700" y="153" text-anchor="middle">judgeDraft</text>
  </g>
  <!-- the planner never runs anything, it only looks at the board -->
  <g class="bb-edge" :class="{ 'is-on': $clicks >= 3 }">
    <path class="bb-base" d="M700,468 L700,424" :marker-end="$clicks >= 3 ? 'url(#bb-head-on)' : 'url(#bb-head)'" />
    <path class="bb-hl" stroke="url(#bb-gradient)" pathLength="1" d="M700,468 L700,424" />
    <text x="718" y="452">world state</text>
  </g>
  <g class="bb-actor" :class="{ 'is-on': $clicks >= 3 }">
    <rect x="570" y="470" width="260" height="72" rx="36" />
    <text x="700" y="513" text-anchor="middle">GOAP planner</text>
  </g>
  <text class="bb-next" :class="{ 'is-on': $clicks >= 3 }" x="860" y="513">next: acceptDraft</text>
</svg>

<!--
- Blackboard - shared state of one agent run
  - typed objects, not chat messages
  - the boxes from the A* graph
- Actions have no references to each other
  - only to the board
- [click] Parameters bound by type
  - TripDossier + ReviewOutcome taken from the board
  - type missing - action not possible yet
- [click] Return value goes onto the board
  - Judgement next to the rest
  - append only, nothing removed
  - latest object of a type wins
- [click] Planner only reads the board
  - world state = types present + conditions
  - DRAFT_ACCEPTED evaluated on Judgement
  - next step: acceptDraft
- Goal = EmbabelReview on the board
- Blackboard = the memory
  - no growing chat transcript
  - fresh prompt per action from typed objects
  - long tool loops: sliding window, truncated tool results
- Classic AI pattern - Hearsay-II, 1970s
-->

---
class: code-slide
---

<div class="pillar-tag"><ph-brackets-curly />type safety</div>

# The caller names an input and a goal type, never the steps

```kotlin no-compile
data class TripReviewRequest(val tripId: Long)
data class TripDossier(val trip: TripSnapshot, val costs: CostBreakdown)
data class EmbabelReview(val outcome: ReviewOutcome, val revised: Boolean)
```

```kotlin no-compile
AgentInvocation.create<EmbabelReview>(agentPlatform)
    .invoke(TripReviewRequest(tripId))
    .outcome
```

<!--
- Three data classes - agent vocabulary
  - the objects on the blackboard
  - actions read inputs, write results
- Invocation: TripReviewRequest in, EmbabelReview wanted
  - no step names, no graph, no loop
- .outcome unwraps ReviewOutcome for TripReviewer
-->

---
class: code-slide code-dense
---

<div class="pillar-tag"><ph-brackets-curly />type safety</div>

# A model-backed action asks for an object, not a string

```kotlin no-compile
@Action
fun draftReview(dossier: TripDossier, context: OperationContext): ReviewOutcome =
    context.ai()
        .withDefaultLlm()
        .withSystemPrompt(Prompts.REVIEWER)
        .createObject("Review this trip: $dossier")
```

<!--
- TripDossier in, ReviewOutcome out - runs after loadDossier
  - loadDossier = plain code action, no model
- createObject - return type defines the output
- Prompt runner resembles ChatClient - not a coincidence
- Tools line omitted - next slide
-->

---
class: code-slide
---

<div class="pillar-tag"><ph-plugs-connected />integration</div>

# Tools are granted per model call, not per agent

```kotlin no-compile
@Component
class EmbabelTravelTools(private val facts: TravelFacts) {
    @LlmTool(description = "List all company travel policies")
    fun listPolicies(): List<PolicyText> = facts.policies()
}
```

```kotlin no-compile
context.ai().withDefaultLlm().withToolObject(tools)  // draftReview
context.ai().withDefaultLlm()                        // judgeDraft
```

<!--
- Tool looks like everywhere else
  - bean, method, description, TravelFacts
  - schema from the signature
- Per call, not per agent
  - reviewer gets tools, judge does not
- Why
  - least privilege by default
  - fewer tools - fewer wrong calls, fewer tokens
- Trip + costs in the dossier - only listPolicies needed
-->

---
class: graphic-slide
---

# Nobody wrote this order down, the planner derived it from the types

<div class="flow flow-5 flow-compact">
  <div class="flow-node"><ph-airplane-tilt /><span><code>TripReviewRequest</code></span></div>
  <div class="flow-arrow"><span>loadDossier</span></div>
  <div class="flow-node"><ph-folder-open /><span><code>TripDossier</code></span></div>
  <div class="flow-arrow"><span>draftReview</span></div>
  <div class="flow-node"><ph-file-text /><span><code>ReviewOutcome</code></span></div>
</div>

<div v-click class="reveal">
  <div class="flow flow-5 flow-turn">
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div class="flow-turn-cell"><div class="flow-arrow flow-arrow-down"></div><span>judgeDraft</span></div>
  </div>
  <div class="flow flow-5 flow-compact flow-return">
    <div class="flow-node"><ph-arrow-circle-right /><span><code>.outcome</code></span></div>
    <div class="flow-arrow flow-arrow-back"></div>
    <div class="flow-node"><ph-flag-checkered /><span><code>EmbabelReview</code></span></div>
    <div class="flow-arrow flow-arrow-back"><span>accept | revise</span></div>
    <div class="flow-node"><ph-gavel /><span><code>Judgement</code></span></div>
  </div>
</div>

<!--
- Plan found for our invocation
  - arrows = @Action, boxes = blackboard types
  - sequence never written down
- [click] Branch = the conditions
  - @Condition predicates on Judgement
  - accepted - acceptDraft, rejected - reviseDraft
  - both @AchievesGoal, produce EmbabelReview
- Why compelling
  - new action, e.g. exchange rates - used without rewiring
  - refactor a type - compiler flags misfit actions
- Trade-off
  - flow not in one place - read from the planner log
  - opposite of the Spring AI repeat
-->

---
class: graphic-slide
---

# Every new agent needs fewer new actions

<div class="reuse">
  <div class="reuse-row">
    <span class="reuse-agent">trip review</span>
    <i class="reuse-new">loadDossier</i>
    <i class="reuse-new">draftReview</i>
    <i class="reuse-new">judgeDraft</i>
    <i class="reuse-new">accept | revise</i>
  </div>
  <div v-click class="reuse-row reveal">
    <span class="reuse-agent">expense claim</span>
    <i>loadDossier</i>
    <i>trip review</i>
    <i class="reuse-new">matchReceipts</i>
    <i class="reuse-new">settle</i>
  </div>
  <div v-click class="reuse-row reveal">
    <span class="reuse-agent">visa check</span>
    <i>loadDossier</i>
    <i>trip review</i>
    <i class="reuse-new">checkVisa</i>
  </div>
</div>

<!--
- The Embabel takeaway
- First agent: four new actions
  - solid = new, outlined = reused
- [click] Second: two of four reused
  - dossier + trip review as subagent
  - RunSubagent - whole agent = one action
- [click] Third, visa check: one new action
  - planner wires an unwritten flow
- Why Embabel
  - connected by types, not drawn edges or loops
  - Spring AI: copy the loop, graph: redraw edges
- Agents 2 and 3 illustrative
-->
