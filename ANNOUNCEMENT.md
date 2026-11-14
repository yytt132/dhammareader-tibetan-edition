[한국어](ANNOUNCEMENT.ko.md)

# Dhammareader Tibetan Canon Edition

14 November 2026 · Yoon Insang

## What is released

The *Dhammareader Tibetan Canon Edition* is now public. It is a corrected Unicode edition of the Tibetan Kangyur, Tengyur and Sungbum (collected works), converted into Tibetan script. Anyone may download, study and translate it, and you may adapt or redistribute it as long as you credit the source.

| | |
| --- | --- |
| Kangyur | 565 works (Lhasa edition) |
| Tengyur | 3,384 works (Derge edition) |
| Sungbum (collected works) | 1,613 works |
| Format | One file per work, one line per segment: ACIP original, Tibetan Unicode, corrected Unicode |
| Apparatus | Type, evidence and status for each correction |
| Catalogues | Titles in Tibetan, Sanskrit, Chinese and Korean; authors, translators and revisors |
| License | CC BY 4.0 |

- DOI: [10.5281/zenodo.24213725](https://doi.org/10.5281/zenodo.24213725)
- Repository: [github.com/yytt132/dhammareader-tibetan-edition](https://github.com/yytt132/dhammareader-tibetan-edition)
- Reading site: [dhammareader.com](https://dhammareader.com)

## Sources and licenses

The texts were keyed in by the Asian Classics Input Project (ACIP). Over many years ACIP entered the Kangyur, the Tengyur and a large body of Sungbum in its own romanization. The collection is now kept by the [Asian Legacy Library](https://asianlegacylibrary.org/library/downloads/), which has dedicated these input texts to the public domain, commercial use included. This edition could be made only because of that dedication, and I am very grateful for it.

| Material | Rights |
| --- | --- |
| ACIP input texts (Asian Legacy Library) | Public domain dedication |
| Esukhia Derge Kangyur and Tengyur, used as collation witnesses | Public domain |
| rKTs catalogue data (University of Vienna): standard titles, authors, translators | CC0 |
| Bibliographic facts from Lancaster (1979), KABC and the Otani concordance (1930–32) | Facts only, kept in separate folders |
| This edition: corrected text, apparatus, catalogues | [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) |

Because the edition is released under CC BY 4.0, you may copy, translate, adapt and redistribute it for any purpose, commercial use included, provided you credit it. The ACIP dedication, however, covers ACIP's input of the texts. It does not waive the authors' own copyright. For that reason recent works have been left out, on the criteria given below.

## What is included and what is not

Please check the scope of the collection before you rely on it.

The Kangyur (Lhasa edition) and the Tengyur (Derge edition) are the canon shared by all Tibetan traditions, so they can be used whichever tradition you study. The Sungbum is different, because it is the collection ACIP assembled and it follows ACIP's interests. In other words, it consists mainly of the collected works of Gelug authors, and other traditions are much less represented. Works that circulated outside the canon are not included either. The *Bardo Thödol*, known in English as the *Tibetan Book of the Dead*, is one example.

145 Sungbum works have been left out for copyright reasons. They are works by authors who died in 1955 or later, modern compilations, and the *Bod rgya tshig mdzod chen mo* (1985). Every excluded work is listed in [`catalog/left_out.tsv`](catalog/left_out.tsv). Finally, the dataset contains no translations. Dhammareader's Korean translations can be read on [dhammareader.com](https://dhammareader.com).

## Why Dhammareader, and why this edition

I started Dhammareader because there was no open foundation for studying Buddhism from its sources in Korean. I wanted a place where the Pali, Chinese and Tibetan canons could be laid side by side, read against one another, and traced back to their sources. Dhammareader is therefore free to read and needs no account.

Of the three canons, the Tibetan was the hardest to bring in. This is because the ACIP texts are written in ACIP's own romanization rather than in Tibetan script. They also contain input errors, editors' notes and page markers, and in places Sanskrit or romanized Tibetan runs into the text. Before anyone can read or translate them with confidence, then, the texts have to be converted into Tibetan script, checked line by line, and given a record of what was changed.

I first took on this work for Dhammareader's own translations. As it went on, I came to think the result would be useful not only to Dhammareader's readers but to anyone who works with Tibetan texts, scholars and translators included. So I prepared it separately as a dataset that anyone in Buddhist studies can use.

## How the edition was made

One principle held throughout the work: the source is never overwritten, and every change is recorded. In practice the work went in the following order.

1. **Segmenting.** Each work was divided into segments at natural breaks in meaning, and each segment was given a stable identifier (SID). Translations and other editions can then be aligned with the text segment by segment.
2. **Conversion.** The ACIP romanization was converted into Tibetan Unicode and then converted back, to check that the result matched the original. This round trip showed that the converter had been misreading certain syllables without any warning, and that error was fixed.
3. **Rule corrections.** Editors' notes, page markers, uncertain-reading marks and punctuation were cleaned according to published rules. Sanskrit passages were left untouched and only tagged as Sanskrit.
4. **Comparison with a second edition.** The Kangyur and Tengyur were aligned syllable by syllable with the Esukhia digital Derge edition. For the Tengyur, 98.65% of its 49.4 million syllables were aligned. Where Lhasa and Derge differ but both readings are valid, the text was not corrected and the difference was recorded as a variant.
5. **Apparatus.** Every correction is recorded with its type, its evidence (witness and folio line) and its status. The corrected text is rebuilt mechanically from the accepted entries alone, so for any changed syllable you can always look up what was changed and why.
6. **Catalogues.** Titles are given in Tibetan, Sanskrit, Chinese and Korean, with Korean readings. Authors, translators and revisors are taken from rKTs.
7. **Copyright review.** Before release, every Sungbum work was checked against its author's dates and against marks of modern publication.

The corrections were reviewed by sampling for each rule, and part of that review was done with the help of an AI tool. No independent expert evaluation has been made yet. In the Sungbum in particular, which has no second edition to compare against, some readings remain unresolved. Each entry therefore records its status, and you may use only the corrections you judge reliable.

## What I hope for

I hope this edition lets readers in Korean and in other languages download the Tibetan canon easily, translate it, and read it in their own words. I think it will also be of use to scholars, translators and people building new tools. There is surely still much to fix, and I would be grateful for your help in making it better.

- **Download and cite.** Each release is archived on Zenodo. When citing, please use [doi.org/10.5281/zenodo.24213725](https://doi.org/10.5281/zenodo.24213725). The [README](README.md) describes the files and gives short examples.
- **Report errors.** If you find a misreading, a wrong title or a missing work, please open an issue on [GitHub](https://github.com/yytt132/dhammareader-tibetan-edition/issues). I will check it and include the fix in a later release.
- **Tell me what you make.** If you translate, study or build a tool with this edition, please let me know. I would be glad to hear about it.

Finally, my thanks to the Asian Legacy Library, Esukhia, the rKTs team, and everyone who has kept and handed down these texts over the centuries.

Yoon Insang, Dhammareader
