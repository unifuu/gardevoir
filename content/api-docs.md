---
title: "API Documentation"
date: 2026-01-22
---

# JSON API Documentation

This site provides several JSON API endpoints for mobile app integration.

## Available Endpoints

### 1. Complete API
- **URL**: `/api/`
- **Description**: Returns all posts and books data in a single response
- **Use case**: Initial app data loading

### 2. Posts API
- **URL**: `/api/posts/`
- **Description**: Returns all blog posts
- **Features**: 
  - Sorted by date (newest first)
  - Includes reading time and word count
  - Full content and metadata

### 3. Books API
- **URL**: `/api/books/`
- **Description**: Returns all books data
- **Features**:
  - Grouped by reading status (reading, finished, unfinished)
  - Sorted by rating (highest first)
  - Includes author, year, country, and rating information

### 4. Statistics API
- **URL**: `/api/stats/`
- **Description**: Returns statistical data about the site
- **Features**:
  - Book statistics (total, reading status, ratings, countries)
  - Post statistics (total, word count, tags)
  - Overview data

## Data Structure

### Post Object
```json
{
  "id": "unique-file-id",
  "title": "Post Title",
  "summary": "Post summary...",
  "content": "Full post content...",
  "permalink": "https://unifuu.dev/posts/example/",
  "relativeURL": "/posts/example/",
  "date": "2026-01-22T10:00:00Z",
  "lastmod": "2026-01-22T10:00:00Z",
  "tags": ["tag1", "tag2"],
  "categories": ["category1"],
  "author": "Fū",
  "readingTime": 5,
  "wordCount": 1200,
  "language": "ja",
  "cover": {
    "image": "https://example.com/image.jpg",
    "alt": "Image description"
  }
}
```

### Book Object
```json
{
  "id": "unique-file-id",
  "title": "Book Title",
  "summary": "Book summary...",
  "content": "Book content...",
  "permalink": "https://unifuu.dev/books/country/book/",
  "relativeURL": "/books/country/book/",
  "date": "2026-01-22T10:00:00Z",
  "lastmod": "2026-01-22T10:00:00Z",
  "author": "Author Name",
  "year": 1925,
  "origin": "🇫🇷",
  "rating": 9,
  "finished": true,
  "isReading": false,
  "isFemale": false,
  "language": "ja",
  "cover": {
    "image": "https://example.com/cover.jpg",
    "alt": "Book cover"
  },
  "country": "france"
}
```

## Usage Examples

### Android Retrofit Integration
```java
// API Interface
public interface BlogAPI {
    @GET("api/posts/")
    Call<PostsResponse> getPosts();
    
    @GET("api/books/")
    Call<BooksResponse> getBooks();
    
    @GET("api/stats/")
    Call<StatsResponse> getStats();
}

// Usage
Retrofit retrofit = new Retrofit.Builder()
    .baseUrl("https://unifuu.dev/")
    .addConverterFactory(GsonConverterFactory.create())
    .build();

BlogAPI api = retrofit.create(BlogAPI.class);
```

### JavaScript Fetch
```javascript
// Fetch posts
fetch('https://unifuu.dev/api/posts/')
  .then(response => response.json())
  .then(data => {
    console.log('Posts:', data.data);
  });

// Fetch books
fetch('https://unifuu.dev/api/books/')
  .then(response => response.json())
  .then(data => {
    console.log('Reading:', data.data.reading);
    console.log('Finished:', data.data.finished);
  });
```

## Features

- **Multilingual Support**: Content available in Japanese and English
- **Rich Metadata**: Includes reading time, word count, ratings, and more
- **Structured Data**: Well-organized JSON with consistent schema
- **Real-time Updates**: Generated automatically when site is rebuilt
- **Mobile Optimized**: Designed specifically for mobile app consumption

## Notes

- All dates are in ISO 8601 format with timezone
- Images URLs are absolute for easy mobile app loading
- Content includes both summary and full text
- API responses include metadata for caching and pagination