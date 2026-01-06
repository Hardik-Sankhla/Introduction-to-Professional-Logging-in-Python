# Best Practices

## General Guidelines

### 1. Use Appropriate Log Levels

- **DEBUG**: Detailed information for developers
- **INFO**: General information about application flow
- **WARNING**: Potential problems that don't stop execution
- **ERROR**: Errors that affect functionality
- **CRITICAL**: Serious errors that may crash the application

### 2. Be Descriptive

```python
# Bad
logger.info("Error")

# Good
logger.error("Failed to connect to database: %s", str(e))
```

### 3. Use Structured Logging

```python
# Instead of
logger.info("User %s logged in from %s", user_id, ip_address)

# Consider
logger.info("User login successful", extra={
    'user_id': user_id,
    'ip_address': ip_address,
    'timestamp': datetime.now().isoformat()
})
```

### 4. Avoid Logging Sensitive Information

```python
# Bad
logger.info("User password: %s", password)

# Good
logger.info("User authentication successful for user: %s", user_id)
```

## Configuration Best Practices

### 1. Configure Logging Early

Set up logging as one of the first things in your application:

```python
import logging
import logging.config

# Configure logging first
logging.config.dictConfig(LOGGING_CONFIG)

# Then import other modules
import my_module
```

### 2. Use Hierarchical Loggers

```python
# Good structure
root_logger = logging.getLogger()
app_logger = logging.getLogger('myapp')
db_logger = logging.getLogger('myapp.database')
api_logger = logging.getLogger('myapp.api')
```

### 3. Set Log Levels Appropriately

```python
# Development
logging.getLogger().setLevel(logging.DEBUG)

# Production
logging.getLogger().setLevel(logging.INFO)
```

## Exception Handling

### 1. Log Exceptions Properly

```python
try:
    risky_operation()
except Exception as e:
    logger.error("Operation failed: %s", str(e))
    logger.debug("Exception details:", exc_info=True)
    raise
```

### 2. Use `exc_info` for Tracebacks

```python
import traceback

try:
    # some code
    pass
except Exception as e:
    logger.error("An error occurred: %s", str(e))
    logger.debug("Full traceback:", exc_info=True)
```

## Performance

### 1. Use Lazy Formatting

```python
# Good - only formats if DEBUG level is enabled
logger.debug("Expensive data: %s", expensive_computation())

# Bad - always computes even if not logged
logger.debug("Expensive data: %s" % expensive_computation())
```

### 2. Check Log Levels Before Expensive Operations

```python
if logger.isEnabledFor(logging.DEBUG):
    expensive_debug_data = compute_expensive_data()
    logger.debug("Debug data: %s", expensive_debug_data)
```

## Security

### 1. Sanitize Log Data

```python
import re

def sanitize_log_message(message):
    # Remove or mask sensitive patterns
    message = re.sub(r'password=\w+', 'password=***', message)
    return message

logger.info(sanitize_log_message(user_input))
```

### 2. Control Log File Permissions

```python
import os

# Set restrictive permissions on log files
os.chmod('app.log', 0o600)
```

## Monitoring and Alerting

### 1. Log Key Metrics

```python
logger.info("Request processed", extra={
    'request_id': request_id,
    'duration_ms': duration,
    'status_code': status_code
})
```

### 2. Use Correlation IDs

```python
import uuid

correlation_id = str(uuid.uuid4())
logger = logging.LoggerAdapter(logger, {'correlation_id': correlation_id})
```

## Testing

### 1. Test Logging Behavior

```python
import unittest
from unittest.mock import patch, MagicMock

class TestLogging(unittest.TestCase):
    def test_error_logging(self):
        with patch('logging.Logger.error') as mock_error:
            # Code that should log an error
            function_that_logs_error()
            mock_error.assert_called_once_with("Expected error message")
```

### 2. Use Log Capture for Testing

```python
import logging
from io import StringIO

def test_log_output(self):
    log_stream = StringIO()
    handler = logging.StreamHandler(log_stream)
    logger.addHandler(handler)

    # Code that logs
    my_function()

    log_output = log_stream.getvalue()
    self.assertIn("Expected message", log_output)
```

## Common Pitfalls to Avoid

1. **Over-logging**: Don't log everything - focus on important events
2. **Under-logging**: Ensure critical errors and state changes are logged
3. **Inconsistent formatting**: Use consistent log formats across your application
4. **Blocking operations**: Avoid synchronous I/O in logging that could block your application
5. **Log injection**: Sanitize user input in log messages
6. **Hardcoded paths**: Use configurable log file locations
7. **Ignoring log rotation**: Implement log rotation to prevent disk space issues

## Tools and Libraries

- **structlog**: Structured logging for Python
- **loguru**: Python logging made (stupidly) simple
- **sentry-sdk**: Application monitoring and error tracking
- **ELK Stack**: Elasticsearch, Logstash, Kibana for log analysis

## Conclusion

Effective logging is a skill that improves with practice. Start with the basics, implement proper configuration, and gradually adopt advanced techniques as your application grows. Remember that logs are not just for debugging - they're also valuable for monitoring, auditing, and understanding your application's behavior in production.