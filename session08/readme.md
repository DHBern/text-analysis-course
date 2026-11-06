# LLM classification and BERTopic

## Notebooks

- [Few-shot prompting with an LLM](LLM-classification.ipynb): classifying the Frankenstein variants.
- [BERTopic](BERTopic.ipynb): finding topics in the paragraphs of our three novels, with exercises.

# Questions from the homework of session 07

## Is BART-MNLI closer to an LLM-made ground truth than to a human one?

> I worked with the "fake" ground truth only so far. Since this was created by a LLM too, my hypothesis is that it might lead to more matches than a human-created truth (since I presume both models use similar tactics to classify, though I might be way off in that assumption)

Good hypothesis! Let's test it. 

**Frankestein's variants**

Does BART-MNLI score higher on `gt_fake` than on `gt_manual`? These are the results of the [zero-shot notebook](../session07/zero_shot_variants_all.ipynb):

| BART-MNLI | `gt_fake` (LLM) | `gt_manual` (human) |
|---|---|---|
| Accuracy | 42% | 48% |
| Correct on error | 0 of 20 | 0 of 21 |
| Correct on substantive | 8 of 20 | 4 of 12 |
| Correct on vocabulary | 17 of 20 | 25 of 27 | selected to be simple.
| Average of the three categories | 42% | 42% |

The score is lower on `gt_fake` or, if each category counts the same, the two scores are identical.

Attention: the two files contain different variants and `gt_fake` has a selection of "simple" variants. In both cases, zero-shot text classification is not an ideal method here, the results are low.

**Paragraphs**

Zero-shot classification works much better here, because the answer is in the text (e.g., a paragraph about a meadow does support "this text is about nature"; the premise of a variant only says that a word was replaced, and the answer isn't written in it).

`gt_fake.csv` leaves 38 of the 150 paragraphs empty. BART-MNLI and gpt-oss-120b give the same answer on 82% of the labelled paragraphs (87 of 106), but only on 34% of the empty ones (13 of 38). They converge when the theme is obvious, as a human reader probably would too, so this agreement doesn't need "similar tactics".

For paragraphs, we do not have anything human to test the hypothesis.

**BART-MNLI vs LLM**

BART-MNLI and the LLMs are both transformers, and they both learned from large amounts of text.

But their tasks are very different:
- BART-MNLI scores whether a hypothesis follows from a premise (fine-tuned on sentence pairs labelled by people; it does not follow instructions, see examples nor explain anything).
- An LLM reads the tokens in the definitions and in the examples and writes some new tokens. It is trained on a much larger dataset.

-> From their architecture, we cannot assume that they classify in a similar way.

**Accuracy vs correctness**

Accuracy measures agreement with a ground truth, not correctness.

If the ground truth comes from a model, a high score means "agrees with that model".

`gt_fake` is useful for checking that the code works to a certain extent, but not for evaluating a method.

