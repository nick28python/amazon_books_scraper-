import requests
from bs4 import BeautifulSoup

url = "https://www.amazon.in/s?k=best+selling+books&crid=3KJFS0WSXKQZE&sprefix=%2Caps%2C253&ref=nb_sb_ss_recent_1_0_recent"

headers = {
    "User-Agent": "Mozilla/5.0"
}

response = requests.get(url, headers=headers)

if response.status_code == 200:
    print("HTML fetched successfully!")
else:
    print("Failed to fetch HTML!")

soup = BeautifulSoup(response.text, "html.parser")

books = soup.find_all("span", class_='a-size-medium a-color-base a-text-normal')
prices = soup.find_all("span", class_='a-price-whole')
ratings = soup.find_all("span", class_='a-icon-alt')

for i in range(min(10, len(books))):  # safely loop through only available books
    book_name = books[i].text.strip()
    price = prices[i].text.strip() if i < len(prices) else "Price not found"
    rating = ratings[i].text.strip() if i < len(ratings) else "No rating"

    print(f"Name: {book_name} | Price: ₹{price} | Rating: {rating}")
