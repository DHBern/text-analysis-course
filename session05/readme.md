# Recap

Terminology
- token, type \[-> see session02\]
- lemma \[-> see session02\[
- part of speech (POS) \[-> see session03\]

NLP pipeline \[-> see session03\]
1. Tokenisation
2. POS
3. Lemmatisation

# In addition to the recap: NER

Using SpaCy's NLP pipeline, you can do more than POS and lemmatisation. For example, you can perform Named Entity Recognition (NER). Here it is an example, you can run it in the notebook.

```python

myText = """
Across the courtesy bay the white palaces of fashionable East Egg glittered along the water, and the history of the summer really begins
on the evening I drove over there to have dinner with the Tom Buchanans. Daisy was my second cousin once removed, and I’d known Tom
in college. And just after the war I spent two days with them in Chicago.
[...]
“You live in West Egg,” she remarked contemptuously. “I know somebody there.”
“I don’t know a single—”
“You must know Gatsby.”
"""

import spacy

# load language model for English
nlp = spacy.load("en_core_web_sm")

# process the text. This is where SpaCy does all the magic.
doc = nlp(myText)

print(doc.ents) # print the list of entities

# for each entity in the document, print its details
for ent in doc.ents:
    print(ent.text, ent.start_char, ent.end_char, ent.label_, spacy.explain(ent.label_))
```

For reference, this is a tutorial on the NLP pipeline in SpaCy: https://learn.scds.ca/text-analysis-2/. Note that we are running the code in a different environment (myBinder), so you do not need to follow the installation instructions there.


# Bag of Words and TF/IDF

See notebook.


# Homework

Read:
- Blanke, Tobias, and Chloe Papadopoulou. “Keywords in Digital Humanities – A Critical Assessment of Computational Techniques for Mapping Security and Freedom in Historical Debates.” Digital Humanities Quarterly 019, no. 3 (2025). https://dhq.digitalhumanities.org/vol/19/3/000804/000804.html. \[Paragraphs: 1-32, 54-55 \]
- Hitchcock, Tim, and William J. Turkel. “Making Sense of the Emergence of Manslaughter in British Criminal Justice.” Digital Humanities Quarterly 019, no. 4 (2026). https://dhq.digitalhumanities.org/vol/19/4/000812/000812.html. \[1-34\]
  - Attention. They use F1 as a symmetric measure of overlap between two sets of documents, rather than as a measure of performance.
- Elo, Kimmo. “A Text Network Analysis of Discursive Changes in German, Austrian and Swiss New Year’s Speeches 2000-2021.” Digital Humanities Quarterly 016, no. 1 (2022). https://dhq.digitalhumanities.org/vol/16/1/000598/000598.html. \[Paragraphs: 1-7, 16-28\]




<!--
# From counting words to meaning

Look for hidden semantic structure.

Latent Semantic Analysis (LSA). Words are not independent, if they happen together they must be related in meaning: distributional hypothesis. Singular Value Decomposition (SVD). Huge sparse document matrix and factorise it, break it down. Find main axes of variation in your data: doc-topics, stregnths of topics, terms-topics. Matrix with 100'000 dimensions, but then you only keep the top dimensions that represents the strongest signal --> force the model to generalise.

Solve synonymes problem

Applications: search, document clustering, cross language information retrieval.

CONS:
- Computational cost, especially in the 90's
- Theoretical problem: assume data follows Gaussian or normal distribution. But word counts are discrete, do not work like data.

**Latent Dietricht Allocation (LDA)**

Every document is a random mix of hidden topics and each topic is a distribution over words. LDA looks at the document and try to infer the topics backwords. No need for label data, analyse how words co-occur. If some words occur often together, they should belong to the same topic.

Conceptual clustering.

Applications: research, historical patterns, event detection.

Dynamic Topic Model (DTM).

Syntax + Semantic = POS LDA, understand elements of grammar, filter out noise.


**Neural networks**
From high dimensional sparse vectors to low dimensional dense ones --> embeddings.

Model how words relate to their immediate neighbours. --> understand sequence in context.

-->

  










