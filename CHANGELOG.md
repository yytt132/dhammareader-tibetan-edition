# Changelog

## Unreleased (segments, plain conversion, pages)

The text is cut into new segments, the plain conversion takes the fixes of the older converter run, nearly all
witness corrections left unapplied in 2026-11-14 now apply, and each segment gives the pages of the input it spans
(see *Segments*, *From the 2026-11-14 release* and *The plain conversion* in the README). The ACIP text is unchanged.

- `text/`: 848,576 segments that followed the page blocks of the ACIP input become 1,576,285 segments cut at
  headings, stanzas, mantra runs, speech turns and sentence ends; 2,996 boundaries had to be forced
  (`boundary_status` `review_required`). New fields `block_type`, `boundary_status` and `children` (sentences, verse
  lines, mantra phrases).
- New SIDs: each work continues after its last old SID; no old SID is given to other text.
- `crosswalk/` (new): every old SID and character range, with the new segments that hold it.
- `apparatus/`: all 1,825,583 entries anchored on the new segments (`sid`, `span_start`, `span_end`, `base_sha256`;
  `end_sid` for 1,710 spans that run on into a later segment); `old_sid` keeps the SID of 2026-11-14.
- `tibetan_unicode`: converted again from the same input; taken are the long vowel as U+0F71 instead of U+0FB0
  (14,297 places), retroflex letters the older run dropped (2,780), the wa-zur apostrophe of rule `P03` and Latin
  leftovers; everything else keeps the published reading. Apparatus offsets on the Unicode text are moved with it.
- `tibetan_unicode_corrected`: built from the accepted entries alone, clause by clause, instead of converting whole
  segments again. Seventeen accepted normalizations and eleven removals that run over a new boundary are split between
  the two segments.
- 1,631 of the 1,635 accepted Derge-witness entries that 2026-11-14 left unapplied now apply (19 were already given by
  a rule correction): their lemma, from the syllable alignment, lacks the base's shads and spaces, so the reading's
  changes are made on the base and its punctuation is kept. Most are Sanskrit stacks the converter left as `+`
  (ར+ཡ → རྱ); `+` in the reading text goes from 4,070 to 2,571. 4 stay unapplied: they would remove a shad.
