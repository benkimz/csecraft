# csecraft

The `csecraft` Python library allows you to easily interact with the <b>Google Custom Search API</b>. This library provides methods for executing search queries, retrieving search results, paginating through search results, and accessing search metadata. Additionally, the library includes utility methods for displaying and analyzing search results, making it an ideal tool for applications that require embedded search functionality.

## Features

- **Search the web**: Perform Google Custom Search queries.
- **Paginate**: Navigate through multiple pages of search results.
- **Retrieve Results & Metadata**: Access detailed search information, metadata, and page maps.
- **Get Snippets**: Retrieve plain and HTML snippets for each result.
- **Spelling Suggestions**: Receive corrected search queries when available.

## Installation

To install this library, follow either of the following methods:

```bash
pip install csecraft
```

Or

```bash
git clone https://github.com/benkimz/csecraft.git
cd csecraft
pip install .
```

## Prerequisites

To use this library, you'll need:

- A Google Cloud Platform (GCP) project with the **Custom Search API** enabled.
- A Custom Search Engine ID (CX) and an API Key.

Visit the [Google Custom Search documentation](https://developers.google.com/custom-search) for setup instructions.

## Usage

### Importing and Initializing the CustomSearchEngine

```python
from cse-api import CustomSearchEngine

# Initialize the search engine with your API key and engine ID
api_key = "YOUR_API_KEY"
engine_id = "YOUR_ENGINE_ID"
search_engine = CustomSearchEngine(api_key, engine_id)
```

### Performing a Search

```python
query = "Python programming"
response = search_engine.search(query)

if response:
    print("Search Results:")
    search_engine.display_results()
else:
    print("No results found.")
```

### Accessing Search Metadata

```python
# Get total number of results
total_results = search_engine.get_total_results()
print(f"Total results: {total_results}")

# Get search time
search_time = search_engine.get_search_time()
print(f"Search time: {search_time} seconds")
```

### Retrieving Results Information

```python
# Get all items (results) from the search
items = search_engine.get_items()
for i, item in enumerate(items, start=1):
    print(f"{i}. Title: {item['title']}")
    print(f"   URL: {item['link']}")
    print(f"   Snippet: {item['snippet']}")
    print(f"   Display Link: {item['displayLink']}\n")

# Get promotions (if any)
promotions = search_engine.get_promotions()
for promo in promotions:
    print(f"Promotion: {promo['title']}, Link: {promo['link']}")
```

### Fetching Corrected Query and Suggested Next Page

```python
# Get spelling-corrected query, if available
corrected_query = search_engine.get_corrected_query()
if corrected_query:
    print(f"Did you mean: {corrected_query}?")

# Fetch search terms for the next page, if available
next_page_query = search_engine.get_next_page_query()
if next_page_query:
    print(f"Suggested terms for next page: {next_page_query}")
```

### Pagination: Fetching Specific Pages

```python
# Fetch results for a specific page
page_number = 2
page_response = search_engine.fetch_page(page_number)
if page_response:
    print(f"Results for page {page_number}:")
    search_engine.display_results()
```

### Getting Plain and HTML Snippets

```python
# Retrieve plain and HTML snippets for each result
snippets = search_engine.get_snippets()
for snippet, html_snippet in snippets:
    print("Plain Snippet:", snippet)
    print("HTML Snippet:", html_snippet)
    print()
```

## API Reference

### `CustomSearchEngine`

#### `__init__(api_key: str, engine_id: str, base_url: str = "https://customsearch.googleapis.com/customsearch/v1")`

Initialize the CustomSearchEngine with API key, engine ID, and an optional base URL.

#### `search(query: str, **kwargs) -> Optional[CustomSearchResponse]`

Perform a search with the given query and optional parameters.

- **query** (`str`): The search term or phrase.
- **kwargs**: Additional parameters, such as `num`, `start`, `language`, etc.
- **Returns**: A `CustomSearchResponse` object with the search results.

#### `get_total_results() -> int`

Returns the total number of results found in the last search response.

#### `get_search_time() -> float`

Returns the time taken for the last query in seconds.

#### `get_items() -> List[Dict[str, Any]]`

Retrieves a list of dictionaries with title, link, snippet, and display link for each search result.

#### `get_promotions() -> List[Dict[str, Any]]`

Retrieves a list of promotions from the last response, if available.

#### `get_corrected_query() -> Optional[str]`

Returns a spelling-corrected query if the API provides a suggestion.

#### `get_next_page_query() -> Optional[str]`

Retrieves the search terms for the next page of results if available.

#### `get_metadata() -> Dict[str, Any]`

Returns metadata from the last search context.

#### `get_pagemap_data() -> List[Dict[str, Any]]`

Retrieves page map data (metadata, images, etc.) from search results.

#### `fetch_page(page_number: int) -> Optional[CustomSearchResponse]`

Fetches a specific page of results based on the page number.

- **page_number** (`int`): The page number to fetch.
- **Returns**: A `CustomSearchResponse` for the specified page or `None` if an error occurs.

#### `display_results()`

Prints a formatted display of the search results for easy viewing in the console.

#### `last_search_info`

Property returning a dictionary with a summary of the last search results, including total results, search time, and formatted total results.

#### `get_snippets() -> List[Tuple[str, str]]`

Returns a list of tuples containing plain and HTML snippets for each result.

## Example Workflow

```python
# Initialize the engine
search_engine = CustomSearchEngine(api_key="YOUR_API_KEY", engine_id="YOUR_ENGINE_ID")

# Perform a search
search_engine.search("machine learning")
search_engine.display_results()

# Access total results and search time
print(f"Total Results: {search_engine.get_total_results()}")
print(f"Search Time: {search_engine.get_search_time()}")

# Fetch snippets
snippets = search_engine.get_snippets()
for plain_snippet, html_snippet in snippets:
    print(f"Plain Snippet: {plain_snippet}")
    print(f"HTML Snippet: {html_snippet}")
```

## License

This library is provided under the MIT License.
