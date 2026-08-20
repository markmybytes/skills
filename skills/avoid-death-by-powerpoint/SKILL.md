---
name: avoid-death-by-powerpoint
description: Plan, create, review, refine, or transform presentations and slide decks from briefs, source material, existing decks, or scripts; also help produce speaker scripts and delivery plans using cognitive design principles.
---

# Avoid Death by PowerPoint

## Intent and readiness

First inspect all available input:

- The user's prompt
- An existing deck, if provided
- Source documents, data, images, and links
- The requested output

Detect two things separately: what the user has and what the user wants next.

### Readiness modes

Classify available material as one or more of:

- **Deck-ready:** An existing deck is mostly or fully prepared.
- **Content-ready:** A script, outline, or slide-by-slide content is ready to organize.
- **Source-ready:** Documents, data, links, or notes exist, but content still needs extraction or organization.
- **Brief-only:** The user provides a topic or partial brief with little source material.
- **No-material:** Only a goal or request exists, with no brief, source material, or artifact.
- **Mixed:** Multiple artifacts exist but contain inconsistencies.
- **Unclear:** Available material or its intended role is ambiguous.

### Intent modes

Classify the requested next action as one or more of:

- **Create:** Produce a missing deck, content plan, or script.
- **Improve:** Refine an existing artifact.
- **Review:** Identify problems without changing the artifact.
- **Transform:** Convert one artifact into another, such as script to deck or deck to script.
- **Prepare delivery:** Improve timing, transitions, rehearsal prompts, or Q&A preparation.
- **Research:** Fill factual, evidence, or source gaps.
- **Evaluate:** Check against visual principles or stated requirements.

If the user provides an artifact or describes preparation without explicitly requesting a next action, classify readiness but do not infer intent. Ask whether they want review, improvement, transformation, delivery support, or another action instead of creating an artifact automatically.

Presentation context is not an operation request. A topic, audience, duration, source, or statement such as “I want to present X” does not specify an artifact or goal. Ask what help the user wants before generating an outline, script, slide plan, deck, or delivery advice.

If intent is unclear, ask what the user wants next instead of choosing an artifact to create automatically.

Extract from the available input:

- **Audience:** Who will see this?
- **Goal:** What should they understand, decide, feel, or do?
- **Constraint:** Time, format, branding, accessibility, slide count, or other limits
- **Message:** What subject or meaning should the presentation communicate?
- **Operation:** Create, Improve, Review, Transform, Prepare delivery, Research, or Evaluate

Classify each item as **known**, **inferred**, **missing**, **questionable**, or **conflicting**.

- Do not ask users to repeat information already present in the input.
- If an important item is missing, request it before proceeding when it could materially change the result.
- If an item is inferred, state the assumption.
- If input is questionable—contradictory, ambiguous, unsupported, or likely mistaken—do not silently accept or correct it. State the issue, explain why it matters, and ask the user to confirm or correct it.
- If sources conflict, show the conflict and ask which source should take precedence.
- For non-critical gaps, proceed with clearly stated assumptions.
- Do not invent facts, evidence, examples, or sources without labeling them as proposed.

### Choose a workflow

Select the smallest workflow that matches intent and readiness:

- **Brief-only:** Gather consequential missing information, research when requested or necessary, propose content, then create the requested artifact.
- **No-material:** Clarify goal and audience, gather or confirm source material, then create the requested artifact.
- **Source-ready:** Extract and organize source material, flag gaps, then propose or create the requested artifact.
- **Content-ready:** Review content structure, then generate or refine the deck.
- **Deck-ready:** Review or improve the deck, then offer script or delivery support when requested.
- **Mixed:** Compare artifacts, flag conflicts, and ask which source takes precedence before transforming or merging them.
- **Any mode with research intent:** Identify unsupported claims and research only the gaps relevant to the presentation goal.

Do not generate an additional artifact unless requested or explicitly approved as the next step.

## Content design

- Give every slide one job: orient, explain, compare, show evidence, demonstrate, instruct, summarize, or prompt discussion.
- Choose an organization that fits the subject and goal. Do not impose one narrative pattern on every presentation.
- Titles categorize or summarize slide content. They do not need to be conclusions. Multiple slides may share a title when they cover the same topic or section.
- Make titles accurate and useful for orientation; avoid titles that are vague or unrelated to slide content.
- Keep facts, claims, examples, and recommendations distinguishable.
- Remove content unrelated to the slide's job, even when it is interesting or factually correct.
- Do not split slides merely to satisfy the Rule of Six; preserve understandable grouping.

## Visual principles

Apply principles in [references/visual-principles.md](references/visual-principles.md) to every slide.

## Agent workflow

1. Complete intent and readiness recognition, including the user's explicitly requested output.
2. If no explicit operation is requested, stop after recognition and ask a targeted question. Do not generate an artifact or unsolicited presentation advice.
3. Resolve consequential missing, questionable, or conflicting information before proceeding.
4. If an existing deck is available, parse its accessible titles, text, speaker notes, visuals, charts, and structure only when the task needs deck analysis; identify inaccessible or ambiguous content. Skip deck-specific parsing when no deck exists.
5. Branch by intent:
    - **Review / Evaluate:** Analyze slides against [references/visual-principles.md](references/visual-principles.md) and the slide's content-design role, and report findings only. Do not change or refine anything unless the user asks for changes.
    - **Create / Transform / Improve:** Produce the requested artifact only. Review against the visual principles as you work, but do not generate extra artifacts.
    - **Research:** Research only the factual, evidence, or source gaps relevant to the presentation goal; do not automatically make a deck.
    - **Prepare delivery:** Provide timing, transitions, rehearsal prompts, and Q&A support; keep them separate from slide text and script content.
6. For each suggested or revised slide, provide:
    - **Text**
    - **Visual/Image**
    - **Size hierarchy**
    - Optional **Rationale**
7. Approval gates are conditional: ask before creating a materially different next artifact, or when consequential assumptions or conflicts remain. Do not ask for approval after every artifact.
8. Generate a point-form script only when requested or explicitly approved as the next step:

    ```md
    [Slide 1: Title]
    - Speaking point
    - Explanation, example, or interpretation
    - Emphasis or transition
    ```

    Keep script points additive rather than repeating slide text verbatim. Scripts remain opt-in.
9. When helping with delivery, remind the user: **“You are the presentation. PowerPoint is merely your visual aid.”** Use the reminder only in delivery support, not after every task.

## Output standard

- Preserve the user's meaning and facts.
- Do not add decorative elements without a communication purpose.
- Prefer fewer elements when fewer are sufficient.
- Keep any later script separate from slide text.
- Flag assumptions, unsupported claims, inaccessible content, and unresolved conflicts.
- Do not invent presentation goals, narrative structures, artifacts, evidence, or rigid text-count rules.
- Avoid generic praise, slogans, and closing advice unless requested or useful to the task.
