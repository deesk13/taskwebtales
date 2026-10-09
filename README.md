## Selenium Web Tables Automation
## Aim

To automate a web table using Python and Selenium WebDriver to retrieve column headings, extract the first and last data rows, search for an employee by name, retrieve email addresses, identify a Due column, inspect website links, and count data rows.

## Technologies Used
Python
Selenium WebDriver
XPath
Google Chrome
## URl
https://assertqa.com/practice/webtables

## Algorithm
Import the required Selenium modules.
Initialize Chrome WebDriver and WebDriverWait.
Open the web tables practice webpage.
Wait until the table is loaded.
Locate the table and print all column headings.
Retrieve and print the first data row.
Retrieve and print the last data row on the current page.
Enter Smith into the search field and display matching employees.
Clear the search field and extract email addresses.
Check whether a Due column exists and calculate the highest amount if numeric data is available.
Locate employee website links and print their URLs.
Count the data rows on the current page.
Close the browser.
Test Cases
Test Case	Description
TC01	Print column headings
TC02	Print first data row
TC03	Print last data row
TC04	Search employee by last name
TC05	Extract email addresses
TC06	Find highest Due amount if available
TC07	Retrieve employee website links
TC08	Count data rows
## Expected Result

The browser should open the practice webpage and execute the table operations. Available table data should be printed in the terminal. If a Due column or website links are absent, the corresponding test should report that the feature is unavailable.
## code:
```
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

driver = webdriver.Chrome()
wait = WebDriverWait(driver, 10)

try:
    driver.get("https://assertqa.com/practice/webtables")
    wait.until(EC.presence_of_element_located((By.TAG_NAME, "table")))

    table = driver.find_element(By.TAG_NAME, "table")

    # TC01 - Print all column headings
    print("\nTC01 - Column Headings")
    headers = table.find_elements(By.XPATH, ".//th")
    for header in headers:
        print(header.text)

    # TC02 - Print first data row
    print("\nTC02 - First Data Row")
    rows = table.find_elements(By.XPATH, ".//tbody/tr")
    if rows:
        print(rows[0].text)

    # TC03 - Print last data row on current page
    print("\nTC03 - Last Data Row on Current Page")
    if rows:
        print(rows[-1].text)

    # TC04 - Search employee by last name
    print("\nTC04 - Search by Last Name")
    search = driver.find_element(
        By.XPATH, "//input[@placeholder='Search by name, email...']"
    )
    search.send_keys("Smith")

    wait.until(
        EC.visibility_of_element_located((By.XPATH, "//tbody/tr"))
    )
    rows = driver.find_elements(By.XPATH, "//tbody/tr")
    matches = [row.text for row in rows if "Smith" in row.text]

    if matches:
        for row in matches:
            print(row)
        print("PASS")
    else:
        print("FAIL - No matching employee")

    # Clear search before the next test
    search.clear()

    # TC05 - Extract email addresses on current page
    print("\nTC05 - Email Addresses")
    rows = driver.find_elements(By.XPATH, "//tbody/tr")
    for row in rows:
        cells = row.find_elements(By.TAG_NAME, "td")
        if len(cells) >= 3:
            print(cells[2].text)

    # TC06 - Due amount
    print("\nTC06 - Highest Due Amount")
    headings = [
        h.text.strip().lower()
        for h in driver.find_elements(By.XPATH, "//th")
    ]
    if "due" in headings:
        print("Due column found; implement maximum calculation.")
    else:
        print("SKIP - This page has no Due column.")

    # TC07 - Employee website link
    print("\nTC07 - Website Link")
    links = driver.find_elements(By.XPATH, "//tbody//a")
    if links:
        print("PASS - Website link exists")
        for link in links:
            print(link.get_attribute("href"))
    else:
        print("SKIP - No employee website links found.")

    # TC08 - Count data rows, excluding header
    print("\nTC08 - Data Row Count")
    rows = driver.find_elements(By.XPATH, "//tbody/tr")
    print("Rows on current page:", len(rows))

finally:
    driver.quit()
```
## output:
<img width="926" height="801" alt="image" src="https://github.com/user-attachments/assets/05b0276f-4f1e-4e20-8757-ec13459b81bf" />


