# Dutch Parliament Proceedings Scraper

This repository contains a collection of Python scripts for scraping, processing, and analyzing proceedings from the Dutch Parliament (Handelingen). The scraper collects data from the official government website [officielebekendmakingen.nl](https://zoek.officielebekendmakingen.nl/).

## Overview

The scripts are organized into three main phases:

1. **Data Collection** - Scraping proceedings links and downloading XML files
2. **Metadata Collection** - Gathering metadata for each proceeding
3. **Processing** - Parsing XML files and extracting structured data into CSV files

## Prerequisites

- Python 3.7+
- Required packages: `requests`, `lxml`, `pandas`, `cssselect`

Install dependencies:
```bash
pip install requests lxml pandas cssselect
```

## Scripts

In most files you can find the variable "vergaderjaren" or something similar that looks like this ["2019-2020", "2020-2021"]. Adjust it to scrape other years.

### Phase 1: Data Collection

#### 1. `officiele_bekendmakingen_site.py`
Scrapes links to parliamentary proceedings from the official website.
- Creates CSV files in `data/links/` containing links for each parliamentary year
- No command line arguments required

#### 1a. `handelingen_scraper.py`
Downloads the XML files for each proceeding.
- Downloads files to `data/handelingen/<year>/`
- Maintains a resume file to continue interrupted downloads
- Logs errors to `error_log.txt`
- No command line arguments required

#### 1b. `validate_downloads.py`
Validates the downloaded files against the collected links.
- Generates a validation report in `validation_report.txt`
- No command line arguments required

### Phase 2: Metadata Collection

#### 2. `scrape_meta.py`
Collects metadata for each proceeding from their HTML pages.
- Creates CSV files in `data/meta/` containing metadata
- Maintains a resume file to continue interrupted scraping
- Logs errors to `meta_error_log.txt`
- No command line arguments required

#### 2a. `validate_metadata.py`
Validates the collected metadata against the downloaded files.
- Generates a validation report in `meta_validation.log`
- No command line arguments required

#### 2b. `retry_failed_meta_scrape.py`
Retries metadata collection for failed files.
- Updates existing metadata files
- Logs retries to `meta_retry.log`
- No command line arguments required

### Phase 3: Processing

#### 3. `process.py`
Processes the XML files and extracts structured data about speeches.
- Creates CSV files in `data/parsed/` containing processed speeches
- Logs processing to `parser.log`
- No command line arguments required

#### 3a. `preview_csv.py`
Previews the processed speeches data.
- Optional arguments:
  - `year`: Parliamentary year to preview (default: "2022-2023")
  - `max_rows`: Number of rows to show (default: 10)
  - `text_length`: Maximum length for speech text preview (default: 100)

#### `analyse_xml.py`
Development tool for analyzing XML structure and content.
- Processes first 5 files from 2022-2023
- Useful for debugging and understanding the XML structure
- No command line arguments required

## Directory Structure

```
.
├── data/
│   ├── handelingen/     # Downloaded XML files
│   ├── links/          # CSV files with proceeding links
│   ├── meta/           # Metadata CSV files
│   └── parsed/         # Processed speech data
├── *.log               # Various log files
└── error_log.txt       # Error logs
```


## Error Handling

- All scripts include error logging and can resume from interruptions
- Failed downloads and metadata collection are logged to respective error log files
- Validation scripts help identify any missing or problematic files

## Notes

- The scraper includes rate limiting and random delays to avoid overloading the server
- Downloads are resumable in case of interruption
- The scripts process parliamentary years 2021-2022, 2022-2023, and 2023-2024 by default

## License

This work is licensed under GPLv3

## Collaboration

Contact Pieter van Boheemen p.vanboheemen@democratiemonitor.nl
