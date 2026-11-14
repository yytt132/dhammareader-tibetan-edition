# Dhammareader Tibetan Canon Edition

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.24213725.svg)](https://doi.org/10.5281/zenodo.24213725)

A corrected Unicode edition of the ACIP Tibetan Kangyur (Lhasa edition), Tengyur (Derge edition) and Sungbum, with an
apparatus that records every correction, its type, its evidence and what is still unresolved.
Reading site: [dhammareader.com](https://dhammareader.com). 한국어 요약은 아래에 있다.
Release announcement: [English](ANNOUNCEMENT.md) · [한국어](ANNOUNCEMENT.ko.md).

## What is here

| Path | Content |
|---|---|
| `text/<collection>/<work>.jsonl.gz` | One line per segment (SID), in reading order: `block_type`, `boundary_status`, `acip_original`, `tibetan_unicode` (plain conversion), `tibetan_unicode_corrected` (only where a correction applies), `children` (its sentences, verse lines or mantra phrases), `toh` (see *Segments*) |
| `apparatus/<collection>/<work>.jsonl.gz` | Apparatus entries: span in the ACIP original or the Unicode text of a segment (`sid`; `end_sid` where a span runs on into a later segment), lemma, reading, type, action, rule, confidence, status (`accepted` / `proposed` / `rejected` / `unresolved`), evidence (witness and folio.line), and `old_sid`, the entry's SID in the 2026-11-14 release |
| `crosswalk/<collection>/<work>.tsv.gz` | From the SIDs of the 2026-11-14 release to the segments of this release: for every old SID, the part of its text that each new segment holds, as offsets in the ACIP and in the Unicode text on both sides |
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

## Segments

A segment is cut on what the text says, not on the layout of the input: a work, or one source file of it, is read as
one string, with line breaks and blank lines as spaces.

- Titles (Sanskrit and Tibetan), the translator's homage, *bam po* headings (with the running title before them),
  chapter-end titles and colophons are segments of their own.
- Verse: four or more lines of the same syllable count that end in a double shad. Four lines make a stanza, and a
  stanza is one segment.
- A run of mantra or dhāraṇī is one segment.
- Prose: a new segment begins at a speech turn (… *gsol pa*, … *bka' stsal pa*) and at a discourse transition
  (*de nas* …). Inside, sentences are joined until the segment is about 300 Tibetan characters long, and it closes at
  the next sentence end: a final particle that agrees with the verb (*yod do*, *yin no*, *'gyur ro*, *pa'o*) or a
  question particle (*snyam mam*). A segment longer than about 800 characters may close at a clause that ends in a
  verb, and one longer than about 1,500 at any shad.

`block_type` names what a segment is (`prose_block`, `verse_stanza`, `mantra_dharani`, `sanskrit_title`,
`tibetan_title`, `homage`, `chapter_title`, `title`, `colophon`). `boundary_status` says how sure its end is:
`rule_confident` for headings and titles, `provisional` for sentence and stanza ends, `review_required` where a long
stretch with no sentence end was cut at a shad or, with no shad at all, between syllables (2,996 segments).

`children` are the sentences of a prose segment, the lines of a stanza and the phrases of a mantra. `start` and `end`
are offsets into `acip_original`; `unicode_start` and `unicode_end` are offsets into `tibetan_unicode`, `null` where
the shads of the two texts do not line up.

### From the 2026-11-14 release

The segments of the 2026-11-14 release followed the page blocks of the ACIP input (the blank lines between groups of
lines). A third of them ended inside a phrase, and some ran to 110,000 characters. They are replaced here; the text is
the same. Read in order, the new segments of a work give exactly the ACIP, Unicode and corrected text of the old ones
(spaces aside), and every apparatus entry is kept, anchored on the segment that now holds its span.

Old SIDs are not given to other text: the new segments of a work are numbered on from its last old SID (TD03824 had
`s000000001`–`s000000210`; its new segments begin at `s000000211`). `crosswalk/` follows any old SID and character
range into the new segments, and the 2026-11-14 release stays on Zenodo
([10.5281/zenodo.24213726](https://doi.org/10.5281/zenodo.24213726)). The reading site still shows the 2026-11-14 segments and
their SIDs while the Korean translations are being moved to the new segments.

## Known issues

- In 588 segments the corrected text is not what the segment's accepted entries alone give. The converter fix
  (`P03`) and rule corrections were applied by converting a whole old segment again, and that also changed a few
  places that have no entry of their own (mostly `W'A`, written ཝཱ instead of ཝྰ). This release keeps the corrected
  text as it was published.
- In 31,187 segments (2%) `tibetan_unicode` is not what today's converter makes of `acip_original`: it was converted
  from a cleaned input (notes in brackets left out) or before the converter fix. Apparatus offsets refer to the stored
  `tibetan_unicode`.

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
        text = seg['tibetan_unicode_corrected']
        print(seg['sid'], seg['tibetan_unicode'] if text is None else text)
```

`tibetan_unicode_corrected` is `null` where no accepted correction applies; the segment then reads as
`tibetan_unicode`. It is an empty string where the corrections remove the whole segment (a note moved out of the
text, for example), so test it against `null`, not for being empty. `acip_original` is the ACIP romanized text as
published.

The verses of the same work, stanza by stanza and line by line:

```python
with gzip.open('text/tengyur/TD03824.jsonl.gz', 'rt', encoding='utf-8') as f:
    for line in f:
        seg = json.loads(line)
        if seg['block_type'] == 'verse_stanza':
            for c in seg['children']:
                print(seg['sid'], c['id'], seg['tibetan_unicode'][c['unicode_start']:c['unicode_end']])
```

Where an SID of the 2026-11-14 release went:

```python
import csv

with gzip.open('crosswalk/tengyur/TD03824.tsv.gz', 'rt', encoding='utf-8') as f:
    for row in csv.DictReader(f, delimiter='\t'):
        if row['old_sid'] == 'tibetan:tengyur:TD03824:s000000001':
            print(row['new_sid'], row['old_start'], row['old_end'])
```

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
6. **Segments.** The text was cut again by the rules under *Segments* (see *From the 2026-11-14 release*). The rules
   were checked on 20 works of every kind (sūtra, Prajñāpāramitā, Vinaya, dhāraṇī, tantra, verse and prose treatises,
   praise, sādhana, grammar, Sungbum commentary, songs and ritual) before they were applied to the whole canon.

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

### 마디

마디는 입력본의 줄과 빈 줄이 아니라 글 자체를 보고 나눈다. 제목·역자 귀경·바모 표제·품 끝 표제·간기는 따로 한
마디이고, 게송은 같은 음절 수의 구 넷이 한 연으로 한 마디, 진언·다라니는 이어진 한 덩어리가 한 마디다. 산문은
말차례(… *gsol pa*, … *bka' stsal pa*)와 전환어(*de nas* …)에서 새 마디를 열고, 그 안에서는 문장을 이어 약 300자가
되면 다음 문장 끝(동사와 맞는 종결어미, 의문 조사)에서 닫는다. `block_type`은 마디의 종류, `boundary_status`는 경계의
확실성(`review_required`는 문장 끝이 오래 없어 쎄에서 강제로 자른 곳)이다. `children`은 산문의 문장, 게송의 구, 진언의
구절이며 `acip_original`과 `tibetan_unicode`의 글자 위치로 적는다.

2026-11-14 판의 마디는 ACIP 입력본의 쪽 덩어리를 따라 나뉘어, 3분의 1이 구 가운데서 끝나고 아주 긴 마디도 있었다. 이
판에서 그 마디를 바꿨고 글은 바꾸지 않았다. 새 마디를 차례로 이으면 옛 마디의 ACIP·유니코드·교정본과 (띄어쓰기를
빼고) 글자까지 같고, 교정 기록은 모두 그 자리를 담은 새 마디에 그대로 남는다(옛 번호는 `old_sid`). 옛 번호는 다른
글에 다시 쓰지 않는다. 새 마디는 문헌마다 옛 마지막 번호 다음부터 번호를 받으며(중론: 옛 `s000000001`~`s000000210`,
새 `s000000211`부터), `crosswalk/`가 옛 번호와 글자 범위를 새 마디로 잇는다. 2026-11-14 판은 Zenodo에 그대로 있다. 읽기 사이트는 한국어 번역을 새 마디로 옮기는
동안 2026-11-14 판의 마디와 번호를 그대로 보여 준다.

알려진 문제: 588개 마디는 교정본이 채택된 기록만으로 만든 결과와 다르다. 변환기 수정(P03)과 규칙 교정을 옛
마디 전체를 다시 변환하는 방식으로 적용해서, 기록이 없는 몇 곳(주로 `W'A`: ཝྰ 대신 ཝཱ)도 함께 바뀌었기 때문이다. 이
판은 공개된 교정본을 그대로 둔다. 31,187개 마디(2%)는 `tibetan_unicode`가 지금 변환기로 `acip_original`을 바꾼 것과
다르다(괄호 주석을 뺀 입력본이나 변환기 수정 전에 변환한 것). 교정 기록의 글자 위치는 저장된 `tibetan_unicode`를 따른다.

### 사용법

`.jsonl.gz`는 gzip으로 압축한 JSON Lines 파일이다. 한 줄이 JSON 객체 하나다. `text/`에서는 한 줄이 문헌의 마디(SID) 하나이고,
`apparatus/`에서는 한 줄이 교정 기록 하나다. `crosswalk/`의 `.tsv.gz`는 압축을 풀면 탭으로 나눈 표다. 프로그램 없이 보려면 반디집이나 7-Zip으로 압축을 풀고 `.jsonl`을 메모장이나
VS Code로 연다. `catalog/*.tsv`는 엑셀의 「데이터 → 텍스트/CSV에서 가져오기」로 UTF-8, 탭 구분을 골라 연다.

1. `catalog/titles.tsv`에서 Toh 번호나 티벳어·범어·한문·한글 제목으로 문헌을 찾는다. 중론은 `work_id`가
   `tibetan:tengyur:TD03824`이고, 파일은 `text/tengyur/TD03824.jsonl.gz`와 `apparatus/tengyur/TD03824.jsonl.gz`다.
2. 본문 파일의 각 줄에서 `acip_original`은 ACIP 로마자 원본, `tibetan_unicode`는 그것을 티벳 문자로 바꾼 것,
   `tibetan_unicode_corrected`는 교정한 마디에만 있는 교정본이다. 값이 `null`이면 `tibetan_unicode`가 곧 교정본이다.
   빈 문자열이면 교정에서 그 마디의 글이 모두 빠진 것이다(본문 밖 주석으로 옮긴 경우 등).
   위 *Quick start*의 첫 번째 코드가 문헌 전체의 교정본을 마디 번호와 함께 뽑는다.
3. 교정 기록의 각 줄은 어느 마디(`sid`)의 몇 번째 글자부터 몇 번째 글자까지(`span_start`, `span_end`)를 무엇에서
   (`lemma`) 무엇으로(`reading`) 고쳤는지, 근거 판본과 쪽·줄(`evidence`), 상태(`status`)를 담는다. 본문에 반영된 것은
   `accepted`뿐이다. `proposed`는 기록만 한 것으로, 라싸판과 데르게판이 갈리는 곳 같은 이본(`type` `V`)이 여기에 든다.
   `action`은 그 기록이 하는 일이다. `correct`는 `lemma`를 `reading`으로 바꾸고, `keep_ours`는 우리 읽기를 두고 대조본
   읽기를 기록하며, `keep_tagged`는 본문을 그대로 두고 표시만 한다(범어 음사).
