# House style

Applies while writing (Phase 4), enforced in the human pass (Phase 5, move 3) and checked at QA (Phase 7).
These are about register and craft, not SEO mechanics — but a page that reads like machine output does not get
cited, linked or trusted, whatever its schema says.

## 1. Strip the machine fingerprints

The em dash is the loudest one. Do not use `—` as a default connector. It is the single most recognisable
tell of unedited model output, and almost every use has a better replacement:

| Instead of an em dash | Use |
|---|---|
| Apposition: `the loop — about fifty lines — is small` | commas, or parentheses |
| A sharp turn: `it looks fixed — it isn't` | a full stop. Two sentences. |
| Introducing a list or explanation: `three spans — inference, memory, tool` | a colon |
| An aside you could delete | delete it, or make it its own sentence |

A page may carry one or two em dashes where nothing else does the job. Eighteen is a fingerprint.

Two things this rule does not cover. An em dash rendered by a template as an attribution prefix ("— author
name" under a pull quote) is ordinary typography, not prose, and should be left alone. Quotes are verbatim
under Hard Rule 2, so a source's own em dashes stay exactly as they were written. Count the ones you wrote.

Also cut on sight:

- **Words:** delve, leverage (as a verb), robust, seamless, landscape, realm, tapestry, testament to, elevate,
  unlock, harness (as a verb), navigate (figuratively), crucial, vital, pivotal, myriad, plethora.
  Sense matters: "harness the power of AI" is filler, "the eval harness" is the name of a thing. Do not let a
  word blacklist strip vocabulary that is doing real work — check the sense before cutting.
- **Constructions:** "It's not just X, it's Y." "In today's fast-paced…" "Let's dive in." "In conclusion."
  "It's worth noting that." Rule-of-three padding where two items would do ("faster, cheaper, and more
  reliable" when only speed and cost were measured).
- **Structural tells:** every paragraph the same length; every section built to the same template; Title Case
  On Every Heading; bold scattered for emphasis rather than for terms; emoji as section markers.

## 2. Reading level: target US grade 9

Aim for Flesch–Kincaid grade **8–10** on the prose. Measure it; do not eyeball it:

```python
import re
def fk_grade(text):
    def syllables(w):
        w = re.sub(r"[^a-z]", "", w.lower())
        n, prev = 0, False
        for ch in w:
            v = ch in "aeiouy"
            if v and not prev: n += 1
            prev = v
        if w.endswith("e") and n > 1: n -= 1
        return max(1, n)
    sents = [x for x in re.split(r"(?<=[.!?])\s+", text) if x.strip()]
    words = re.findall(r"[A-Za-z']+", text)
    return (0.39 * len(words) / len(sents)
            + 11.8 * sum(map(syllables, words)) / len(words) - 15.59)
```

Measure prose only — exclude code blocks, identifiers and tables, which wreck the score without affecting
readability.

**Do not hit the number by dumbing down the vocabulary.** A term that names a real thing (`idempotency`,
`prompt injection`, `semantic conventions`) stays, and the reader is assumed capable of it. Grade level is a
measure of *sentence construction*, not vocabulary. The levers that actually work: one idea per sentence,
average sentence under ~20 words, active voice, and killing nominalisations ("performs a validation of" →
"validates").

## 3. No clickbait. Write for a competent reader.

The audience is a practitioner deciding something, not a lead to be captured.

- **The title states the subject and the payoff.** It does not withhold them. No "You won't believe", no "This
  one trick", no "…and what it means for you".
- **No curiosity gaps.** If the page promises an answer, the answer appears above the fold, not after eight
  hundred words of throat-clearing. The direct-answer block exists to *answer*.
- **No manufactured urgency or scarcity.** No "before it's too late", no invented deadlines.
- **Numbers in titles only when the thing is genuinely enumerable** — a roundup of nine tools, not "7 secrets".
- **Never flatter the reader or the topic.** "Great question", "game-changing", "revolutionary" — all out.

Condescension is the same failure in the other direction: do not explain what an API is to an audience reading
about tool schemas.

## 4. Bullets are a data structure, not a formatting choice

Prose is the default. A bulleted list is a claim that the items are genuinely parallel and separable, and it
should be true.

**Use bullets when** the items are parallel *and* enumerable: steps in a sequence (numbered), mutually
exclusive options, a checklist, criteria, parameters, or a spec list. These are exactly the shapes an answer
engine can lift as discrete facts, which is the one place bullets beat prose for machine parsing.

**Do not use bullets for** a chain of reasoning where each step depends on the last — that is a paragraph, and
splitting it destroys the logic that made it worth reading. Nor for emphasis, nor to break up text that feels
long. Long is not the problem; shapeless is.

Two supporting rules:

- Each bullet should survive being lifted out of context and still make sense on its own. If it only reads
  correctly after the bullet above it, it belongs in prose.
- One level of nesting, maximum.

As a sanity check, a deep explainer that is mostly prose will land somewhere near one list block per major
section, not one per screen. If bullets outnumber paragraphs, the page has been formatted rather than written.
