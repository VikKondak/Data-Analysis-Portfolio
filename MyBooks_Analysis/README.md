# 📚 Goodreads Book Analysis

This is a simple project exploring my Goodreads library where I've saved information about the books I've read since 2020 to present day.
This project analyzes my personal book reading data, including the dates I've finished the books, my ratings, the site's average ratings, and pages read over time. The goal is to explore my reading habits, compare my ratings, and quantify reading volume. This is a way for me to visualize any trends in my reading and possibly help me in choosing my future reads.

## Project Overview
This project examines a dataset of books I’ve read, focusing on:
- Trends in my personal ratings on a monthly period.
- Comparison of my average ratings to Goodreads' average ratings collected from every other readers.
- Total pages read per year.
- Average publishing time of the books I've read each year.

The analysis is implemented in multiple Python scripts that I will break down below.

First, prepping my dataset.

## Dataset
- Source: Goodreads export `.csv` (file name "goodreads_library_export.csv")
- Size: 516 rows, 24 columns
- Example snippet:

| Book Id  | Title                    | Author              | Author l-f           | Additional Authors | ISBN       | ISBN13        | My Rating | Average Rating |
|----------|--------------------------|---------------------|----------------------|--------------------|------------|---------------|-----------|----------------|
| 17899948 | Rebecca                  | Daphne du Maurier   | Maurier, Daphne du   |                    | 0316323705 | 9780316323703 | 5         | 4.25           |
| 34606688 | Tales of Norse Mythology | Hoala ne A. Guerber | Guerber, Hoala¨ne A. |                    | 1435164989 | 9781435164987 | 0         | 3.91           |

## Data Cleaning & Transformation
Brief explanation of what was cleaned:
- Removed unnecessary columns
- Renamed columns (`'Date Read'` → `date_read`)
- Converted strings to datetime format
- Removed empty rows

### Code
<details>
<summary><strong>📄 View cleaning script</strong></summary>

```python
import pandas as pd

# Load data
df = pd.read_csv("data/goodreads_library_export.csv")

# Show initial shape and columns
print("Initial shape:", df.shape)
print("Initial columns:", df.columns)

# Delete unnecessary columns
dropped_cols = [
    'Book Id', 'ISBN', 'ISBN13', 'Publisher', 'Binding',
    'Author l-f', 'My Thoughts', 'Private Notes', 'Spoiler', 
    'Recommended For', 'Recommended By', 'Owned Copies', 
    'Original Publication Year', 'Bookshelves with positions',
    'Cover Image Url'
]
df = df.drop(columns=[col for col in dropped_cols if col in df.columns])

# Clean the column names
df.columns = df.columns.str.strip().str.lower().str.replace(' ', '_')

# Convert date columns to datetime
df['date_read'] = pd.to_datetime(df['date_read'], errors='coerce')
df['date_added'] = pd.to_datetime(df["date_added"], errors='coerce')

# Fill in missing values
df['number_of_pages'] = df['number_of_pages'].fillna(0).astype(int)
df['my_rating'] = df['my_rating'].fillna(0)
df['average_rating'] = df['average_rating'].fillna(0)

# Filter to only "read" books
df = df[df['exclusive_shelf'] == 'read']

# Save cleaned version
df.to_csv('output/cleaned_books.csv', index=False)

print("Cleaned data saved to output/cleaned_books.csv")
print("Final shape:", df.shape)
</details> ```
