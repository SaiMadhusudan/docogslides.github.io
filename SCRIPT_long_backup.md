# DoCoG · ECCV 2026 Spotlight — spoken script

Written up in advance, per the advisor: *"simplicity cannot be done on the fly —
you have to write it up."* Conversational, not read off the slides. `▸` = press
next (reveal a fragment / step).

**The spine.** Slides 3–6 are a ladder of questions. Each slide answers the one
the previous slide left open, and its kicker states that question out loud. If
you read only the kickers, you get the whole argument:

> what is the task → a longer question, every step correct but uncheckable →
> what if every step pointed at the image → are boxes enough?

Never explain a slide before the audience has felt the need for it. Let each
"is that enough?" hang for a beat before you answer it.

**Division of labour.** The slides are deliberately sparse — each card carries
the *claim* and the number, nothing more. The elaboration lives here and is
spoken, not read. If a slide ever starts growing a second sentence, it belongs
in this file instead.

---

## 1 · Title
"Hi, I'm presenting DoCoG — mask-based grounded chain-of-thought for document
QA. Joint work between IIIT Hyderabad and BharatGen."

## 2 · Find x
"First, a small question. Find x."
▸ *(ring)* "One way to answer is to point: here it is."
▸ *(red card)* "That's the red pill — you see **where**."
▸ *(blue card)* "The other way is to compute: x equals five. The blue pill —
you know **what**."
▸ *(Matrix offer)* "So which pill do you take?"
▸ *(big reveal, top)* "**Why not both?** Hold on to that."

