# Basic Logging

## Log Levels

Python's logging module defines several log levels:

| Level | Numeric Value | Description |
|-------|---------------|-------------|
| DEBUG | 10 | Detailed information for debugging |
| INFO | 20 | General information about program execution |
| WARNING | 30 | Indication of potential problems |
| ERROR | 40 | Errors that don't stop the program |
| CRITICAL | 50 | Serious errors that may stop the program |

## Configuring Logging

### Basic Configuration

```python
import logging

logging.basicConfig(
    level=logging.DEBUG,
    format='%(levelname)s:%(name)s:%(message)s'
)
```

### Advanced Configuration

```python
import logging

logging.basicConfig(
    filename='app.log',
    filemode='w',
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    datefmt='%Y-%m-%d %H:%M:%S'
)
```

## Creating Loggers

```python
# Get logger for current module
logger = logging.getLogger(__name__)

# Get logger with specific name
logger = logging.getLogger('my_app.database')
```

## Logging Messages

```python
logger.debug('Debug message')
logger.info('Info message')
logger.warning('Warning message')
logger.error('Error message')
logger.critical('Critical message')
```

## Formatting

### Format String Parameters

- `%(asctime)s` - Timestamp
- `%(name)s` - Logger name
- `%(levelname)s` - Log level
- `%(message)s` - Log message
- `%(filename)s` - Filename
- `%(lineno)d` - Line number
- `%(funcName)s` - Function name

### Example Format

```python
'%(asctime)s - %(name)s - %(levelname)s - %(message)s'
```

Output: `2023-12-01 10:30:45 - my_module - INFO - Application started`

## Next Steps

Learn about [Advanced Topics](advanced-topics.md) like handlers, formatters, and filters.