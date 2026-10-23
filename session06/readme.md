## Back to the past
- Results of MATTR on additional corpus --> data/additional/Cather-Fitzgerald-Hemingway.pdf
- Does it make sense to lemmatise before doing TF-IDF? 

## Frankestein

See https://shelleygodwinarchive.org/
- *Frankestein*: https://shelleygodwinarchive.org/contents/frankenstein/
- One notebook: https://shelleygodwinarchive.org/contents/ms_abinger_c56/
- One page: https://shelleygodwinarchive.org/sc/oxford/frankenstein/notebook/a/#/p5

Variants extracted (XML > CSV).

<img src="exampleVariantsCategories.png">


## Manual and computational analysis
See [notebook](Variants-clustering.ipynb).

Categories discussed in the classroom:

- **error**
  - typos
  - grammatical problems

- **substantive** variation, meaning change

- language improved, better word choice, **vocabulary** (water vs lake)
  - precision
  - style, sophistication
  - more effective, punchiness
  - intensity
  - synonyms



## Homeworks

- Analyse `variants_features.csv`
  - Edit distance between A and B. A very small distance suggests a spelling correction (“unatainable” → “unattainable”). A large one suggests a real rewording. **Letter similarity** --> Kerrie
  - Length change in words. **Number of words added/removed** --> Carmiella
  - Change of **case** only --> Mateenuddin (who does these changes)
  - Same **lemma**? --> Noah

- Manually annotate 30 variants with the categories decided together (error, substantive, vocabulary) and upload them to ILIAS (exercise: Create ground truth).

- Read:
  - Suddaby, Lee, and Gordon J. Ross. “Did Mary Shelley Write Frankenstein? A Stylometric Analysis.” *Digital Scholarship in the Humanities* 38, no. 2 (2023): 750–65. https://doi.org/10.1093/llc/fqac061. \[until section 3.1, then 8 and Appendix B\]
