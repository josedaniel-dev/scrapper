import csv
import json
import os
import time
import requests
from bs4 import BeautifulSoup

BASE_URL = "https://prima-coffee.com"
CATEGORY_URL = f"{BASE_URL}/brew/coffee"
HEADERS = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"}

# ========== Step 1: SCRAPE ALL PRODUCTS (with pagination) ==========
all_products = []
page = 1
print("[*] Loading category pages and scraping products...")

while True:
    url = f"{CATEGORY_URL}?page={page}"
    print(f"    - Scraping page {page}: {url}")
    r = requests.get(url, headers=HEADERS)
    if r.status_code != 200:
        print(f"[!] Stopping, got status {r.status_code}")
        break

    soup = BeautifulSoup(r.text, "html.parser")
    cards = soup.select("article.card")

    if not cards:
        print(f"[✔] No more products found after page {page-1}.")
        break

    for card in cards:
        link_tag = card.select_one("a[href]")
        if not link_tag:
            continue
        href = link_tag.get("href").strip()
        if href.startswith("http"):
            product_url = href
        else:
            product_url = BASE_URL + href

        title_tag = card.select_one("h4.card-title")
        price_tag = card.select_one(".price--main, .price--withoutTax")
        img_tag = card.select_one("img.card-image")

        title = title_tag.get_text(strip=True) if title_tag else ""
        price_text = price_tag.get_text(strip=True) if price_tag else ""
        image = img_tag.get("src") if img_tag else ""

        all_products.append({
            "url": product_url,
            "title": title,
            "price_text": price_text,
            "image": image
        })

    page += 1
    time.sleep(0.5)  # be polite

print(f"[✔] Found {len(all_products)} products.")

# ========== Step 2: SAVE products_basic.csv ==========
basic_file = "products_basic.csv"
try:
    with open(basic_file, "w", newline="", encoding="utf-8") as f:
        writer = csv.DictWriter(f, fieldnames=["url", "title", "price_text", "image"])
        writer.writeheader()
        writer.writerows(all_products)
    print(f"[✔] Saved {basic_file} with {len(all_products)} products.")
except PermissionError:
    print(f"[!] Permission denied: {basic_file} is open. Close it and rerun.")
    exit()

# ========== Step 3: SORT BY PRICE and SAVE products_sorted.csv ==========
def price_to_float(price_text):
    try:
        return float(price_text.replace("$", "").replace(",", "").strip())
    except:
        return float("inf")

products_with_price = [p for p in all_products if p["price_text"].strip()]
products_sorted = sorted(products_with_price, key=lambda x: price_to_float(x["price_text"]))

sorted_file = "products_sorted.csv"
with open(sorted_file, "w", newline="", encoding="utf-8") as f:
    writer = csv.DictWriter(f, fieldnames=["url", "title", "price_text", "image"])
    writer.writeheader()
    writer.writerows(products_sorted)
print(f"[✔] Saved {sorted_file} with {len(products_sorted)} products.")

# ========== Step 4: SCRAPE DETAILS FOR FIRST 5 CHEAPEST ==========
detailed_products = []
print("[*] Scraping details of the 5 cheapest products...")
for p in products_sorted[:5]:
    print(f"    - Scraping {p['title']} ...")
    try:
        r = requests.get(p["url"], headers=HEADERS)
        r.raise_for_status()
        soup = BeautifulSoup(r.text, "html.parser")

        # Title
        title = soup.select_one("h1.productView-title")
        title = title.get_text(strip=True) if title else p["title"]

        # Images
        images = []
        for img in soup.select("img[data-zoom-image]"):
            src = img.get("data-zoom-image") or img.get("src")
            if src and src.startswith("http"):
                images.append(src)

        # SKU / UPC (if available)
        sku = ""
        upc = ""
        sku_tag = soup.find("dt", string=lambda s: s and "SKU" in s)
        if sku_tag and sku_tag.find_next_sibling("dd"):
            sku = sku_tag.find_next_sibling("dd").get_text(strip=True)

        upc_tag = soup.find("dt", string=lambda s: s and "UPC" in s)
        if upc_tag and upc_tag.find_next_sibling("dd"):
            upc = upc_tag.find_next_sibling("dd").get_text(strip=True)

        detailed_products.append({
            "url": p["url"],
            "title": title,
            "images": images,
            "sku": sku,
            "upc": upc,
            "price_text": p["price_text"]
        })

    except Exception as e:
        print(f"[!] Error scraping {p['url']}: {e}")

# Save to JSON
with open("products_detailed.json", "w", encoding="utf-8") as f:
    json.dump(detailed_products, f, indent=2, ensure_ascii=False)
print(f"[✔] Saved products_detailed.json with {len(detailed_products)} detailed products.")

print("\nAll done! 🎉")
