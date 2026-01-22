# Hugo JSON API for Android App

This implementation creates a complete JSON API for your Hugo website, specifically designed for Android app consumption.

## 🎯 Implementation Overview

### Core Concept
Leverage Hugo's static generation capabilities to automatically generate JSON API files during each build, without requiring an additional backend server.

### Architecture Advantages
- **Zero Maintenance Cost**: Pure static files deployed alongside the website
- **High Performance**: CDN caching and global distribution
- **Real-time Updates**: Automatically updated with each Hugo build
- **Type-Safe**: Structured JSON Schema

## 📡 API Endpoints

### 1. Statistics API - `/api/stats/`
Provides an overview of site statistics and analytics

```json
{
  "meta": {
    "generated": "2026-01-22T18:02:31+09:00",
    "endpoint": "stats"
  },
  "data": {
    "books": {
      "total": 26,
      "reading": 1,
      "finished": 0,
      "averageRating": 8,
      "femaleAuthors": 2,
      "countries": {
        "total": 7,
        "list": ["russia", "germany", "united_kingdom", ...]
      }
    },
    "posts": {
      "total": 5,
      "totalWords": 1257,
      "totalReadingTime": 8,
      "tags": {
        "total": 15,
        "list": ["ai", "backend", "career", ...]
      }
    }
  }
}
```

**Android Use Cases**: 
- Dashboard statistics display
- Progress tracking
- Data visualization

### 2. Posts API - `/api/posts/`
Returns all blog posts sorted by date (newest first)

```json
{
  "meta": {
    "total": 5,
    "generated": "2026-01-22T18:02:31+09:00",
    "endpoint": "posts"
  },
  "data": [
    {
      "id": "unique-file-id",
      "title": "Post Title",
      "summary": "Post summary...",
      "content": "Full post content...",
      "permalink": "https://unifuu.dev/posts/example/",
      "relativeURL": "/posts/example/",
      "date": "2026-01-20T00:00:00Z",
      "tags": ["tag1", "tag2"],
      "author": "Fū",
      "readingTime": 3,
      "wordCount": 629,
      "language": "ja",
      "cover": {
        "image": "https://example.com/image.jpg",
        "alt": "Image description"
      }
    }
  ]
}
```

**Android Use Cases**:
- Post list display
- Offline reading
- Search and filtering

### 3. Books API - `/api/books/`
Returns all books data grouped by reading status

```json
{
  "meta": {
    "total": 26,
    "reading": 1,
    "finished": 0,
    "generated": "2026-01-22T18:02:31+09:00"
  },
  "data": {
    "all": [...],
    "reading": [...],
    "finished": [...],
    "unfinished": [...]
  }
}
```

**Book Object Structure**:
```json
{
  "id": "unique-file-id",
  "title": "Book Title",
  "author": "Author Name",
  "year": 1925,
  "origin": "🇫🇷",
  "rating": 10,
  "finished": false,
  "isReading": true,
  "isFemale": false,
  "country": "france",
  "cover": {
    "image": "https://example.com/cover.jpg",
    "alt": "Book cover"
  }
}
```

### 4. Complete API - `/api/`
Get all data in a single request

```json
{
  "meta": {
    "version": "1.0",
    "generated": "2026-01-22T18:02:31+09:00",
    "baseURL": "https://unifuu.dev/",
    "totalPosts": 5,
    "totalBooks": 26
  },
  "data": {
    "posts": [...],
    "books": [...]
  }
}
```

## 🔧 Technical Implementation

### Hugo Template Structure
```
layouts/
├── api/
│   ├── api.json          # Complete API
│   ├── posts-api.json    # Posts API
│   ├── books-api.json    # Books API
│   └── stats-api.json    # Statistics API
└── _default/
    └── api.json          # Fallback template
```

### Content Structure
```
content/
└── api/
    ├── _index.md         # Main API endpoint
    ├── posts/
    │   └── _index.md     # Posts API endpoint
    ├── books/
    │   └── _index.md     # Books API endpoint
    └── stats/
        └── _index.md     # Statistics API endpoint
```

