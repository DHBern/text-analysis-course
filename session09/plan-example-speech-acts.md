# Example plan: do characters give orders or make requests in the Grimm fairy tales?

This is an example of a detailed plan, coded in [speech_acts.ipynb](speech_acts.ipynb). 

## 1. Question

What do the characters *do* when they speak: do they give orders, make requests, ask questions...? Do female and male characters do different things with their words? For example, do male characters give more orders and female characters make more requests?

## 2. Corpus

- **Texts**: 22 tales from *Grimms' Fairy Tales* by Jacob and Wilhelm Grimm, translated by Edgar Taylor and Marian Edwardes, Project Gutenberg eBook #2591 (https://www.gutenberg.org/ebooks/2591). Only tales with a female protagonist were selected.
- **Language**: English.
- **Format**: TEI XML, one file per tale, in `data/fairy-tales/`, file names starting with a number (pattern `[0-9]*.xml`). Do not use `GrimmFairyTales_EN_Gutenberg_female.xml`, which contains the whole collection.
- **Encoding already in the files**: every speech is in `<said who="#id">`, and the header (`<listPerson>`) gives the `gender` of each id. Thoughts are `<said aloud="false">`. There are no `<said>` inside other `<said>`.

## 3. Unit of analysis

The **speech**: what one character says in one turn of the dialogue.

Usually a speech is one `<said>`. But when the narrator interrupts it, it is split into two `<said>`:

> ‘Pray lift me upon chair,’ *said he to the princess,* ‘and let me sit next to you.’

The second half alone (‘and let me sit next to you.’) is not a complete request, and the first half alone is not either. So two `<said>` are **merged** into one speech if they have the same `who` and the narration between them is 5 words or fewer. Without merging, 687 `<said>`; after merging, fewer speeches (the notebook prints the number).

Why the speech and not the sentence: a character does one thing in a turn more often than in a sentence (*‘Quick, Rose-red, open the door, it must be a traveller who is seeking shelter.’* is one order with its reason).

## 4. Methods, step by step

1. **Find the files** (pattern above). Output: 22 files.
2. **Read the genders.** For each file, read the header with `xml.etree.ElementTree`: for every child of `<listPerson>`, store `xml:id` → `gender`.
3. **Split the tale into narration and speeches.** Read the file as plain text, keep only what comes after `<body>`, and split it with the regular expression in point 5 (as in [speech_verbs.ipynb](speech_verbs.ipynb)). Remove the XML tags, the line breaks and multiple spaces.
4. **Merge the interrupted speeches** (rule in point 3). Output: one row per speech, with `speech_id` (a number starting at 0 over all the tales), `tale` (file name without extension), `who` (as in the file, e.g. `#redcap-wolf`) and `speech` (the text, with the quotation marks `‘ ’` removed).
5. **Classify each speech** with zero-shot classification (settings in point 5). Output: for each speech, the score of each category, the predicted category (highest score) and the confidence (score of the winner).
6. **Make the speaker table.** One row per speech and speaker (one speech has two speakers: split `who` at spaces), with the gender of the speaker.
7. **Count.**
   - the number of speeches per predicted category;
   - for each gender, the percentage of its speeches in each category. Percentages, not counts, because the genders have a different number of speeches;
   - the 10 characters who give the most orders, with their gender.
8. **Examples.** A function that prints the speeches of a given category, optionally only for one gender: speaker, confidence, speech.

## 5. Settings

- **Regular expression for speeches**: `<said[^>]*who="([^"]+)"[^>]*>(.*?)</said>`, with the flag `re.S`.
- **Merging**: same `who`, narration between ≤ 5 words (counted with `split()`).
- **Model**: `facebook/bart-large-mnli`, with the `transformers` pipeline `zero-shot-classification`.
- **Premise**: the text of the speech only, without quotation marks and **without** the reporting clause. The reporting verb (*asked*, *begged*, *commanded*) would help the model, but we want to know if the model can recognise the category from the words of the character. (Using it is an exercise.)
- **Hypotheses** (one per category, `hypothesis_template="{}"`):

  | Category | Hypothesis |
  |---|---|
  | order | *This is an order.* |
  | request | *This is a request.* |
  | question | *This is a question.* |
  | statement | *This is a statement.* |
  | promise/threat | *This is a promise or a threat.* |
  | exclamation | *This is an exclamation.* |

  These short hypotheses were chosen after a test on 17 speeches chosen by hand (15 correct). Longer ones (*The speaker gives an order.*, *The speaker expresses a feeling.*) did worse: 7 to 11 correct. The test speeches are not in the ground truth, otherwise we would evaluate on the data we used to choose the hypotheses.
