# Review Scraper with LangChain and Groq

A Python-based web scraping solution that extracts customer reviews from various e-commerce websites using LangChain and Groq LLM.

## Features

- Extracts customer reviews from multiple e-commerce websites
- Processes review data into structured JSON format
- Uses LangChain for web scraping and content processing
- Leverages Groq's LLama 3.1 model for intelligent text extraction
- Handles different website structures seamlessly

## Prerequisites

- Python 3.x
- pip (Python package manager)

## Installation

1. Clone this repository or download the script
2. Install the required dependencies:

```bash
pip install langchain
pip install langchain-groq
pip install langchain-community
```

## Configuration

The script uses Groq API for processing. You'll need to:

1. Get a Groq API key (a test key is included but recommended to use your own)
2. Set up the API key in the script or as an environment variable

## Usage

The script demonstrates scraping reviews from three sample websites:

1. 2717 Recovery Cream (https://2717recovery.com/products/recovery-cream)
2. BHUMI (https://bhumi.com.au/pages/over-14000-customer-reviews)
3. LyfeFuel (https://www.reviews.io/company-reviews/store/lyfefuel.com)

To run the script:

```bash
python scrapping_assignment_gomarble.py
```

## Output Format

The script extracts reviews in the following JSON format:

```json
{
  "reviews_count": <total_reviews>,
  "reviews": [
    {
      "title": "<review_title>",
      "body": "<review_body_text>",
      "rating": <rating_value>,
      "reviewer": "<reviewer_name>"
    }
  ]
}
```

## Components

- `WebBaseLoader`: Handles web page content extraction
- `PromptTemplate`: Structures the extraction prompt for the LLM
- `ChatGroq`: Processes the content using Groq's LLama 3.1 model

## Limitations

- Website structure changes may affect scraping accuracy
- Rate limiting may apply based on API usage
- Some websites may block automated scraping

## Contributing

Feel free to submit issues, fork the repository, and create pull requests for any improvements.

## License

This project is open-source and available under the MIT License.