### Hugo Configuration
```yaml
outputs:
  home:
    - HTML
    - RSS
    - JSON
  section:
    - HTML
    - RSS
  page:
    - HTML
```

## 📱 Android Integration Example

### Retrofit Interface Definition
```java
public interface BlogAPI {
    @GET("api/posts/")
    Call<PostsResponse> getPosts();
    
    @GET("api/books/")
    Call<BooksResponse> getBooks();
    
    @GET("api/stats/")
    Call<StatsResponse> getStats();
    
    @GET("api/")
    Call<CompleteResponse> getAllData();
}
```

### Data Models
```java
public class Post {
    public String id;
    public String title;
    public String summary;
    public String content;
    public String permalink;
    public String date;
    public List<String> tags;
    public String author;
    public int readingTime;
    public int wordCount;
    public Cover cover;
}

public class Book {
    public String id;
    public String title;
    public String author;
    public int year;
    public String origin;
    public int rating;
    public boolean finished;
    public boolean isReading;
    public String country;
    public Cover cover;
}
```

### Usage Example
```java
// Initialize Retrofit
Retrofit retrofit = new Retrofit.Builder()
    .baseUrl("https://unifuu.dev/")
    .addConverterFactory(GsonConverterFactory.create())
    .build();

BlogAPI api = retrofit.create(BlogAPI.class);

// Fetch posts
api.getPosts().enqueue(new Callback<PostsResponse>() {
    @Override
    public void onResponse(Call<PostsResponse> call, Response<PostsResponse> response) {
        if (response.isSuccessful()) {
            List<Post> posts = response.body().data;
            // Update UI
        }
    }
    
    @Override
    public void onFailure(Call<PostsResponse> call, Throwable t) {
        // Handle error
    }
});
```

## 🚀 Deployment and Updates

### Automatic Update Flow
1. **Content Update**: Add new posts or books to Hugo
2. **Build Trigger**: Git push triggers GitHub Actions
3. **API Generation**: Hugo build automatically generates new JSON files
4. **Deployment**: Static files deployed to GitHub Pages
5. **App Sync**: Android app periodically checks for updates

### Caching Strategy
```java
// OkHttp cache configuration
OkHttpClient client = new OkHttpClient.Builder()
    .cache(new Cache(cacheDir, 10 * 1024 * 1024)) // 10MB cache
    .addInterceptor(new CacheInterceptor())
    .build();
```

## 📊 Performance Optimization

### Data Size Optimization
- **Compression**: Gzip compression reduces transfer size by 70%
- **Pagination**: Consider pagination for large datasets
- **Incremental Updates**: Implement incremental sync based on `lastmod` field

### Loading Strategy
```java
// Load statistics first
api.getStats().enqueue(statsCallback);

// Load complete data in background
api.getAllData().enqueue(dataCallback);
```

## 🔍 Testing and Validation

### API Test Page
Created `test-api.html` for testing all endpoints:
```bash
# Local testing
hugo server
# Visit http://localhost:1313/test-api.html
```

### Data Validation
- JSON Schema validation
- Field completeness checks
- Performance benchmarking

## 🛠️ Troubleshooting

### Common Issues
1. **JSON Format Errors**: Check Hugo template syntax
2. **Missing Data**: Verify front matter in content files
3. **Encoding Issues**: Ensure UTF-8 encoding

### Debugging Tips
```bash
# Check generated JSON
hugo && cat public/api/index.json | jq '.'

# Validate JSON format
jsonlint public/api/posts/index.json
```

## 📈 Future Enhancements

### Possible Features
- **Search API**: Full-text search endpoint
- **Category API**: Filter by tags/categories
- **RSS Integration**: RSS to JSON conversion
- **Multi-language Support**: Language-specific API endpoints

### Version Control
```json
{
  "meta": {
    "version": "1.0",
    "apiVersion": "2024-01-22",
    "deprecated": false
  }
}
```

## 📚 Additional Resources

- **API Documentation**: See `content/api-docs.md`
- **Test Page**: Open `test-api.html` in browser
- **Hugo Docs**: https://gohugo.io/templates/output-formats/

This implementation provides a complete, efficient, and maintainable JSON API solution perfectly suited for Android app consumption.