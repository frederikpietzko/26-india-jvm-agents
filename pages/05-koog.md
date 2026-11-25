---
layout: intro
class: section-intro
---

# Koog

## the agent is a graph you can read

<!--
- JetBrains, Kotlin first, Java API
- Born in JetBrains products, open source 2025, 1.x stable
- JVM, Android, iOS, JS, Wasm
- Control flow: I draw the graph, Koog walks it
  - opposite of Embabel - every edge written down
- Same demo, ReviewOutcome, judge
- Graph by hand first, then library subgraphs
- LangGraph, but typed
-->

---
class: graphic-slide
clicks: 2
---

# The same review becomes a graph of nodes and edges

<svg class="koog-graph" viewBox="0 40 1400 420" role="img" aria-label="Koog strategy graph with a tool loop and a revision loop">
  <defs>
    <linearGradient id="kg1-gradient" gradientUnits="userSpaceOnUse" x1="0" y1="0" x2="1400" y2="0">
      <stop offset="0" stop-color="var(--fundamentals-purple)" />
      <stop offset="1" stop-color="var(--fundamentals-pink)" />
    </linearGradient>
    <marker id="kg1-head" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path class="kg-head" d="M0,0 L10,5 L0,10 z" />
    </marker>
    <marker id="kg1-head-on" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path class="kg-head kg-head-on" d="M0,0 L10,5 L0,10 z" />
    </marker>
  </defs>
  <!-- edges: one edge(...) call each, grouped by the loop they belong to -->
  <g class="kg-edge">
    <path class="kg-base" d="M130,265 L176,265" marker-end="url(#kg1-head)" />
  </g>
  <!-- tool loop -->
  <g class="kg-edge" :class="{ 'is-on': $clicks === 1 }">
    <path class="kg-base" d="M330,233 Q330,132 446,132" :marker-end="$clicks === 1 ? 'url(#kg1-head-on)' : 'url(#kg1-head)'" />
    <path class="kg-hl" stroke="url(#kg1-gradient)" pathLength="1" d="M330,233 Q330,132 446,132" />
    <text x="318" y="185" text-anchor="end">tool call</text>
  </g>
  <g class="kg-edge" :class="{ 'is-on': $clicks === 1 }">
    <path class="kg-base" d="M650,132 L726,132" :marker-end="$clicks === 1 ? 'url(#kg1-head-on)' : 'url(#kg1-head)'" />
    <path class="kg-hl" stroke="url(#kg1-gradient)" pathLength="1" d="M650,132 L726,132" />
  </g>
  <g class="kg-edge" :class="{ 'is-on': $clicks === 1 }">
    <path class="kg-base" d="M820,100 C820,40 560,40 560,96" :marker-end="$clicks === 1 ? 'url(#kg1-head-on)' : 'url(#kg1-head)'" />
    <path class="kg-hl" stroke="url(#kg1-gradient)" pathLength="1" d="M820,100 C820,40 560,40 560,96" />
    <text x="690" y="78" text-anchor="middle">tool call</text>
  </g>
  <!-- done researching -->
  <g class="kg-edge" :class="{ 'is-on': $clicks === 2 }">
    <path class="kg-base" d="M380,265 L726,265" :marker-end="$clicks === 2 ? 'url(#kg1-head-on)' : 'url(#kg1-head)'" />
    <path class="kg-hl" stroke="url(#kg1-gradient)" pathLength="1" d="M380,265 L726,265" />
    <text x="555" y="252" text-anchor="middle">text</text>
  </g>
  <g class="kg-edge">
    <path class="kg-base" d="M865,164 L865,229" marker-end="url(#kg1-head)" />
    <text x="880" y="204">text</text>
  </g>
  <!-- revision loop -->
  <g class="kg-edge" :class="{ 'is-on': $clicks === 2 }">
    <path class="kg-base" d="M1000,265 L1066,265" :marker-end="$clicks === 2 ? 'url(#kg1-head-on)' : 'url(#kg1-head)'" />
    <path class="kg-hl" stroke="url(#kg1-gradient)" pathLength="1" d="M1000,265 L1066,265" />
  </g>
  <g class="kg-edge">
    <path class="kg-base" d="M1220,265 L1266,265" marker-end="url(#kg1-head)" />
    <text x="1245" y="222" text-anchor="middle">accepted</text>
  </g>
  <g class="kg-edge" :class="{ 'is-on': $clicks === 2 }">
    <path class="kg-base" d="M1145,297 C1145,420 280,420 280,301" :marker-end="$clicks === 2 ? 'url(#kg1-head-on)' : 'url(#kg1-head)'" />
    <path class="kg-hl" stroke="url(#kg1-gradient)" pathLength="1" d="M1145,297 C1145,420 280,420 280,301" />
    <text x="712" y="428" text-anchor="middle">rejected</text>
  </g>
  <!-- nodes: every box is one node declaration in the strategy -->
  <g class="kg-node kg-terminal">
    <rect x="10" y="233" width="120" height="64" rx="32" />
    <text x="70" y="272" text-anchor="middle">start</text>
  </g>
  <g class="kg-node">
    <rect x="180" y="233" width="200" height="64" rx="14" />
    <text x="280" y="272" text-anchor="middle">review</text>
  </g>
  <g class="kg-node">
    <rect x="450" y="100" width="200" height="64" rx="14" />
    <text x="550" y="139" text-anchor="middle">run-tools</text>
  </g>
  <g class="kg-node">
    <rect x="730" y="100" width="270" height="64" rx="14" />
    <text x="865" y="139" text-anchor="middle">send-tool-results</text>
  </g>
  <g class="kg-node">
    <rect x="730" y="233" width="270" height="64" rx="14" />
    <text x="865" y="272" text-anchor="middle">structure-draft</text>
  </g>
  <g class="kg-node">
    <rect x="1070" y="233" width="150" height="64" rx="14" />
    <text x="1145" y="272" text-anchor="middle">judge</text>
  </g>
  <g class="kg-node kg-terminal">
    <rect x="1270" y="233" width="120" height="64" rx="32" />
    <text x="1330" y="272" text-anchor="middle">finish</text>
  </g>
