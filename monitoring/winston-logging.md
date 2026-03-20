
# Winston Logging Setup

## Core Configuration

```javascript
// logger.js
const winston = require('winston');
require('winston-daily-rotate-file');

const fileTransport = new winston.transports.DailyRotateFile({
  filename: 'logs/vigil-%DATE%.log',
  datePattern: 'YYYY-MM-DD',
  zippedArchive: true,
  maxSize: '20m',
  maxFiles: '14d'
});
```

## Structured Logging Helpers

```javascript
// utils/logger.js
const logAction = (req, action, details = {}) => {
  logger.info(JSON.stringify({
    userId: req.user?.id || 'anonymous',
    action,
    ...details
  }));
};
```

## Viewing Logs

```bash
# Live tail
tail -f logs/vigil-*.log

# Search errors
grep -r "ERROR" logs/
```

