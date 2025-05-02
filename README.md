# Binance Futures Data Downloader & Verifier

This project provides a unified tool to discover, download, verify, and extract historical market data for Binance USDⓈ-M Futures (e.g., klines, trades, funding rates).

## Project Structure

-   `unified_downloader.py` - The main script for discovering data availability, downloading, verifying checksums, extracting archives, and reporting missing files.
-   `data_availability.json` - Stores the discovered earliest available date for each data type/symbol combination to speed up subsequent runs.
-   `data_verification_plan.md` - Documentation of the verification process (may need updates based on the unified script).
-   `venv_utils.py` - Virtual environment utilities.
-   `requirements.txt` - Lists the required Python packages.

## Directory Structure

-   `downloads/` - Contains downloaded raw `.zip` archive files, organized by symbol/type/interval. (Can be cleaned up automatically by the script).
-   `data/` - Contains the extracted `.csv` data files, organized by symbol/type/interval.
-   `logs/` - Contains timestamped application log files.
-   `reports/` - Contains `.csv` reports detailing any missing or invalid files found during the process.

## Features

-   **Data Discovery:** Automatically finds the earliest available date for requested data types and symbols using efficient binary search. Caches results in `data_availability.json`.
-   **Unified Process:** Downloads, verifies checksums, extracts `.zip` files, and verifies `.csv` files in a single run.
-   **Asynchronous Operations:** Uses `asyncio` and `aiohttp` for efficient, concurrent downloading and checking.
-   **Configurable:** Supports multiple symbols, various data types (klines, trades, funding rates, etc.), and a configurable start date.
-   **Robust:** Includes retries for network operations and checksum verification for data integrity.
-   **Reporting:** Generates a report of any missing or invalid data files.
-   **Logging:** Provides detailed logging to both console and timestamped files.

## Getting Started

1.  **Setup Environment:** Create and activate a Python virtual environment (Python 3.8+ recommended).
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows use `venv\Scripts\activate`
    ```
2.  **Install Dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
3.  **Run the Downloader:** Execute the `unified_downloader.py` script. See "How to Use" for options.

## How to Use

Run the script from your terminal. You can specify symbols and a start date.

**Basic Usage (Defaults: BTCUSDT, BTCUSDC starting from 2020-01-01):**
```bash
python unified_downloader.py
```

**Specify Symbols:**
```bash
python unified_downloader.py --symbols BTCUSDT,ETHUSDT,BNBBTC
```

**Specify Start Date (YYYY-MM-DD):**
```bash
python unified_downloader.py --start-date 2022-01-01
```

**Specify Symbols and Start Date:**
```bash
python unified_downloader.py --symbols ETHUSDT --start-date 2021-06-15
```

**Enable Verbose Logging:**
```bash
python unified_downloader.py --verbose
```

The script will:
1.  Load existing data availability from `data_availability.json` or run discovery if needed.
2.  Iterate through the specified date range for each symbol and data type.
3.  Check if the final `.csv` file exists and is valid.
4.  If not, check for the `.zip` file, verify its checksum, and download/re-download if necessary.
5.  Extract the `.zip` file and verify the resulting `.csv`.
6.  Log progress and errors.
7.  Generate a report in the `reports/` directory if any files are missing or invalid after processing.

## Logs and Reports

-   Log files are generated in the `logs/` directory with timestamps (e.g., `logs/unified_downloader_YYYYMMDD_HHMMSS.log`).
-   Missing file reports are saved in the `reports/` directory (e.g., `reports/missing_files_report_YYYYMMDD_HHMMSS.csv`).