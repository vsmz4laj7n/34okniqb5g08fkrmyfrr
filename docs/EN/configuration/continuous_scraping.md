# Continuous Scraping and Message Deduplication

The Enhanced TEx tool now supports intelligent continuous scraping that prevents reprocessing already downloaded messages and implements efficient incremental updates.

## Overview

The continuous scraping system provides three main modes:

1. **Continuous Scraping (Default)**: Only downloads new messages since the last scrape
2. **Gap Filling**: Downloads older messages to fill gaps in the database
3. **Force Rescrape**: Downloads all messages regardless of what's already in the database

## How It Works

### Message ID Tracking

The system tracks message IDs in the database to determine what has already been downloaded:

- **Minimum Message ID**: The oldest message ID in the database
- **Maximum Message ID**: The newest message ID in the database
- **Message Range**: The complete range of message IDs currently stored

### Scraping Strategies

#### 1. Continuous Scraping (Default)
- **Behavior**: Downloads only messages newer than the maximum message ID in the database
- **Use Case**: Regular updates, monitoring active groups
- **Command**: `python3 -m TEx download_messages --config config.ini`
- **Output**: 
  ```
  Found existing messages in database: ID range 1,000 - 5,000
  Continuous scraping: downloading messages newer than ID 5,000
  ```

#### 2. Gap Filling
- **Behavior**: Downloads messages older than the minimum message ID in the database
- **Use Case**: Filling historical gaps, complete archival
- **Command**: `python3 -m TEx download_messages --config config.ini --no-scrape-new-only`
- **Output**:
  ```
  Found existing messages in database: ID range 1,000 - 5,000
  Gap filling: downloading messages older than ID 1,000
  ```

#### 3. Force Rescrape
- **Behavior**: Downloads all messages regardless of what's already in the database
- **Use Case**: Database corruption, complete refresh, testing
- **Command**: `python3 -m TEx download_messages --config config.ini --force-rescrape`
- **Output**:
  ```
  Found existing messages in database: ID range 1,000 - 5,000
  Force rescrape enabled - will download all messages
  ```

## Command Line Options

### `--force-rescrape`
- **Type**: Flag (no value)
- **Default**: `False`
- **Description**: Force rescrape all messages even if already downloaded
- **Use Case**: Complete database refresh, testing, data corruption recovery

### `--scrape-new-only`
- **Type**: Flag (no value)
- **Default**: `True`
- **Description**: Only download new messages (continuous scraping mode)
- **Use Case**: Regular updates, monitoring

### `--no-scrape-new-only`
- **Type**: Flag (no value)
- **Description**: Disable new-only scraping (enables gap filling mode)
- **Use Case**: Fill historical gaps, complete archival

## Usage Examples

### Regular Daily Updates
```bash
# Download only new messages since last run
python3 -m TEx download_messages --config config.ini --group-id 123456789
```

**Output**:
```
Found existing messages in database: ID range 1,000 - 5,000
Continuous scraping: downloading messages newer than ID 5,000
Starting batch download process (batch size: 100, delay: 0.1s)...
Starting from offset_id: 5,000
Batch 1 completed: 50 new messages | Total fetched: 50/100 (50 remaining) | Skipped: 0
Successfully fetched 100 new messages for group 123456789 in 1 batches (skipped 0 already processed)
```

### Fill Historical Gaps
```bash
# Download older messages to fill gaps
python3 -m TEx download_messages --config config.ini --group-id 123456789 --no-scrape-new-only
```

**Output**:
```
Found existing messages in database: ID range 1,000 - 5,000
Gap filling: downloading messages older than ID 1,000
Starting batch download process (batch size: 100, delay: 0.1s)...
Starting from offset_id: 0
Batch 1 completed: 100 new messages | Total fetched: 100/500 (400 remaining) | Skipped: 0
Successfully fetched 500 new messages for group 123456789 in 5 batches (skipped 0 already processed)
```

### Complete Database Refresh
```bash
# Force rescrape all messages
python3 -m TEx download_messages --config config.ini --group-id 123456789 --force-rescrape
```

**Output**:
```
Found existing messages in database: ID range 1,000 - 5,000
Force rescrape enabled - will download all messages
Starting batch download process (batch size: 100, delay: 0.1s)...
Starting from offset_id: 0
Batch 1 completed: 100 new messages | Total fetched: 100/10,000 (9,900 remaining) | Skipped: 0
Successfully fetched 10,000 new messages for group 123456789 in 100 batches (skipped 0 already processed)
```

### Limited Continuous Scraping
```bash
# Download only 1000 new messages
python3 -m TEx download_messages --config config.ini --group-id 123456789 --limit 1000
```

