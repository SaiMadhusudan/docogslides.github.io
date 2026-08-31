# DoCoG · ECCV 2026 — spoken script (~4 min)

Written up in advance, per the advisor: *"simplicity cannot be done on the fly —
you have to write it up."* Conversational, not read off the slides. `▸` = press
next (reveal a fragment / step).

---

## 1 · Title
"Hi, I'm presenting DoCoG — grounded chain-of-thought for document QA. Joint
work between IIIT Hyderabad and BharatGen."

## 2 · Find x
"First, a small question. Find x."
▸ *(ring)* "One way to answer is to point: here it is."
▸ *(red card)* "That's the red pill — you see **where**."
▸ *(blue card)* "The other way is to compute: x equals five. The blue pill —
you know **what**."
▸ *(Matrix offer)* "So which pill do you take?"
▸ *(big reveal, top)* "**Why not both?**"

## 3 · The answer is right. Is every step?
*(Opens on just the document and the question — let them look.)*
"This is a credit card summary. And we ask a checking question: do the Net
Sales values actually add up to the printed total?"
▸ *(Today card)* "A system today reads down the column and tells you: twenty,
twenty, twenty, fifty, fifty, twenty — that's 180, not 150, so the total is
wrong. And that answer is **correct**."
▸ *(red underline)* "But look at the fifth value."
▸ *(the doubt)* "Fifty is JCB's *Sales*. Its Net Sales is thirty. The answer
came out right, one step is invented — and nothing in the reply tells you
which one to trust."
▸ *(DoCoG card)* "DoCoG answers the same question — but every value it names
is anchored to the cell it came from. You can check any of them."
▸ *(pills)* "And the same holds for charts, tables, documents, flowcharts."

## 4 · Previous work — and what it can't do
*(Three cards cascade in on their own; the fourth slot stays empty until you
click — let it hang for a beat.)*
"So how has this been tried? Most systems don't ground the reasoning at all —
there is simply nothing to check."
"Some ground only the final answer — DLaVA, DocVXQA — so every intermediate
step stays unchecked."
"Others do ground each step, but with **bounding boxes** — DOGR, Qwen3-VL,
ChartPoint, and that one only on charts. And look what a box does to a pie
wedge: it covers the wedge *and* a piece of both neighbours. A rectangle
cannot hug a wedge, follow curved text, or trace an arrow."
▸ *(DoCoG lands)* "DoCoG grounds every step **and** the answer, with the exact
region — pixel masks, over graphics as well as text."
▸ *(pills)* "Everything before hands you one pill. We hand you both."

## 5 · Food chain example (interactive — press ▶ Play)
"Let's see DoCoG in action. This is a food chain — a who-eats-whom diagram.
And we ask it: who eats penguins?"
*(▶ Play; narrate over the flow, don't read the output:)*
"It first finds the penguins — and grounds them. Then it looks at the
out-arrows from the penguins, and grounds those. The arrows point to smaller
toothed whales and leopard seals — grounds them too. And only then it answers:
smaller toothed whales, leopard seals. Every step you just saw is backed by a
mask on the image."
*(Optional, one click:)* "And each [GND] tag is clickable — click it and its
evidence lights up."

## 6 · Transition
"That was DoCoG in action. Now — how does it work?"

## 7 · Architecture (interactive — "Next step ›", presenter-paced)
"We have an image — a report about stockholdings — and a question about it.
A VLM processes the inputs."
"What's non-standard: the model's output contains extra grounding tokens —
the coloured boxes. Their hidden states go to our **grounding interaction
module**. In parallel, the image goes through a segmentation encoder, and the
module fuses the two — language on one side, segmentation features on the
other."
"The enriched representations come out as **prompts** for the segmenter, which
produces the masks — the evidence for each step."
*(Don't try to do justice to every box — five minutes. Name only what's
non-standard.)*

## 8 · Two datasets
"To train this we built two datasets. DoCoG-QA: one and a half million
grounded QA chains over 325K documents — every step supervised with text and
a mask."
▸ "And DoCoG-PQA: twenty thousand preference pairs — chains ranked by answer,
reasoning, and grounding quality; a preferred chain versus a rejected one."
▸ "For evaluation, DoCoG-Bench — human-verified."

## 9 · Training
"The model is trained in two stages, using the datasets you just saw. Stage
one is standard supervised fine-tuning on DoCoG-QA."
▸ "Stage two refines it with preferences — DPO on DoCoG-PQA. Why? SFT only
imitates the data. Preferences teach the model to *prefer* better chains —
better answers, better reasoning, better grounding. And it's text-only: the
masks stay frozen."

## 10 · Results
"Evaluated on answer quality and grounding quality together, DoCoG gets the
best of both worlds — it outperforms state-of-the-art open-source models and
the commercial ones."
▸ "And it's a 9B model — ahead of GPT-5, Gemini 3 Pro, Claude, which are far
larger."
▸ "General QA is unaffected."

## 11 · Callback (close)
"So — back to question one. What is x?"
▸ "DoCoG shows you **where** — every value points to its pixels."
▸ "And tells you **what** — x is five."
▸ *(both pills)* "Yes — it takes both."
▸ *(full-screen QR overlay, last)* "To find out how you can take both — come
find us at the poster: number 113 in the ExHall, Session 6, 3 PM. Thank you."

---

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
