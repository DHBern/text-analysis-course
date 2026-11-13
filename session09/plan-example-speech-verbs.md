# Example plan: how is speech introduced in the Grimm fairy tales?

This is an example of a detailed plan, coded in [speech_verbs.ipynb](speech_verbs.ipynb).

## 1. Question

Which verbs introduce the speeches of the characters (*say*, *cry*, *answer*, *ask*...)? Are female and male characters introduced with different verbs?

## 2. Corpus

- **Texts**: 22 tales from *Grimms' Fairy Tales* by Jacob and Wilhelm Grimm, translated by Edgar Taylor and Marian Edwardes, Project Gutenberg eBook #2591 (https://www.gutenberg.org/ebooks/2591). Only tales with a female protagonist were selected.
- **Language**: English.
- **Format**: TEI XML, one file per tale, in `data/fairy-tales/`, file names starting with a number (pattern `[0-9]*.xml`). Do not use `GrimmFairyTales_EN_Gutenberg_female.xml`, which contains the whole collection.
- **Encoding already in the files**: every speech is in `<said who="#id">`, and the header (`<listPerson>`) gives the `gender` of each id. Thoughts are `<said aloud="false">`. There are no `<said>` inside other `<said>`.

## 3. Unit of analysis

The speech (`<said>`), together with its reporting clause (the short piece of narration that introduces it).

- after the speech, if the sentence goes on: ‘Why, how now, good mother,’ ***said the princess***
- before the speech, otherwise: ***Then the young prince said,*** ‘...’

The verb that introduces a speech is almost always in the same clause as the narrator's mention of the speaker. A whole sentence or paragraph would bring in many other verbs (*go*, *come*, *take*).

## 4. Methods, step by step

1. **Find the files** (pattern above). Output: 22 files.
2. **Read the genders.** For each file, read the header with `xml.etree.ElementTree`: for every child of `<listPerson>`, store `xml:id` → `gender`.
3. **Split the tale into narration and speeches.** Read the file as plain text, keep only what comes after `<body>`, and split it with the regular expression in point 5. Output: three lists, narration pieces, speakers and speeches. The narration list has one item more than the speeches (before the first and after the last speech).
4. **Clean the texts.** In the narration and the speeches, remove all XML tags (e.g. `<persName>`) and replace line breaks and multiple spaces with one space.
5. **Find the two clauses of each speech** (speech number `i`):
   - **after**: if the narration piece `i + 1` starts with a lowercase letter, take its text up to the first punctuation mark; otherwise empty;
   - **before**: remove spaces, commas and colons at the end of narration piece `i`, then take the text after its last punctuation mark;
   - if **before** is identical to the **after** of the previous speech, set it to empty. This happens with interrupted speeches (‘Well,’ *said the grandmother*, ‘we will shut the door’): the clause must be counted once.
6. **Find the verbs.** Join the two clauses with `". "` and run spaCy on the result. Keep the lemma of every token whose POS is `VERB`. Output: a list of lemmas for each speech (possibly empty).
7. **Make the table.** One row per speech and speaker (one speech has two speakers: split `who` at spaces). Give every speech a number, `speech_id`, starting at 0 over all the tales (the two rows of the speech with two speakers have the same number).
8. **Count.**
   - the 25 most frequent lemmas, over all speeches;
   - the number of speeches with no verb, and the first 10 of them;
   - for each gender, the percentage of its speeches that contain each of these verbs: *say, cry, answer, think, call, ask, reply, weep, sing*. Percentages, not counts, because the genders have a different number of speeches.
9. **Examples.** A function that prints the speeches containing a given verb, optionally only for one gender: speaker, before clause, first 80 characters of the speech, after clause.

## 5. Settings

- **Regular expression for speeches**: `<said[^>]*who="([^"]+)"[^>]*>(.*?)</said>`, with the flag `re.S` (so that `.` also matches line breaks). It works because `aloud` can come before `who`, and `<said>` is never nested.
- **Regular expression for tags**: `<[^>]+>`, replaced by nothing.
- **Clause boundaries**: the characters `, . ; : ! ?`.
- **"The sentence goes on"**: the first character of the narration after the speech is a lowercase letter (`str.islower()`).
- **spaCy**: model `en_core_web_sm` 3.8.0; keep tokens with `pos_ == "VERB"` (not `AUX`); use `lemma_`.
- **No filter of verbs**: all verbs of the clause are kept, also non-speech verbs. Noise is discussed in the results, not removed.
- **Gender table**: a speech "contains" a verb if the lemma is in its list at least once.

