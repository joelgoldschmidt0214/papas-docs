# TOMOSU Backend API Usage Examples

## Overview

This document provides comprehensive examples of how to use the TOMOSU Backend API. The API is designed for high performance with 95% of requests responding within 200ms.

## Base URL

- **Development**: `http://localhost:8000`
- **Production**: `https://tomosu-api.azurewebsites.net`

## Authentication

The MVP version uses simplified authentication with session cookies.

### Login

```bash
curl -X POST "http://localhost:8000/api/v1/auth/login" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "password"
  }' \
  -c cookies.txt
```

### Using Session Cookie

```bash
curl -X GET "http://localhost:8000/api/v1/posts" \
  -b cookies.txt
```

## Posts API

### Get Posts List

Retrieve posts with pagination (newest first):

```bash
# Basic request
curl -X GET "http://localhost:8000/api/v1/posts"

# With pagination
curl -X GET "http://localhost:8000/api/v1/posts?skip=0&limit=20"

# Response example
{
  "posts": [
    {
      "post_id": 1,
      "user_id": 1,
      "content": "地域のお祭り情報です！今年も盛大に開催予定です。",
      "created_at": "2024-01-15T10:30:00Z",
      "updated_at": "2024-01-15T10:30:00Z",
      "author": {
        "user_id": 1,
        "username": "tanaka_taro",
        "display_name": "田中太郎",
        "profile_image_url": "https://example.com/avatar1.jpg",
        "bio": "地域イベント大好きです",
        "area": "東京都渋谷区",
        "created_at": "2024-01-01T00:00:00Z"
      },
      "tags": [
        {"tag_id": 1, "tag_name": "イベント", "posts_count": 25},
        {"tag_id": 2, "tag_name": "お祭り", "posts_count": 12}
      ],
      "likes_count": 15,
      "comments_count": 3,
      "is_liked": false,
      "is_bookmarked": true
    }
  ]
}
```

### Get Single Post

```bash
curl -X GET "http://localhost:8000/api/v1/posts/1"
```

### Create New Post

```bash
curl -X POST "http://localhost:8000/api/v1/posts" \
  -H "Content-Type: application/json" \
  -b cookies.txt \
  -d '{
    "content": "新しい地域イベントの提案です！",
    "tags": ["イベント", "地域"]
  }'

# Response (201 Created)
{
  "post_id": 101,
  "user_id": 1,
  "content": "新しい地域イベントの提案です！",
  "created_at": "2024-01-15T10:30:00Z",
  "updated_at": "2024-01-15T10:30:00Z",
  "author": {
    "user_id": 1,
    "username": "tanaka_taro",
    "display_name": "田中太郎"
  },
  "tags": [
    {"tag_id": 1, "tag_name": "イベント", "posts_count": 26}
  ],
  "likes_count": 0,
  "comments_count": 0,
  "is_liked": false,
  "is_bookmarked": false
}
```

### Get Posts by Tag

```bash
curl -X GET "http://localhost:8000/api/v1/posts/tags/イベント?skip=0&limit=10"
```

### Get Post Comments

```bash
curl -X GET "http://localhost:8000/api/v1/posts/1/comments"

# Response example
[
  {
    "comment_id": 1,
    "post_id": 1,
    "user_id": 2,
    "content": "楽しみにしています！",
    "created_at": "2024-01-15T11:00:00Z",
    "author": {
      "user_id": 2,
      "username": "sato_hanako",
      "display_name": "佐藤花子"
    }
  }
]
```

## Users API

### Get User Profile

```bash
curl -X GET "http://localhost:8000/api/v1/users/1"

# Response example
{
  "user_id": 1,
  "username": "tanaka_taro",
  "display_name": "田中太郎",
  "email": "tanaka@example.com",
  "profile_image_url": "https://example.com/avatar1.jpg",
  "bio": "地域イベント大好きです",
  "area": "東京都渋谷区",
  "created_at": "2024-01-01T00:00:00Z",
  "updated_at": "2024-01-01T00:00:00Z",
  "followers_count": 10,
  "following_count": 15,
  "posts_count": 5
}
```

### Get User Followers

```bash
curl -X GET "http://localhost:8000/api/v1/users/1/followers?skip=0&limit=20"
```

### Get User Following

```bash
curl -X GET "http://localhost:8000/api/v1/users/1/following?skip=0&limit=20"
```

## Tags API

### Get All Tags

```bash
curl -X GET "http://localhost:8000/api/v1/tags"

# Response example
[
  {
    "tag_id": 1,
    "tag_name": "イベント",
    "posts_count": 25
  },
  {
    "tag_id": 2,
    "tag_name": "お祭り",
    "posts_count": 12
  }
]
```

