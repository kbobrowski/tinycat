## tinycat

Tiny Computer-Assisted translation tool. Created for [endcoronavirus.org](https://endcoronavirus.org).

#### Installation

```
pip3 install tinycat
```

#### Example

Text you want to translate is in `document.txt` file:

```text
This is the first paragrapth.

This is the second paragraph. Paragraphs have multiple sentences.

This is the third paragraph.
This is still the third paragraphs.
Paragraphs are divided by empty lines.
```

Run following command to generate translation from English to Polish:

```
python3 -m tinycat.cli translate --input-file document.txt --patch-file patch.txt --lang-in english --lang-out polish
```

Generated `patch.txt`:

```text
----------------------------------------------------------------
This is the first paragrapth.

To jest pierwszy paragraf.
----------------------------------------------------------------

----------------------------------------------------------------
This is the second paragraph. Paragraphs have multiple sentences.

To jest drugi akapit. Akapity mają wiele zdań.
----------------------------------------------------------------

----------------------------------------------------------------
This is the third paragraph.
This is still the third paragraphs.
Paragraphs are divided by empty lines.

To jest trzeci akapit. To wciąż trzeci akapit. Akapity są podzielone pustymi wierszami.
----------------------------------------------------------------
```

Modify `patch.txt` and save it to `patch_corrected.txt`.
In this case we corrected `paragraf` to `akapit` in the first sentence, and in third paragraph put each sentence
in the new line for readability.

Apply the patch using:

```
python3 -m tinycat.cli update --patch-file patch_corrected.txt --dict-file en-pl.dict
```

Now the text can be translated as following (note that we are passing `en-pl.dict`:

```
python3 -m tinycat.cli translate --input-file document.txt --patch-file translated.txt --dict-file en-pl.dict --lang-in english --lang-out polish
```

`translated.txt` file does not contain any paragraphs to translate, as all the translations are taken from `en-pl.dict` dictionary. Content of `translated.txt` is:

```text
To jest pierwszy akapit.

To jest drugi akapit. Akapity mają wiele zdań.

To jest trzeci akapit.
To wciąż trzeci akapit.
Akapity są podzielone pustymi wierszami.
```

In the next step we modify `document.txt` to add new paragraph, and correct typo in first sentence:

```text
This is the first paragraph.

This is the second paragraph. Paragraphs have multiple sentences.

This is the third paragraph.
This is still the third paragraphs.
Paragraphs are divided by empty lines.

Final paragraph.
```

Let's create new patch for human translator:

```
python3 -m tinycat.cli translate --input-file document.txt --patch-file patch-2.txt --dict-file en-pl.dict --lang-in english --lang-out polish
```

`patch-2.txt` contains new translations that has to be checked:

```text
----------------------------------------------------------------
This is the first paragraph.

To jest pierwszy akapit.
----------------------------------------------------------------

To jest drugi akapit. Akapity mają wiele zdań.

To jest trzeci akapit.
To wciąż trzeci akapit.
Akapity są podzielone pustymi wierszami.

----------------------------------------------------------------
Final paragraph.

Ostatni akapit.
----------------------------------------------------------------
```

This time all is fine so we can apply this patch:

```
python3 -m tinycat.cli update --patch-file patch-2.txt --dict-file en-pl.dict
```

And see final translated text with (if we don't pass `--patch-file` it will print to the console):

```
python3 -m tinycat.cli translate --input-file document.txt --dict-file en-pl.dict --lang-in english --lang-out polish
```


#### Help

```text
python3 -m tinycat.cli translate --help
python3 -m tinycat.cli update --help
```
