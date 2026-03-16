# features.md — Velothrix Implementation Checklist
> Full feature list for Field and Lab modes. Use this as a build checklist.
> Cross-reference: CLAUDE.md (architecture), revision_a.md (UI/pentagons), revision_b.md (session arc/win conditions), HANDOFF.md (implementation notes)

---

## Field Mode

- [ ] Default view on load
- [ ] KPI strip — 5 values: w̄, Population N, Genetic Diversity, Env. Stress, Lineage Divergence
- [ ] Centerpiece: trait pentagon (left) + specimen viewer (right)
- [ ] Specimen viewer driven by live simulation data — no sliders
  - [ ] Crest hue/saturation ← p(crest)
  - [ ] Crown spine count ← spine gene mean
  - [ ] Body scale ← body size mean
  - [ ] Tail length ← dispersal allele
- [ ] Status pentagon heads the data section below centerpiece
- [ ] Three line charts: crest freq vs. predator acuity, population N, mean fitness
- [ ] Event log — field dispatch voice, italic/narrative, no parameter values
- [ ] Notification banners at critical thresholds
- [ ] Pause/resume button
- [ ] No sliders, no direct parameter access
- [ ] Event cards as sole interaction (see Event Cards below)

---

## Event Cards

- [ ] Surface at biologically meaningful thresholds only — not on timer
- [ ] 2–3 environmental/habitat choices per card — never trait choices
- [ ] Crusader Kings aesthetic — pentagons as portrait
- [ ] Split card includes: mini timeline since early warning, trait change summary, side-by-side pentagon comparison of both populations
- [ ] Card closes after decision, sim resumes in Field mode
- [ ] Choices affect habitat routing, migration corridors, refugium seeding

---

## Population Splitting & Ghost Lineages

- [ ] Early warning notification when homogeneous subgroup is forming
  - [ ] No prediction of whether split will occur — player discovers
  - [ ] Subgroup may reintegrate, trickle away, or found new colony
- [ ] Buffer simulation: founding group simulated N generations in background before split card surfaces
- [ ] Split card: player chooses which population to follow
- [ ] Ghost populations continue running at reduced cadence (every 5–10 gens)
- [ ] Ghost population visible data: N, one summary trait, Lineage Divergence, major event flag
- [ ] Ghost population extinction fires a log entry
- [ ] Reintegration: same card mechanic triggered by significant crossbreeding
  - [ ] Player decides whether to follow merged population
  - [ ] Tradeoff: genetic rescue vs. maladaptation risk visible in data

---

## Speciation

- [ ] FST proxy computed per generation between all living population pairs
  - [ ] Formula: F_ST = (H_T − H_S) / H_T
  - [ ] Displayed as Lineage Divergence % in UI
- [ ] Below threshold: interbreeding possible, migrants produce fertile offspring
- [ ] Above threshold: reproductive isolation, hybrid infertility (Dobzhansky-Muller proxy)
- [ ] Full speciation triggers pentagon zero-reset — new species becomes new baseline
- [ ] Speciation zero-reset animation: pentagon collapses and re-expands from center

---

## Lab Mode

- [ ] Full tab structure: Overview · Selection · Life History · Stats
- [ ] All three pentagons visible
- [ ] Ghost pentagon visible (Lab mode only)
  - [ ] Faint static overlay of ancestor's final pentagon shape
  - [ ] Layers across multiple speciation events
- [ ] Full chart suite (all tracked variables, not just the three in Field mode)
- [ ] Progressive disclosure — overview first, sections expandable on demand
- [ ] Scientific reference panel with equation tooltips (see below)

---

## Three Pentagons

### Trait Pentagon
- [ ] Axes: CREST · SPINE · BODY · LITTER · DISP
- [ ] Plots current population mean of each trait, normalised against viable range
- [ ] Speciation threshold rings — pulsing dashed boundary line
- [ ] Ghost overlay (Lab mode only)

### Status Pentagon
- [ ] Axes: w̄ · VIAB · DIV · STRESS · DIVERG
- [ ] Environmental Stress is relational — mismatch between trait state and antagonist state
  - [ ] High p(crest) + high Leviathan acuity = high stress contribution
  - [ ] Low litter size + low predation = negligible contribution
- [ ] Speciation threshold rings — pulsing dashed boundary line
- [ ] Ghost overlay (Lab mode only)

### Custom Pentagon (Lab Mode)
- [ ] Player-configurable — any 5 tracked variables from full dataset
- [ ] Axes reconfigurable at any time
- [ ] Configuration history logged (not discarded on reconfigure)
- [ ] Only renders if at least one axis is configured
- [ ] Radial counterpart to the comparison tool

