# Shopify Exchange Data

The Shopify Exchange is a marketplace for buying and selling e-commerce businesses built on Shopify’s platform.

Browsing inventory on the Exchange is a pretty bad experience, but luckily, Shopify exposes an [endpoint](https://exchangemarketplace.com/shops.json?page=1) that has richer listing data. 

I collected some data from the exchange on 2/20/2020 and 4/10/2020, wrote about it here, and shared the CSVs in this repo. 

If you're interested, I'll leave a little script you could use to collect more data at the bottom of this readme.

It would be cool to periodically scrape the exchange to generate a Shopify Exchange Index with aggregate statistics that could be tracked over time.

Relevant: [OpenStore](https://open.store/)

Happy to answer any questions: first dot last on gmail.

```
import requests
import json
import time
import random

BASE_URL = 'https://exchangemarketplace.com/shops.json'
current_page = 1
TOTAL_PAGES = 401

all_shops = []

# note: periodically, the site will put up Google's "check this box if you're a human"
# and this scraping will begin failing / retrying. to fix, manually open up the current_url 
# and check the box. so you should supervise the scraping.

while current_page <= TOTAL_PAGES:
    time.sleep(random.randint(1, 3))
    
    current_url = BASE_URL + f'?page={current_page}'
    for attempt in range(5):
        try:
            time.sleep(random.randint(1, 3))
            response = requests.get(current_url)
            shops = response.json()['shops']
        except:
            print(f'failed attempt {attempt} on page {current_page}!')
            continue
        else:
            print(f'found {len(shops)} shops on page {current_page} and url {current_url}')
            all_shops = all_shops + shops
            break
    current_page = current_page + 1
```
