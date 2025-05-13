import csv
import os

FILENAME = "books.csv"

# Load books from CSV file

def load\_books():
books = \[]
try:
with open(FILENAME, mode='r', newline='', encoding='utf-8') as file:
reader = csv.DictReader(file)
for row in reader:
row\['rating'] = float(row\['rating'])  # Convert rating to float
books.append(row)
except FileNotFoundError:
print("No books file found. Starting with an empty list.")
return books

# Save books to CSV file

def save\_books(books):
with open(FILENAME, mode='w', newline='', encoding='utf-8') as file:
fieldnames = \['title', 'author', 'genre', 'rating']
writer = csv.DictWriter(file, fieldnames=fieldnames)
writer.writeheader()
for book in books:
writer.writerow(book)

# Display all books

def view\_books(books):
if not books:
print("No books to display.")
return
for book in books:
print(f"{book\['title']} by {book\['author']} - Genre: {book\['genre']} | Rating: {book\['rating']}")

# Search books

def search\_books(books, key, value):
results = \[book for book in books if value.lower() in book\[key].lower()]
return results

# Add new book

def add\_book(books):
title = input("Enter book title: ").strip()
author = input("Enter author name: ").strip()
genre = input("Enter genre: ").strip()

```
if not title or not author or not genre:
    print("Title, author, and genre cannot be empty.")
    return

try:
    rating = float(input("Enter rating (1.0 - 5.0): "))
    if rating < 1 or rating > 5:
        raise ValueError
except ValueError:
    print("Invalid rating. It must be a number between 1 and 5.")
    return

new_book = {
    'title': title,
    'author': author,
    'genre': genre,
    'rating': rating
}

books.append(new_book)
save_books(books)
print("Book added successfully!")
```

# Get recommendations

def recommend\_books(books):
genre = input("Enter preferred genre: ").strip()
try:
min\_rating = float(input("Enter minimum rating (1.0 - 5.0): "))
except ValueError:
print("Invalid rating input.")
return

```
filtered = [book for book in books if genre.lower() in book['genre'].lower() and book['rating'] >= min_rating]
sorted_books = sorted(filtered, key=lambda x: x['rating'], reverse=True)

if not sorted_books:
    print("No recommendations found.")
else:
    print("\nRecommended books:")
    view_books(sorted_books)
```

# Main menu

def main():
books = load\_books()

```
while True:
    print("\n--- Book Recommendation System ---")
    print("1. View all books")
    print("2. Search book by title")
    print("3. Search book by author")
    print("4. Search book by genre")
    print("5. Add a new book")
    print("6. Get book recommendations")
    print("7. Exit")

    choice = input("Enter your choice (1-7): ")

    if choice == '1':
        view_books(books)
    elif choice == '2':
        title = input("Enter title to search: ")
        results = search_books(books, 'title', title)
        view_books(results)
    elif choice == '3':
        author = input("Enter author to search: ")
        results = search_books(books, 'author', author)
        view_books(results)
    elif choice == '4':
        genre = input("Enter genre to search: ")
        results = search_books(books, 'genre', genre)
        view_books(results)
    elif choice == '5':
        add_book(books)
    elif choice == '6':
        recommend_books(books)
    elif choice == '7':
        print("Thank you for using the Book Recommendation System.")
        break
    else:
        print("Invalid choice. Please enter a number between 1 and 7.")
```

if **name** == "**main**":
main()