## 3 · What is document question answering?
"Two examples, and that's the whole task. Here's a credit-card summary — we ask
what JCB's Net Sales is, and the model returns thirty dollars. Here's a pie
chart — which category is the largest, and it returns Housing."
"A document, a question, an answer. Over text **and** over graphics — hold on to
that second one, it comes back."
*(Don't dwell. Ten seconds. The room knows this task; you are setting a frame.)*

## 4 · “Is the Net Sales total in this image correct?”
*(Same document as the last slide — they know it, so move.)*
"That was one value. What about a longer question — one the model has to reason
through? Same table: is the Net Sales total in this image correct?"
*(the steps)* "And the system answers in steps. Look at the Net Sales column.
Read the values — twenty, twenty, twenty, fifty, thirty, twenty. They sum to a
hundred and sixty. The printed total is a hundred and fifty, so no, it's wrong."
"Now — every one of those steps is **correct**. I checked."
▸ *(key idea)* "But how would *you* check even one of them? Nothing points
anywhere. They could just as easily have been invented, and you would never
know."
*(That is the whole problem. Hold it for a beat.)*

## 5 · What if every step pointed at the image?
"So here's the idea. What if each step carried the thing it was read from —
the column header, each of those six values, the printed total?"
*(the chain, now with chips)* "Same chain, same answer. Except now every value
points at the exact region of the document it came from. You can check any one
of them."
*(the two pills)* "The chain gives you the *what*. The evidence gives you the
*where*. Grounded chain-of-thought is both, on every step."
▸ "That is the whole idea."

## 6 · Are boxes enough?
*(One picture, no text on the slide. Let them look before you say anything.)*
"One catch. If every step is going to point at a region — what shape is the
pointer? We ask which share the third segment is."
"A box covers the wedge — and a piece of both neighbours. A rectangle cannot hug
a wedge, follow curved text, or trace an arrow. A mask is exactly the wedge."
"And that is the gap: every method that grounds today does it with a box."
*(In your pocket, not on the slide: no prior resource predicts a mask; masks
cost 580 ms against 505 for boxes on one H100; ChartPoint grounds a chain but
only on charts, DOGR covers all document types but text regions only, no chain.)*

## 7 · DoCoG in one sentence
*(Opens on the figure alone. The three points land one click each — let each one
sit before you press for the next.)*
"DoCoG is mask-based, multi-type, grounded, step-wise chain-of-thought. Here it
is on a line chart: the question, then each step carrying its own evidence."
▸ *(1)* "Every step, not just the answer — a [GND] token inside every
intermediate step **and** the final answer."
▸ *(2)* "A mask, not a box — each [GND] triggers a segmentation mask, the exact
region."
▸ *(3)* "Text **and** graphics — wedges, legends, flowchart nodes, curved text."

## 8 · Food chain example (interactive — press ▶ Play)
"Let's see it. This is a food chain — a who-eats-whom diagram. We ask: who eats
penguins?"
*(▶ Play; narrate over the flow, don't read the output:)*
"It first finds the penguins — and grounds them. Then it looks at the out-arrows
from the penguins, and grounds those. The arrows point to smaller toothed whales
and leopard seals — grounds them too. And only then it answers. Every step you
just saw is backed by a mask on the image."
*(Optional, one click:)* "And each [GND] tag is clickable — click it and its
evidence lights up."

## 9 · Transition
"That was DoCoG in action. Now — how does it work?"

## 10 · Architecture (interactive — "Next step ›", presenter-paced)
"We have an image — a report about stockholdings — and a question about it.
A VLM processes the inputs."
"What's non-standard: the model's output contains extra grounding tokens — the
coloured boxes. Their hidden states go to our **grounding interaction module**.
In parallel, the image goes through a segmentation encoder, and the module fuses
the two — language on one side, segmentation features on the other."
"The enriched representations come out as **prompts** for the segmenter, which
produces the masks — the evidence for each step."
*(Don't try to do justice to every box — five minutes. Name only what's
non-standard.)*

## 11 · Data
"To train this we had to build the data. DoCoG-QA: 325 thousand documents, one
and a half million grounded QA chains — every step supervised with text **and**
a mask."
"And DoCoG-PQA: twenty thousand preference pairs — chains ranked by answer,
reasoning and grounding quality; a preferred chain against a rejected one."
▸ "For evaluation, DoCoG-Bench — 3K documents, 10K QA, human-verified."

## 12 · Training
"Two stages, using the datasets you just saw. Stage one is supervised
fine-tuning on DoCoG-QA."
▸ *(why DPO)* "Stage two refines it with preferences. Why? SFT only imitates the
data. Preferences teach the model to *prefer* a better chain — better answer,
better reasoning, better grounding. As far as we know this is the first use of
DPO in grounded document VQA."
*(In your pocket, not on the slide: the DPO is text-only — the masks stay
frozen — and it needs an anchor loss holding the grounding embeddings to the
reference model. Without it, grounding collapses: 81.9 down to 41.7.)*

## 13 · Results
"Against everyone, on answer quality and grounding quality together, DoCoG is
best on both axes — 85.8 and 81.9."
▸ "At 9B, ahead of GPT-5, Gemini 3 Pro and Claude."
▸ "And look at the bottom-right corner: the frontier models answer well and
cannot point at all. GPT-5 scores 82.9 on answers and **4.8** on grounding."
▸ "And general document QA isn't traded away for this — best of its size on
eight of the ten standard benchmarks."

## 14 · Callback (close)
"So — back to question one. What is x?"
▸ "DoCoG shows you **where** — every value points to its pixels."
▸ "And tells you **what** — x is five."
▸ *(both pills)* "Yes — it takes both."
▸ *(full-screen QR overlay, last)* "Come find us at the poster — number 113 in
the ExHall, Session 6. Thank you."

---

### If you need to cut
In order: **9** (fold the transition line into 8's last sentence), then **3**
(open on slide 4 and set the task up in one spoken sentence), then **11** (say
the dataset sizes over slide 12's opening beat). Never cut 5 or 6 — they are the
reason the paper exists.

### Rehearsed answers (know these; do not put them on a slide)
- **"Doesn't DocVQA drop?"** Yes, slightly: 93.4 against the Qwen3-VL backbone's
  96.1, and InfoVQA 82.6 against 83.1. Those are the two of ten where we are not
  best. Say it plainly — it is a small, honest cost for grounding, and the other
  eight (ChartQA 92.7, TabFact 89.4, WTQ 64.4, TextVQA 82.1, DeepForm, KLC,
  TextCaps, VisualMRC) all improve.
- **"Is there a controlled box-vs-mask ablation?"** No — and don't imply one.
  Slide 7's argument is geometric (a rectangle cannot express a wedge), plus the
  cost line. The measured claims are the build-up ablation on slide 16.
- **"Doesn't ChartPoint already do grounded CoT?"** Yes — that's exactly why it
  clears every row of slide 8's table except two: no pixel masks, and charts only.
  Never say "nobody does grounded CoT"; say "nobody does it with masks, on every
  document type".
- **The JCB chain on slide 4** is a constructed illustration on a real document,
  not a measured result. If pressed, say so.
- **Weak spots not in the deck:** CharXiv 73.1 (Gemini 3 Pro 80.3), WTQ 64.4
  (GPT-5 78.8).

### Advisor's narration rules (keep for future edits)
- Describe, don't read: *"you don't want to lean on the system output —
  system output is not how people talk."*
- Say what the document **is** ("a food chain", "a report about
  stockholdings"), never "given a document".
- Audience knows VLMs — "a VLM processes the inputs" is enough; spend words
  only on what's **non-standard**.
- Architecture as a linked list: input → what's novel → net effect.
- Motivate DPO; never sound like "we did DPO because code was available".
- Datasets before training, so the stages have context.
- Close: grounded answer → "come find out how" → QR + poster location is the
  very last reveal.
