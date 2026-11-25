---
class: graphic-slide
---

# Agents in production need what the JVM has delivered for decades

<div class="pillar-grid">
  <div class="pillar"><ph-books /><span>context</span></div>
  <div class="pillar"><ph-lifebuoy /><span>reliability</span></div>
  <div class="pillar"><ph-brackets-curly /><span>type safety</span></div>
  <div class="pillar"><ph-lightning /><span>performance</span></div>
  <div class="pillar"><ph-chart-line-up /><span>observability</span></div>
  <div class="pillar"><ph-plugs-connected /><span>integration</span></div>
</div>

<!--
- Demo agent: prompt + API key
- Production agent needs more
  - context - data, domain model, business rules
  - reliability - retries, timeouts, transactions
  - type safety - output checked by the compiler
  - performance - concurrent network calls
  - observability - what the agent did and why
  - integration - systems we already run
- Nothing new - the JVM's job for decades
-->

---
class: graphic-slide
---

# Most AI projects fail because they automate the wrong thing

<div class="compare">
  <div class="compare-card compare-bad">
    <ph-chats-circle class="compare-icon" />
    <span>replacing human interaction</span>
  </div>
  <div v-click class="compare-card compare-good reveal">
    <ph-arrows-clockwise class="compare-icon" />
    <span>boring, repetitive, time-consuming processes</span>
  </div>
</div>

<!--
- Most AI projects fail
- Automating human interaction = usually wrong
  - chatbots replacing support, sales, advisors
- [click] Boring, repetitive, time-consuming processes
- Legacy processes already on the JVM
- Need all six tiles from the start
-->
