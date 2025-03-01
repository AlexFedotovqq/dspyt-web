---
title: "How to python check proxy with aiohttp - DSPYT"
date: "May 2, 2022"
excerpt: "A proxy server is a remote server through which you connect to obfuscate your initial address. The proxy overlays your authentic IP address."
cover_image: "/images/posts/proxy/proxy-proxy-server-proxy-online-proxy-proxy-site-proxy-list-768x575.webp"
time_read: "5 min"
tags: ["proxy", "Python", "aiohttp", "proxy-server", "proxy-scraper"]
---

To conduct data analysis, for example during market research, we first need to determine the scope and collect the necessary data. Some websites and companies provide an easy and convenient way to access data via API. However, many limit the number of requests from one IP address. Therefore to scrape data anonymously and prevent your IP from being blocked by the web server, we recommend that you implement python scrapper for proxy servers.

## What is a proxy server?

A proxy server is a remote server through which you connect to obfuscate your initial address. Since the proxy hides and overlays your authentic IP address with its IP, the destination server can only see the IP of the proxy. Hence, if you rotate proxies with each request, the endpoint will recognize them as separate ones since they are coming from different IP addresses. Thus, you increase the speed and chance of obtaining data for research.

![python proxy server](/images/posts/proxy/Proxy-Server.webp)

## Where is my proxy?

