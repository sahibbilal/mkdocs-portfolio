# <i class="fas fa-plug"></i> Building REST APIs for Headless WordPress: Best Practices and Patterns

**Published:** November 2024  
**Category:** API Development, Headless WordPress

Headless WordPress is gaining traction, but building robust REST APIs that work seamlessly with modern frontend frameworks requires careful planning. This post covers the patterns and practices I use to create production-ready WordPress REST APIs.

## Why Headless WordPress?

Headless WordPress separates the content management (WordPress backend) from the presentation layer (frontend framework). This architecture offers:

- **Flexibility**: Use any frontend framework (React, Vue, Next.js)
- **Performance**: Optimize frontend independently
- **Scalability**: Scale frontend and backend separately
- **Developer Experience**: Modern development workflows

## Custom REST API Endpoint Design

### Registering Custom Endpoints

```php
add_action('rest_api_init', function () {
    register_rest_route('myplugin/v1', '/posts', array(
        'methods' => 'GET',
        'callback' => 'get_custom_posts',
        'permission_callback' => '__return_true'
    ));
});
```

### RESTful Design Principles

- **Use proper HTTP methods**: GET, POST, PUT, DELETE
- **Resource-based URLs**: `/posts/123` not `/get_post?id=123`
- **Consistent response format**: Always return JSON with consistent structure
- **Proper status codes**: 200, 201, 400, 404, 500

## Authentication and Authorization

### Authentication Methods

1. **Application Passwords**: Built-in WordPress feature
2. **OAuth 2.0**: For third-party integrations
3. **JWT Tokens**: For stateless authentication
4. **API Keys**: Simple but less secure

### Authorization Strategies

```php
'permission_callback' => function($request) {
    return current_user_can('edit_posts');
}
```

## Rate Limiting and Security

### Rate Limiting

Implement rate limiting to prevent abuse:

```php
function check_rate_limit($user_id) {
    $transient_key = 'api_rate_limit_' . $user_id;
    $requests = get_transient($transient_key);
    
    if ($requests === false) {
        set_transient($transient_key, 1, 60); // 1 minute
        return true;
    }
    
    if ($requests >= 100) { // 100 requests per minute
        return false;
    }
    
    set_transient($transient_key, $requests + 1, 60);
    return true;
}
```

### Security Best Practices

- **Validate all input**: Sanitize and validate request data
- **Use nonces**: For state-changing operations
- **HTTPS only**: Never expose APIs over HTTP
- **CORS configuration**: Properly configure CORS headers

## Data Serialization and Response Formatting

### Consistent Response Structure

```php
function format_api_response($data, $status = 200) {
    return new WP_REST_Response(array(
        'success' => $status < 400,
        'data' => $data,
        'timestamp' => current_time('mysql')
    ), $status);
}
```

### Error Handling

```php
function handle_api_error($message, $code = 400) {
    return new WP_Error(
        'api_error',
        $message,
        array('status' => $code)
    );
}
```

## Integration with Frontend Frameworks

### React Example

```javascript
// Fetch posts from WordPress REST API
const fetchPosts = async () => {
  const response = await fetch(
    'https://yoursite.com/wp-json/wp/v2/posts'
  );
  const posts = await response.json();
  return posts;
};
```

### Next.js Example

```javascript
// Server-side rendering with WordPress API
export async function getStaticProps() {
  const res = await fetch('https://yoursite.com/wp-json/wp/v2/posts');
  const posts = await res.json();
  
  return {
    props: { posts },
    revalidate: 60 // ISR: revalidate every 60 seconds
  };
}
```

## Performance Optimization

### Caching API Responses

- **Object caching**: Cache database queries
- **Response caching**: Cache full API responses
- **CDN caching**: Cache at CDN level for public endpoints

### Query Optimization

- **Limit fields**: Use `_fields` parameter to reduce payload
- **Pagination**: Implement proper pagination
- **Filtering**: Allow filtering to reduce data transfer

## Key Takeaways

- Design RESTful APIs with proper HTTP methods and status codes
- Implement robust authentication and authorization
- Use rate limiting and security best practices
- Format responses consistently
- Optimize for performance with caching and query optimization
- Test thoroughly with frontend frameworks

---

**Tags:** WordPress, REST API, Headless WordPress, API Development, React, Next.js

