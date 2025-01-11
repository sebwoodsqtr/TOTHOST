# Asynchronous Web Scraping with Python, aiohttp, and asyncio

Efficiently scraping web data at scale often requires asynchronous programming techniques to maximize concurrency and minimize time spent waiting on requests. In this guide, we’ll walk through how to perform asynchronous web scraping in Python using `aiohttp` and `asyncio`.

## Why Async is Better Than Multithreading

When scaling up web scraping, multithreading can become complex due to the challenges of writing thread-safe code. Asynchronous programming avoids these pitfalls by allowing you to handle multiple tasks concurrently without the need for multiple threads.

Below, we’ll explore a simple example to fetch multiple URLs simultaneously using `aiohttp`.

---

## Example: Fetching Multiple URLs with aiohttp

### Step 1: Defining the URLs to Scrape
Here’s an example array of URLs we’ll scrape simultaneously:

```python
urls = [
    'https://nytimes.com',
    'https://github.com',
    'https://google.com',
    'https://reddit.com',
    'https://producthunt.com'
]
```

### Step 2: Importing Required Modules
We begin by importing the necessary Python libraries:

```python
import asyncio
from timeit import default_timer
from aiohttp import ClientSession
import requests
```

### Step 3: Writing the Fetch Function
The `fetch` function handles individual URL fetches and measures how long each request takes:

```python
async def fetch(url, session):
    fetch.start_time[url] = default_timer()
    async with session.get(url) as response:
        r = await response.read()
        elapsed = default_timer() - fetch.start_time[url]
        print(url + ' took ' + str(elapsed) + ' seconds')
        return r
```

### Step 4: Fetching All URLs
The `fetch_all` function schedules and manages multiple asynchronous tasks for fetching URLs:

```python
async def fetch_all(urls):
    tasks = []
    fetch.start_time = dict()
    async with ClientSession() as session:
        for url in urls:
            task = asyncio.ensure_future(fetch(url, session))
            tasks.append(task)
        _ = await asyncio.gather(*tasks)
```

### Step 5: Setting Up the Event Loop
The `fetch_async` function sets up the event loop and executes all fetch tasks:

```python
def fetch_async(urls):
    start_time = default_timer()

    loop = asyncio.get_event_loop()
    future = asyncio.ensure_future(fetch_all(urls))
    loop.run_until_complete(future)

    tot_elapsed = default_timer() - start_time
    print('Total time taken: ' + str(tot_elapsed) + ' seconds')
```

### Step 6: Running the Script
Here’s the complete code, ready to execute:

```python
import asyncio
from timeit import default_timer
from aiohttp import ClientSession
import requests


def fetch_async(urls):
    start_time = default_timer()

    loop = asyncio.get_event_loop()
    future = asyncio.ensure_future(fetch_all(urls))
    loop.run_until_complete(future)

    tot_elapsed = default_timer() - start_time
    print('Total time taken: ' + str(tot_elapsed) + ' seconds')


async def fetch_all(urls):
    tasks = []
    fetch.start_time = dict()
    async with ClientSession() as session:
        for url in urls:
            task = asyncio.ensure_future(fetch(url, session))
            tasks.append(task)
        _ = await asyncio.gather(*tasks)


async def fetch(url, session):
    fetch.start_time[url] = default_timer()
    async with session.get(url) as response:
        r = await response.read()
        elapsed = default_timer() - fetch.start_time[url]
        print(url + ' took ' + str(elapsed) + ' seconds')
        return r


if __name__ == '__main__':
    urls = [
        'https://nytimes.com',
        'https://github.com',
        'https://google.com',
        'https://reddit.com',
        'https://producthunt.com'
    ]
    fetch_async(urls)
```

Run the script with:

```bash
python3 async.py
```

---

## Scaling Up for Production

While this example works well for a small set of URLs, scraping at scale requires a solution to handle IP blocking. Many websites block requests based on usage patterns, locations, or automated detection algorithms. To overcome this, using a **rotating proxy service** is essential.

---

## Simplify Scaling with ScraperAPI

Stop wasting time on proxies and CAPTCHAs! **[ScraperAPI](https://bit.ly/Scraperapi)** is a powerful solution for web scraping at scale. It automatically rotates IPs, handles CAPTCHAs, and simulates real-user behavior, ensuring your scraping tasks run smoothly.

### Key Features:
- Millions of rotating proxies worldwide.
- Automatic IP and User-Agent rotation.
- Built-in CAPTCHA solving.
- Supports scraping with tools like Python, Node.js, Puppeteer, and Scrapy.

**Try it free today!**  
Use the code **SCRAPE9837861** to get started with **1,000 free API calls**.  
👉 [Start your free trial now](https://bit.ly/Scraperapi)

---

## Conclusion

By leveraging asynchronous programming with `aiohttp` and `asyncio`, you can scrape multiple URLs concurrently, saving significant time. For scaling your scraping tasks and avoiding IP bans, services like **[ScraperAPI](https://bit.ly/Scraperapi)** provide a hassle-free and reliable solution. Happy scraping!
