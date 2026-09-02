---
name: yanliudreamer-design-methodology
description: Use when turning an idea, product, or visual reference into an AI-native design direction, prototype plan, product teardown, portfolio case, or reusable design Skill grounded in explicit problems, context, and testable visual rules.
metadata:
  short-description: AI-native design method from taste to reusable system
---

# Yanliudreamer Design Methodology

Use this skill to move from **Taste → System → Prototype → Test → Artifact → Distribution**. It distills a public research corpus into a reusable design workflow; it is not a request to imitate a person or copy a reference.

## Research basis

The method was synthesized from the public work and authenticated Edge reading of @yanliudreamer’s X `Posts` and `Replies` timelines on 2026-09-01, plus the author’s public Mono-color and Product Teardown Skills. That Edge session yielded 607 status links in Posts; a later Replies retry exposed 1,381 raw links, and a direct date-window search produced 686 dated author status IDs after deduplication, before the timeline became blank or stalled again. To test one-by-one access, 221 official status URLs were attempted in Edge and 80 same-ID public mirror URLs were tested for supplemental recovery; this validates the reading path but does not expand the formal Posts count. The profile displayed 3,589 posts; this is a bounded synthesis rather than a claim that every account activity item was reviewed. Treat the method as an independent framework, preserve attribution in research notes, and never copy distinctive compositions, wording, logos, or brand assets.

## When to use

Use for AI product design, visual direction, design-system extraction, rapid prototypes, product teardowns, portfolio cases, or turning repeated design judgment into a Skill. Do not use it for a generic logo request, unconstrained image generation, or a simple copy edit unless the user also asks for the system behind the result.

## Required workflow

1. **Write the brief.** State user, desired outcome, context, constraints, success signals, non-goals, and delivery format.
2. **Collect references.** Use 3–6 references. Extract transferable properties; never copy a distinctive composition, wording, logo, or brand asset.
3. **Encode the design language.** Specify palette, typography, spacing, layout grammar, imagery/texture, motion, content hierarchy, voice, and accessibility rules. Replace vague adjectives such as “premium” with observable rules.
4. **Design AI-native collaboration.** Define the agent’s role, context sources, memory boundary, proactive/waiting behavior, human-control points, handoff, and failure fallback. Ask whether AI changes the problem itself or merely adds a feature.
5. **Build one vertical slice.** Make one end-to-end task work before adding breadth. Treat the AI as an implementation partner, while the human owns the problem, priorities, taste, and acceptance criteria.
6. **Test with evidence.** Run the task, experience, and system checks. Record each issue as `expected → actual → impact → next change`; keep at least three representative cases.
7. **Package the result.** Choose a report, portfolio case, design system, prototype, or Skill. Include inputs, rules, examples, quality gates, and known limits so another person can reuse it.
8. **Publish and learn.** Convert one project into platform-appropriate short posts, long-form reasoning, visual evidence, and reusable assets. Feed audience feedback back into the system.
9. **Protect judgment with a production system.** Batch high-friction work with SOPs or AI, while keeping problem framing, context, taste, trade-offs, and acceptance criteria human-owned.

## Product analysis frame

When analyzing a product, answer in this order:

1. What job is the user hiring it to do, and what does it replace?
2. What is the smallest core loop from trigger to result to return?
3. What objects, states, interfaces, integrations, and constraints make the loop work?
4. Where is AI assistive, embedded, or autonomous? Give evidence and identify the human control point.
5. Where does the experience leak: cold start, middle funnel, trust, latency, context, or advanced-user ceiling?
6. What 3–5 next opportunities could actually ship, and why?

## Visual-system minimum

Do not deliver a visual direction until these fields are explicit:

```yaml
palette: {primary: "", secondary: "", accent: "", background: "", contrast_rule: ""}
type: {display: "", body: "", hierarchy: "", line_height: ""}
layout: {grid: "", spacing_scale: "", whitespace_rule: "", density: ""}
imagery: {crop: "", texture: "", source_rule: ""}
motion: {entrance: "", feedback: "", transition: ""}
content: {hierarchy: "", voice: "", metadata: ""}
```

Add `ai_collaboration` fields when AI is part of the product or workflow:

```yaml
ai_collaboration:
  agent_role: ""
  context_sources: []
  human_control_points: []
  fallback: ""
```

## Quality gate

Before finishing, verify that the problem is concrete, the reference has been abstracted, the rules are observable, one vertical slice runs, the result has been tested with representative cases, and the output is original rather than a visual imitation. If evidence is missing, label the claim as an inference or limitation.

## Text-heavy poster iteration

When the vertical slice is a generated event poster or another text-heavy raster asset:

- Treat exact copy, thumbnail legibility, and factual restraint as acceptance criteria alongside visual style.
- If a first pass is weak, preserve the copy, palette, and composition and change one causal group of visual variables at a time; record expected → actual → next change.
- If official event details or brand assets were not supplied, label the output as a concept and do not invent dates, venues, URLs, sponsors, or logos.
- Keep the final prompt and the iteration note with the artifact so the next pass can improve a rule rather than merely add decoration.

See [05-海报实例复盘.md](./05-海报实例复盘.md) for a worked example.