### Get Specific Tag

```bash
curl -X GET "http://localhost:8000/api/v1/tags/イベント"
```

## Surveys API

### Get Surveys List

```bash
curl -X GET "http://localhost:8000/api/v1/surveys"

# Response example
[
  {
    "survey_id": 1,
    "title": "地域イベントについて",
    "question_text": "どのようなイベントに参加したいですか？",
    "points": 10,
    "deadline": "2024-02-15T23:59:59Z",
    "target_audience": "全住民",
    "created_at": "2024-01-01T00:00:00Z",
    "response_count": 25
  }
]
```

### Get Survey Details

```bash
curl -X GET "http://localhost:8000/api/v1/surveys/1"
```

### Get Survey Responses

```bash
curl -X GET "http://localhost:8000/api/v1/surveys/1/responses"

# Response example
{
  "survey_id": 1,
  "total_responses": 25,
  "responses": [
    {"option": "お祭り", "count": 15},
    {"option": "スポーツイベント", "count": 10}
  ]
}
```

## System Monitoring

### Health Check

```bash
curl -X GET "http://localhost:8000/api/v1/system/health"

# Response example
{
  "status": "healthy",
  "timestamp": "2024-01-15T10:30:00Z",
  "version": "1.0.0",
  "components": {
    "cache": {
      "status": "healthy",
      "initialized": true
    },
    "database": {
      "status": "healthy",
      "error": null
    }
  }
}
```

### System Metrics

```bash
curl -X GET "http://localhost:8000/api/v1/system/metrics"

# Response example
{
  "timestamp": "2024-01-15T10:30:00Z",
  "cache": {
    "status": "initialized",
    "initialization_time_seconds": 2.5,
    "data_counts": {
      "posts": 100,
      "users": 50,
      "comments": 200,
      "tags": 20,
      "surveys": 5,
      "likes": 500,
      "bookmarks": 150,
      "follows": 300
    },
    "memory_usage": {
      "total_mb": 128.5,
      "total_bytes": 134742016
    }
  },
  "performance": {
    "total_requests": 1000,
    "average_response_time_ms": 150.5,
    "min_response_time_ms": 50.0,
    "max_response_time_ms": 300.0,
    "requests_under_200ms": 950,
    "performance_target_percentage": 95.0,
    "meets_200ms_target": true
  },
  "system": {
    "status": "operational",
    "uptime": {
      "seconds": 3600.0,
      "hours": 1.0,
      "days": 0.04
    },
    "version": "1.0.0"
  }
}
```

## Error Handling

### Common Error Responses

#### 401 Unauthorized

```json
{
  "error_code": "AUTHENTICATION_REQUIRED",
  "message": "Authentication required",
  "timestamp": "2024-01-15T10:30:00Z"
}
```

#### 404 Not Found

```json
{
  "error_code": "RESOURCE_NOT_FOUND",
  "message": "Post not found",
  "resource_type": "Post",
  "resource_id": "999",
  "timestamp": "2024-01-15T10:30:00Z"
}
```

#### 422 Validation Error

```json
{
  "error_code": "VALIDATION_ERROR",
  "message": "Request validation failed",
  "field_errors": {
    "content": "Content is required"
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

#### 503 Service Unavailable

```json
{
  "error_code": "SERVICE_UNAVAILABLE",
  "message": "Service temporarily unavailable - cache not initialized",
  "service": "Cache",
  "timestamp": "2024-01-15T10:30:00Z"
}
```

## Performance Guidelines

### Response Time Expectations

- **95% of requests**: Under 200ms
- **Health checks**: Under 100ms
- **Post creation**: Under 300ms
- **Cache initialization**: Under 5 seconds

### Pagination Best Practices

- **Default limit**: 20 items
- **Maximum limit**: 100 items for posts, 500 for tags
- **Skip parameter**: Use for pagination, maximum 10,000

### Rate Limiting

- No explicit rate limiting in MVP
- System designed to handle 100 concurrent users
- Monitor response times and adjust load as needed

## JavaScript/TypeScript Examples

### Using Fetch API

```javascript
// Get posts
async function getPosts(skip = 0, limit = 20) {
  try {
    const response = await fetch(`/api/v1/posts?skip=${skip}&limit=${limit}`);
    
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    const posts = await response.json();
    return posts;
  } catch (error) {
    console.error('Error fetching posts:', error);
    throw error;
  }
}

