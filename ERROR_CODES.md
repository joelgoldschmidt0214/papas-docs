# TOMOSU Backend API Error Codes

## Overview

This document provides a comprehensive reference for all error codes returned by the TOMOSU Backend API. All errors follow a consistent format and include appropriate HTTP status codes.

## Error Response Format

All API errors return a consistent JSON structure:

```json
{
  "error_code": "ERROR_TYPE",
  "message": "Human-readable error description",
  "details": {
    "additional": "context-specific information"
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

## HTTP Status Codes

### 2xx Success
- **200 OK**: Request successful
- **201 Created**: Resource created successfully

### 4xx Client Errors
- **400 Bad Request**: Invalid request format
- **401 Unauthorized**: Authentication required
- **403 Forbidden**: Access denied
- **404 Not Found**: Resource not found
- **422 Unprocessable Entity**: Validation error

### 5xx Server Errors
- **500 Internal Server Error**: Unexpected server error
- **503 Service Unavailable**: Service temporarily unavailable

## Error Code Reference

### Authentication Errors (401)

#### AUTHENTICATION_REQUIRED
**HTTP Status**: 401 Unauthorized

**Description**: Request requires authentication but no valid session was provided.

**Example Response**:
```json
{
  "error_code": "AUTHENTICATION_REQUIRED",
  "message": "Authentication required",
  "timestamp": "2024-01-15T10:30:00Z"
}
```

**Common Causes**:
- No session cookie provided
- Session cookie expired
- Invalid session token

**Resolution**:
- Login via `/api/v1/auth/login`
- Ensure session cookies are included in requests
- Check session expiration

#### SESSION_EXPIRED
**HTTP Status**: 401 Unauthorized

**Description**: User session has expired and needs to be renewed.

**Example Response**:
```json
{
  "error_code": "SESSION_EXPIRED",
  "message": "Session has expired",
  "details": {
    "expired_at": "2024-01-15T09:30:00Z"
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

**Resolution**:
- Re-authenticate via login endpoint
- Implement automatic session renewal

#### INVALID_CREDENTIALS
**HTTP Status**: 401 Unauthorized

**Description**: Login credentials are invalid.

**Example Response**:
```json
{
  "error_code": "INVALID_CREDENTIALS",
  "message": "Invalid email or password",
  "timestamp": "2024-01-15T10:30:00Z"
}
```

**Resolution**:
- Verify email and password
- Check for typos in credentials
- Ensure user account exists

### Authorization Errors (403)

#### ACCESS_DENIED
**HTTP Status**: 403 Forbidden

**Description**: User does not have permission to access the requested resource.

**Example Response**:
```json
{
  "error_code": "ACCESS_DENIED",
  "message": "Access denied to resource",
  "details": {
    "resource_type": "Post",
    "resource_id": "123",
    "required_permission": "write"
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

**Resolution**:
- Check user permissions
- Ensure user has appropriate role
- Contact administrator if needed

### Resource Not Found Errors (404)

#### RESOURCE_NOT_FOUND
**HTTP Status**: 404 Not Found

**Description**: Requested resource does not exist.

**Example Response**:
```json
{
  "error_code": "RESOURCE_NOT_FOUND",
  "message": "Post not found",
  "details": {
    "resource_type": "Post",
    "resource_id": "999"
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

**Common Resource Types**:
- `Post`: Blog posts/articles
- `User`: User profiles
- `Tag`: Content tags
- `Survey`: Community surveys
- `Comment`: Post comments

**Resolution**:
- Verify resource ID is correct
- Check if resource was deleted
- Use list endpoints to find valid IDs

#### ENDPOINT_NOT_FOUND
**HTTP Status**: 404 Not Found

**Description**: API endpoint does not exist.

**Example Response**:
```json
{
  "error_code": "ENDPOINT_NOT_FOUND",
  "message": "API endpoint not found",
  "details": {
    "path": "/api/v1/invalid-endpoint",
    "method": "GET"
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

**Resolution**:
- Check API documentation for correct endpoints
- Verify API version in URL path
- Check HTTP method (GET, POST, etc.)

### Validation Errors (422)

#### VALIDATION_ERROR
**HTTP Status**: 422 Unprocessable Entity

**Description**: Request data failed validation.

**Example Response**:
```json
{
  "error_code": "VALIDATION_ERROR",
  "message": "Request validation failed",
  "details": {
    "field_errors": {
      "content": "Content is required",
      "tags": "Tags must be an array of strings"
    }
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

**Common Field Errors**:
- `content`: Post content validation
- `email`: Email format validation
- `tags`: Tag format validation
- `skip`: Pagination parameter validation
- `limit`: Pagination parameter validation

**Resolution**:
- Check required fields are provided
- Verify data types match expected format
- Ensure values are within allowed ranges

#### INVALID_PAGINATION
**HTTP Status**: 422 Unprocessable Entity

**Description**: Pagination parameters are invalid.

**Example Response**:
```json
{
  "error_code": "VALIDATION_ERROR",
  "message": "Request validation failed",
  "details": {
    "field_errors": {
      "skip": "Skip must be non-negative",
      "limit": "Limit must be between 1 and 100"
    }
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

**Pagination Limits**:
- `skip`: 0 to 10,000
- `limit`: 1 to 100 (posts), 1 to 500 (tags)

**Resolution**:
- Ensure skip ≥ 0
- Ensure limit is within allowed range
- Use appropriate limits for different endpoints

#### CONTENT_TOO_LONG
**HTTP Status**: 422 Unprocessable Entity

**Description**: Content exceeds maximum length limit.

**Example Response**:
```json
{
  "error_code": "VALIDATION_ERROR",
  "message": "Request validation failed",
  "details": {
    "field_errors": {
      "content": "Content exceeds maximum length of 1000 characters"
    },
    "current_length": 1250,
    "max_length": 1000
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

**Content Limits**:
- Post content: 1,000 characters
- Comment content: 500 characters
- Bio: 200 characters

**Resolution**:
- Reduce content length
- Split long content into multiple posts
- Use external links for lengthy content

### Service Unavailable Errors (503)

#### SERVICE_UNAVAILABLE
**HTTP Status**: 503 Service Unavailable

**Description**: Service is temporarily unavailable, usually due to cache initialization issues.

**Example Response**:
```json
{
  "error_code": "SERVICE_UNAVAILABLE",
  "message": "Service temporarily unavailable - cache not initialized",
  "details": {
    "service": "Cache",
    "retry_after": 30
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

**Common Causes**:
- Cache not initialized on startup
- Database connection issues
- System maintenance

**Resolution**:
- Wait for cache initialization to complete
- Retry request after specified delay
- Check system status endpoints

#### CACHE_INITIALIZATION_FAILED
**HTTP Status**: 503 Service Unavailable

**Description**: Cache failed to initialize properly.

**Example Response**:
```json
{
  "error_code": "SERVICE_UNAVAILABLE",
  "message": "Failed to initialize cache during startup",
  "details": {
    "service": "Cache",
    "startup_phase": "cache_initialization",
    "error": "Database connection timeout"
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

**Resolution**:
- Check database connectivity
- Restart application
- Contact system administrator

#### DATABASE_UNAVAILABLE
**HTTP Status**: 503 Service Unavailable

**Description**: Database is not accessible.

**Example Response**:
```json
{
  "error_code": "SERVICE_UNAVAILABLE",
  "message": "Database temporarily unavailable",
  "details": {
    "service": "Database",
    "error": "Connection timeout"
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

**Resolution**:
- Check database server status
- Verify network connectivity
- Contact system administrator

### Internal Server Errors (500)

#### INTERNAL_SERVER_ERROR
**HTTP Status**: 500 Internal Server Error

**Description**: Unexpected server error occurred.

**Example Response**:
```json
{
  "error_code": "INTERNAL_SERVER_ERROR",
  "message": "An unexpected error occurred",
  "details": {
    "error_id": "err_123456789",
    "timestamp": "2024-01-15T10:30:00Z"
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

**Resolution**:
- Retry the request
- Report error with error_id to support
- Check system status

#### CACHE_ERROR
**HTTP Status**: 500 Internal Server Error

**Description**: Cache operation failed unexpectedly.

**Example Response**:
```json
{
  "error_code": "INTERNAL_SERVER_ERROR",
  "message": "Cache operation failed",
  "details": {
    "operation": "get_posts",
    "error": "Memory allocation failed"
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

**Resolution**:
- Retry the request
- Check system memory usage
- Contact system administrator

## Error Handling Best Practices

### Client-Side Error Handling

```javascript
async function handleAPICall(apiFunction) {
  try {
    const response = await apiFunction();
    return response.data;
  } catch (error) {
    if (error.response) {
      // Server responded with error status
      const { status, data } = error.response;
      
      switch (status) {
        case 401:
          // Handle authentication errors
          if (data.error_code === 'SESSION_EXPIRED') {
            // Redirect to login
            window.location.href = '/login';
          }
          break;
          
        case 404:
          // Handle not found errors
          if (data.error_code === 'RESOURCE_NOT_FOUND') {
            showNotification('Resource not found', 'error');
          }
          break;
          
        case 422:
          // Handle validation errors
          if (data.error_code === 'VALIDATION_ERROR') {
            displayValidationErrors(data.details.field_errors);
          }
          break;
          
        case 503:
          // Handle service unavailable
          if (data.error_code === 'SERVICE_UNAVAILABLE') {
            showRetryMessage(data.details.retry_after);
          }
          break;
          
        default:
          showGenericError();
      }
    } else if (error.request) {
      // Network error
      showNetworkError();
    } else {
      // Other error
      showGenericError();
    }
    
    throw error;
  }
}
```

### Retry Logic

```javascript
async function retryableAPICall(apiFunction, maxRetries = 3) {
  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      return await apiFunction();
    } catch (error) {
      if (error.response?.status === 503 && attempt < maxRetries) {
        // Service unavailable - retry with exponential backoff
        const delay = Math.pow(2, attempt) * 1000; // 2s, 4s, 8s
        await new Promise(resolve => setTimeout(resolve, delay));
        continue;
      }
      throw error;
    }
  }
}
```

### Error Logging

```javascript
function logError(error, context) {
  const errorInfo = {
    timestamp: new Date().toISOString(),
    context,
    status: error.response?.status,
    error_code: error.response?.data?.error_code,
    message: error.response?.data?.message,
    details: error.response?.data?.details,
    stack: error.stack
  };
  
  // Send to logging service
  console.error('API Error:', errorInfo);
  
  // In production, send to error tracking service
  // errorTracker.captureException(error, errorInfo);
}
```

## Monitoring and Alerting

### Error Rate Monitoring

Monitor these key metrics:
- Overall error rate (should be < 5%)
- 5xx error rate (should be < 1%)
- Authentication error rate
- Cache initialization failures

### Alert Thresholds

Set up alerts for:
- Error rate > 5% over 5 minutes
- 5xx errors > 1% over 5 minutes
- Service unavailable errors > 10 in 1 minute
- Cache initialization failures

### Health Check Integration

Use health check endpoints to monitor service status:
- `/api/v1/system/health` - Overall system health
- `/api/v1/system/metrics` - Performance metrics

## Troubleshooting Guide

### Common Issues

1. **High 503 Errors**
   - Check cache initialization status
   - Verify database connectivity
   - Monitor system resources

2. **Authentication Failures**
   - Check session cookie configuration
   - Verify session timeout settings
   - Monitor login endpoint performance

3. **Validation Errors**
   - Review API documentation
   - Check client-side validation
   - Verify data formats

4. **Performance Issues**
   - Monitor response times
   - Check cache hit rates
   - Analyze slow queries

### Debug Information

Include these details when reporting issues:
- Error code and message
- Request details (method, URL, headers)
- Response status and body
- Timestamp
- User session information (if applicable)
- Browser/client information