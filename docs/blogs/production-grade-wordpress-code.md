# <i class="fas fa-shield-alt"></i> Writing Production-Grade WordPress Code: Standards and Best Practices

**Published:** August 2024  
**Category:** Code Quality, WordPress Best Practices

Writing code that works is one thing; writing code that's maintainable, secure, and scalable is another. This post outlines the coding standards and best practices I follow when building production WordPress plugins and themes.

## Why Coding Standards Matter

Coding standards ensure:
- **Consistency**: Code looks the same across the project
- **Readability**: Easier for team members to understand
- **Maintainability**: Easier to update and fix bugs
- **Security**: Reduces vulnerabilities
- **Performance**: Optimized code from the start

## WordPress Coding Standards

### PHP Coding Standards

WordPress follows specific PHP coding standards:

```php
<?php
/**
 * Plugin Name: My Plugin
 * Description: A well-structured WordPress plugin
 * Version: 1.0.0
 */

// Use spaces, not tabs (indent with 4 spaces)
if ( condition ) {
    do_something();
}

// Use single quotes for strings when possible
$message = 'Hello World';

// Use meaningful variable names
$user_id = get_current_user_id();
$post_title = get_the_title();
```

### Naming Conventions

- **Functions**: Use lowercase with underscores (`my_function_name`)
- **Classes**: Use PascalCase (`My_Plugin_Class`)
- **Constants**: Use uppercase with underscores (`MY_CONSTANT`)
- **Variables**: Use lowercase with underscores (`$my_variable`)

## Security Best Practices

### Data Sanitization

Always sanitize user input:

```php
// Sanitize text input
$user_input = sanitize_text_field( $_POST['user_input'] );

// Sanitize email
$email = sanitize_email( $_POST['email'] );

// Sanitize URL
$url = esc_url_raw( $_POST['url'] );

// Sanitize for database
$safe_value = sanitize_text_field( wp_unslash( $_POST['value'] ) );
```

### Data Validation

Validate data before using it:

```php
// Validate email
if ( ! is_email( $email ) ) {
    return new WP_Error( 'invalid_email', 'Invalid email address' );
}

// Validate nonce
if ( ! wp_verify_nonce( $_POST['nonce'], 'my_action' ) ) {
    wp_die( 'Security check failed' );
}

// Validate capabilities
if ( ! current_user_can( 'edit_posts' ) ) {
    return;
}
```

### SQL Injection Prevention

Always use prepared statements:

```php
// ❌ Bad: Direct SQL
$wpdb->query( "SELECT * FROM {$wpdb->posts} WHERE ID = $post_id" );

// ✅ Good: Prepared statement
$wpdb->prepare(
    "SELECT * FROM {$wpdb->posts} WHERE ID = %d",
    $post_id
);

// ✅ Better: Use WordPress functions
get_post( $post_id );
```

### Nonces for Forms

Always use nonces for form submissions:

```php
// Generate nonce
wp_nonce_field( 'my_action', 'my_nonce' );

// Verify nonce
if ( ! isset( $_POST['my_nonce'] ) || 
     ! wp_verify_nonce( $_POST['my_nonce'], 'my_action' ) ) {
    wp_die( 'Security check failed' );
}
```

## Code Organization and Structure

### Plugin Structure

```
my-plugin/
├── my-plugin.php          # Main plugin file
├── includes/
│   ├── class-plugin-core.php
│   ├── class-database.php
│   └── class-api.php
├── admin/
│   ├── class-admin.php
│   └── css/
│       └── admin.css
├── public/
│   ├── class-public.php
│   └── js/
│       └── public.js
├── languages/
│   └── my-plugin.pot
└── readme.txt
```

### Class Organization

```php
<?php
/**
 * Plugin Core Class
 *
 * @package My_Plugin
 */

if ( ! defined( 'ABSPATH' ) ) {
    exit; // Exit if accessed directly
}

class My_Plugin_Core {
    
    /**
     * Plugin version
     */
    const VERSION = '1.0.0';
    
    /**
     * Instance
     */
    private static $instance = null;
    
    /**
     * Get instance
     */
    public static function get_instance() {
        if ( null === self::$instance ) {
            self::$instance = new self();
        }
        return self::$instance;
    }
    
    /**
     * Constructor
     */
    private function __construct() {
        $this->init_hooks();
    }
    
    /**
     * Initialize hooks
     */
    private function init_hooks() {
        add_action( 'init', array( $this, 'init' ) );
    }
    
    /**
     * Initialize plugin
     */
    public function init() {
        // Plugin initialization
    }
}
```

## Testing Strategies

### Unit Testing

```php
class Test_My_Plugin extends WP_UnitTestCase {
    
    public function test_function_exists() {
        $this->assertTrue( function_exists( 'my_plugin_function' ) );
    }
    
    public function test_function_output() {
        $result = my_plugin_function();
        $this->assertNotEmpty( $result );
    }
}
```

### Integration Testing

Test how your code integrates with WordPress:

```php
public function test_post_creation() {
    $post_id = $this->factory->post->create();
    $this->assertGreaterThan( 0, $post_id );
}
```

## Documentation

### Inline Documentation

```php
/**
 * Get user posts
 *
 * @param int $user_id User ID
 * @param int $limit   Number of posts to retrieve
 * @return WP_Post[] Array of post objects
 */
function get_user_posts( $user_id, $limit = 10 ) {
    // Function implementation
}
```

### README Documentation

Always include:
- Installation instructions
- Configuration options
- Usage examples
- Changelog
- Support information

## Version Control Best Practices

### Commit Messages

Use clear, descriptive commit messages:

```
feat: Add user authentication functionality
fix: Resolve SQL injection vulnerability
docs: Update installation instructions
refactor: Improve code organization
```

### Git Workflow

- Use feature branches
- Write meaningful commit messages
- Review code before merging
- Tag releases properly

## Coding Standards Checklist

- [ ] Follow WordPress PHP coding standards
- [ ] Use proper naming conventions
- [ ] Sanitize all user input
- [ ] Validate all data
- [ ] Use prepared statements for database queries
- [ ] Include nonces for forms
- [ ] Check user capabilities
- [ ] Escape output properly
- [ ] Organize code logically
- [ ] Write inline documentation
- [ ] Write unit tests
- [ ] Follow version control best practices

## Security Audit Checklist

- [ ] All user input sanitized
- [ ] All output escaped
- [ ] Nonces used for forms
- [ ] Capabilities checked
- [ ] Prepared statements used
- [ ] No direct file access
- [ ] Secure file uploads
- [ ] HTTPS enforced where needed
- [ ] Secrets not hardcoded
- [ ] Regular security updates

## Tools and Resources

### Code Quality Tools

- **PHP_CodeSniffer**: Enforce coding standards
- **PHPStan**: Static analysis
- **WordPress Coding Standards**: WPCS rules
- **PHPUnit**: Unit testing framework

### Resources

- [WordPress Coding Standards](https://developer.wordpress.org/coding-standards/)
- [WordPress Plugin Handbook](https://developer.wordpress.org/plugins/)
- [WordPress Security](https://wordpress.org/support/article/hardening-wordpress/)

## Key Takeaways

- Follow WordPress coding standards consistently
- Always sanitize input and escape output
- Use prepared statements for database queries
- Organize code logically and document it
- Write tests for your code
- Use version control properly
- Regular security audits are essential
- Keep learning and improving

---

**Tags:** WordPress, Code Quality, Security, Best Practices, Coding Standards

