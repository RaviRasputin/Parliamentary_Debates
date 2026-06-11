# Complete Parliament Debates of India, 1952–2026

**A full-text, machine-readable corpus of the debates of both Houses of the Indian Parliament — the Lok Sabha and the Rajya Sabha — from the first sittings of 1952 to the present.**

- **149 files** of plain Markdown: one consolidated volume per House per year
- **~989 million words** (~6.4 GB of raw text)
- **Lok Sabha:** 1952–2026 · 75 files · ~584 million words
- **Rajya Sabha:** 1952–2025 · 74 files · ~405 million words
- Digitised from the scanned **Official Reports** (the verbatim record published by the two Secretariats)

Seventy-five years of Indian parliamentary history — every Question Hour, every Budget speech, every no-confidence motion, every bill debate for which an Official Report was published — in a form you can search in seconds with ordinary text tools.

## Getting the data

GitHub does not allow files larger than 100 MB inside a repository, so the corpus is attached to this repository's **[Releases page](../../releases/latest)** as two compressed archives:

| Archive | Contents | Raw size | Compressed |
|---|---|---|---|
| `lok-sabha-debates-1952-2026.tar.xz` | `lok-sabha/LS 1952.md` … `LS 2026.md` (75 files) | 3.4 GB | 853 MB |
| `rajya-sabha-debates-1952-2025.tar.xz` | `rajya-sabha/RS 1952.md` … `RS 2025.md` (74 files) | 2.6 GB | 497 MB |

The archives are in `.tar.xz` format (LZMA2, maximum compression), which shrinks the OCR text to roughly a fifth of its raw size while remaining a completely standard format.

**To extract:**