</svg>

<!--
- Every box = one line of Kotlin
- Left to right
  - review asks the model
  - tool call - run-tools - send-tool-results
  - repeats until a text answer
  - structure-draft -> ReviewOutcome
  - judge accepts or sends back
- Two cycles
  - [click] tool loop - Spring AI: hidden in an advisor
  - [click] revision loop - Spring AI: a repeat
  - here: visible edges
- Long form first - shows what subgraphs do later
-->

---
class: code-slide
---

<div class="pillar-tag"><ph-plugs-connected />integration</div>

# Tools live in a `ToolRegistry` the agent is built with

```kotlin no-compile
class KoogTravelTools(private val facts: TravelFacts) : ToolSet {
    @Tool
    @LLMDescription("Load a submitted trip with all bookings")
    fun getTrip(@LLMDescription("The trip id") tripId: Long): TripSnapshot =
        facts.trip(tripId)
}
```

```kotlin no-compile
val tools = ToolRegistry {
    tools(KoogTravelTools(facts))
}

AIAgent(promptExecutor = executor, strategy = strategy,
    agentConfig = config, toolRegistry = tools)
```

<!--
- Like @Tool, @LlmTool - methods + descriptions
  - ToolSet groups them, also listPolicies, calculateCosts
  - same TravelFacts
- Registry = everything the agent may call
  - Kotlin DSL, same style as strategy + prompt
  - Koog executes, model only asks
  - combinable with plus - shared registries
- Alternatives
  - class based tools, typed Args
  - built-in tools: talk to user, end run
-->

---
class: code-slide code-dense
---

