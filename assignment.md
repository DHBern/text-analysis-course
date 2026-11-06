# Assignment

For the assignment, you prepare a **detailed plan** of your analysis. 

**The test for your plan**: could someone who doesn't know your project code it without asking you any questions?

**Methods**: combine at least two methods, one from sessions 1-6 and one from session 7 onwards. See the [recap](recap.md) to choose them.

## What the plan must contain

1. **Question**: what you want to find out, and what you expect.
2. **Corpus**: which texts, in which edition, from where, in which language and format, and how they should be cleaned. Attach the files.
3. **Unit of analysis**: word, sentence, paragraph, chapter or whole text. Explain how to split the texts, and why this unit fits your question: a theme can be clear in a paragraph but mixed in a chapter.
4. **Methods, step by step**: the order of the steps, the input and output of each one, and how one step feeds the next. 
5. **Settings**: every choice the code depends on, such as:
   - regular expressions: the exact pattern;
   - pre-processing: lowercase or not, tokens or lemmas, stop words, parts of speech to keep;
   - TTR and MATTR: window size;
   - TF-IDF: what counts as a document, how many top words to keep;
   - zero-shot classification: the premise and the exact hypotheses;
   - LLM: the exact prompt, the definitions, the number of examples and which ones.
   - BERTopic: `min_cluster_size`; long texts are cut, so check the length of your units.
6. **Categories**: if you classify, the name and the definition of each category, a clear example, a difficult example, and what to do with texts that fit no category.
7. **Results**: what the code must produce, for example which tables with which columns, which files.

**Level of detail**: "Use TF-IDF on the novel" is not enough. "Split the novel into chapters, lemmatise, remove stop words, calculate TF-IDF with each chapter as a document, and produce a table with the 10 top lemmas per chapter" is.

## Mandatory steps

Every plan must include a ground truth and a validation.

### 1. Create a ground truth by hand

- **Write your definitions first**: what does each category or label mean?
- **Choose a sample at random**, so that it also contains the difficult cases. For reference, we used 60 variants in session 7.
- **Annotate it yourself, before looking at the results of the methods**. Labels made by an LLM are not a ground truth: they are only useful for testing your code.
- **Note your doubts**, for example in a `Notes` column. Hard cases are often the most interesting ones.
- **Save it as a CSV file**, using the same unit of analysis as your methods, and hand it in with your plan.

### 2. Plan the validation against it

- **Classification**: accuracy. Texts shown to a model as examples can't be used for validation.
- **Extraction (regular expressions, NER, POS tagging)**: on an annotated passage, count how many results are correct and how many cases were missed.
- **Measures and rankings (MATTR, TF-IDF)**: there is no single correct score, so validate what you can. Annotate a sample by hand, for example whether the top words really characterise a document according to your reading, and check the tokens on a short passage.
- **Topics (BERTopic)**: there are no correct topics, so read the paragraphs of each topic and name it. Compare the topics with your annotated sample in a table, and run the method with other seeds to check that the topics are stable.

## Timeframe

**Detailed plan**
- Send the detailed plan to Elena by 25.11 by email (use the 20 days! Ask questions when needed). Include:
  - the detailed plan, in Markdown or PDF;
  - the corpus, each text in a separate TXT file (or a CSV file, for tabular data such as the variants);
  - the ground truth, in a CSV file.

- You will hopefully receive a coded version by 4.12. Run it and study it.

**Presentation**
- Present your work on 18.12, including the following points:
  - Interpret the results: explain what the numbers mean for your question, and what they can't show.
  - Report the results of the validation and their limits, including the size of your sample.
  - Document your choices: pre-processing, settings, model names, prompts.
  
**Self-assessment paper**
- Send to Elena by 30.01.27 a text (max 2 pages, in PDF) in which you discuss
  - what you have learned in the course and in the assignment (focus on two or three points);
  - one of the readings in [biblio.md](biblio.md) + one of the readings done during the course (or one of the talks at the Infoclio conference), if possible associated with what you have learned;
  - something that did not work out in the course or in the assignment (a limit or something that did not work out as expected; and possibly why).

