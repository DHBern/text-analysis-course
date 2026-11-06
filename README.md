
[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/DHBern/text-analysis-course/HEAD)

# Text Analysis Course

Digital Humanities Master. University of Bern.

Instructor: Elena Spadini.


## Content

| Date  | Topic                                           | Notes                         | Corpus |
| ----- | ----------------------------------------------- | ----------------------------- | --- |
| 18.09 | Intro and [regex](session01/readme.md)                                 |                               | TGG |
| 25.09 | [Voyant Tools](session02/readme.md)                                    | With Ursula Loosli (UniBE)           | TGG+ |
| 2.10  | [Lexical diversity](session03)                               |                               | TGG+ |
| 9.10  | [Tokenization, lemmatization, and POS tagging](session04)                       |                               | TGG |
| 16.10 |  [BoW and TF/IDF (distinctiveness)](session05)  |                               | TGG+ |
| 23.10 | [Case study 1. Formal features of the variants](session06/readme.md)                      |                               | Frankestein |
| 30.10 | [Case study 1. Zero-shot classification](session07/readme.md) |                               | Frankestein |
| 6.11  | [Case study 1. LLM classification (few shot). BERTopic](session08/readme.md)                                |                               | Frankestein |
| 13.11 | Case study 2. Agency and gender                                |  | TGG+ |                             |
| 20.11 | Infoclio Conference (see below)                 |                               |
| 4.12  | Data visualisations + Case study 2                      |  With Tommaso Elli (UniBE)                             | TGG+ |
| 11.12 | Case study 2                                 |                               | TGG+
| 18.12 | Presentations                                   |                               | |


## Calendar

**Infoclio Conference 2026**
On the 20th of November, we go to the https://www.infoclio.ch/en/programm-tagung2026 from 10:00 to 12:00. If you want to stay for the whole day, you will have to pay the registration fee of 15 CHF.

**Reschedule**
On the 27th of November, our lesson is cancelled. This class is rescheduled to the 4th of December, when our course will be longer: from 9:15 to 12:45.


## Corpus

`FITZ-TGG-25` or `TGG` = Francis Scott Fitzgerald, *The Great Gatsby**, 1925 (~47'000 words)

`CATH-ALL-23` = Willa Cather, *A Lost Lady*, 1923 (~37'500 words)

`HEMI-TSAR-26` = Ernest Hemingway, *The Sun Also Rises*, 1926 (~67'500 words)

`TGG+` = `FITZ-TGG-25` + `CATH-ALL-23` + `HEMI-TSAR-26`.

These texts are available via the Project Gutenberg, e.g. https://www.gutenberg.org/ebooks/64317. We will use the Plain Text format (see Download options).

`Frankestein` = materials from https://shelleygodwinarchive.org/.  

Create a folder `data` in your computer, where you collect the texts to work with. A similar folder [data](data) is available here.



## Assignment

See [assignment.md](assignment.md).

### Using LLMs and AI assistants

You may use LLMs and AI assistants for this course and for the assignment under two conditions:
- Document it. State which tools you used and for which steps.
- Check it. You are responsible for every claim and every reference.



## Tools and more

**Markdown**
- If you have never encountered Markdown before, please follow the tutorial at https://www.markdowntutorial.com/ (use the English version only).

**XML**

**Text and code editor**
- Recommended: [VSCodium](https://vscodium.com).

**Up-to-date browser**
- Recommended: Firefox or Chrome/Chromium.

**Jupyter Notebooks**
- See [helpme](helpme/notebooks.md)

**Regular expressions**
- Tutorials online, for example https://www.regexone.com/ and https://regexlearn.com/. 
- A regex **cheat sheet** is available at https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions/Cheatsheet.
