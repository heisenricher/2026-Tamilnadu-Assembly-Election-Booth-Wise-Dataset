# Tamil Nadu 2026 Assembly Election Booth-Wise Dataset

This dataset contains the parsed booth-wise election results (Form 20) and official polling station details for all 234 Assembly Constituencies in Tamil Nadu for the 2026 State Legislative Assembly Elections.

The source PDFs were obtained from the Chief Electoral Officer (CEO), Tamil Nadu. The CSVs were generated using a custom extraction pipeline built on Python and PDFPlumber.

## Repository Layout

The dataset is organized by constituency folders. Each folder contains:
1. The official Form 20 PDF source.
2. The parsed booth-wise election results in CSV format.
3. The official Polling Station Details PDF containing address and location metadata.

```text
boothwise_dataset/
├── 001-Gummidipoondi/
│   ├── 001-Gummidipoondi_Form_20.csv
│   ├── 001-Gummidipoondi_Form_20.pdf
│   └── 001-Gummidipoondi_Poll_Station_Details.pdf
├── 002-Ponneri (SC)/
│   └── ...
└── 234-Killiyoor/
```

Note: A few polling station details PDFs (17 out of 234, mostly in Coimbatore, Erode, and Mayiladuthurai districts) are missing because they were not uploaded/available on the official CEO Tamil Nadu server (returning 404 errors during extraction).

## CSV Column Schema (`*_Form_20.csv`)

The columns match the structure of the official Form 20 tables:

* **Polling Station No**: Integer (booth number, sequential from 1).
* **[Candidate Name]**: Integer (number of EVM votes cast for the candidate).
* **Total Valid Votes**: Integer (sum of EVM votes cast across all candidates).
* **No. of Rejected Votes**: Integer (invalid/rejected votes).
* **NOTA**: Integer (None of the Above votes).
* **Total**: Integer (total votes polled at the booth).
* **No. of Tendered Votes**: Integer (tendered votes).

Note: The "No. of Electors" column has been intentionally excluded to keep the focus on cast votes.

## Pipeline and Cleaning Logic

The raw Form 20 PDFs have multiple template inconsistencies and scanning variations. The extraction pipeline applied the following cleaning steps:
1. **Header and Footer Removal**: Table headers repeated on each page, column indexing guide rows, and page footers were systematically identified and stripped.
2. **Round Totals and Summaries**: Intermediate round-wise summary rows were filtered out to avoid double-counting.
3. **Scanned Documents (AC151 - AC159)**: Scanned image PDFs for constituencies 151 to 159 were digitized using layout-aware OCR extraction and verified against physical table row counts.
4. **Mock Poll Filtering**: Diagnostic entries (e.g. mock poll tests) were stripped.
5. **Auxiliary Booths**: Handled alphanumeric auxiliary booth entries (e.g., suffix 'A' booths) to ensure proper sequential ordering.

## Verification

The row counts in the generated CSVs were verified programmatically against the physical booth count inside the official Form 20 PDFs. All 234 CSVs have been checked and confirmed to have a 100% match rate with 0 mismatches.

## Keywords & Tags

* **Kaggle Tags**: `india`, `politics`, `government`, `tabular-data`, `geographic-data`
* **GitHub Topics**: `tamil-nadu`, `elections-data`, `booth-level-results`, `form-20`, `india-elections`, `dataset`

## License

This dataset is dedicated to the public domain under CC0 1.0 Universal (Public Domain Dedication).