- New rule `W05` (`apparatus/rules.tsv`): ACIP `KSH` in a Sanskrit word is kṣa, `Ksh` (ཀྵ, not ཀྴ); 356 accepted
  entries. *Pakshi*/*Bakshi* and *Trakshad* keep ka over sha. ཀྴ in the reading text: 514 → 138.
- Rule `W01` (stray lowercase upper-cased) had also upper-cased the Sanskrit lowercase of retroflex letters and the
  anusvara: 21 entries keep the `sh` of kṣa (`AAKshI`, not `AAKSHI`), 40 keep the lowercase the corpus clearly
  attests (`H'Um`, not `H'UM`), and 9 are `rejected` where a Derge-witness entry corrects the same word. The old
  reading or status is in each entry's `note`.
- 14,444 segments read differently from 2026-11-14, all of them in the kinds above, spaces and tshegs (823) and a few
  modern dates the cleaned input leaves out.
- `text/`: new field `pages`, the page markers of the ACIP input (`@2B`) each segment spans, with the offsets in
  `acip_original` and `tibetan_unicode` where each page begins: 350,960 page breaks; the page is unknown in 5,868
  segments, all but 10 in the Sungbum.
- Checked against 2026-11-14: the same 5,562 works and the same ACIP text in order; the same apparatus entries
  besides the `W05` entries and the 70 revised `W01` entries; every old SID in the crosswalk. Page breaks checked
  against the ACIP files in 40 random streams (1,700 breaks).
- `catalog/works.tsv`: the columns `sids` and `corrected_sids` are now `segments` and `corrected_segments`.

## 2026-11-14 (known issue)

- `README.md`: known issue on segment boundaries. Segments follow the page blocks of the ACIP input, not sentences;
  a re-segmentation, with a table from the current SIDs to the new segments, is in preparation. Data unchanged.

## 2026-11-14 (public release)

- The repository is public, with the edition as one commit; the earlier history is not published.
- `CITATION.cff` and `.zenodo.json` added: author Yoon, Insang (Dhammareader, ORCID 0009-0003-5325-5875). Releases
  are archived on Zenodo with a DOI: 10.5281/zenodo.24213725 for all releases, 10.5281/zenodo.24213726 for the
  release of 2026-11-14.
- `README.md`: Sungbum counts brought up to date (1,613 works; 145 left out for copyright).

## 2026-11-14 (modern works left out)

- 17 Sungbum works of authors who died in 1955 or later, and of modern compilers, left out for copyright: they had
  escaped the modern-author list and were found by author dates, ACIP file-name dates and modern title pages (works
  of Khri byang rin po che, Rta mgrin rab brtan, Bka' chen Blo bzang bzod pa, Tre hor Rdo rje bkra shis, Ldan ma Blo
  bzang rdo rje; Shakabpa's political history; the 1992 *Gangs can mkhas grub rim byon ming mdzod*). Their text and
  apparatus files are removed and they are listed in `catalog/left_out.tsv`; `catalog/titles.tsv` now has 5,562
  works.
- `NOTICE.md`: the ACIP public-domain dedication covers the Sungbum download as well; it does not waive the authors'
  own copyright, which is why modern works are left out.

## 2026-11-10 (authors and titles)

- `catalog/persons.tsv`: Tengyur authors are now the rKTs standard attribution where rKTs gives one (2,421 rows), else
  the colophon name. The Derge catalogue had taken the colophon's last name as the author, which for D3824
  (Mūlamadhyamakakārikā) and D3829 is the buddha Nāgārjuna is to become; D3829, held anonymous by rKTs, now has no
  author. New column `colophon_name`; the column `name_wylie` is now `name`, since standard names are in IAST.
- `catalog/titles.tsv`: S0466I has its title from the text's title line (its file name held none), and KL00504 the ACIP
  title instead of a shortened Wylie one.
- Sungbum author P297 named (rgyal mkhan po grags pa rgyal mtshan, BDRC P297).
- R00003E (*Shes bya'i gter mdzod*, 3 volumes) left out for copyright: a compilation of biographies by Mi rigs dpe
  mdzod khang (BDRC W19837), a 1980s publication. Its text and apparatus files are removed and it is listed in
  `catalog/left_out.tsv`; `catalog/titles.tsv` now has 5,579 works.

## 2026-11-10 (person names)

- `catalog/persons.tsv`: Korean readings of Kangyur and Tengyur names corrected (2,581 of 3,657 distinct rKTs names).
  Indian names written without Sanskrit marks had been split and read as Tibetan (까마 라 실라 → 까말라실라); titles,
  places and clan names are now written apart from the name. Malformed BDRC ids removed (63 `#`; 9 double ids keep the
  first).
- `catalog/titles.tsv`: 16 Korean title readings corrected (*ng* inside Sanskrit words: 간가이 → 강가이), and the
  Sanskrit title "n.a." of one work dropped.

## 2026-11-09 (titles and persons)

New catalogue files; text and apparatus are unchanged.

- `catalog/titles.tsv`: 5,580 works and 264 texts of the ACIP catch-all files (`part_of`), with the titles the reading
  site publishes. Works: Tibetan script title 5,544, Sanskrit title 3,869, Korean reading of the Tibetan title 5,544
  and of the Sanskrit title 3,869; Chinese title source: established 124, parallel 178, coined 5,278.
- `catalog/persons.tsv`: 8,145 rows (authors, translators, revisors).
- Data from 20th-century publications kept apart in `catalog/sources/`: `lancaster1979/chinese_parallels.tsv` (703
  rows), `kabc/koryo_titles.tsv` (552 titles). `titles.tsv` gives rKTs translated-from-Chinese links
  (`translated_from_chinese_taisho`).
- `catalog/sources/otani1930/chinese_parallels.tsv`: 256 Kangyur parallels from the Otani concordance (title_match
  221, part_of_taisho 7, taisho_title_differs 21, no_taisho 7), in its own commit.

## 2026-11-06 (Sungbum)

Sungbum added: 1,631 works, 293,271 segments; 127 works left out for copyright.

- Corrected segments: 23,683 (text changed in 2,043). Rebuild hash
  `f6ad5bd415baf6803a5418b9b6f43d5db7a7f59ba4ca608ce99fe335f66af9cc`.
- Converter fix P03: 1,899 places in 1,500 segments.
- Kangyur and Tengyur files are unchanged.

## 2026-11-06

First data release: Kangyur (Lhasa, ACIP) and Tengyur (Derge, ACIP) corrected text and apparatus.

- Corrected segments: Kangyur 18,413 (text changed in 16,646), Tengyur 74,524 (text changed in 59,569).
- Rebuild hashes: Kangyur `d37473054d9d048c8f7411ccd80ef46fcf7e939d15081fb5748c58727d440915`,
  Tengyur `3cddf959c07b5f84d2bac7519f71b5c477de58f49d6c71523acb256ce75ddeb3`.
- Converter fix P03 (wa-zur + apostrophe): Kangyur 607 places in 504 segments, Tengyur 949 in 788.
