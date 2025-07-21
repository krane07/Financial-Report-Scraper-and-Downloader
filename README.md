# Financial Report Scraper and Downloader

This Python script automates the process of extracting report metadata and downloading associated PDF documents from a paginated web interface. It's designed to work with websites that display financial or research reports in a tabular format, where each report has detailed information spread across multiple table rows and a downloadable PDF linked via an intermediary JavaScript function.

**Disclaimer:** The URLs used in this script (and the accompanying notebook) are dummy placeholders (`https://www.example.com/`) for demonstration purposes and privacy. This script is intended as an educational example of web scraping techniques using Selenium, BeautifulSoup, and Requests. To use it on a real website, you would need to replace the dummy URLs and potentially adjust selectors (`class_`, `id`, etc.) to match the target site's actual HTML structure.

---

## 1. Description of the Target Website Structure

This script is built to interact with a hypothetical financial research platform that presents search results or report listings in a specific way:

* **Report Listings:** Reports are displayed in a main table. Each logical report entry spans two HTML table rows (`<tr>`).
    * One row (`base_data`) contains primary information like Document Date, Contributor, Headline, Author, Language, and Pages. It also contains the initial (often JavaScript-based) link to download the PDF.
    * The subsequent row (`add_data`) contains additional details like Tickers, Company Names, Category, Countries, Industries, Regions, Subjects, and Report Styles. These are typically presented with bold (`<b>`) tags as headers for each data point.
* **Pagination:** The search results are paginated, with a fixed number of reports (e.g., 25) displayed per page. Navigation to the next page is handled via specific links usually found within a `<span class="pagenav">` element.
* **PDF Download Mechanism:** Directly clicking the PDF link from the main results page might trigger a JavaScript function (e.g., `javascript:loadDocument('docid')`). The script handles this by:
    1.  Extracting a `docid` from this JavaScript call.
    2.  Constructing an intermediate URL (e.g., `https://www.example.com/investextsearchlive.php?opt=loadDocument&docid=[%22{docid}%22]&doctype=pdf`).
    3.  Requesting this intermediate URL, which then serves a page containing a `<script>` tag.
    4.  Within that `<script>` tag, the *actual* direct download URL for the PDF is embedded, which the script extracts and then uses to download the file.

## 2. Prerequisites

Before running this script, ensure you have the following installed:

* **Python 3.x**
* **pip** (Python package installer)
* **Google Chrome browser**
* **ChromeDriver:** The WebDriver executable for Chrome. Download the version matching your Chrome browser from [Chromium Downloads](https://chromedriver.chromium.org/downloads). Place `chromedriver.exe` in a location accessible by your system's PATH, or update the `driver = webdriver.Chrome(r'path/to/chromedriver.exe')` line in the notebook with the full path to the executable.

## 3. Installation

1.  **Clone the repository (or copy the notebook):**
    ```bash
    git clone [https://github.com/your-username/your-repo-name.git](https://github.com/your-username/your-repo-name.git)
    cd your-repo-name
    ```
2.  **Install the required Python libraries:**
    ```bash
    pip install selenium beautifulsoup4 requests pandas
    ```
3.  **Download and configure ChromeDriver:**
    * Download `chromedriver.exe` from [https://chromedriver.chromium.org/downloads](https://chromedriver.chromium.org/downloads).
    * **Option A (Recommended):** Add the directory containing `chromedriver.exe` to your system's PATH environment variable.
    * **Option B:** In the Python notebook, update the line `driver = webdriver.Chrome(r'path/to/chromedriver.exe')` to point to the exact location of your `chromedriver.exe` file. For example:
        ```python
        driver = webdriver.Chrome(r'C:\WebDriver\chromedriver.exe') # Windows
        # or
        driver = webdriver.Chrome(r'/usr/local/bin/chromedriver') # macOS/Linux
        ```
4.  **Create an `output` directory:** The script will attempt to save PDFs into a folder named `output` in the same directory as your notebook. Create this folder manually:
    ```bash
    mkdir output
    ```

## 4. Usage

1.  **Open the Python notebook** (e.g., `scraper_notebook.ipynb`) in your preferred environment (Jupyter Lab, VS Code, etc.).
2.  **Update the initial URL:**
    * Locate the commented line `# driver.get(url)` in the notebook. Uncomment it and replace `url` with the actual URL of the search results page you want to scrape.
3.  **Run all cells** in the notebook.

The script will launch a Chrome browser instance, navigate through the pages, extract data, and download PDFs. Progress will be printed to the console within the notebook.

## 5. Output

Upon successful execution, the script will generate:

* **`Financial_Analyst_report.xlsx`**: An Excel file containing all the extracted report metadata (Document Date, Contributor, Headline, etc.) in a tabular format, saved in the same directory as the notebook.
* **PDF files**: Downloaded PDF documents saved into the `output/` directory, named uniquely (e.g., `202300.pdf`, `202301.pdf`).
````