- **One category per speech**: `multi_label=False`, the scores sum to 1, the highest wins.
- **Uncertain**: confidence below 0.4 (arbitrary, 6 categories → 1/6 = 0.17 would be pure chance).

## 6. Categories

Inspired by the speech acts of Searle (1976), simplified. **One category per speech: the main thing the speaker does.** If a speech does two things, the category is the one that asks something of the listener (order, request, question) before the others, because it is the one that moves the story on.

| Category | Definition | Clear example | Difficult example |
|---|---|---|---|
| `order` | the speaker tells the listener to do something, and expects to be obeyed | ‘Quick, Rose-red, open the door’ | ‘Put your plate nearer to me, that I may eat out of it.’ (the frog to the princess: an order, because she must keep her promise) |
| `request` | the speaker asks the listener to do something, and the listener can refuse | ‘Pray lift me upon chair’ | ‘Let me speak with the bridegroom.’ (imperative form, but polite: request); ‘Dear God, do help us’ (a prayer: request) |
| `question` | the speaker asks for information | ‘What ails you, dear wife?’ | ‘Is it TOM?’ (a guess in a game: still a question) |
| `statement` | the speaker tells, describes, answers or explains | ‘I must spin this straw into gold, and I know not how.’ | ‘No,’ (an answer: statement); ‘The better to see you with, my dear.’ (a lie, but a statement) |
| `promise/threat` | the speaker commits to doing something, or announces something bad for the listener | ‘I will give thee a casket’ | ‘If you do not do it, you shall die.’ (threat, although it is also an order) |
| `exclamation` | the speaker expresses a feeling (surprise, joy, anger, grief), without asking or telling anything | ‘What a maypole!’ | ‘What nonsense,’ (a thought, `aloud="false"`: classified like a speech) |

The difference between `order` and `request` is the most interesting and the most difficult: it depends on the **relation** between speaker and listener (a king, a servant), which is not always in the speech.

## 7. Results

1. A table (DataFrame) with one row per speech, with the columns `speech_id`, `tale`, `who`, `speech`, `predicted`, `confidence`, and `score_` + each category. Saved as `results/speech_acts.csv`.
2. The number of speeches per predicted category, and the uncertain ones (confidence < 0.4).
3. A table with the categories as rows, the genders (`female`, `male`, `unspecified`) as columns, and the percentage of speeches as values, plus a row with the number of speeches per gender.
4. The 10 characters with the most orders.
5. Examples: 5 orders and 5 requests by female characters, 5 orders and 5 requests by male characters.
6. The ground-truth file to annotate, and the results of the validation (see below).

## Ground truth

1. **Write the definitions first** (point 6).
2. **Choose 60 speeches at random** from the table of point 7 (`df.sample(n=60, random_state=1)`). The code saves them in `ground_truth_speech_acts.csv` with the columns `speech_id`, `tale`, `who`, `speech`, and two empty columns `category_by_hand` and `notes`. The prediction of the model is **not** in the file. The file is created only if it does not exist yet, so the annotations are never overwritten.
3. **Annotate them by hand, before looking at the results of the model**: read each speech in the tale (who speaks to whom? what happens next?) and write one of the six categories.
4. **Note the doubts** in `notes`, especially between `order` and `request`.
   ```
   speech_id,tale,who,speech,category_by_hand,notes
   47,14_the-goose-girl,#goosegirl-old_king,Why?,question,
   524,61_cat-skin,#catskin-king,"Then let Cat-skin come up,",order,"'let' could be a request, but the king speaks to his servants"
   ```

Random sampling keeps the proportions of the corpus, so rare categories (e.g. `promise/threat`) may have only a few examples. This is a limit to report; it could be fixed by annotating more speeches.

## Validation

This is a classification task: join the ground truth with the table on `speech_id`.

- **Accuracy**: share of speeches where model and ground truth agree. If the file is not annotated yet (empty cells), the code says so and stops.
- **Confusion matrix**: rows = human, columns = model. Look especially at the cells `order` ↔ `request`: the gender comparison depends on them.
- **Per category**: for each category of the ground truth, how many speeches the model got right (recall), because accuracy can be high just by getting the frequent categories right.
- **Disagreements**: print them, with the notes, and explain some of them by reading the tale.
- **Confidence**: is the model more often right when its confidence is high? (accuracy for confidence ≥ 0.4 and < 0.4).

**Limits** to report: one category per speech, although many speeches do several things; the model does not know who speaks to whom, and the difference between order and request depends on it; the hypotheses were chosen on a small test set; BART-MNLI was trained on modern English; the ground truth is annotated by one person; the results describe this English translation, not the German tales.
