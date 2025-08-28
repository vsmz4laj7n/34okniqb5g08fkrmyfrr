# Download Configuration

The Enhanced TEx tool now supports configurable download settings through the `[DOWNLOAD_CONFIG]` section in your `config.ini` file. This allows you to customize message downloading behavior without modifying command-line arguments.

## Configuration Section

Add the following section to your `config.ini` file:

```ini
[DOWNLOAD_CONFIG]
# Download configuration settings
# Set to None or 0 to download all messages, or specify a number to limit downloads
default_message_limit=None
# Batch size for downloading messages (recommended: 100-500)
batch_size=100
# Delay between batches in seconds (to avoid rate limiting)
batch_delay=0.1
# Delay between individual message processing in seconds
message_delay=0.05
# Progress update interval (show progress every N messages)
progress_update_interval=100
```

## Configuration Options

### `default_message_limit`
- **Type**: String/Integer
- **Default**: `None`
- **Description**: Default limit for message downloads when no `--limit` argument is provided
- **Values**:
  - `None` or `0`: Download all available messages (recommended)
  - `1000`: Download only the first 1,000 messages
  - `5000`: Download only the first 5,000 messages
  - Any positive integer: Download up to that many messages

**Example**:
```ini
# Download all messages by default
default_message_limit=None

# Download only first 1000 messages by default
default_message_limit=1000
```

### `batch_size`
- **Type**: Integer
- **Default**: `100`
- **Description**: Number of messages to fetch in each batch from Telegram API
- **Recommended Range**: 100-500
- **Impact**: 
  - Smaller batches: More API calls, slower but safer
  - Larger batches: Fewer API calls, faster but may hit rate limits

**Example**:
```ini
# Conservative approach (safer)
batch_size=100

# Faster approach (may hit rate limits)
batch_size=500
```

### `batch_delay`
- **Type**: Float (seconds)
- **Default**: `0.1`
- **Description**: Delay between batch requests to avoid Telegram rate limiting
- **Recommended Range**: 0.1-1.0 seconds
- **Impact**:
  - Lower values: Faster downloads but higher risk of rate limiting
  - Higher values: Slower downloads but safer from rate limits

**Example**:
```ini
# Fast downloads (may hit rate limits)
batch_delay=0.05

# Conservative approach (safer)
batch_delay=0.5
```

### `message_delay`
- **Type**: Float (seconds)
- **Default**: `0.05`
- **Description**: Delay between processing individual messages
- **Recommended Range**: 0.01-0.1 seconds
- **Impact**:
  - Lower values: Faster processing
  - Higher values: More time for media downloads and user processing

**Example**:
```ini
# Fast processing
message_delay=0.01

# More time for media processing
message_delay=0.1
```

### `progress_update_interval`
- **Type**: Integer
- **Default**: `100`
- **Description**: Show progress updates every N messages processed
- **Recommended Range**: 50-500
- **Impact**:
  - Lower values: More frequent progress updates
  - Higher values: Less verbose output

**Example**:
```ini
# Frequent progress updates
progress_update_interval=50

# Less verbose output
progress_update_interval=500
```

## Usage Examples

### Download All Messages (Recommended)
```ini
[DOWNLOAD_CONFIG]
default_message_limit=None
batch_size=100
batch_delay=0.1
message_delay=0.05
progress_update_interval=100
```

### Conservative Download (Safe from Rate Limits)
```ini
[DOWNLOAD_CONFIG]
default_message_limit=None
batch_size=50
batch_delay=0.5
message_delay=0.1
progress_update_interval=50
```

### Fast Download (May Hit Rate Limits)
```ini
[DOWNLOAD_CONFIG]
default_message_limit=None
batch_size=500
batch_delay=0.05
message_delay=0.01
progress_update_interval=200
```

### Limited Download (First 1000 Messages Only)
```ini
[DOWNLOAD_CONFIG]
default_message_limit=1000
batch_size=100
batch_delay=0.1
message_delay=0.05
progress_update_interval=100
```

## Command Line Override

The configuration file settings can be overridden by command-line arguments:

```bash
# Override config file limit
python3 -m TEx download_messages --config config.ini --limit 5000

# Override config file limit to download all messages
python3 -m TEx download_messages --config config.ini --limit 0
```

## Rate Limiting Considerations

Telegram has rate limits to prevent abuse. The default settings are designed to be safe, but you may need to adjust based on:

1. **Account Age**: Newer accounts have stricter limits
2. **Download Volume**: Large downloads may trigger rate limits
3. **API Usage**: Other tools using the same API credentials
4. **Network Conditions**: Slower connections may need longer delays

### Signs of Rate Limiting
- `FloodWaitError` messages
- `TooManyRequestsError` messages
- Downloads stopping unexpectedly
- Connection timeouts

### Adjusting for Rate Limits
If you encounter rate limiting, try these adjustments:

```ini
[DOWNLOAD_CONFIG]
# More conservative settings
batch_size=50
batch_delay=1.0
message_delay=0.2
```

## Performance Optimization

For optimal performance, consider your use case:

### Large Groups (100K+ messages)
```ini
[DOWNLOAD_CONFIG]
default_message_limit=None
batch_size=200
batch_delay=0.2
message_delay=0.05
progress_update_interval=500
```

### Small Groups (<10K messages)
```ini
[DOWNLOAD_CONFIG]
default_message_limit=None
batch_size=100
batch_delay=0.1
message_delay=0.05
progress_update_interval=100
```

### Media-Heavy Groups
```ini
[DOWNLOAD_CONFIG]
default_message_limit=None
batch_size=50
batch_delay=0.3
message_delay=0.2
progress_update_interval=50
```

## Troubleshooting

### Common Issues

1. **Downloads stopping at 10,000 messages**
   - Check if `default_message_limit` is set to `10000`
   - Change to `None` to download all messages

2. **Rate limiting errors**
   - Increase `batch_delay` and `message_delay`
   - Decrease `batch_size`

3. **Slow downloads**
   - Decrease `batch_delay` and `message_delay`
   - Increase `batch_size` (cautiously)

4. **No progress updates**
   - Check `progress_update_interval` value
   - Ensure it's not set too high

### Debug Mode

To see detailed batch information, enable debug logging:

```bash
python3 -m TEx download_messages --config config.ini --debug
```

This will show:
- Individual batch requests
- Offset IDs being used
- Rate limiting delays
- Detailed error messages

## Best Practices

1. **Start Conservative**: Begin with default settings and adjust as needed
2. **Monitor for Errors**: Watch for rate limiting messages
3. **Test on Small Groups**: Test your settings on smaller groups first
4. **Backup Configuration**: Keep a backup of working configurations
5. **Document Changes**: Note which settings work best for your use case

## Migration from Previous Versions

If you're upgrading from a previous version:

1. **Add the new section** to your `config.ini`
2. **Set `default_message_limit=None`** to download all messages (previous default was 10,000)
3. **Test with a small group** to ensure settings work correctly
4. **Adjust as needed** based on your specific requirements