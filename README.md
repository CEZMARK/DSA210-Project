# DSA210-Project
# Project Title: Analyzing the Relationship Between Socioeconomic Factors and Turkish Election Results

## Motivation

This project aims to explore the relationship between socioeconomic indicators and voting patterns in Turkey. By analyzing official election results at the provincial (or district) level alongside corresponding socioeconomic data, we hope to identify potential correlations and gain insights into the factors that may influence electoral outcomes in different regions of Turkey. Understanding these relationships can contribute to a deeper understanding of Turkish politics and the diverse factors shaping voter behavior. This project will avoid overly sensitive or subjective interpretations and will remain focused on a neutral, data-driven analysis.

## Data Sources

This project will utilize two primary data sources, with a preference for one due to data accessibility:

*   **Data Source 1: Turkish Election Results (Primary)**

    *   **Preferred Source:** `secim.sozcu.com.tr` (2023 Presidential Election Results)
        *   **Reasoning:** This news website provides election results in a readily accessible HTML format, making data extraction significantly easier than dealing with PDF files. Initial investigation suggests a well-structured website suitable for web scraping.
        *   **Data Granularity:** Provincial (il) level.
        *   **Data Elements:**
            *   Province Name (İl Adı)
            *   Candidate Names (Aday Adları)
            *   Vote Counts for each candidate (Oy Sayıları)
            *   Vote Percentages for each candidate (Oy Oranları)
        *   **Collection Method:** Web scraping using `requests` and `BeautifulSoup4` libraries in Python. The code will navigate to the main election results page, extract links to individual province pages, and then scrape the relevant data from each province's page.  The scraping will be conducted with extreme caution, including long delays between requests and a clear User-Agent, due to the lack of an easily accessible `robots.txt` file and Terms of Service on the Sözcü website. The intellectual property part of Sozcu ToS has been reviewed and scraping will only be attempted if absolutely necessary and with utmost respect for their rights.
        *   **Example URL Structure:** `https://secim.sozcu.com.tr/secim2023mayis14` (main page), with individual province pages linked from there.

    *   **Alternative Source (Less Preferred):** YSK (Yüksek Seçim Kurulu) Website
        *   **Reasoning:**  The official source for Turkish election results. However, initial investigation indicates that the results are primarily available in PDF format, which makes data extraction significantly more complex and error-prone.
        *   **Data Granularity:** Ideally district (ilçe) level, but provincial (il) level will be used if district-level data is consistently unavailable.
        *   **Data Elements:**
            *   Province/District Name (İl/İlçe Adı)
            *   Registered Voters
            *   Total Valid Votes
            *   Votes for each political party (and independent candidates, if significant)
        *   **Collection Method:** If used, data collection would involve a multi-step process: (1) potentially scraping the YSK website to find links to PDF files containing election results; (2) downloading those PDF files; (3) using the `PyPDF2` library to extract text from the PDFs; (4) employing complex regular expressions to parse the extracted text and identify the relevant data. This approach is significantly more challenging due to the inconsistent formatting of PDFs.
        * **Example URL Structure (Hypothetical):** `https://www.ysk.gov.tr/tr/secim-sonuclari/[year]/[election-type]/il/[province-code]` (This is a placeholder; the actual URL structure would need to be determined.)

*   **Data Source 2: Socioeconomic Indicators (Enrichment)**

    *   **Source:** Turkish Statistical Institute (TÜİK - Türkiye İstatistik Kurumu).
    *   **Data Granularity:** The data will be collected at the same geographic level (province or district) as the election results.
    *   **Data Elements:**
        *   GDP per capita (Kişi Başına GSYİH)
        *   Unemployment Rate (İşsizlik Oranı)
        *   Literacy Rate (Okuryazarlık Oranı) or other education indicators.
        *   Urbanization Rate (Kentleşme Oranı)
        *   Median Household Income (Ortalama Hanehalkı Geliri) - if available.
        *   Age distribution
        *   Other relevant demographic/economic indicators as available and relevant.
    *   **Collection Method:** Data from TÜİK will be collected primarily through their official website. If data is available in downloadable formats (CSV, Excel), these will be used. If data is only presented in tables on web pages, web scraping (using `requests` and `BeautifulSoup4` or potentially `pandas.read_html`) will be employed. All scraping will be done politely and in compliance with `robots.txt`.
    *   **Example URL Structure (Hypothetical):** `https://www.tuik.gov.tr/tr/istatistikler/[topic]/[year]` (This is illustrative; the actual URL structure will be used.)

