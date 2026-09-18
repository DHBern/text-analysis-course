

# Regex exercise

If you are not familiar with regular expressions (regex), follow an online **tutorial**, for example https://www.regexone.com/ or https://regexlearn.com/. 

A regex **cheat sheet** is available at https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions/Cheatsheet. 

Once you understand the basics of regex, find patterns in the **first chapter of TGG**. You can use https://regex101.com/ or a text editor for running the regular expressions.



## Find dialogues

Should match, e.g.:
- `"You will,"`
- `"You will if you stay in the East."`
- `"I'm stiff,"`
- `"I've been lying on that sofa for as long as I can remember."`

Solution
```
“.*?”
```

Limitations?
<!-- Quando due dialoghi sulla stessa linea, inframmezzati da parole, niente
cose tra double quotes che non sono dialoghi -->

## Find every two-word proper noun

Match consecutive capitalized-word pairs — place and person names like `Tom Buchanan`, `West Egg`, `East Egg`, `New Haven`, `Jordan Baker`, `New York`, `Long Island`. You need two `[A-Z][a-z]+` blocks separated by exactly one space.

Solution
```
\b[A-Z][a-z]+ [A-Z][a-z]+\b
```

Limitations?
<!-- Prende gli inizi delle frasi anche -->

---

## Dialogue-tag verbs

Write a regex that matches any of the following verbs as a whole word, wherever they appear in the text:

```
said, asked, cried, whispered, demanded, inquired, objected,
insisted, confirmed, corroborated, retorted, complained,
answered, told, remarked, murmured, confessed, advised, called
```


Solution

```
\b(said|asked|cried|whispered|demanded|inquired|objected|insisted|confirmed|corroborated|retorted|complained|answered|told|remarked|murmured|confessed|advised|called)\b
```

Limitations?
<!-- if capital letter, nothing -->


<!--
# XPath exercise
- Find 'I' in Austen's novels
- Gatsby, dialogues
-->


# Homework

**Reading**
- Geoffrey Rockwell and Stéfan Sinclair, "The Measured Words: How Computers Analyze Text", in *Hermeneutica: Computer-Assisted Interpretation in the Humanities*, MIT Press, 2016 [available in ILIAS].

**Organise the corpus**
- Download `FITZ-TGG-25`, `CATH-ALL-23`, and `HEMI-TSAR-26` in Plain Text from Project Gutenberg (you don't remember what these codes stand for? See [Corpus](../README.md))
- Name the files with the codes used here. Check that the extension (.txt) is visible in your computer (e.g. `FITZ-TGG-25.txt`).
- In each file, remove the starting and ending texts that come from Project Gutenberg. Keep the originals (with the Project Gutenberg bits) with a different name, e.g. `FITZ-TGG-25_ProjectGutenberg.txt`. See [Project Gutenberg Permissions and Licensing](https://www.gutenberg.org/policy/permission.html)

**The Great Gatsby**
- (Start to) Read *The Great Gatsby* or watch one of the many movies based on the novel.

**Markdown**
- If you have never encountered Markdown before, please follow the tutorial at https://www.markdowntutorial.com/ (use the English version only).

**VSCodium**
- Install [VSCodium](https://vscodium.com).