- **Linux / macOS** — `tar -xf lok-sabha-debates-1952-2026.tar.xz`
- **Windows 11 (23H2 or later)** — right-click the file → *Extract All* (File Explorer supports `.tar.xz` natively)
- **Older Windows** — open with the free [7-Zip](https://www.7-zip.org/)
- **Python** — `import tarfile; tarfile.open("lok-sabha-debates-1952-2026.tar.xz").extractall()`

Each archive unpacks into its own folder (`lok-sabha/` or `rajya-sabha/`). SHA-256 checksums for both archives are in [`SHA256SUMS.txt`](SHA256SUMS.txt).

## How this corpus was built

1. **Collection.** The scanned Official Report volumes were downloaded from the Parliament of India's digital archives — the Lok Sabha Digital Library ([eparlib.nic.in](https://eparlib.nic.in)), the Rajya Sabha Debates repository ([rsdebate.nic.in](https://rsdebate.nic.in)) and, for recent years, Digital Sansad ([sansad.in](https://sansad.in)). Lok Sabha debates were collected document-wise and Rajya Sabha debates sitting-by-sitting (the original day-file names, e.g. `F13.05.1952.pdf`, are preserved inside the corpus).
2. **Consolidation.** For each House and each calendar year, the source PDFs were merged into one consolidated volume (for the Lok Sabha these companion PDFs run to tens of thousands of pages per year — `2005.pdf` alone is 22,673 pages).
3. **Text extraction.** The OCR text layer of the scanned pages was extracted and written, page by page and sitting by sitting, into one Markdown file per House per year. The text is reproduced **as-is from the OCR layer** — no manual correction, no editing, no omission.
4. **Packaging.** The 149 Markdown files were compressed with `xz -9` into the two archives published under Releases, and this README was written to document the corpus.

Compilation completed in May–June 2026.

## What the files look like

**Lok Sabha files** (`LS 1952.md` … `LS 2026.md`) open with a short header identifying the year and the companion consolidated PDF, followed by the OCR text with an HTML comment marking every page:

```
# Lok Sabha Debates — 2005

Source: `2005.pdf` (22673 pages)

<!-- Page 1 -->
Fourteenth Series, Vol. X, No. 31
Wednesday, May 4, 2005
Vaisakha 14, 1927 (Saka)
LOK SABHA DEBATES
(English Version)
...
```

Sitting dates appear in the running heads (`Wednesday, May 4, 2005`). Dates are also printed in the Saka era (`Vaisakha 14, 1927 (Saka)`) — add 78 (January–March: 79) to convert a Saka year to CE. The 1952 volume also contains the **Provisional Parliament** debates of early 1952, and the earliest volumes use the House's original name, *House of the People*.

**Rajya Sabha files** (`RS 1952.md` … `RS 2025.md`) are divided into one section per sitting day, each headed by the date and the name of the original source PDF:

```
# Rajya Sabha Debates — 1952

## 13 May 1952

`Source: F13.05.1952.pdf`

THE PARLIAMENTARY DEBATES
OFFICIAL REPORT
IN THE FIRST SESSION OF THE COUNCIL OF STATES ...
```

There are 5,607 such day-sections across the 74 Rajya Sabha files. The `## DD Month YYYY` markers make it easy to recover the sitting date for any passage. Volumes before August 1954 use the House's original name, *Council of States*.

Header layout varies slightly between volumes (the files were built in batches), but the body is always the raw OCR text of the Official Report.

## Coverage and known gaps

- **Lok Sabha:** 1952 (including the Provisional Parliament) through the **2026 Budget Session (April 2026)**.
- **Rajya Sabha:** 13 May 1952 (the first sitting of the Council of States) through **12 December 2025**. Sittings after that date — including the remainder of the 2025 Winter Session — and all of 2026 are not yet included.
- Coverage within a year depends on what the official archives have published; individual sittings or volumes missing from the archives are missing here too.

## Caveats — read before quoting

This is an **OCR corpus of an official record, not the official record itself.**

1. **OCR noise is real.** The text layer is reproduced uncorrected, so artefacts are everywhere, especially in older volumes ("LOK SABRA DEBATES", "Thirteenth Serin"). English text is nonetheless highly searchable and quotable; expect to silently normalise small artefacts when quoting.
2. **Numbers can misread.** A division figure can come out wrong by a digit (the corpus's OCR of the July 2008 trust vote, for instance, shows the Noes as 258 where the established figure is 256). **Always verify names, figures and exact wording against the official PDF before publishing.**
3. **Hindi and other non-English text is unreliable.** Devanagari passages in the scans are frequently garbled by font-encoding faults in the OCR layer. Treat this as an **English-language corpus**; recover Hindi speeches from the original PDFs.
4. **20 of the 149 files contain stray NUL bytes** (an OCR by-product). Many search tools treat such files as binary and silently skip or truncate matches — use `grep -a` / `rg -a`, or read the files in Python, which is unaffected.
   <details><summary>The 20 affected files</summary>

   LS 1958, LS 1965, LS 1981, LS 1982, LS 1987, LS 2012, LS 2013, LS 2021, LS 2022, RS 1999, RS 2000, RS 2002, RS 2005, RS 2008, RS 2014, RS 2021, RS 2022, RS 2023, RS 2024, RS 2025
   </details>
5. **Line layout varies by year.** Some volumes are one-OCR-fragment-per-line (very short lines), others are reflowed paragraphs, depending on the source scan. Reflow before reading: `sed -n 'START,ENDp' file.md | tr '\n' ' ' | tr -s ' '`.

## Working with the corpus

The corpus is too large to read; the way in is **search**. A workflow that works well:

```bash
# 1. Heat map: how often does a phrase occur in each year?
grep -ac "atomic energy" lok-sabha/LS*.md | sort -t: -k2 -rn | head

# 2. Locate the passages inside a promising year
grep -an "atomic energy" "lok-sabha/LS 1962.md" | head

# 3. Extract and reflow a window around line N for reading
sed -n '41200,41400p' "lok-sabha/LS 1962.md" | tr '\n' ' ' | tr -s ' '
```

In Python:

```python
from pathlib import Path

text = Path("rajya-sabha/RS 1974.md").read_text(errors="ignore")
for chunk in text.split("\n## ")[1:]:          # one chunk per sitting day
    date, body = chunk.split("\n", 1)
    if "nuclear" in body.lower():
        print(date, body.lower().count("nuclear"))
```

Tips that save time:

- Recover any quote's sitting date from the nearest preceding `## DD Month YYYY` marker (Rajya Sabha) or `Weekday, DD Month YYYY` running head (Lok Sabha).
- Speaker turns begin with a name label (`Shri X:`, `Dr. Y:`, `The Minister of … (Shri Z):`, `Mr. Speaker:`). OCR sometimes renders the colon as a semicolon — make patterns tolerant.
- Sitting dates can be sanity-checked: the day-of-week in the running head should match the calendar date (Parliament did not sit on Sundays).

## Using it as a primary source

These are the verbatim Official Reports — what was actually said on the floor of both Houses, with the date, the House and (recoverable from context) the speaker. That supports research that secondary accounts cannot:

- **Legislative history** — follow any Act from introduction through debate to passage across both Houses and across years.
- **Political and institutional history** — track how an idea, phrase or policy entered and left parliamentary discourse over 75 years.
- **Biography** — extract every recorded intervention of a particular member across their career.
- **Quantitative discourse analysis** — count, by year and by House, the vocabulary of any debate you care about.
- **NLP and digital humanities** — to our knowledge one of the largest open full-text corpora of Indian parliamentary proceedings (~989 million words spanning 75 years).

For published work, treat the corpus as the *finding aid* and the official publication as the *authority*: locate the passage here, then verify it in the official PDF identified by the file's year (and, for the Rajya Sabha, the day-file named in the `Source:` line).

## Citing

Cite the underlying record and the corpus, for example:

> Lok Sabha Debates, Official Report, 4 May 2005, Lok Sabha Secretariat, New Delhi. Retrieved via *Complete Parliament Debates of India, 1952–2026*, https://github.com/USERNAME/REPOSITORY (accessed DD Month YYYY).

## Status and licence

The parliamentary proceedings themselves are the public record of the Parliament of India, published by the Lok Sabha and Rajya Sabha Secretariats; no rights are claimed over that text, and the authoritative versions remain the official publications. This unofficial compilation (the consolidation, file structure and documentation) is released under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — use it freely with attribution.

This project is not affiliated with or endorsed by the Parliament of India. Errors introduced by OCR are unintentional; corrections are welcome via issues and pull requests.

## Acknowledgements

The Lok Sabha and Rajya Sabha Secretariats and the teams behind eparlib.nic.in, rsdebate.nic.in and sansad.in, whose digitisation of the Official Reports made this corpus possible.
