# Recap

Terminology
- token, type \[-> see session02\]
- lemma \[-> see session02\]
- part of speech (POS) \[-> see session03\]
- stopwords \[-> see session02\]

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

See [notebook](BoW-TFIDF.ipynb).


# Homework

Play around with the code in the [notebook](BoW-TFIDF.ipynb):
- look for another word, instead of "voice" and "daisy";
- change the number of words to display for each paragraph in 'Document tagging'.

Think about combining lemmatisation with TF/IDF: calculate TF/IDF on the lemmas, instead of on the tokens. Would that constitute a gain? You can compare the results in the last section of the notebook (Lemmatisation + TF/IDF).



Read one of the following and answer these questions:
- What is TF-IDF used for? Explain, providing one example.
- What are the pros and cons of TF-IDF?

Readings:
- Blanke, Tobias, and Chloe Papadopoulou. “Keywords in Digital Humanities – A Critical Assessment of Computational Techniques for Mapping Security and Freedom in Historical Debates.” Digital Humanities Quarterly 019, no. 3 (2025). https://dhq.digitalhumanities.org/vol/19/3/000804/000804.html. \[Paragraphs: 1-32, 54-55 \]
- Hitchcock, Tim, and William J. Turkel. “Making Sense of the Emergence of Manslaughter in British Criminal Justice.” Digital Humanities Quarterly 019, no. 4 (2026). https://dhq.digitalhumanities.org/vol/19/4/000812/000812.html. 
  - Attention. They use F1 as a symmetric measure of overlap between two sets of documents, rather than as a measure of performance.
- Elo, Kimmo. “A Text Network Analysis of Discursive Changes in German, Austrian and Swiss New Year’s Speeches 2000-2021.” Digital Humanities Quarterly 016, no. 1 (2022). https://dhq.digitalhumanities.org/vol/16/1/000598/000598.html. \[Paragraphs: 1-7, 16-28\]









