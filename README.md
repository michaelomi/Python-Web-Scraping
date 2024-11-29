# 🧑‍💻 Web Scraping: Largest Companies in the U.S. by Revenue

This Python script scrapes data from the Wikipedia page on the "List of largest companies in the United States by revenue". The data is extracted from the HTML table and saved into a CSV file for further analysis.

### 🛠 Requirements:
- Python 3.x
- BeautifulSoup (`bs4`)
- Requests
- Pandas

You can install the required libraries using pip:

```bash
pip install beautifulsoup4 requests pandas
```

### 📋 Script Overview:

- **Requesting the Page**:
  - The script sends an HTTP GET request to the Wikipedia URL using the `requests` library to retrieve the HTML content of the page.

- **Parsing the HTML**:
  - BeautifulSoup is used to parse the HTML content. It helps navigate and search through the page structure to locate the data.

- **Extracting Data**:
  - The script targets the first table on the page, which contains the list of the largest companies.
  - It finds the table headers (`th`) and extracts the column titles.
  - The script then extracts all rows (`tr`) of the table, and from each row, it grabs the cell data (`td`).

- **Creating a DataFrame**:
  - The column titles are used to create a Pandas DataFrame.
  - The row data is looped through, and each row is added to the DataFrame.

- **Saving Data to CSV**:
  - Finally, the data is saved to a CSV file at the specified path on your desktop (you can modify the path).

### 🔎 Code:
```python
from bs4 import BeautifulSoup
import requests
import pandas as pd

# Step 1: Send HTTP request to the URL
url = 'https://en.wikipedia.org/wiki/List_of_largest_companies_in_the_United_States_by_revenue'
page = requests.get(url)

# Step 2: Parse the HTML content using BeautifulSoup
soup = BeautifulSoup(page.text, 'html.parser')

# Step 3: Find the first table on the page
scrap_table = soup.find('table', class_='wikitable sortable')

# Step 4: Extract the column headers (titles)
world_titles = scrap_table.find_all('th')
world_table_titles = [title.text.strip() for title in world_titles]

# Step 5: Create a Pandas DataFrame with the column titles
dataframe = pd.DataFrame(columns=world_table_titles)

# Step 6: Extract the table rows and populate the DataFrame
column_data = scrap_table.find_all('tr')
for row in column_data[1:]:  # Skip the header row
    row_data = row.find_all('td')
    row_values = [data.text.strip() for data in row_data]
    dataframe = dataframe.append(pd.Series(row_values, index=dataframe.columns), ignore_index=True)

# Step 7: Save the DataFrame to a CSV file
dataframe.to_csv(r'C:/Users/PC/Desktop/Data Analytics/Pandas/web_scrap.csv', index=False)
```

### 🏢 Companies Data
Example of output table:
| Rank | Name               | Industry               | Revenue (USD millions) | Revenue Growth | Employees | Headquarters               |
|------|--------------------|------------------------|-------------------------|-----------------|-----------|----------------------------|
| 1    | Walmart            | Retail                 | 648,125                 | 6.0%            | 2,100,000 | Bentonville, Arkansas      |
| 2    | Amazon             | Retail and cloud computing | 574,785             | 11.9%           | 1,525,000 | Seattle, Washington        |
| 3    | Apple              | Electronics industry   | 383,482                 | -2.8%           | 161,000   | Cupertino, California      |
| 4    | UnitedHealth Group | Healthcare             | 371,622                 | 14.6%           | 440,000   | Minnetonka, Minnesota      |
| 5    | Berkshire Hathaway | Conglomerate            | 364,482                 | 20.7%           | 396,500   | Omaha, Nebraska            |


### 📝 What the Script Does:
- Scrapes data from the Wikipedia page that lists the largest companies in the U.S. by revenue.
- Extracts the table from the page that contains company names, revenues, and other details.
- Creates a structured DataFrame with column titles such as "Rank", "Company", "Revenue", etc.
- Saves the data to a CSV file, making it ready for further analysis.

### 📂 Output:
The output will be a CSV file containing the extracted data from the Wikipedia page, saved at the specified location (e.g., `C:/Users/PC/Desktop/Data Analytics/Pandas/web_scrap.csv`).

### ⚠️ Notes:
- The script assumes the structure of the page and table does not change. If the page's HTML structure changes, the script might need adjustments.
- Ensure the destination path for the CSV file exists on your system to avoid errors.
