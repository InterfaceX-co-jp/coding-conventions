# PHP Coding Conventions

## Core Standards

### PSR Standards
- **PSR-12** (Extended Coding Style) - This supersedes PSR-2 and should be our primary style guide
  - [PSR-12](https://www.php-fig.org/psr/psr-12/)
  - Includes updates for new PHP features introduced after PSR-2 was created
- **PSR-1** (Basic Coding Standard) - Foundational coding standards
  - [PSR-1](https://www.php-fig.org/psr/psr-1/)
- **PSR-4** (Autoloading Standard) - For class autoloading conventions
  - [PSR-4](https://www.php-fig.org/psr/psr-4/)

### Client-Specific Standards
- Adapt to client's coding conventions when provided
- However, aligning with generally accepted coding conventions such as PSR-12 is beneficial as a development team
- Requires discussion with our client when their standards differ significantly from PSR standards

## Additional Conventions

### Naming Conventions
- **Classes**: PascalCase (e.g., `UserAuthentication`)
- **Methods/Functions**: camelCase (e.g., `getUserById()`)
- **Variables**: camelCase (e.g., `$userEmail`)
- **Constants**: UPPER_SNAKE_CASE (e.g., `MAX_LOGIN_ATTEMPTS`)
- **Private/Protected Properties**: Consider prefixing with underscore (e.g., `$_internalState`)
- **Namespaces**: PascalCase (e.g., `App\Services\Authentication`)
- **File Names**: Should match the class name including case (e.g., `UserAuthentication.php`)

### Code Structure
- One class per file
- Group related classes in namespaces
- Organize files in a logical directory structure that reflects the namespace hierarchy
- Keep methods small and focused on a single responsibility
- Limit nesting levels (max 3-4 levels deep)
- Avoid "magic numbers" - use named constants instead
- Use short array syntax `[]` instead of `array()` for better readability, consistency, and performance
  ```php
  // Preferred
  $items = ['apple', 'banana', 'orange'];
  
  // Avoid
  $items = array('apple', 'banana', 'orange');
  ```

### Documentation
- Use PHPDoc comments for classes, methods, and properties
- Include type hints in docblocks and use PHP 7.4+ typed properties where possible
- Document parameters, return types, and exceptions
- Add meaningful comments for complex logic
- Include a file header with copyright information and brief description

```php
/**
 * User authentication service
 *
 * Handles user login, registration, and session management
 *
 * @package App\Services
 */
class AuthenticationService
{
    /**
     * Attempt to authenticate a user
     *
     * @param string $email User's email address
     * @param string $password User's password (plain text)
     * @return User|null Authenticated user or null if authentication fails
     * @throws AuthenticationException If the authentication service is unavailable
     */
    public function authenticate(string $email, string $password): ?User
    {
        // Implementation
    }
}
```

### Error Handling
- Use exceptions for error handling rather than return codes
- Create custom exception classes for different error types
- Log exceptions appropriately
- Avoid silencing errors with `@` operator

### Security Practices
- Always validate and sanitize user input
- Use prepared statements for database queries
- Implement proper password hashing (using `password_hash()` and `password_verify()`)
- Follow OWASP security guidelines
- Avoid exposing sensitive information in error messages

### Performance Considerations
- Use strict comparison operators (`===`, `!==`) when possible
- Be mindful of memory usage in loops and with large datasets
- Consider caching for expensive operations
- Use appropriate data structures for the task

## Enforcement Tools

### Static Analysis
- **PHP_CodeSniffer**: For enforcing PSR-12 and custom rules
  - Include a `phpcs.xml` configuration file in projects
- **PHPStan**: For static code analysis
  - Start with level 5 and gradually increase
- **Psalm**: Alternative to PHPStan with some different capabilities

### CI Integration
- Run coding standards checks as part of CI/CD pipeline
- Fail builds that don't meet the standards
- Consider using pre-commit hooks for local enforcement

### IDE Configuration
- Configure IDEs (PhpStorm, VS Code) to highlight coding standard violations
- Share IDE configuration files in the project repository

## Version-Specific Considerations
- For PHP 7.4+: Use typed properties
- For PHP 8.0+: Use constructor property promotion, named arguments, and attributes
- For PHP 8.1+: Use enums, readonly properties, and first-class callable syntax

## Framework-Specific Guidelines
- If using Laravel, follow Laravel's conventions where they differ from PSR
- If using Symfony, follow Symfony's conventions where they differ from PSR
- Document framework-specific exceptions to the general rules

### WordPress Considerations
- Follow the [WordPress PHP Coding Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/) when working on WordPress projects
- Use WordPress naming conventions for functions and hooks (snake_case)
- Prefix functions, classes, and global variables with a project-specific prefix to avoid conflicts
- Use WordPress built-in functions for database operations instead of direct SQL queries
- Follow WordPress security best practices:
  - Validate, sanitize, and escape data (using functions like `sanitize_text_field()`, `esc_html()`, `esc_url()`)
  - Use nonces for form submissions
  - Use capability checks for user permissions
- Properly enqueue scripts and styles using WordPress functions
- Use WordPress hooks (actions and filters) instead of modifying core files

## Code Review Process
- Include coding standards compliance as part of code review checklist
- Automate what can be automated, focus manual review on logic and design
- Maintain a consistent approach to feedback on coding standards