---

## Comparison Tool

- [ ] Persistent drawer — accessible from any Lab mode tab without losing context
- [ ] X and Y axes each selectable from any tracked variable in full dataset
- [ ] Scatter plot renders immediately on axis selection
- [ ] Cross-population comparison: followed lineage as full point cloud
- [ ] Ghost lineage overlay as dimmer, sparser point cloud (data cadence respected)

---

## Antagonists

### Reactive (coevolving)
- [ ] Kelp Leviathan visual acuity — rises as p(crest) rises, tightens s_predation dynamically
- [ ] Benthic beetle population — responds to spine frequency, arms race dynamic

### Independent — Directional
- [ ] Caterpillar emergence timing — slow climate drift shifting C* (litter size optimum) over run

### Independent — Stochastic
- [ ] Drought / resource hardness events — sudden directional selection on body size, shifts θ_body
- [ ] Environmental hostility draws for dispersing founders

---

## Environmental Trend System

- [ ] Environmental variables on coordinate plane: terrain roughness, water coverage, biome type, elevation proxy
- [ ] Trends randomized each playthrough
- [ ] Sufficient noise to prevent solved-game syndrome
- [ ] Trend data visible in charts — reading them is the core player skill

---

## Stochastic Events

- [ ] Predetermined multiplicative value matrices per event type
  - [ ] Each event defines: affected axes, timescale factors, rates of change
  - [ ] Example: volcano → temperature decrease rate, cloud coverage rate, air quality rate
- [ ] Events hit multiple axes simultaneously
- [ ] Pool-drawn with complexity filter during onboarding
- [ ] Random intervals in open play
- [ ] Onboarding sequence: small event → breathing room → first major event
  - [ ] Disableable for experienced players (true random from gen 0)

---

## Win Conditions

- [ ] **Speciation** — Lineage Divergence crosses reproductive isolation threshold with all living populations
- [ ] **Predominance** — Followed lineage becomes dominant among intraspecific competitors
  - [ ] Requires lateral intraspecific competitors to be implemented
- [ ] **Predator Outlasting** — All significant antagonist predators decline to non-threatening levels
- [ ] **Resilience** — Survive a crisis event with heterozygosity above defined floor
- [ ] Win conditions are lineage-specific — ancestral population extinction is not player loss
- [ ] Game can continue indefinitely past any win condition

---

## Loss Condition

- [ ] Followed population N reaches 0 or 1 — hard extinction screen
- [ ] Player offered option to end session or continue observing remaining ghost lineages

---

## Post-Mortem

- [ ] Unlocked after any terminal event (extinction, speciation, win condition reached)
- [ ] Optional toggle: "Show full population record"
- [ ] Reveals complete per-generation data for all ghost lineages
- [ ] Full Lineage Divergence history between all population pairs
- [ ] Framed as field researcher's complete dataset
- [ ] Not available during active play

---

## Scientific Reference Panel (Lab Mode)

- [ ] All equations listed with plain-language explanations
- [ ] Every scientific term has a hoverable `?` tooltip
  - [ ] Tooltip: plain-language explanation + link to research or educational resource
  - [ ] Tooltips only in this panel — nowhere else in the UI
- [ ] Equations to include:
  - [ ] Hardy-Weinberg Equilibrium
  - [ ] Selection (dominance formula)
  - [ ] Mean fitness w̄
  - [ ] Genetic drift variance
  - [ ] Gene flow (island model)
  - [ ] Kimura fixation probability
  - [ ] Mutation-selection balance
  - [ ] Breeder's equation
  - [ ] Gaussian stabilizing selection
  - [ ] Lack clutch size optimum
  - [ ] Euler-Lotka intrinsic rate
  - [ ] Logistic growth
  - [ ] FST proxy (Lineage Divergence)

---

## Dispersal Allele Mechanics

- [ ] Dispersal allele frequency modulates split probability
- [ ] High dispersal allele carriers face elevated founding mortality
- [ ] Small founding colony N naturally brakes runaway splitting (mate-finding, drift, inbreeding)
- [ ] Nudge penalty available if natural brake proves insufficient

---

## Not Yet Designed (flagged open problems)

- Lateral intraspecific competitor design (required for Predominance win condition)
- Exact speciation FST threshold value (tuning required)
- Buffer simulation generation count (tuning required)
- Early warning confidence interval threshold (tuning required)
- Resilience win condition heterozygosity floor value (tuning required)
- Event card visual design beyond described concept
