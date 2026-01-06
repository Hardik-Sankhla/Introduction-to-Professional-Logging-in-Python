# Advanced Topics

## Handlers

Handlers determine where log messages are sent. Common handlers include:

### StreamHandler (Console)

```python
import logging
import sys

handler = logging.StreamHandler(sys.stdout)
logger.addHandler(handler)
```

### FileHandler

```python
handler = logging.FileHandler('app.log')
logger.addHandler(handler)
```

### RotatingFileHandler

```python
from logging.handlers import RotatingFileHandler

handler = RotatingFileHandler(
    'app.log',
    maxBytes=1024*1024,  # 1MB
    backupCount=5
)
logger.addHandler(handler)
```

## Formatters

Formatters control the appearance of log messages.

```python
formatter = logging.Formatter(
    '%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)
handler.setFormatter(formatter)
```

## Filters

Filters allow you to control which log records are processed.

```python
class LevelFilter(logging.Filter):
    def __init__(self, level):
        self.level = level

    def filter(self, record):
        return record.levelno >= self.level

# Only log WARNING and above
filter = LevelFilter(logging.WARNING)
handler.addFilter(filter)
```

## Configuration Files

For complex applications, use configuration files:

### Using dictConfig

```python
import logging.config

config = {
    'version': 1,
    'formatters': {
        'detailed': {
            'format': '%(asctime)s %(name)-15s %(levelname)-8s %(message)s'
        }
    },
    'handlers': {
        'console': {
            'class': 'logging.StreamHandler',
            'level': 'INFO',
            'formatter': 'detailed'
        },
        'file': {
            'class': 'logging.FileHandler',
            'filename': 'app.log',
            'level': 'DEBUG',
            'formatter': 'detailed'
        }
    },
    'root': {
        'level': 'DEBUG',
        'handlers': ['console', 'file']
    }
}

logging.config.dictConfig(config)
```

## Logging in Multi-threaded Applications

Python's logging is thread-safe, but be careful with shared handlers:

```python
import threading
import logging

logging.basicConfig(level=logging.INFO)

def worker():
    logger = logging.getLogger(threading.current_thread().name)
    logger.info("Worker started")

threads = []
for i in range(5):
    t = threading.Thread(target=worker, name=f'Worker-{i}')
    threads.append(t)
    t.start()

for t in threads:
    t.join()
```

## Performance Considerations

- Use lazy formatting: `logger.debug('Value: %s', expensive_function())`
- Set appropriate log levels to avoid unnecessary processing
- Use asynchronous logging for high-performance applications

## Next Steps

Check out [Best Practices](best-practices.md) for professional logging guidelines.