# Section 5: Working with APIs and Web Scraping (Theory)

## Overview
APIs and web scraping let you collect data from the web. Understanding protocols, tools, and ethics is essential for data engineers.

---

## 1. What are APIs? REST Basics
- API: Application Programming Interface
- REST: Standard for web APIs (stateless, resource-oriented)

## 2. HTTP Methods & Status Codes
- **GET:** Retrieve
- **POST:** Create
- **PUT/PATCH:** Update
- **DELETE:** Remove
- Status codes: 200 (OK), 404 (Not Found), 401/403 (Unauthorized), 500 (Error)

## 3. Authentication
- **API keys:** Passed as headers or params
- **OAuth:** Token-based, for user data (Twitter, Google)

## 4. Using requests for API Calls
```python
import requests
resp = requests.get('https://api.example.com/data', headers={'Authorization': 'Bearer TOKEN'})
print(resp.json())
```

## 5. JSON/XML Parsing
- JSON: `resp.json()`, `json.loads()`
- XML: `xml.etree.ElementTree`, `BeautifulSoup('xml')`

## 6. Web Scraping
- Use when no API is available
- Always check robots.txt and terms of service
- Legal/ethical: Don't overload servers, respect privacy

## 7. BeautifulSoup Basics
```python
from bs4 import BeautifulSoup
soup = BeautifulSoup('<html>...</html>', 'html.parser')
titles = soup.find_all('h1')
```

## 8. Scrapy and Selenium
- **Scrapy:** For large-scale, robust scraping
- **Selenium:** For JavaScript-heavy or interactive sites

## 9. Handling Pagination, Rate Limits, CAPTCHAs
- Use loops for paginated APIs
- Respect rate limits (`time.sleep()`)
- CAPTCHAs may block bots (avoid scraping such sites)

## 10. Best Practices
- Log requests and errors
- Use user-agent headers
- Store raw and parsed data

## References
- [Requests Docs](https://docs.python-requests.org/)
- [BeautifulSoup Docs](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
- [Scrapy Docs](https://docs.scrapy.org/)
