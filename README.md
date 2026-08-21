# Mise En Place 🍳

**A voice-first cooking assistant designed for hands-free, context-aware interaction in the kitchen.**

🔗 **Live concept demo:** [miseenpriseconcept.netlify.app](https://miseenpriseconcept.netlify.app/)

> **Status:** Interactive product prototype and system design concept. The current implementation focuses on the front-end experience and interaction model; the speech, recipe-processing, and conversational backend are planned in the technical roadmap.

---

## Overview

**Mise** is a voice-first cooking assistant designed around a simple interaction constraint: **a cook should not need to touch their phone while actively cooking.**

Traditional recipe applications are optimized for search, browsing, and content discovery. Mise focuses instead on the execution layer: navigating a recipe while the user's attention and hands are occupied.

The interface supports conversational commands such as:

* *"What's next?"*
* *"How much garlic again?"*
* *"Set a timer for the pasta."*
* *"Can I substitute olive oil for butter?"*

Rather than treating each request as an independent query, Mise is designed to maintain **recipe state and conversational context** throughout the cooking session.

---

## Product Problem

Cooking presents an unusual HCI problem: users frequently need information while simultaneously performing physical tasks.

Typical friction includes:

* touching a device with wet or dirty hands,
* repeatedly unlocking or scrolling through a phone,
* losing position within a recipe,
* moving between recipe, timer, and search applications,
* retrieving ingredient quantities from earlier steps,
* resolving substitutions without interrupting the cooking flow.

Mise explores whether a **stateful voice interface** can reduce that interaction overhead.

---

## Core Interaction Model

The system is designed around a small set of cooking-specific intents rather than unrestricted conversational interaction.

Example intents include:

```text
next_step
previous_step
repeat_step
get_quantity
set_timer
check_timer
substitute_ingredient
clarify_instruction
```

A cooking session maintains state such as:

```text
current_recipe
current_step
completed_steps
active_timers
ingredient_context
conversation_context
```

This allows a user to ask a side question such as:

> "How much salt was that?"

without changing their current position in the recipe.

---

## Proposed Architecture

```text
User Voice
    ↓
Speech-to-Text
    ↓
Intent / Entity Extraction
    ↓
Cooking Session Manager
    ↓
Recipe State + Context
    ↓
Response Generation
    ↓
Text-to-Speech
    ↓
Voice Response + Glanceable UI
```

### 1. Speech Layer

The speech interface would convert user input into text and support interruption or **barge-in**, allowing users to issue commands while a response is being spoken.

### 2. Intent Layer

Natural-language requests would be mapped to structured cooking actions.

For example:

```text
"How much garlic do I need again?"
```

could resolve to:

```json
{
  "intent": "get_quantity",
  "ingredient": "garlic"
}
```

This separates conversational phrasing from application logic.

### 3. Session / State Manager

The central component maintains the user's location within the recipe and coordinates side interactions such as ingredient lookups and timers.

The goal is to prevent conversational detours from destroying recipe state.

### 4. Structured Recipe Model

Recipes would be normalized into a machine-readable structure containing:

* ingredients and quantities,
* ordered preparation steps,
* timing metadata,
* ingredient-step relationships,
* optional substitutions.

A simplified structure might look like:

```json
{
  "recipe": "Pasta Aglio e Olio",
  "ingredients": [],
  "steps": [],
  "timers": [],
  "substitutions": {}
}
```

### 5. Response Layer

Responses are intentionally short and task-oriented.

Instead of behaving like a general search assistant, Mise should answer more like someone cooking beside the user:

> "Two cloves."

rather than:

> "According to the recipe, you will need approximately two cloves of garlic for this step."

---

## Interface Design

Mise follows a **voice-first, visually supported** interaction model.

The screen provides only information that benefits from being glanceable:

* current recipe step,
* ingredient quantities,
* active timers,
* cooking progress.

The visual interface reinforces the conversation rather than becoming a second interaction workflow.

A useful design test throughout the project is:

> **Could the user do this with flour on their hands?**

If not, the interaction likely depends too heavily on touch.

---

## Current Implementation

The repository currently includes:

* an interactive front-end concept,
* the primary cooking-session UI,
* interaction flows for recipe progression,
* timer and ingredient lookup concepts,
* product and interaction design documentation.

The prototype is intended to validate the interaction model before implementing the full conversational system.

---

## Technical Roadmap

* [ ] Implement speech-to-text input
* [ ] Build cooking-specific intent and entity parsing
* [ ] Define a structured recipe schema
* [ ] Implement persistent cooking-session state
* [ ] Add step-aware timers
* [ ] Implement contextual ingredient quantity retrieval
* [ ] Build ingredient substitution handling
* [ ] Add text-to-speech responses
* [ ] Support barge-in and interruption
* [ ] Parse recipes from URLs or pasted text
* [ ] Evaluate offline or on-device speech processing
* [ ] Add session recovery if the application is interrupted

---

## Engineering Questions

Mise also raises several technical problems I am interested in exploring:

**Context management:**
How much conversational history is actually necessary when the application already has structured recipe state?

**Intent reliability:**
Should common cooking commands use deterministic intent routing while more ambiguous questions fall back to an LLM?

**Latency:**
Voice interaction becomes frustrating quickly if every request requires a long model round trip. Which operations can execute locally or deterministically?

**Recipe normalization:**
Recipes across the web have inconsistent structures. How reliably can ingredient quantities, steps, timings, and dependencies be extracted into a common schema?

**Offline usability:**
Kitchens can have unreliable connectivity. Which parts of the voice pipeline could run locally without materially degrading the experience?

---

## Design Principles

1. **Hands-free by default**
   Core cooking actions should not require touch.

2. **State over chat history**
   The application should understand where the user is in the recipe rather than relying exclusively on conversational memory.

3. **Context survives interruptions**
   Side questions should never reset the primary cooking workflow.

4. **Deterministic where possible**
   Commands such as advancing a step or starting a timer should not require generative reasoning when structured application logic is sufficient.

5. **Short responses win**
   Kitchen interaction should minimize cognitive overhead.

---

## Why I Built It

Mise started from an interaction-design question:

**What would recipe software look like if it were designed around the physical act of cooking rather than around browsing recipes on a screen?**

The project lets me explore the intersection of **voice interfaces, stateful conversational systems, product design, and human-computer interaction** while working on a problem grounded in an everyday environment.

---

## About the Name

*Mise* comes from **mise en place**, the practice of preparing and organizing everything before cooking begins.

The same idea informs the product: the software should manage the information around the cooking process so the cook can focus on the food.