// Create post
async function createPost(content, tags = []) {
  try {
    const response = await fetch('/api/v1/posts', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      credentials: 'include', // Include cookies
      body: JSON.stringify({ content, tags })
    });
    
    if (!response.ok) {
      const error = await response.json();
      throw new Error(error.message || `HTTP error! status: ${response.status}`);
    }
    
    const newPost = await response.json();
    return newPost;
  } catch (error) {
    console.error('Error creating post:', error);
    throw error;
  }
}

// Get user profile
async function getUserProfile(userId) {
  try {
    const response = await fetch(`/api/v1/users/${userId}`);
    
    if (!response.ok) {
      if (response.status === 404) {
        throw new Error('User not found');
      }
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    const user = await response.json();
    return user;
  } catch (error) {
    console.error('Error fetching user profile:', error);
    throw error;
  }
}
```

### Using Axios

```javascript
import axios from 'axios';

// Configure axios instance
const api = axios.create({
  baseURL: 'http://localhost:8000/api/v1',
  withCredentials: true, // Include cookies
  timeout: 5000 // 5 second timeout
});

// Response interceptor for error handling
api.interceptors.response.use(
  response => response,
  error => {
    if (error.response?.status === 401) {
      // Handle authentication error
      window.location.href = '/login';
    }
    return Promise.reject(error);
  }
);

// API functions
export const postsAPI = {
  getPosts: (skip = 0, limit = 20) => 
    api.get(`/posts?skip=${skip}&limit=${limit}`),
  
  getPost: (postId) => 
    api.get(`/posts/${postId}`),
  
  createPost: (content, tags = []) => 
    api.post('/posts', { content, tags }),
  
  getPostsByTag: (tagName, skip = 0, limit = 20) => 
    api.get(`/posts/tags/${tagName}?skip=${skip}&limit=${limit}`),
  
  getPostComments: (postId) => 
    api.get(`/posts/${postId}/comments`)
};

export const usersAPI = {
  getProfile: (userId) => 
    api.get(`/users/${userId}`),
  
  getFollowers: (userId, skip = 0, limit = 20) => 
    api.get(`/users/${userId}/followers?skip=${skip}&limit=${limit}`),
  
  getFollowing: (userId, skip = 0, limit = 20) => 
    api.get(`/users/${userId}/following?skip=${skip}&limit=${limit}`)
};

export const tagsAPI = {
  getTags: () => 
    api.get('/tags'),
  
  getTag: (tagName) => 
    api.get(`/tags/${tagName}`)
};

export const systemAPI = {
  getHealth: () => 
    api.get('/system/health'),
  
  getMetrics: () => 
    api.get('/system/metrics')
};
```

## Testing the API

### Using curl for Quick Tests

```bash
# Test health check
curl -w "@curl-format.txt" -o /dev/null -s "http://localhost:8000/api/v1/system/health"

# Test posts endpoint performance
time curl -s "http://localhost:8000/api/v1/posts" > /dev/null

# Test with authentication
curl -X POST "http://localhost:8000/api/v1/auth/login" \
  -H "Content-Type: application/json" \
  -d '{"email": "test@example.com", "password": "password"}' \
  -c cookies.txt

curl -X POST "http://localhost:8000/api/v1/posts" \
  -H "Content-Type: application/json" \
  -b cookies.txt \
  -d '{"content": "Test post", "tags": ["test"]}'
```

### Performance Testing with curl

Create `curl-format.txt`:

```
     time_namelookup:  %{time_namelookup}\n
        time_connect:  %{time_connect}\n
     time_appconnect:  %{time_appconnect}\n
    time_pretransfer:  %{time_pretransfer}\n
       time_redirect:  %{time_redirect}\n
  time_starttransfer:  %{time_starttransfer}\n
                     ----------\n
          time_total:  %{time_total}\n
```

Then test:

```bash
curl -w "@curl-format.txt" -o /dev/null -s "http://localhost:8000/api/v1/posts"
```

## Best Practices

### Client-Side Implementation

1. **Error Handling**: Always handle HTTP errors and network failures
2. **Loading States**: Show loading indicators for API calls
3. **Caching**: Cache responses when appropriate to reduce API calls
4. **Pagination**: Implement proper pagination for list endpoints
5. **Authentication**: Handle session expiration gracefully

### Performance Optimization

1. **Request Batching**: Combine multiple requests when possible
2. **Lazy Loading**: Load data only when needed
3. **Debouncing**: Debounce search and filter requests
4. **Connection Reuse**: Use HTTP/1.1 keep-alive or HTTP/2
5. **Compression**: Enable gzip compression for responses

### Security Considerations

1. **HTTPS**: Always use HTTPS in production
2. **Input Validation**: Validate all input on client side
3. **XSS Prevention**: Sanitize user-generated content
4. **CSRF Protection**: Include CSRF tokens when required
5. **Session Management**: Handle session cookies securely