In this article, we demonstrate how to obtain, verify and implement python scrapper for free proxies by using python libraries [requests](https://docs.python-requests.org/en/master/), selenium, BeautifulSoup and NumPy.

## Python check proxy with requests

Requests is a python http library that allows for the usage of proxies and multi-threading.

To implement the python script to work with proxies we first need to check those proxy servers are compatible with the target website.
Hence, we create a python scraper that collects the proxy server list from https://free-proxy-list.net/ and saves only those that work with our target.

```python
import requests
from bs4 import BeautifulSoup
import numpy
import concurrent.futures

# Get HTML response
html = requests.get('https://www.free-proxy-list.net/;)

# Parse HTML response
content = BeautifulSoup(html.text, 'lxml')

# Extract proxies table
table = content.find('table')

# Extract table rows
rows = table.findAll('tr')

# Create proxies result list
results = []

# Loop over table rows
for row in rows:
# Use only non-empty rows
    if len(row.findAll('td')):
    # Append rows containing proxies to results list
        results.append(row.findAll('td')[0].text +':' + row.findAll('td')[1].text)

# Create proxies final list
final =[]
def test(proxy):
# test each proxy on whether it access api of hh.ru
    headers = {'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:80.0) Gecko/20100101 Firefox/80.0'}
    try:
        params = {
        'text': f'NAME:C++',
        'area': 113,
        'page': 0,
        'per_page': 100
        }
        requests.get('https://api.hh.ru/vacancies;, headers=headers, proxies={'http' : proxy}, timeout=1, params=params)
        final.append(proxy)
    except:
        pass
    return proxy

# test multiple proxies concurrently
with concurrent.futures.ThreadPoolExecutor() as executor:
    executor.map(test, results)

# to print the number of proxies
print(len(final))

# save the working proxies to a file
numpy.save('file.npy', final)
```

This script yielded 295 proxy servers that work with our target website: HeadHunter.

## Python check proxy with selenium

We also create a script that additionally uses selenium library to scrape and check the proxy server list from https://advanced.name/.

```python
from selenium import webdriver
from bs4 import BeautifulSoup
import numpy
import concurrent.futures
import requests

dr = webdriver.Chrome(executable_path="C:\YourPath")
dr.get("https://advanced.name/freeproxy?ddexp4attempt=1&page=1")
content = BeautifulSoup(dr.page_source,"lxml")
table = content.find('tbody')
rows = table.findAll('tr')
results = []
for row in rows:
# Use only non-empty rows
    if len(row.findAll('td')):
        # Append rows containing proxies to results list
        results.append(row.findAll('td')[1].text +':' + row.findAll('td')[2].text)
        dr.close()

final =[]
def extract(proxy):
    #this was for when we took a list into the function, without conc futures.
    headers = {'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:80.0) Gecko/20100101 Firefox/80.0'}
    try:
        #change the url to https://httpbin.org/ip that doesnt block anything
        params = {
        'text': f'NAME:C++',
        'area': 113,
        'page': 0,
        'per_page': 100
        }
        requests.get('https://api.hh.ru/vacancies;, headers=headers, proxies={'http' : proxy}, timeout=1, params=params)
        final.append(proxy)
    except:
        pass
        return proxy

with concurrent.futures.ThreadPoolExecutor() as executor:
    executor.map(extract, results)

print(len(final))
numpy.save('file.npy', final)

# Parse HTML response
content = BeautifulSoup(html.text, 'lxml')

# Extract proxies table
table = content.find('table')

# Extract table rows
rows = table.findAll('tr')

# Create proxies result list
results = []

# Loop over table rows
for row in rows:
# Use only non-empty rows
    if len(row.findAll('td')):
        # Append rows containing proxies to results list
        results.append(row.findAll('td')[0].text +':' + row.findAll('td')[1].text)


# Create proxies final list
final =[]
def test(proxy):
    #test each proxy on whether it access api of hh.ru
    headers = {'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:80.0) Gecko/20100101 Firefox/80.0'}
    try:
        params = {
        'text': f'NAME:C++',
        'area': 113,
        'page': 0,
        'per_page': 100
        }
        requests.get('https://api.hh.ru/vacancies;, headers=headers, proxies={'http' : proxy}, timeout=1, params=params)
        final.append(proxy)
    except:
        pass
    return proxy

#test multiple proxies concurrently
with concurrent.futures.ThreadPoolExecutor() as executor:
executor.map(test, results)

# to print the number of proxies
print(len(final))

#save the working proxies to a file
numpy.save('file.npy', final)
```

In turn, it determined 99 items in the proxy server list.

## Proxies with asynchronous HTTP client/server library

We can also use the proxies in asynchronous HTTP client/server library [AIOHTTP](https://docs.aiohttp.org/en/stable/) for [asyncio](https://docs.aiohttp.org/en/stable/glossary.html#term-asyncio) and Python. Meanwhile [Aiohttp-proxy library](https://pypi.org/project/aiohttp-proxy/) assists with SOCKS proxy connector for [AIOHTTP](https://docs.aiohttp.org/en/stable/).

```python
import aiohttp
from aiohttp_proxy import ProxyConnector, ProxyType
import asyncio
import sys
import numpy

if sys.version_info[0] == 3 and sys.version_info[1] >= 8 and sys.platform.startswith('win'):
asyncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy())


async def fetch(url, proxy):
    host, port = proxy.split(':')[0], proxy.split(':')[1]
    connector = ProxyConnector(
    proxy_type=ProxyType.HTTP,
    host=host,
    port= int(port),
    )
    async with aiohttp.ClientSession(connector=connector,trust_env=True) as session:
    async with session.get(url) as response:
    return await response.text()


if name == "main":
    data = numpy.load('file.npy')
    loop = asyncio.get_event_loop()
    l = loop.run_until_complete(fetch('http://api.hh.ru/;, data[-1]))
    print(l)
    loop.run_until_complete(asyncio.sleep(0.1))
    loop.close()
```

## Summary

In this article, we demonstrated how to scrape and utilize free proxies. Nevertheless, free easy proxy servers will quickly get flagged since too many people use them.

For more reliable service we recommend paid residential proxy servers that [Bright Data](https://brightdata.grsm.io/ntzo6z21fa32) provides. Formerly known as Luminaty the third-party service has a well-written api documentation for python that you can use to manage your proxies.

![python proxy server api](/images/posts/proxy/image-5.webp)

## Related Posts

- [Blockchain Data Indexer with TrueBlocks](https://dspyt.com/blockchain-data-indexer-with-trueblocks)
- [Advanced Realized Volatility and Quarticity](https://dspyt.com/advanced-realized-volatility-and-quarticity)
- [Machine Learning with Simple Sklearn Ensemble](https://dspyt.com/machine-learning-simple-sklearn-ensemble)
- [Parsiq Triggers for Axie Infinity](https://dspyt.com/blockchain-insights-with-parsiq-triggers-for-axie-infinity)
- [How to illustrate log returns vs simple returns](https://dspyt.com/simple-returns-log-return-and-volatility-simple-introduction)
- [A How to EfficientNet Classification](https://dspyt.com/efficientnet-classification)
