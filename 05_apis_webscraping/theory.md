# Section 5: Working with APIs and Web Scraping

## Overview
APIs and web scraping are crucial skills for collecting, integrating, and automating data extraction from external sources. Data engineers must know protocols (HTTP/HTTPS), common tools, and the legal/ethical considerations surrounding data acquisition.

---

## 1. What are APIs? REST Basics

- **API (Application Programming Interface):** A set of rules that allows software to communicate.
- **REST (Representational State Transfer):** The most common web API architecture; uses HTTP methods to access resources via URLs.
- **Key REST principles:**
    - Stateless operations: Each request contains all necessary context.
    - Resource-based: Interact with resources (data objects) via endpoints.
    - Standard HTTP methods: GET, POST, PUT, PATCH, DELETE.

---

## 2. HTTP Methods & Status Codes

- **Methods:**
    - `GET`: Retrieve data (safe, idempotent)
    - `POST`: Create new resource (not idempotent)
    - `PUT`: Replace resource (idempotent)
    - `PATCH`: Partially update resource
    - `DELETE`: Remove resource
- **Common HTTP Status Codes:**
    - 200 OK: Successful request
    - 201 Created: Resource created
    - 204 No Content: Successful, no response body
    - 400 Bad Request: Invalid input
    - 401 Unauthorized / 403 Forbidden: Authorization issues
    - 404 Not Found: Resource missing
    - 429 Too Many Requests: Rate limit exceeded
    - 500 Internal Server Error

---

## 3. Authentication

- **API Keys:** Simple token passed via headers (`Authorization`) or query params.
- **OAuth2:** Delegated access via access tokens; used for user data (Google, Twitter).
- **Bearer Token Example:**
    ```python
    headers = {'Authorization': 'Bearer YOUR_API_KEY'}
    ```
- **Other methods:** Basic Auth, JWT, custom tokens.

---

## 4. Making API Calls with requests

- Install: `pip install requests`
- **Basic GET Example:**
    ```python
    import requests

    url = 'https://jsonplaceholder.typicode.com/posts'
    resp = requests.get(url)
    print(resp.status_code)
    print(resp.json())  # Parses JSON response
    ```
- **Passing Headers and Params:**
    ```python
    params = {'userId': 1}
    headers = {'Authorization': 'Bearer TOKEN'}

    resp = requests.get(url, headers=headers, params=params)
    ```

- **Error Handling:**
    ```python
    if resp.status_code == 200:
        data = resp.json()
    else:
        print(f'Error: {resp.status_code}')
    ```

---

## 5. JSON and XML Parsing

- **JSON (JavaScript Object Notation):**
    - Native to most APIs
    - Parse with `resp.json()` or `json.loads(resp.text)`

    ```python
    import json
    data = resp.json()  # requests shortcut
    # or
    data = json.loads(resp.text)
    ```

- **XML:**
    - Use `xml.etree.ElementTree` or BeautifulSoup with `'xml'` parser.
    - Example:
    ```python
    import xml.etree.ElementTree as ET

    root = ET.fromstring(xml_string)
    for child in root:
        print(child.tag, child.text)
    ```

---

## 6. Web Scraping

- Use scraping when APIs do not exist.
- **Steps:**
    1. Check `robots.txt` (e.g., `https://example.com/robots.txt`) for allowed paths.
    2. Download HTML using `requests`.
    3. Parse HTML with BeautifulSoup or lxml.
- **Ethical/Legal:**
    - Respect robots.txt and site terms.
    - Limit request rate (`time.sleep()` between calls).
    - Identify your bot with a custom User-Agent.
    - Do not collect sensitive/private data.

---

## 7. BeautifulSoup Basics

- **Install:** `pip install beautifulsoup4`
- **HTML Parsing Example:**
    ```python
    from bs4 import BeautifulSoup

    html = '<html><body><h1>Title</h1><p>Text</p></body></html>'
    soup = BeautifulSoup(html, 'html.parser')
    print(soup.h1.text)         # 'Title'
    print(soup.find('p').text)  # 'Text'
    ```
- **Finding Multiple Elements:**
    ```python
    links = soup.find_all('a')
    for link in links:
        print(link['href'], link.text)
    ```

---

## 8. Scrapy and Selenium

- **Scrapy:** Python framework for scalable, asynchronous scraping and crawling.
    - Use for large sites, crawl multiple pages, export to CSV/JSON.
    - Built-in support for pipelines, middlewares, auto-throttle.
- **Selenium:** Automates browsers; simulates user actions (for JavaScript-heavy sites).
    - Use for sites that require login, clicking, or JS rendering.
    - Slower than requests/BS4; heavier on resources.

---

## 9. Handling Pagination, Rate Limits, CAPTCHAs

- **Pagination:** Loop through paged API endpoints or next-page links.
    ```python
    results = []
    page = 1
    while True:
        resp = requests.get(url, params={'page': page})
        data = resp.json()
        if not data:
            break
        results.extend(data)
        page += 1
    ```
- **Rate Limits:** Check API docs for limits; use `time.sleep()` to pause.
- **CAPTCHAs:** Anti-bot measures; often impossible to bypass ethically—avoid scraping such pages.

---

## 10. Best Practices

- **Respectful Scraping:** Identify your bot, space requests, and cache results when possible.
- **Error Handling:** Use `try`/`except`, log failed requests, handle timeouts/retries.
- **Data Storage:** Store both raw HTML/JSON and parsed data for reproducibility.
- **Documentation:** Keep track of data sources, API versions, and scraping logic.
- **Testing:** Write scripts that can be rerun reliably; add unit tests for parsing code.

---

## References & Further Reading

- [Requests: HTTP for Humans](https://docs.python-requests.org/en/latest/)
- [BeautifulSoup4 Documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
- [Scrapy Documentation](https://docs.scrapy.org/en/latest/)
- [Selenium Docs](https://www.selenium.dev/documentation/)
- [robots.txt Info](https://www.robotstxt.org/)

---
