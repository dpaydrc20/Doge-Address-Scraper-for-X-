# Doge-Address-Scraper-for-X-
posts and pages
WORK IN PROGRESS 


### Script to Fetch Replies and Find Dogecoin Addresses

```python
import tweepy
import re
import os

# Twitter API credentials
api_key = os.getenv('TWITTER_API_KEY')
api_secret_key = os.getenv('TWITTER_API_SECRET_KEY')
access_token = os.getenv('TWITTER_ACCESS_TOKEN')
access_token_secret = os.getenv('TWITTER_ACCESS_TOKEN_SECRET')
bearer_token = os.getenv('TWITTER_BEARER_TOKEN')

# Authenticate to Twitter using OAuth 2.0 (Application Context) for v2 endpoints
client = tweepy.Client(
    bearer_token=bearer_token,
    consumer_key=api_key,
    consumer_secret=api_secret_key,
    access_token=access_token,
    access_token_secret=access_token_secret
)

# Function to find Dogecoin addresses in text
def find_doge_addresses(text):
    doge_pattern = r'D[5-9A-HJ-NP-UZa-km-z]{25,34}'
    return re.findall(doge_pattern, text)

# Function to fetch replies of a tweet and find Dogecoin addresses
def fetch_replies_and_find_addresses(tweet_id, username):
    query = f'conversation_id:{tweet_id} to:{username}'
    try:
        replies = client.search_recent_tweets(query=query, tweet_fields=['conversation_id', 'author_id', 'created_at', 'text'], max_results=100)
        doge_addresses = []
        if replies.data:
            for reply in replies.data:
                addresses = find_doge_addresses(reply.text)
                if addresses:
                    print(f"Dogecoin address found in reply: {addresses}")
                doge_addresses.extend(addresses)
        return doge_addresses
    except tweepy.TweepyException as e:
        print(f"Error occurred: {e}")
        return []

# Tweet ID and username
tweet_id = '1790810563989357013'
username = 'Dogepay_DRC20'

# Fetch replies and find Dogecoin addresses
doge_addresses = fetch_replies_and_find_addresses(tweet_id, username)

if doge_addresses:
    print("Found Dogecoin addresses:")
    for address in doge_addresses:
        print(address)
else:
    print("No Dogecoin addresses found.")
```

### Instructions for Using the Script

1. **Set Up Twitter Developer Account**:
    - Go to the [Twitter Developer website](https://developer.twitter.com/en) and sign up for a developer account.
    - Create a new project and generate API keys and access tokens.

2. **Create Environment Variables**:
    - Store your Twitter API credentials in environment variables for security. You can set these in your operating system or a `.env` file.

    For Windows (Command Prompt):
    ```bash
    set TWITTER_API_KEY=your_api_key
    set TWITTER_API_SECRET_KEY=your_api_secret_key
    set TWITTER_ACCESS_TOKEN=your_access_token
    set TWITTER_ACCESS_TOKEN_SECRET=your_access_token_secret
    set TWITTER_BEARER_TOKEN=your_bearer_token
    ```

    For macOS/Linux (Terminal):
    ```bash
    export TWITTER_API_KEY=your_api_key
    export TWITTER_API_SECRET_KEY=your_api_secret_key
    export TWITTER_ACCESS_TOKEN=your_access_token
    export TWITTER_ACCESS_TOKEN_SECRET=your_access_token_secret
    export TWITTER_BEARER_TOKEN=your_bearer_token
    ```

    Alternatively, create a `.env` file in the same directory as your script:
    ```
    TWITTER_API_KEY=your_api_key
    TWITTER_API_SECRET_KEY=your_api_secret_key
    TWITTER_ACCESS_TOKEN=your_access_token
    TWITTER_ACCESS_TOKEN_SECRET=your_access_token_secret
    TWITTER_BEARER_TOKEN=your_bearer_token
    ```

    And load it in your script using `python-dotenv`:
    ```bash
    pip install python-dotenv
    ```

    Then add this to the top of your script:
    ```python
    from dotenv import load_dotenv
    load_dotenv()
    ```

3. **Install Required Libraries**:
    - Install `tweepy` and `python-dotenv` (if using `.env` file).

    ```bash
    pip install tweepy python-dotenv
    ```

4. **Run the Script**:
    - Save your script (e.g., `fetch_doge_addresses.py`).
    - Open your terminal or command prompt and navigate to the directory where your script is located.
    - Run the script:

    ```bash
    python fetch_doge_addresses.py
    ```

This script will fetch replies to the specified tweet and search for any Dogecoin addresses within those replies. The credentials are securely stored in environment variables, and the script prints any found addresses.
