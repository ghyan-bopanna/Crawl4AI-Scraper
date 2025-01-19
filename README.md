# Advanced Web Crawling and Data Extraction Using Crawl4AI

Crawl4AI is an advanced web crawling and data extraction tool that leverages Large Language Models (LLMs) for intelligent and dynamic data retrieval. With built-in support for schema-driven extraction, pagination handling, and dynamic CSS identification, it simplifies the process of extracting meaningful data from web pages.

---

## Features

### 1. Dynamic CSS Identification
- Utilizes LLMs to identify dynamic CSS selectors for extracting data elements such as reviews or product details.

### 2. Pagination Handling
- Automatically navigates through all pages of a website to ensure complete data extraction.

### 3. LLM-Powered Data Extraction
- Extracts data using schema-driven strategies with custom instructions tailored to the content structure.

### 4. Schema-Driven Extraction
- Define structured schemas using `pydantic` models for accurate and reliable data extraction.

---

## Solution Approach

### Architecture Overview

1. **Input**: Provide a URL and specify extraction requirements (e.g., reviews, ratings).
2. **Crawling**: Use `AsyncWebCrawler` to fetch and parse content.
3. **Dynamic CSS Identification**: LLMs identify CSS selectors dynamically for reliable data extraction.
4. **Data Extraction**: LLMs extract structured data based on user-defined schemas.
5. **Pagination Handling**: Automatically traverse through all pages using CSS selectors for pagination controls.
6. **Output**: Return data in a structured JSON format.

### Workflow Diagram

```plaintext
[Input URL] --> [AsyncWebCrawler] --> [LLM CSS Identification] --> [Data Extraction (Schema)] --> [Pagination Handling] --> [Structured JSON Output]
```

---

## Installation

### Prerequisites
- Python 3.8+
- An OpenAI API Key

### Setup
1. **Install System Dependencies**:
   ```bash
   sudo apt-get update && sudo apt-get install -y \
       libwoff1 libopus0 libwebp6 libwebpdemux2 libenchant1c2a \
       libgudev-1.0-0 libsecret-1-0 libhyphen0 libgdk-pixbuf2.0-0 \
       libegl1 libnotify4 libxslt1.1 libevent-2.1-7 libgles2 libvpx6 \
       libxcomposite1 libatk1.0-0 libatk-bridge2.0-0 libepoxy0 \
       libgtk-3-0 libharfbuzz-icu0
   ```

2. **Install Python Dependencies**:
   ```bash
   pip install crawl4ai nest-asyncio
   playwright install
   ```

3. **Set OpenAI API Key**:
   Export your OpenAI API key:
   ```bash
   export OPENAI_API_KEY='your-openai-api-key'
   ```

---

## Usage

### Example: Extracting User Reviews

#### Python Code
```python
import asyncio
import os
from crawl4ai import AsyncWebCrawler
from crawl4ai.extraction_strategy import LLMExtractionStrategy
from pydantic import BaseModel, Field
import nest_asyncio

nest_asyncio.apply()

# Define schema for reviews
class UserReviewSchema(BaseModel):
    title: str = Field(..., description="Title of the review")
    body: str = Field(..., description="Body text of the review")
    rating: int = Field(..., description="Rating given in the review")
    reviewer: str = Field(..., description="Name of the reviewer")

# Function to extract reviews
async def extract_user_reviews():
    async with AsyncWebCrawler(verbose=True) as crawler:
        result = await crawler.arun(
            url='https://example.com/product-page',  # Replace with the product page URL
            extraction_strategy=LLMExtractionStrategy(
                provider="openai/gpt-4o-mini-2024-07-18",
                api_token=os.getenv('OPENAI_API_KEY'),
                schema=UserReviewSchema.schema(),
                instruction="""
                Extract user reviews in the following format:
                {
                    "reviews_count": 100,
                    "reviews": [
                        {
                            "title": "Review Title",
                            "body": "Review body text",
                            "rating": 5,
                            "reviewer": "Reviewer Name"
                        }
                    ]
                }
                Ensure the response includes all reviews across pagination.
                """
            ),
            pagination=True,  # Enable pagination handling
            pagination_selector="button.next-page"  # Update with the correct CSS selector
        )
        print(result.extracted_content)

# Run the function
await extract_user_reviews()
```

#### Sample JSON Response
```json
{
  "reviews_count": 100,
  "reviews": [
    {
      "title": "Great product!",
      "body": "I love this product. It works perfectly.",
      "rating": 5,
      "reviewer": "John Doe"
    },
    {
      "title": "Not worth it",
      "body": "The quality was disappointing.",
      "rating": 2,
      "reviewer": "Jane Smith"
    }
  ]
}
```