# Koog ships the whole tool loop as one `subgraphWithTask`

````md magic-move
```kotlin no-compile
val review by nodeLLMRequest("review")
val runTools by nodeExecuteTools("run-tools")
val sendToolResults by nodeLLMSendToolResults("send-tool-results")
val draft by nodeLLMRequestStructured<ReviewOutcome>("structure-draft")

edge(review forwardTo runTools onToolCalls { true })
edge(runTools forwardTo sendToolResults)
edge(sendToolResults forwardTo runTools onToolCalls { true })
edge(review forwardTo draft onTextMessage { true } transformed { STRUCTURE })
edge(sendToolResults forwardTo draft onTextMessage { true } transformed { STRUCTURE })
```

```kotlin no-compile
val review by subgraphWithTask<String, ReviewOutcome>(name = "review") { task ->
    task
}
```
````

<!--
- The map as code: four nodes, five edges
  - every node typed - mismatched types do not compile
- [click] What the demo ships
  - same loop as a library subgraph
  - String in, ReviewOutcome out - typed node
  - lambda: input -> task, here input = task
- Inside
  - finish tool finalize_task_result with ReviewOutcome schema
  - forced tool calls until the finish tool
  - finish tool call = structured output, no extra request
- Principle moved into the library
- Gotcha: inner nodes count against maxAgentIterations - raised to 200
-->

---
class: graphic-slide
---

# Two library subgraphs and one node of mine are the whole agent

<svg class="koog-graph" viewBox="0 160 1400 410" role="img" aria-label="Koog agent built from two library subgraphs and one custom node">
  <defs>
    <linearGradient id="kg2-gradient" gradientUnits="userSpaceOnUse" x1="0" y1="0" x2="1400" y2="0">
      <stop offset="0" stop-color="var(--fundamentals-purple)" />
      <stop offset="1" stop-color="var(--fundamentals-pink)" />
    </linearGradient>
    <marker id="kg2-head" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path class="kg-head" d="M0,0 L10,5 L0,10 z" />
    </marker>
    <marker id="kg2-head-on" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path class="kg-head kg-head-on" d="M0,0 L10,5 L0,10 z" />
    </marker>
  </defs>
  <!-- frames: the two library subgraphs, each is a single node in my graph -->
  <g class="kg-frame">
    <rect x="180" y="175" width="440" height="220" rx="18" />
    <text x="202" y="208">subgraphWithTask</text>
  </g>
  <g class="kg-frame">
    <rect x="700" y="175" width="400" height="220" rx="18" />
    <text x="722" y="208">subgraphWithVerification</text>
  </g>
  <!-- edges: library edges stay muted, the edges of my policy are drawn in -->
  <g class="kg-edge">
    <path class="kg-base" d="M130,285 L176,285" marker-end="url(#kg2-head)" />
  </g>
  <g class="kg-edge">
    <path class="kg-base" d="M380,274 L446,274" marker-end="url(#kg2-head)" />
    <path class="kg-base" d="M450,296 L384,296" marker-end="url(#kg2-head)" />
    <text x="415" y="345" text-anchor="middle">tool calls</text>
  </g>
  <g class="kg-edge">
    <path class="kg-base" d="M620,285 L696,285" marker-end="url(#kg2-head)" />
  </g>
  <g class="kg-edge is-on">
    <path class="kg-base" d="M1100,285 L1246,285" marker-end="url(#kg2-head-on)" />
    <path class="kg-hl" stroke="url(#kg2-gradient)" pathLength="1" d="M1100,285 L1246,285" />
    <text x="1173" y="268" text-anchor="middle">accepted</text>
  </g>
  <g class="kg-edge is-on">
    <path class="kg-base" d="M900,395 Q900,480 784,480" marker-end="url(#kg2-head-on)" />
    <path class="kg-hl" stroke="url(#kg2-gradient)" pathLength="1" d="M900,395 Q900,480 784,480" />
    <text x="915" y="462">rejected</text>
  </g>
  <g class="kg-edge is-on">
    <path class="kg-base" d="M580,480 Q400,480 400,399" marker-end="url(#kg2-head-on)" />
    <path class="kg-hl" stroke="url(#kg2-gradient)" pathLength="1" d="M580,480 Q400,480 400,399" />
  </g>
  <!-- nodes -->
  <g class="kg-node kg-terminal">
    <rect x="10" y="253" width="120" height="64" rx="32" />
    <text x="70" y="292" text-anchor="middle">start</text>
  </g>
  <g class="kg-node">
    <rect x="210" y="253" width="170" height="64" rx="14" />
    <text x="295" y="292" text-anchor="middle">review</text>
  </g>
  <g class="kg-node">
    <rect x="450" y="253" width="140" height="64" rx="14" />
    <text x="520" y="292" text-anchor="middle">tools</text>
  </g>
  <g class="kg-node">
    <rect x="750" y="253" width="300" height="64" rx="14" />
    <text x="900" y="292" text-anchor="middle">judge</text>
  </g>
  <text class="kg-note" x="900" y="352" text-anchor="middle">freshHistory = true</text>
  <g class="kg-node kg-mine">
    <rect x="580" y="448" width="200" height="64" rx="14" />
    <text x="680" y="487" text-anchor="middle">revise</text>
  </g>
  <g class="kg-node kg-terminal">
    <rect x="1250" y="253" width="130" height="64" rx="32" />
    <text x="1315" y="292" text-anchor="middle">finish</text>
  </g>
  <!-- legend: library versus mine, said once -->
  <g class="kg-legend">
    <g class="kg-frame"><rect x="10" y="535" width="44" height="30" rx="7" /></g>
    <text x="66" y="558">Koog library subgraph</text>
    <g class="kg-node kg-mine"><rect x="380" y="535" width="44" height="30" rx="7" /></g>
    <text x="436" y="558">my node and edges</text>
  </g>