## Planned Analysis

The project will proceed through the following stages:

1.  **Data Collection and Cleaning:**
    *   Scrape election results (preferably from `secim.sozcu.com.tr`, or from YSK if necessary) and socioeconomic data from TÜİK.
    *   Clean and preprocess the data:
        *   Handle missing values (if any) appropriately (e.g., through imputation or removal, with justification).
        *   Ensure consistent naming conventions for provinces/districts across datasets.
        *   Convert data types as needed (e.g., numeric values from strings).
        *   Create a merged dataset, joining election results and socioeconomic data based on province/district.

2.  **Exploratory Data Analysis (EDA):**
    *   **Descriptive Statistics:** Calculate summary statistics (mean, median, standard deviation, etc.) for all variables.
    *   **Data Visualization:**
        *   Create choropleth maps of Turkey to visualize:
            *   Vote shares for major political parties in each province/district.
            *   Key socioeconomic indicators (e.g., GDP per capita, unemployment rate).
        *   Generate scatter plots to explore relationships between socioeconomic indicators and vote shares.
        *   Create histograms and box plots to visualize the distributions of variables.

3.  **Hypothesis Testing:**

    *   **Hypothesis 1:** Provinces with higher GDP per capita tend to vote for Party X at a higher rate.
        *   **Test:** Pearson correlation coefficient between GDP per capita and vote share for Party X. A t-test could also be used to compare the mean GDP per capita of provinces where Party X exceeded a certain vote share threshold vs. those where it did not.

    *   **Hypothesis 2:** There is a significant difference in the voting patterns of urban vs. rural areas.
        *   **Test:** Classify provinces/districts as "urban" or "rural" based on the urbanization rate (using a defined threshold). Perform a t-test (or ANOVA if comparing more than two groups) to compare the mean vote shares for specific parties between urban and rural areas.

    *   **Hypothesis 3:** Unemployment rate and support for opposition parties are correlated.
        *   **Test:** Calculate the Pearson correlation coefficient between the unemployment rate and the combined vote share of opposition parties (defined prior to analysis).

4. **Reporting:**
    *   Summarize all findings of the EDA.
    *   Interpret and report the results of the hypothesis tests.
    *   Discuss the limitations of the analysis, including any challenges related to data collection and potential biases.

## Expected Challenges

*   **Data Accessibility:** The primary challenge is data accessibility. While `secim.sozcu.com.tr` appears promising, the lack of clear terms of service and a `robots.txt` file necessitates extreme caution during scraping.  The YSK website, while official, presents significant data extraction difficulties due to the PDF format.
*   **Website Structure Changes:** The structure of any website may change, requiring adjustments to the scraping code.
*   **Data Cleaning:** Data inconsistencies or errors in the source data may require significant cleaning and preprocessing effort.
*   **Turkish Language:** Dealing with Turkish characters and potentially needing to translate data labels or website content may present challenges.
*   **Interpreting Correlation:** It is crucial to avoid implying causation based on correlation. The analysis will focus on identifying associations, not proving causal relationships.
* **Ethical Web Scraping**: Scraping, especially in the absence of a clear robots.txt and easily findable ToS, must be done with care and respect. We will prioritize contacting Sözcü for permission if possible.

## Tech Stack

*   Python
*   Pandas (data manipulation and analysis)
*   NumPy (numerical operations)
*   Requests (making HTTP requests for web scraping)
*   BeautifulSoup4 (parsing HTML)
*   PyPDF2 (if YSK data is used, for parsing PDF files)
*   Matplotlib (data visualization)
*   Seaborn (data visualization)
*   SciPy (statistical testing)
*   Geopandas (Potentially, for creating choropleth maps)
