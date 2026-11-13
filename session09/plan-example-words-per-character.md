# Example plan: who speaks in the Grimm fairy tales?

This is an example of a detailed plan, coded in [words_per_character.ipynb](words_per_character.ipynb).

## 1. Question

In Grimm tales with a female protagonist, do female characters speak more than male characters? And in each tale, who speaks the most?

## 2. Corpus

- **Texts**: 22 tales from *Grimms' Fairy Tales* by Jacob and Wilhelm Grimm, translated by Edgar Taylor and Marian Edwardes, Project Gutenberg eBook #2591 (https://www.gutenberg.org/ebooks/2591). Only tales with a female protagonist were selected.
- **Language**: English.
- **Format**: TEI XML, one file per tale, in `data/fairy-tales/`. The file names start with the tale number, e.g. `07_briar-rose.xml`. The file `GrimmFairyTales_EN_Gutenberg_female.xml` contains the whole collection and must not be used.
- **Encoding already in the files**:
  - in the header, `<listPerson>` lists the characters: `<person>` for one character, `<personGrp>` for a group (e.g. the dwarfs). Each one has an `xml:id`, a `gender` and one or more `<persName>`:
    `<person xml:id="redcap-wolf" gender="male"><persName>wolf</persName></person>`
  - in the text, every speech is in `<said>`, and `who` points to the speaker:
    `<said who="#redcap-wolf">‘Good day, Little Red-Cap,’</said>`
  - thoughts are encoded as `<said aloud="false">` and are counted as speech.

## 3. Unit of analysis

- **Counting unit**: the speech (`<said>`). Each speech is counted for the character in its `who`.
- **Result unit**: the character in one tale. The same type of character in two tales (e.g. the wolf in *Little Red-Cap* and in *The Wolf and the Seven Little Kids*) are two different rows, because they have different ids.


## 4. Methods, step by step

1. **Find the files.** Input: the folder `data/fairy-tales/`. 
2. **Read the characters of each tale.** Input: one file. Read the XML with `xml.etree.ElementTree`. For each child of `<listPerson>` (both `<person>` and `<personGrp>`), take `xml:id`, `gender`, and the name as in the first `<persName>`. Take the tale title from `<titleStmt>/<title>`. Output: for each character, id, name, gender, and a word count set to 0. Characters who never speak must stay in the results with 0 words.
3. **Count the words of each speech.** Input: each `<said>` of the tale. Count the words with the regular expression in point 5. Output: a number for each speech.
4. **Add the words to the speaker.** Add the number of words to **each** speaker based on the value of `@who`. Output: the total number of words per character.
5. **Make the table and save it.** One row per character, for all the tales. Save it as a CSV (see point 7). Also create a ground-truth table, with one row per speech (`speech_id`, a number starting at 0 over all the tales; `tale`; `speaker`; `speech`, the text without line breaks; `words`).
6. **Print the top 5 per tale.** Input: the table. For each tale, in the order of the files: leave out the characters with 0 words, sort by number of words (highest first), print the first 5.

## 5. Settings

- **Words**: `[A-Za-z]+(?:[’'-][A-Za-z]+)*` - letters, optionally joined to more letters by a curly apostrophe `’`, a straight apostrophe `'` or one hyphen `-`. So *king’s* and *Red-Cap* are 1 word each, and *me--you* (the double hyphen is a dash) is 2 words. The quotation marks `‘ ’` around the speech are not words.
- **No lowercasing, no lemmas, no stop words**: we count every word.
- **Thoughts** (`aloud="false"`) are counted like speech.
- **Groups** (`<personGrp>`) are counted like characters.
- **Two speakers in one `<said>`**: the words count for both.

## 6. Categories

The only category is the gender of the characters. It is already in the header (`gender`), with three values.

Characters who change form keep the gender of their final form: the frog in *The Frog-Prince* is `male`. Groups take the gender of their members if the text gives it (the dwarfs: `male`), otherwise `unspecified` (the kids).

## 7. Results

1. A CSV file `words_per_character.csv`, one row per character, with the columns:
   - `tale`: the title from the header, e.g. `Little Red-Cap`;
   - `character`: the `xml:id`, e.g. `redcap-wolf`;
   - `name`: the first `<persName>` in the header;
   - `gender`: `female`, `male` or `unspecified`;
   - `words`: the number of words spoken (0 if the character never speaks).
2. A printout (result in the notebook) with, for each tale, the title and the 5 characters who speak the most, one per line:
   ```
   Hansel and Gretel
       stepmother (female): 295 words
       Hansel (male): 212 words
   ```
3. A check: the total number of words and the number of characters per gender.
4. The two ground-truth files to annotate, and the results of the validation (see below).

## Ground truth

The results depend on the word count.

The code chooses the samples and saves them with an empty column to fill in by hand. A file is created only if it does not exist yet, so the annotations are never overwritten.

1. **Words**: 20 speeches at random from the table of speeches (`sample(n=20, random_state=1)`), without the count of the code. Count their words by hand, following the definition in point 5.
   `ground_truth_words.csv`:
   ```
   speech_id,tale,speech,words_by_hand,notes
   0,Briar Rose,"‘I know what your wish is, and it shall be fulfilled, in return for your kindness to me--you will soon have a daughter.’",24,
   ```

## Validation

If a file is not annotated yet (empty cells), the code says so and stops.
- **Words**: join the ground truth with the table of speeches on `speech_id`. Difference = count of the code − count by hand. Print how many are exact, and the speeches with a difference; explain the differences by reading them (e.g. numbers, abbreviations).
