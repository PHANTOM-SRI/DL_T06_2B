# Annotation Guide – Romanized Telugu–English Film Review Sentiment (Task 2B)

Team 6 · MGIT Deep Learning

## 1. Unit of annotation
One review (a sentence or short paragraph written in Romanized Telugu mixed with English).

## 2. Labels

| Label | Definition | Typical cues |
|---|---|---|
| **Positive** | The review expresses only favourable opinions about the film or its parts. | *chala bagundi, super ga undi, excellent, paisa vasool, must watch* |
| **Negative** | The review expresses only unfavourable opinions. | *boring ga undi, nachaledu, baaledu, waste of money, avoid* |
| **Mixed** | The review contains **at least one clearly positive and at least one clearly negative** opinion about the film or its parts. | contrast words: *kani, kaani, but, ayithe* |

## 3. Rules
1. **Negation flips polarity.** "boring ga ledu" is positive; "bagaledu" is negative.
2. **Sarcasm follows the intended meaning**, not the surface word. "super ga undi, nidra vachesindi" (it is super, I fell asleep) is Negative.
3. **Final complaint counts.** A long review that praises something early and complains at the end is Mixed, never Positive.
4. **Neutral filler is ignored** (viewing context, runtime, ticket price, who the reviewer went with).
5. **Hedged praise** ("konchem bagundi" = "a little good") combined with stronger complaints is labelled Mixed and **flagged as ambiguous**, because annotators could reasonably call it Negative.
6. **Spelling variants carry the same meaning** (*bagundi / baagundi / bagundhi*, *chala / chaala*).
7. Do not label by the reviewer's star rating or by the film's reputation.

## 4. Edge cases
| Case | Decision |
|---|---|
| One positive and one negative aspect | Mixed |
| Three praises and one mild complaint | Mixed |
| Only a neutral statement | Not included in this dataset |
| Unclear sarcasm | Flag as ambiguous |

## 5. How labels were produced in this submission (important)
**No human annotation was performed.** Labels are assigned by the generation rule that built each review (see Section 2 of the notebook). This guide documents the intended label semantics, and is the guide that human annotators would follow if the dataset were extended with real reviews. Because no second annotator labelled the data, **inter-annotator agreement was not measured**.