**Output**:
```
Found existing messages in database: ID range 1,000 - 5,000
Continuous scraping: downloading messages newer than ID 5,000
Will download up to 1,000 messages (limit: 1,000, total available: 10,000)
Starting batch download process (batch size: 100, delay: 0.1s)...
Starting from offset_id: 5,000
Batch 1 completed: 100 new messages | Total fetched: 100/1,000 (900 remaining) | Skipped: 0
Successfully fetched 1,000 new messages for group 123456789 in 10 batches (skipped 0 already processed)
```

## Performance Benefits

### Efficiency Improvements

1. **No Reprocessing**: Already downloaded messages are automatically skipped
2. **Faster Updates**: Only new messages are processed
3. **Reduced API Calls**: Fewer requests to Telegram API
4. **Lower Resource Usage**: Less CPU and memory consumption
5. **Faster Completion**: Updates complete much faster than full rescrapes

### Example Performance Comparison

| Scenario | Messages to Process | Time | API Calls |
|----------|-------------------|------|-----------|
| Full Rescrape | 10,000 | 30 minutes | 100 |
| Continuous Update | 100 | 3 minutes | 1 |
| Gap Filling | 500 | 15 minutes | 5 |

## Database Integration

### Message ID Tracking

The system automatically tracks message IDs in the database:

```sql
-- Example database state
SELECT MIN(id) as min_id, MAX(id) as max_id, COUNT(*) as total_messages 
FROM telegram_message 
WHERE group_id = 123456789;

-- Result: min_id=1000, max_id=5000, total_messages=4001
```

### Automatic Deduplication

Messages are automatically deduplicated based on:
- **Message ID**: Unique identifier for each message
- **Group ID**: Ensures messages are unique per group
- **Database Constraints**: Primary key prevents duplicates

## Error Handling

### Missing Messages

If messages are missing from the database (due to errors, deletions, etc.):

1. **Gap Detection**: The system can detect gaps in message sequences
2. **Gap Filling**: Use `--no-scrape-new-only` to fill missing messages
3. **Force Rescrape**: Use `--force-rescrape` for complete recovery

### Database Corruption

If the database becomes corrupted:

```bash
# Complete database refresh
python3 -m TEx download_messages --config config.ini --group-id 123456789 --force-rescrape
```

## Best Practices

### Regular Updates

1. **Use Default Mode**: Run without flags for regular updates
2. **Schedule Regular Runs**: Set up cron jobs for automatic updates
3. **Monitor Progress**: Check logs for successful updates

### Historical Data

1. **Initial Scrape**: Use `--force-rescrape` for first-time downloads
2. **Gap Filling**: Use `--no-scrape-new-only` periodically to fill gaps
3. **Archive Maintenance**: Regular gap filling ensures complete archives

### Performance Optimization

1. **Batch Size**: Adjust `batch_size` in config for optimal performance
2. **Rate Limiting**: Use appropriate `batch_delay` to avoid API limits
3. **Resource Management**: Monitor system resources during large downloads

## Troubleshooting

### Common Issues

1. **No New Messages Downloaded**
   - Check if group is active
   - Verify message ID range in database
   - Use `--force-rescrape` to test

2. **Missing Historical Messages**
   - Use `--no-scrape-new-only` to fill gaps
   - Check for database corruption
   - Verify group access permissions

3. **Slow Performance**
   - Adjust `batch_size` and `batch_delay` in config
   - Check network connectivity
   - Monitor system resources

### Debug Information

Enable debug logging to see detailed information:

```bash
python3 -m TEx download_messages --config config.ini --group-id 123456789 --debug
```

This will show:
- Message ID ranges
- Batch processing details
- Skipped message counts
- Database operations

## Migration from Previous Versions

If you're upgrading from a previous version:

1. **Existing Databases**: The system will automatically detect existing messages
2. **First Run**: Use `--force-rescrape` for the first run to ensure consistency
3. **Regular Updates**: Switch to default mode for regular updates
4. **Monitor**: Check logs to ensure proper operation

## Advanced Usage

### Custom Offset

You can specify a custom starting point:

```bash
# Start from a specific message ID
python3 -m TEx download_messages --config config.ini --group-id 123456789 --offset-id 1000
```

### Multiple Groups

Process multiple groups with different strategies:

```bash
# Continuous update for active groups
python3 -m TEx download_messages --config config.ini --group-id 123456789,987654321

# Gap filling for archival groups
python3 -m TEx download_messages --config config.ini --group-id 111111111,222222222 --no-scrape-new-only
```

### Automated Scripts

Create automated update scripts:

```bash
#!/bin/bash
# Daily update script

# Update active groups
python3 -m TEx download_messages --config config.ini --group-id 123456789,987654321

# Fill gaps in archival groups (weekly)
if [ $(date +%u) -eq 1 ]; then
    python3 -m TEx download_messages --config config.ini --group-id 111111111,222222222 --no-scrape-new-only
fi
```