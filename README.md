# Dhammareader Tibetan Canon Edition

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.24213725.svg)](https://doi.org/10.5281/zenodo.24213725)

A corrected Unicode edition of the ACIP Tibetan Kangyur (Lhasa edition), Tengyur (Derge edition) and Sungbum, with an
apparatus that records every correction, its type, its evidence and what is still unresolved.
Reading site: [dhammareader.com](https://dhammareader.com). 한국어 요약은 아래에 있다.
Release announcement: [English](ANNOUNCEMENT.md) · [한국어](ANNOUNCEMENT.ko.md).

## What is here

| Path | Content |
|---|---|
| `text/<collection>/<work>.jsonl.gz` | One line per segment (SID): `acip_original`, `tibetan_unicode` (plain conversion), `tibetan_unicode_corrected` (only where a correction applies), `toh` |
| `apparatus/<collection>/<work>.jsonl.gz` | Apparatus entries: span in the ACIP original or the Unicode text, lemma, reading, type, action, rule, confidence, status (`accepted` / `proposed` / `rejected` / `unresolved`), evidence (witness and folio.line) |
| `apparatus/rules.tsv` | Rule table for the rule-based corrections (approved 2026-11-03) |
| `catalog/works.tsv` | Work list with ACIP id, Toh number and mapping status, counts |
| `catalog/parts.tsv` | Toh mapping of the texts gathered in the ACIP catch-all `unknown` files |
| `catalog/left_out.tsv` | Parts left out: exact duplicate files, chapter files that repeat a volume, texts without a Derge counterpart, Sungbum works under copyright |
| `catalog/titles.tsv` | Titles of every work and of every text in the ACIP catch-all files (`part_of`): ACIP, Wylie, Tibetan script, rKTs standard Tibetan, Sanskrit, Chinese, Korean, and Korean readings of the Tibetan and Sanskrit titles (see *Titles and persons*) |
| `catalog/persons.tsv` | Authors, translators and revisors per work: name, Korean reading, BDRC person id, source, colophon name |
| `catalog/sources/<source>/` | Data taken from 20th-century or later publications, one folder per source (see *Sources kept apart*): `lancaster1979/chinese_parallels.tsv` (Koryŏ and Taishō parallels), `kabc/koryo_titles.tsv` (Koryŏ titles), `otani1930/chinese_parallels.tsv` (Kangyur parallels from the Otani concordance) |
| `CHANGELOG.md` | Releases |

Collections: Kangyur 565 works (Lhasa), Tengyur 3,384 works (Derge), Sungbum 1,613 works. 145 Sungbum works are
left out for copyright: works of authors who died in 1955 or later, modern compilations, and the *Bod rgya tshig mdzod
chen mo* (1985).
Korean translations are not part of this repository.

## Known issue: segment boundaries

Segments (SIDs) follow the page blocks of the ACIP input (the blank lines between groups of lines), not sentences.
286,340 of the 848,576 segments (34%) end inside a phrase, without a shad, and more end inside a sentence; 38,875 are
longer than 2,000 Tibetan characters, the longest about 110,000. The text itself is complete and in order: read
neighbouring segments together. A re-segmentation into sentence-based units is in preparation: paragraph-sized
segments with sentences and verse lines as sub-units, and one stanza per segment in verse. It will come as a new
release with a table that maps every current SID and character span to the new segments. Current SIDs will not be
reused for other text, so references to this release stay valid.

## Quick start

Each `.jsonl.gz` file is gzip-compressed JSON Lines: one JSON object per line. In `text/` a line is one segment (SID)
of the work; in `apparatus/` a line is one apparatus entry. Without programming, unpack a file (7-Zip, Bandizip,
`gunzip`) and open the `.jsonl` in a text editor; open the `catalog/*.tsv` files in a spreadsheet as UTF-8,
tab-separated.

Find a work in `catalog/titles.tsv` (Toh number, Tibetan, Sanskrit, Chinese or Korean title) or `catalog/works.tsv`.
Its `work_id` `tibetan:tengyur:TD03824` (*Mūlamadhyamakakārikā*, D3824) names the files
`text/tengyur/TD03824.jsonl.gz` and `apparatus/tengyur/TD03824.jsonl.gz`.

The corrected text, segment by segment (Python 3, standard library only):

```python
import gzip, json

with gzip.open('text/tengyur/TD03824.jsonl.gz', 'rt', encoding='utf-8') as f:
    for line in f:
        seg = json.loads(line)
        print(seg['sid'], seg['tibetan_unicode_corrected'] or seg['tibetan_unicode'])
```

`tibetan_unicode_corrected` is `null` where no accepted correction applies; the segment then reads as
`tibetan_unicode`. `acip_original` is the ACIP romanized text as published.

The accepted corrections of the same work, with their evidence:

```python
with gzip.open('apparatus/tengyur/TD03824.jsonl.gz', 'rt', encoding='utf-8') as f:
    for line in f:
        entry = json.loads(line)
        if entry['status'] == 'accepted' and entry['action'] == 'correct':
            print(entry['sid'], entry['lemma'], '->', entry['reading'], entry['rule_id'],
                  [e['locator'] for e in entry['evidence']])
```

Only `accepted` entries are applied to the corrected text. `proposed` entries are recorded but not applied: among them
the variants (`type` `V`), such as the Lhasa–Derge differences in the Kangyur. `action` says what an entry does: for
example `correct` replaces the lemma with the reading, `keep_ours` keeps our reading and records the witness's, and
`keep_tagged` leaves the text as it is and tags it (Sanskrit transliteration). `span_start` and `span_end` are character offsets (Python
string indices) into the segment's base text, named by `base_form` (`tibetan_unicode`, or `romanized_original` for
`acip_original`) and fixed by `base_sha256`. `lemma` is normally the text at that span; it is `null` for an insertion,
and for an alignment variant it gives the syllables as aligned, without punctuation.

## How the edition is made

1. **Source.** The Asian Legacy Library (ACIP) romanized texts, dedicated to the public domain
   ([downloads page](https://asianlegacylibrary.org/library/downloads/)). Archive SHA-256:
   `KANGYUR.zip` `14c06e3a…2fbc9ce1`, `TENGYUR.zip` `9b473331…4d441d37`, `SUNGBUM.zip` `68e0aefd…9a411716`.
2. **Conversion.** ACIP → EWTS → Unicode with pyewts 0.2.0 and a round-trip check. We fixed one converter bug:
   wa-zur followed by an apostrophe and a vowel was read as a long vowel (`RVA'I` → རྭཱི instead of རྭའི,
   without a warning). Affected segments are converted again (rule `P03_converter_wazur_apostrophe`).
3. **Rule corrections.** Editor notes, bibliographic intrusions, folio markers, uncertain-reading marks,
   lowercase Wylie and punctuation, by the rules in `apparatus/rules.tsv`. Sanskrit transliteration is tagged, not changed.
4. **Witness alignment.** Syllable alignment with the Esukhia Derge Kangyur (commit `a582cf4`) and Tengyur
   (commit `1746531`), anchored on unique 6-syllable sequences. Tengyur: 98.65% of 49.4 million syllables aligned.
   Our Kangyur is the Lhasa edition, so where both readings are valid syllables the difference is recorded as an
   edition variant (`V`), not corrected. The Sungbum has no witness: only rule corrections and the converter fix apply.
5. **Decisions.** High-confidence witness rules were sample-reviewed before acceptance; medium and low entries stay
   `proposed`. The corrected text is rebuilt mechanically from the accepted entries; two rebuilds give the same hash.

Unresolved (2026-11-06), share of segments flagged by the converter: Kangyur 5.42% (191 of 3,522), Tengyur 0.73%
(122 of 16,751). Sungbum: 5,257 of 293,271 segments (1.79%) carry an unresolved entry, nearly all converter warnings
that no witness can settle. Known open issue: Sanskrit-style stacks split by the converter (`PADMA` → པདམ) where no witness covers them.

## Titles and persons

A Tibetan title translates the Sanskrit title; it does not transcribe it. The Sanskrit, Tibetan and Chinese titles
of one text are therefore three different names (Kālacakra / *dus kyi 'khor lo* / 時輪), and each gets its own Korean
reading.

- `title_wylie`, `title_tibetan`: the short title shown in the reading site; `title_full_*`: the full title from the
  ACIP file. For 112 Tengyur works whose ACIP title is the opening line *rgya gar skad du* …, the rKTs standard title
  is used instead.
- `title_sanskrit`: rKTs standard Sanskrit title (`title_sanskrit_source` = `rKTs`), else the Sanskrit given with the
  ACIP file (`ACIP`). Sungbum works are Tibetan compositions and have none.
- `title_han` and `title_han_source`: `established` = the name of an extant Chinese translation, `parallel` = the name
  of a corresponding Chinese scripture, `coined` = a Chinese title made by this project because no Chinese
  translation exists. A coined title is not a historical title. Extant translations are listed in
  `catalog/sources/lancaster1979/chinese_parallels.tsv`; `translated_from_chinese_taisho` gives the Taishō number of
  the Chinese text where the Tibetan text was translated from Chinese (rKTs).
- `title_ko`: Korean title (Sino-Korean reading of `title_han`). `title_han` and `title_ko` are the titles the reading
  site publishes, including the `coined` ones, which serve as the working titles until an extant translation is found.
- `part_of`: for the 264 texts gathered in the ACIP catch-all files (`unknown`), the catch-all work they belong to; the
  reading site shows each of them as a work.
- `reading_tibetan_ko`: Korean reading of the Tibetan title by the 동국대학교 경주캠퍼스 티벳장경연구소
  「티벳어 한글 표기안」 (2010): Lhasa pronunciation, tense consonants, vowel change before d n l s. Rule-based; the
  rules reproduce 131 of the 134 examples given in that guide.
- `reading_sanskrit_ko`: Korean reading of the Sanskrit title (tense consonants, *v* read as ㅂ).
- `persons.tsv`: Kangyur and Tengyur translators and revisors come from the rKTs Derge catalogue (taken from the
  colophons). Tengyur authors are the rKTs team's standard attribution where rKTs gives one (`source` *rKTs standard
  author*, name as rKTs writes it, IAST for Indian authors: *Nāgārjuna* 나가르주나), else the colophon name; the colophon
  can name someone else, as D3824 does (the future buddha *de bzhin gshegs pa ye shes 'byung gnas 'od*), and
  `colophon_name` keeps what it says. Sungbum authors come from the ACIP author authority file. `name_ko` reads Tibetan names by the same 표기안 and Indian names written
  in Tibetan (*d+harma pA la b+ha dra*) as one Sanskrit word (다르마빨라바드라). Titles and offices (캔뽀, 로짜와,
  롭뾘, 겔롱), places (갸가르기, 카체이) and clan names (쪽로, 빠찹) are written apart; the personal name is one word
  (쪽로 뤼걜챈). A name after an Indian title (*rgya gar gyi mkhan po*, *paN+Di ta*, *slob dpon*) is read as Sanskrit
  even without Sanskrit marks (*kha che'i mkhan po ha su ma ti* → 카체이 캔뽀 하수마띠). `bdrc` keeps only well-formed
  BDRC ids (rKTs writes `#` for an unknown person).
- `sources/lancaster1979/chinese_parallels.tsv`: Toh → K → T from Lancaster's *Descriptive Catalogue* (1979,
  comparative indexes), with the Koryŏ title as Lancaster prints it. `sources/kabc/koryo_titles.tsv`: the same K
  numbers with their titles in the KABC 통합대장경 list (Dongguk University), which the reading site shows first.
  `sources/otani1930/chinese_parallels.tsv`: Kangyur parallels from the Otani concordance (西藏大藏經甘殊爾勘同目錄,
  1930-32), Peking numbers tied to Derge through rKTs. It adds Chinese translations outside the Koryŏ canon (e.g. T892
  for the *Hevajra Tantra*). `check` tells how far a Taishō number is supported: `title_match` (the book's title agrees
  with the Taishō title), `part_of_taisho` (a part of a larger Taishō text), `taisho_title_differs` (probably a misread
  number; not shown on the reading site), `no_taisho`. The `coined` Chinese titles in `titles.tsv` are left as they
  are; replacing them with the titles of these translations is a separate decision.

## Sources kept apart

Data taken from works published in the 20th century or later sits in `catalog/sources/<source>/`, one folder per
source, each with a README giving the citation and its rights status, and each added in a commit of its own. If a
rights holder asks, that folder is removed in one commit and the rest of the edition is unaffected: the main files
(`titles.tsv`, `persons.tsv`, the text and the apparatus) use only public-domain material (ACIP, Esukhia), CC0 data
(rKTs) and this project's own work (coined titles, Korean readings made by rule). 84000 English titles are prepared as
`sources/84000/` but not published.

## Status of review

Corrections were reviewed by rule-level samples (in part with an AI assistant). An independent expert evaluation
has not yet been done. Use `status` and `confidence` when you need only well-supported corrections.

## License and citation

Corrected text, apparatus and catalogues: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) (`LICENSE`).
The ACIP originals are public domain by the Asian Legacy Library's dedication; the Esukhia witnesses are public
domain reproductions. See `NOTICE.md`.

Cite as: Yoon, Insang. *Dhammareader Tibetan Canon Edition*. Dhammareader, 2026.
https://doi.org/10.5281/zenodo.24213725. `CITATION.cff` holds the same citation (GitHub: *Cite this repository*).
That DOI always leads to the latest release; each release is archived on Zenodo with a DOI of its own (2026-11-14:
[10.5281/zenodo.24213726](https://doi.org/10.5281/zenodo.24213726)). Cite the release DOI when you need the exact data
you used.

## 한국어 요약

ACIP 티벳대장경(칸규르 라싸판, 텐규르 델게판)과 숭붐의 교정 Unicode 원문과 교정 기록이다. 원문은 덮어쓰지 않고, 교정마다
유형·근거·상태를 기록하며, 교정본은 채택된 기록만으로 기계적으로 다시 만든다. 숭붐은 대조 판본이 없어 규칙 교정과 변환기 수정만 반영했고, 저작권이 남은 145편(1955년 이후 사망 저자의 저작, 현대 편찬물,
『장한대사전』)은 뺐다. 한국어 번역문은 이 레포에 없다.

`catalog/titles.tsv`에는 문헌마다 ACIP·와일리·티벳 문자·범어·한문·한글 제목과 티벳어·범어 제목의 한글 발음이 있다. 티벳어
제목은 범어 제목을 뜻으로 옮긴 것이라 세 제목은 서로 다른 이름이며, 한문 제목은 실제 한역본 이름(`established`), 대응
한역 경전 이름(`parallel`), 이 프로젝트가 지은 이름(`coined`)을 구별한다. 티벳어 발음은 동국대 티벳장경연구소
「티벳어 한글 표기안」(2010)을 따른다. `catalog/persons.tsv`는 저자·역자·교정자와 그 한글 표기, `catalog/sources/`는 20세기 이후 출판물에서 온 자료를 출처별 폴더로 따로 둔 것으로 저작권 요청이 있으면 그 폴더만 내린다(Lancaster 목록의
고려대장경·대정장 실제 한역본 대응, KABC 고려장 제목).

### 알려진 문제: 마디 경계

마디(SID)는 문장이 아니라 ACIP 입력본의 쪽 덩어리(줄 묶음 사이의 빈 줄)를 따라 나뉘어 있다. 848,576마디 가운데
286,340개(34%)가 쎄 없이 구 가운데서 끝나고, 문장 가운데서 끝나는 것은 더 많다. 2,000자(티벳 문자)를 넘는 마디가
38,875개이고 가장 긴 것은 약 11만 자다. 본문은 빠짐없이 차례대로 들어 있으니 앞뒤 마디를 이어 읽으면 된다. 문장을
기준으로 다시 나눈 판을 준비하고 있다. 마디는 단락 크기로 하고 문장과 게송의 구는 하위 단위로 두며, 게송은 한 연이 한
마디다. 새 판은 지금의 SID와 글자 범위마다 새 마디를 잇는 대응표와 함께 낸다. 지금 SID를 다른 글에 다시 쓰지
않으므로 이 판을 가리킨 번호는 그대로 유효하다.

### 사용법

`.jsonl.gz`는 gzip으로 압축한 JSON Lines 파일이다. 한 줄이 JSON 객체 하나다. `text/`에서는 한 줄이 문헌의 마디(SID) 하나이고,
`apparatus/`에서는 한 줄이 교정 기록 하나다. 프로그램 없이 보려면 반디집이나 7-Zip으로 압축을 풀고 `.jsonl`을 메모장이나
VS Code로 연다. `catalog/*.tsv`는 엑셀의 「데이터 → 텍스트/CSV에서 가져오기」로 UTF-8, 탭 구분을 골라 연다.

1. `catalog/titles.tsv`에서 Toh 번호나 티벳어·범어·한문·한글 제목으로 문헌을 찾는다. 중론은 `work_id`가
   `tibetan:tengyur:TD03824`이고, 파일은 `text/tengyur/TD03824.jsonl.gz`와 `apparatus/tengyur/TD03824.jsonl.gz`다.
2. 본문 파일의 각 줄에서 `acip_original`은 ACIP 로마자 원본, `tibetan_unicode`는 그것을 티벳 문자로 바꾼 것,
   `tibetan_unicode_corrected`는 교정한 마디에만 있는 교정본이다. 값이 `null`이면 `tibetan_unicode`가 곧 교정본이다.
   위 *Quick start*의 첫 번째 코드가 문헌 전체의 교정본을 마디 번호와 함께 뽑는다.
3. 교정 기록의 각 줄은 어느 마디(`sid`)의 몇 번째 글자부터 몇 번째 글자까지(`span_start`, `span_end`)를 무엇에서
   (`lemma`) 무엇으로(`reading`) 고쳤는지, 근거 판본과 쪽·줄(`evidence`), 상태(`status`)를 담는다. 본문에 반영된 것은
   `accepted`뿐이다. `proposed`는 기록만 한 것으로, 라싸판과 데르게판이 갈리는 곳 같은 이본(`type` `V`)이 여기에 든다.
   `action`은 그 기록이 하는 일이다. `correct`는 `lemma`를 `reading`으로 바꾸고, `keep_ours`는 우리 읽기를 두고 대조본
   읽기를 기록하며, `keep_tagged`는 본문을 그대로 두고 표시만 한다(범어 음사).