</svg>

<!--
- Same agent as the first map - same behavior, same tests
- Tool loop + structuring = one subgraph
- Judge = subgraphWithVerification
  - no tools, fresh history - never sees the reviewer's chat
  - CriticResult: successful + feedback
- revise + its edges = my policy
  - finish on accept or MAX_REVISIONS (2)
  - first matching edge wins - order matters
- Library subgraphs first, single nodes where control is needed
-->

---
class: graphic-slide
---

# The graph is one strategy, Koog lets every agent pick its own

<div class="pillar-grid strategy-grid">
  <div class="pillar pillar-current"><ph-graph /><span>graph strategy</span></div>
  <div v-click class="pillar reveal"><ph-repeat /><span>functional loop</span></div>
  <div v-click class="pillar reveal"><ph-arrows-clockwise /><span>ReAct and chat</span></div>
  <div v-click class="pillar reveal"><ph-sparkle /><span>LLM planner</span></div>
  <div v-click class="pillar reveal"><ph-path /><span>GOAP planner</span></div>
  <div class="pillar pillar-current"><ph-tree-structure /><span>subgraphs</span></div>
</div>

<!--
- Map only, all documented
- Graph + subgraphs - today, highlighted
- [click] Functional strategy
  - suspend lambda, while loop
  - Spring AI repeat inside Koog
- [click] Predefined: single run, ReAct, chat
- [click] LLM planners
  - model writes and revises the plan, optional critic
  - flexible, only as good as the model
- [click] GOAP planner
  - preconditions, effects, A*
  - Embabel idea as a Koog strategy
- Subgraphs nest - big agents from small tested pieces
- Wrap-up
  - Spring AI: I write the loop
  - Embabel: planner finds it
  - Koog: I draw it, or pick any of the above
  - all three: inside our JVM service, same ChatModel, same traces
  - Koog on Spring AI ChatModel via koog-spring-ai
-->
