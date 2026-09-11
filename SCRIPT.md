

### 1 · Title
"DoCoG — grounded chain-of-thought for document question answering. IIIT
Hyderabad and BharatGen."

*(Seven seconds. Don't read the author list.)*

---

### 2 · Find x.
"A small question first. Find x."

▸ "You can point at it — here it is. That's **where**."

▸ "Or compute it — x equals five. That's **what**."

▸ ▸ "One pill each."

▸ "Why not both?"

---

### 3 · What is document question answering?
"The task, in two examples. What is JCB's Net Sales? Thirty dollars. Which
category is largest? Housing."

"A document, a question, an answer — over text **and** graphics."

*(Ten seconds. They know this; you're setting a frame.)*

---

### 4 · "Is the Net Sales total in this image correct?"
"Now a longer question on the same table."

"It answers in steps: reads the Net Sales column, the six values, sums them to a
hundred and sixty — and the printed total says a hundred and fifty."

"Every one of those steps is correct."

▸ "But how would you check one? Nothing points anywhere. They could just as
easily be invented, and you'd never know."

*(Pause. This is the problem the paper exists for.)*

---

### 5 · What if every step pointed at the image?
"So what if each step carried the thing it was read from?"

"Same chain, same answer — but every value now points at the region it came
from. Check any of them."

"The chain is the *what*. The evidence is the *where*."

▸ "Both, on every step."

---

### 6 · Are boxes enough?
*(One picture. Let them look before you speak.)*

"One catch. If every step points at a region — what shape is the pointer?"

"A box takes the wedge, and a piece of both neighbours. A mask is exactly the
wedge."

"That's the gap: everything that grounds today uses a box."

---

### 7 · DoCoG in one sentence
"DoCoG is mask-based, multi-type, grounded, step-wise chain-of-thought."

▸ "Every step, not just the answer."

▸ "A mask, not a box."

▸ "Graphics as well as text."

---

### 8 · Food chain  *(press ▶ Play, then mostly stop talking)*
"Let's watch it. A food web — who eats penguins?"

*(▶ Play. Narrate lightly over the flow; don't read the output:)*

"It finds the penguins, follows the out-arrows — smaller toothed whales, leopard
seals. Then it answers."

"Every step backed by a mask on the image."

---

### 9 · Transition
"So how does it work?"

---

### 10 · Architecture  *("Next step ›", presenter-paced)*
"A report about stockholdings, and a question. A VLM processes both."

"What's non-standard: the output carries grounding tokens, whose hidden states
go to our grounding module and fuse with segmentation features."

"Out come prompts for the segmenter — and the segmenter gives you the masks."

*(Don't do justice to every box. Name only what's new.)*

---

### 11 · Data
"Nothing existed to train this. DoCoG-QA — 1.5 million grounded QA over 325,000
documents, every step supervised with text and a mask."

▸ "DoCoG-PQA — 20,000 preference pairs."

▸ "And DoCoG-Bench to measure it, human-verified."

---

### 12 · Training
"Two stages. One: supervised fine-tuning on DoCoG-QA."

▸ "Two: DPO. SFT only imitates — preferences teach it to *prefer* a better chain."

---

### 13 · Results
"Both axes at once — answers and grounding. DoCoG is best on both: 85.8 and 81.9."

▸ "At nine billion parameters. Ahead of GPT-5, Gemini 3 Pro, Claude."

▸ "GPT-5 answers at 82.9 — and grounds at 4.8. They answer. They can't point."

▸ "General QA isn't traded away."

---

### 14 · Close
"So — what is x?"

▸ "DoCoG shows you **where** — every value points at its pixels."

▸ "And **what** — x is five."

▸ "It takes both pills."

▸ *(QR)* "Come find us at the poster, 106 in the ExHall. Thank you."

---

## If asked  *(know these; none are on a slide)*

- **Does DocVQA drop?** Slightly — 93.4 against the Qwen3-VL backbone's 96.1,
  InfoVQA 82.6 against 83.1. Those are the two of ten where we're not best. Say
  it plainly; the other eight improve.
- **A controlled box-vs-mask ablation?** No — don't imply one. Slide 6 is a
  geometric argument. Masks cost 580 ms against 505 for boxes, on one H100.
- **Doesn't ChartPoint already do grounded CoT?** Yes — on charts only. DOGR
  covers every document type but grounds text regions only, no chain. Never say
  "nobody does grounded CoT"; say "nobody does it with masks, on every document
  type".
- **The anchor loss?** DPO here is text-only, masks stay frozen, and it needs an
  anchor loss on the grounding embeddings. Without it grounding collapses:
  81.9 → 41.7.
- **Removing CoT** costs 12.5 points of answer quality: 85.8 → 73.3.
- **Slide 4's chain** is a constructed illustration on a real document, not a
  measured result. Say so if pressed.
- **Weak spots not in the deck:** CharXiv 73.1 (Gemini 3 Pro 80.3), WTQ 64.4
  (GPT-5 78.8), TabMWP 80.7 (SynTab-LLaVA 88.3).

---

## Delivery rules (from the advisor)

- Describe, don't read. "System output is not how people talk."
- Say what the document **is** — "a food chain", "a report about stockholdings" —
  never "given a document".
- The room knows VLMs. "A VLM processes the inputs" is enough; spend words only
  on what's **non-standard**.
- Motivate DPO. Never sound like "we did DPO because the code was there".
- Datasets before training, so the stages have something to act on.
- Let each question hang for a beat before you answer it.
- If a slide starts growing a second sentence, that sentence belongs here instead.

## If you need to cut
Drop **9** (fold the transition into 8's last line), then **3** (set the task up
in one spoken sentence over slide 4). Never cut 5 or 6 — they are the reason the
paper exists.
