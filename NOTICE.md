# Notice

- **ACIP / Asian Legacy Library texts** (Kangyur, Tengyur, Sungbum): dedicated to the public domain by the Asian
  Legacy Library, including commercial use, https://asianlegacylibrary.org/library/downloads/ (page captured
  2026-11-03; the Sungbum download is listed under the same dedication, checked 2026-11-14). The dedication covers
  ACIP's input of the texts, not the authors' own copyright: Sungbum works by authors who died in 1955 or later,
  and modern compilations and reference works, are left out of this edition (`catalog/left_out.tsv`,
  `excluded_copyright`).
- **Esukhia Derge Kangyur** (https://github.com/Esukhia/derge-kangyur, now OpenPecha P000001) and
  **Esukhia Derge Tengyur** (https://github.com/Esukhia/derge-tengyur): "a mechanical reproduction of a Public
  Domain work, and as such is also in the Public Domain". Used as collation witnesses; witness readings and
  folio locators appear in the apparatus `evidence`.
- **pyewts** (Apache License 2.0, Roger Espel Llima and Buddhist Digital Resource Center): ACIP/EWTS to Unicode
  conversion, with the wa-zur apostrophe fix described in `README.md`.
- **rKTs** (Resources for Kanjur & Tanjur Studies, University of Vienna; data CC0, https://github.com/brunogml/rKTs,
  https://www.rkts.org): standard Tibetan and Sanskrit titles, authors, translators, revisors and Taishō links in
  `catalog/titles.tsv` and `catalog/persons.tsv`.
- **Lewis R. Lancaster with Sung-bae Park, *The Korean Buddhist Canon: A Descriptive Catalogue*** (University of
  California Press, 1979): Tohoku–Koryŏ–Taishō number correspondences and titles in `catalog/sources/lancaster1979/`.
  Only these bibliographic facts are used; the book's text is not reproduced.
- **KABC 통합대장경 list** (동국대학교 불교학술원 아카이브, https://kabc.dongguk.edu/): Chinese and Korean titles of
  Koryŏ canon texts, cited as bibliographic facts, in `catalog/sources/kabc/`.
- **大谷大學圖書館 編『西藏大藏經甘殊爾勘同目錄』** (Kyoto, 1930-1932): Peking Kangyur numbers and the extant Chinese
  translations named in its 〔漢〕 lines (title, translator, Nanjio and Taishō numbers), in `catalog/sources/otani1930/`.
  Only these bibliographic facts are used.
- **「티벳어 한글 표기안」** (동국대학교 경주캠퍼스 티벳장경연구소, 2010): the rules followed for Korean readings of Tibetan.

Data from 20th-century or later publications is kept in `catalog/sources/<source>/`, one folder per source, so that
it can be withdrawn on its own if a rights holder asks (see README, *Sources kept apart*).
