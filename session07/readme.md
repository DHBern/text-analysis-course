

# Classifying variants

Can a computer sort the Frankenstein variants into our categories (**error**, **substantive**, **vocabulary**)?

### Approaches used

1. **Zero Shot Text Classification**
   - Sorting texts into categories without giving the model any labelled examples of those categories.
   - **Natural Language Inference** (NLI): does a sentence B (hypothesis) follow from sentence A (premise)? Yes, no, or can’t tell.
   - NLI is one way to do Zero Shot Classification.
2. **Few-shot prompting with a LLM**
   - Describe the categories and show a few examples to a large generative model.


### Models used
- NLI model: [BART-MNLI](https://huggingface.co/facebook/bart-large-mnli) (downloaded locally). More about it here below.
- LLM: [gpt-oss-120b](https://huggingface.co/openai/gpt-oss-120b) (via UniBE GPUStack)
- LLM: [qwen3.8-27b](https://huggingface.co/Qwen/Qwen3.8-27B) (via UniBE GPUStack)


### See notebooks
1. Zero Shot Classification
   - [only zero-shot](zero_shot_variants_all.ipynb)
     - 48% accuracy on gt_manual
     - 42% accuracy on gt_fake
   - [first identify errors](zero_shot_variants_except-errors-similarity.ipynb)
     - 48% accuracy on gt_manual
     - 37% accuracy on gt_fake
2. [Few-shot prompting with LLM](LLM-classification.ipynb)
   - 59% accuracy on gt_manual using qwen3.8-27b
   - 67% accuracy on gt_manual using gpt-oss-120b
   - 91% accuracy on gt_fake using qwen3.8-27b
   - 87% accuracy on gt_fake using gpt-oss-120b


## Ground truth

- `gt_manual.csv` = a random selection of 60 variants from the corpus, manually annotated by you
- `gt_fake.csv` = a selection of 60 simple variants from the corpus, annotated by LLM


## Homework

1. Read and run the two notebooks in this session. Mybinder does not work here, please use Noto (follow the instructions in [helpme](../helpme/notebooks.md)).

2. (optional) Read and run `LLM-classification.ipynb` (need local installation, see [helpme](../helpme/notebooks.md) and/or contact Elena).

3. Classifying paragraphs by theme: the same two approaches (zero-shot classification and few-shot prompting with LLM) on a different dataset (`data/ALL_paragraphs`, 150 paragraphs from our three novels) with new categories (**social class**, **nature**, **love**). What is in the `session07/homework` folder?
   - `gt_empty.csv`, ground truth to be annotated by you (optional)
   - `gt_fake.csv`, ground truth annotated by LLM for testing purpose
   - `homework.ipynb`, notebook to read (until the end, there are exercises) and run. As above, Mybinder does not work here, please use Noto (follow the instructions in [helpme](../helpme/notebooks.md)). 


## More about BART-MNLI
BART-MNLI is a small neural language model, trained first to rebuild damaged text, then to recognise whether one sentence follows from another. LLMs are much larger models of the same kind, trained to follow any instruction.

It’s a neural network (a **transformer**, the same architecture behind LLMs), trained in two stages.

Stage 1: pre-training BART (Facebook AI, 2019).
- The model reads a huge amount of ordinary text: books, Wikipedia, news and web pages, about 160 GB.
- The text is deliberately damaged: words are hidden, deleted or shuffled. The model learns to rebuild the original.
- Nobody labels anything: the text itself is the answer. This is called self-supervised learning.
- In doing so, the model learns how English works: vocabulary, grammar, which words go together.

Stage 2: fine-tuning on MNLI.
- MNLI (Multi-Genre Natural Language Inference) is a dataset of about 400,000 sentence pairs labelled by people as entailment, contradiction or neutral. The sentences come from ten genres: fiction, letters, government reports, phone conversations, travel guides and more.
- A small “classification head” with three outputs is added on top of BART, and the whole model is trained on these pairs. This is supervised learning: examples plus correct answers.
- Result: a model that does one thing. Given two sentences, it outputs three probabilities: entailment, contradiction, neutral.

Example
- Premise: A cat is sleeping on the sofa.
- Hypothesis: An animal is on the sofa. Label: entailment.
- Hypothesis: The cat is running in the garden. Label: contradiction.
- Hypothesis: The cat is black. Label: neutral.

Example with the variants
- Premise: In the manuscript, "know" was replaced with "have learned".
- Hypothesis: (ERROR) "known" is a mistake (word does not exist or is incorrect), corrected in "have learned". Score: 0.177.
- Hypothesis: (VOCABULARY) "have learned" means something similar to "known". Score: 0.506. 
- Hypothesis: (SUBSTANTIVE) "have learned" means something different from "known". Score: 0.317.


**Why Zero Shot Classification does not perform well here**.
- With the cat, the answer is in the premise: it says “a cat on the sofa”, so “an animal on the sofa” follows.
- With the variants, the premise only says that a word was replaced. Whether it was a mistake or a synonym isn’t written in it; the model has to know the words. NLI wasn’t trained for that, which is why the scores are often close.


With BART-MNLI, we are in the world of **deep learning**, that is machine learning with neural networks that have many layers, which learn by themselves which features of the data matter.

A **transformer** is a kind of neural network, invented in 2017 (Google, “Attention Is All You Need”). Almost all of today’s language models are transformers: BART, BERT, GPT, Qwen.

Its key idea is *attention*: to understand a word, the model looks at all the other words in the sentence and decides which ones matter for it.

Example
- He raised his arm to protect his face. → the word “arm” pays attention to “face” and “protect”: a body part.
- The soldiers laid down every arm they carried. → “arm” pays attention to “soldiers” and “carried”: a weapon.

"arm” (body) vs “arm” (weapon)
- Bag of words (session 5). Same word, same count: no difference.
- Word embeddings. One vector per word: still no difference.
- Transformer. A different vector in each sentence, depending on the context: **contextual embeddings**.

BUT, in the transformer architecture, the unit isn’t always a whole word. Transformers cut rare words into pieces: “igmmatic” might become ig + mm + atic, and each piece gets its own vector. This is also why BART-MNLI is bad at spelling.