## 6. Categories

There is no classification, but we need to define what we are looking for.

**Reporting verb**: a verb that tells us that a character speaks, thinks or makes a sound, and that introduces a specific speech.

- **Clear example**: ‘Why, how now, good mother,’ ***said*** the princess → *say*.
- **Difficult examples**:
  - *and she went to the door and **said*** → *say* is a reporting verb, *go* is not, but the code finds both;
  - ***thought*** the princess → *think* introduces a thought (`aloud="false"`): it counts as a reporting verb;
  - ‘Falada, Falada, there thou hangest!’ ***and the head answered:*** ‘...’ → the clause introduces the **next** speech (Falada's), but the rule of step 5 gives it to the princess, because it starts with a lowercase letter;
  - *cry*: in these tales it usually means "call out loudly", not "weep". Read the examples before interpreting.
- **Speeches with no reporting verb**: they are not introduced (fast dialogue) or they are the second half of an interrupted speech. They stay in the table with an empty list.

## 7. Results

1. A table (DataFrame) with one row per speech and speaker, and the columns `speech_id`, `tale` (file name without extension), `speaker` (id), `gender`, `speech` (text), `before`, `after`, `verbs` (list of lemmas).
2. A printout of the 25 most frequent lemmas with their counts.
3. The number of speeches without a verb, and a table of the first 10 (speaker, speech).
4. A table with the verbs as rows, the genders (`female`, `male`, `unspecified`) as columns, and the percentage of speeches as values, plus a row with the number of speeches per gender.
5. The examples for *cry*: all genders, then only female, then only male (5 each).
6. The ground-truth file to annotate, the results of the validation, and the lemma check (see below).

## Ground truth

1. **Write the definition first** (point 6).
2. **Choose 60 speeches at random** from the table of point 7 (`df.sample(n=60, random_state=1)`), so that the sample also contains interrupted speeches and fast dialogues. The code saves them in `ground_truth_verbs.csv` with the columns `speech_id`, `tale`, `speaker`, `speech`, and two empty columns `verbs_by_hand` and `notes`. The verbs found by the code are **not** in the file. The file is created only if it does not exist yet, so the annotations are never overwritten.
3. **Annotate them by hand, before looking at the results of the code**: read each speech in the tale and write the reporting verb(s) as lemmas separated by a space (e.g. `cry say`), or `none`. Write only reporting verbs, not other verbs in the clause.
4. **Note the doubts** in `notes`.
   ```
   speech_id,tale,speaker,speech,verbs_by_hand,notes
   3,07_briar-rose,briarrose-briar_rose,"‘Why, how now, good mother,’",say,
   4,07_briar-rose,briarrose-briar_rose,‘what are you doing there?’,none,second half of an interrupted speech
   ```

## Validation

This is an extraction task: join the ground truth with the table on `speech_id` and `speaker`, and compare, for each of the 60 speeches, the set of verbs found by the code with the set of verbs by hand (`none` = empty set). If the file is not annotated yet (empty cells), the code says so and stops.

- **Correct**: reporting verbs found by both.
- **Missed**: reporting verbs in the ground truth, not found by the code (e.g. clause cut at a comma).
- **Extra**: verbs found by the code that are not reporting verbs (e.g. *go* in *and she went to the door and said*), or that belong to another speech (Falada).

Print the speeches where code and hand disagree, and the three totals. Report them with one example for each type of error.

Also check the spaCy lemmas on a short passage: *She cried and cries. They were crying. He asked, she answered and replied, and they called and wept.* Print every token tagged `VERB` with its lemma (are *cried*, *cries*, *crying* all → *cry*?).

**Limits** to report: the rule for the clauses is approximate; all verbs of the clause are kept; percentages for `unspecified` are based on few speeches (48); the results describe this English translation, not the German tales.
