# <i class="fas fa-robot"></i> Automating WordPress Workflows with Python: A Developer's Guide

**Published:** October 2024  
**Category:** Automation, Python, WordPress Integration

As I've been learning Python alongside my WordPress expertise, I've discovered powerful ways to automate WordPress workflows. This post shares practical Python scripts and techniques for automating common WordPress tasks.

## Why Python for WordPress Automation?

Python offers excellent libraries and tools for automation:
- **Requests library**: Simple HTTP client for WordPress REST API
- **Beautiful Soup**: Web scraping capabilities
- **Pandas**: Data processing and analysis
- **Schedule**: Task scheduling
- **CLI tools**: Build command-line interfaces easily

## Setting Up Python WordPress Client

### Install Required Libraries

```bash
pip install requests python-wordpress-xmlrpc
```

### Basic WordPress REST API Client

```python
import requests
from requests.auth import HTTPBasicAuth

class WordPressClient:
    def __init__(self, site_url, username, password):
        self.site_url = site_url.rstrip('/')
        self.username = username
        self.password = password
        self.api_url = f"{self.site_url}/wp-json/wp/v2"
    
    def get_posts(self, per_page=10):
        response = requests.get(
            f"{self.api_url}/posts",
            params={'per_page': per_page},
            auth=HTTPBasicAuth(self.username, self.password)
        )
        return response.json()
    
    def create_post(self, title, content, status='draft'):
        data = {
            'title': title,
            'content': content,
            'status': status
        }
        response = requests.post(
            f"{self.api_url}/posts",
            json=data,
            auth=HTTPBasicAuth(self.username, self.password)
        )
        return response.json()
```

## Automating Content Management

### Bulk Post Creation

```python
def bulk_create_posts(client, posts_data):
    """Create multiple posts from a list"""
    created_posts = []
    for post_data in posts_data:
        post = client.create_post(
            title=post_data['title'],
            content=post_data['content'],
            status=post_data.get('status', 'draft')
        )
        created_posts.append(post)
        print(f"Created post: {post['title']['rendered']}")
    return created_posts
```

### Content Updates

```python
def update_post_content(client, post_id, new_content):
    """Update existing post content"""
    response = requests.post(
        f"{client.api_url}/posts/{post_id}",
        json={'content': new_content},
        auth=HTTPBasicAuth(client.username, client.password)
    )
    return response.json()
```

## Database Operations via Python

### Direct Database Access

```python
import mysql.connector

def connect_to_wordpress_db(host, user, password, database):
    """Connect to WordPress database"""
    return mysql.connector.connect(
        host=host,
        user=user,
        password=password,
        database=database
    )

def get_all_posts(db_connection):
    """Fetch all posts from database"""
    cursor = db_connection.cursor()
    cursor.execute("SELECT * FROM wp_posts WHERE post_type = 'post'")
    return cursor.fetchall()
```

## Bulk Operations and Data Processing

### Process CSV and Import to WordPress

```python
import pandas as pd

def import_posts_from_csv(client, csv_file):
    """Import posts from CSV file"""
    df = pd.read_csv(csv_file)
    
    for _, row in df.iterrows():
        client.create_post(
            title=row['title'],
            content=row['content'],
            status=row.get('status', 'draft')
        )
```

### Export WordPress Data

```python
def export_posts_to_csv(client, output_file):
    """Export WordPress posts to CSV"""
    posts = client.get_posts(per_page=100)
    
    df = pd.DataFrame([{
        'id': post['id'],
        'title': post['title']['rendered'],
        'date': post['date'],
        'status': post['status']
    } for post in posts])
    
    df.to_csv(output_file, index=False)
```

## Building CLI Tools

### Simple WordPress CLI Tool

```python
import argparse

def main():
    parser = argparse.ArgumentParser(description='WordPress CLI Tool')
    parser.add_argument('action', choices=['list', 'create', 'update'])
    parser.add_argument('--title', help='Post title')
    parser.add_argument('--content', help='Post content')
    
    args = parser.parse_args()
    
    client = WordPressClient(
        site_url='https://yoursite.com',
        username='your_username',
        password='your_password'
    )
    
    if args.action == 'list':
        posts = client.get_posts()
        for post in posts:
            print(f"{post['id']}: {post['title']['rendered']}")
    
    elif args.action == 'create':
        post = client.create_post(args.title, args.content)
        print(f"Created post: {post['id']}")

if __name__ == '__main__':
    main()
```

## Integration with AI Tools

### AI-Powered Content Generation

```python
import openai

def generate_post_with_ai(topic):
    """Generate WordPress post content using AI"""
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[
            {"role": "system", "content": "You are a WordPress content writer."},
            {"role": "user", "content": f"Write a blog post about {topic}"}
        ]
    )
    return response.choices[0].message.content

# Use with WordPress client
client = WordPressClient(...)
content = generate_post_with_ai("WordPress automation")
client.create_post("AI Generated Post", content)
```

## Scheduled Automation

### Using Python Schedule Library

```python
import schedule
import time

def daily_backup():
    """Backup WordPress posts daily"""
    client = WordPressClient(...)
    export_posts_to_csv(client, f"backup_{time.strftime('%Y%m%d')}.csv")

# Schedule daily backup at 2 AM
schedule.every().day.at("02:00").do(daily_backup)

while True:
    schedule.run_pending()
    time.sleep(60)
```

## Key Takeaways

- Python provides powerful tools for WordPress automation
- Use WordPress REST API for most operations
- Direct database access for complex queries
- Build CLI tools for repetitive tasks
- Integrate AI tools for content generation
- Schedule automated tasks for regular operations

---

**Tags:** Python, WordPress, Automation, REST API, CLI Tools, AI Integration

