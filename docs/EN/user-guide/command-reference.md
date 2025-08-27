# 📋 Command Reference

## Overview

This comprehensive command reference provides detailed information about all Enhanced TEx commands, their options, and usage examples. Each command is designed for specific functionality within the Enhanced TEx ecosystem.

## Command Categories

### 🔐 Authentication & Connection
- [`connect`](#connect) - Establish Telegram API connection

### 📊 Data Management
- [`load_groups`](#load_groups) - Load group information
- [`list_groups`](#list_groups) - List available groups
- [`download_messages`](#download_messages) - Download message history
- [`listen`](#listen) - Real-time message monitoring

### 🔍 Analysis & Intelligence
- [`analyze`](#analyze) - Analyze downloaded data
- [`scrape_user_profiles`](#scrape_user_profiles) - Extract user profiles

### 📤 Export & Reporting
- [`export_html`](#export_html) - Export to HTML format
- [`export_text`](#export_text) - Export to text format
- [`export_file`](#export_file) - Export files by MIME type
- [`report`](#report) - Generate comprehensive reports
- [`stats`](#stats) - Display statistics
- [`sent_report_telegram`](#sent_report_telegram) - Send reports via Telegram

### 🧹 Maintenance
- [`purge_old_data`](#purge_old_data) - Clean old data
- [`purge_temp_files`](#purge_temp_files) - Remove temporary files

---

## 🔐 Authentication & Connection

### `connect`

**Purpose**: Establish connection to Telegram API and store authentication credentials.

**Description**: This command creates a secure connection to Telegram servers and stores the authentication session for future use. It handles the complete authentication flow including SMS verification and two-factor authentication if enabled.

#### Options

| Option | Type | Required | Default | Description |
|--------|------|----------|---------|-------------|
| `--config` | string | ✅ Yes | None | Configuration file path containing API credentials |

#### Usage Examples

```bash
# Basic connection
python3 -m TEx connect --config config.ini

# Connection with debug output
python3 -m TEx connect --config config.ini --debug

# Force re-authentication
python3 -m TEx connect --config config.ini --force
```

#### Configuration Requirements

The `config.ini` file must contain:
```ini
[CONFIGURATION]
api_id=YOUR_API_ID
api_hash=YOUR_API_HASH
phone_number=YOUR_PHONE_NUMBER
```

#### Authentication Flow

1. **API Validation**: Validates API credentials
2. **Session Check**: Checks for existing session
3. **Authentication**: Prompts for SMS code if needed
4. **2FA**: Handles two-factor authentication if enabled
5. **Session Storage**: Saves session for future use

---

## 📊 Data Management

### `load_groups`

**Purpose**: Download and refresh group information and member lists.

**Description**: This command fetches group/channel information from Telegram and stores it in the local database. It can operate in fast-fetch mode for quick group information retrieval or full mode for complete member lists.

#### Options

| Option | Type | Required | Default | Description |
|--------|------|----------|---------|-------------|
| `--config` | string | ✅ Yes | None | Configuration file path |
| `--refresh_profile_photos` | flag | ❌ No | False | Force refresh all profile photos |
| `--fast-fetch` | flag | ❌ No | False | Fast fetch mode: only group info, skip member list |

#### Usage Examples

```bash
# Load all groups with full member lists
python3 -m TEx load_groups --config config.ini

# Fast fetch mode (group info only)
python3 -m TEx load_groups --config config.ini --fast-fetch

# Refresh with profile photos
python3 -m TEx load_groups --config config.ini --refresh_profile_photos

# Fast fetch with profile refresh
python3 -m TEx load_groups --config config.ini --fast-fetch --refresh_profile_photos
```

#### Fast Fetch Mode

When `--fast-fetch` is enabled, only the following information is retrieved:
- Group/Channel ID
- Title/Name
- Username
- Description/Bio
- Member count
- Creation date
- Access hash

This significantly reduces processing time for large groups.

### `list_groups`

**Purpose**: List all downloaded groups from database or API.

**Description**: Displays information about groups that have been loaded into the database. Can also fetch fresh data directly from the Telegram API.

#### Options

| Option | Type | Required | Default | Description |
|--------|------|----------|---------|-------------|
| `--config` | string | ✅ Yes | None | Configuration file path |
| `--list_from_api` | flag | ❌ No | False | List groups directly from Telegram API |

#### Usage Examples

```bash
# List groups from database
python3 -m TEx list_groups --config config.ini

# List groups from API (fresh data)
python3 -m TEx list_groups --config config.ini --list_from_api

# Detailed listing
python3 -m TEx list_groups --config config.ini --detailed
```

#### Output Format

```
Group ID: 123456789
Title: Example Group
Username: @example_group
Members: 1,234
Description: This is an example group description
Created: 2023-01-01 12:00:00
```

### `download_messages`

**Purpose**: Download message history from specified groups.

**Description**: This is the core command for extracting message data from Telegram groups/channels. It supports comprehensive features including media download, user profile scraping, and advanced analysis options.

#### Options

| Option | Type | Required | Default | Description |
|--------|------|----------|---------|-------------|
| `--config` | string | ✅ Yes | None | Configuration file path |
| `--group_id` | string | ❌ No | '*' | Target group IDs (comma-separated) |
| `--ignore_media` | flag | ❌ No | False | Skip media file downloads |
| `--refresh_profile_photos` | flag | ❌ No | False | Force refresh profile photos |
| `--limit` | integer | ❌ No | 10000 | Maximum messages to download |
| `--url-scraping` | flag | ❌ No | False | Enable URL extraction |
| `--forwarding-analysis` | flag | ❌ No | False | Enable forwarding analysis |
| `--reaction-analysis` | flag | ❌ No | False | Enable reaction analysis |

#### Usage Examples

```bash
# Download messages from specific group
python3 -m TEx download_messages --config config.ini --group_id 123456789

# Download with limit
python3 -m TEx download_messages --config config.ini --group_id 123456789 --limit 5000

# Skip media downloads
python3 -m TEx download_messages --config config.ini --group_id 123456789 --ignore_media

# Download with all analysis features
python3 -m TEx download_messages --config config.ini --group_id 123456789 \
  --url-scraping --forwarding-analysis --reaction-analysis

# Download from multiple groups
python3 -m TEx download_messages --config config.ini --group_id "123456789,987654321"
```

#### Analysis Features

**URL Scraping** (`--url-scraping`):
- Extracts all `t.me` links from messages
- Categorizes URLs (channel, user, bot, join link)
- Stores URL metadata and analysis

**Forwarding Analysis** (`--forwarding-analysis`):
- Tracks message forwarding patterns
- Identifies original sources
- Calculates forwarding chain length
- Builds forwarding network

**Reaction Analysis** (`--reaction-analysis`):
- Extracts message reactions
- Calculates engagement metrics
- Analyzes reaction patterns
- Determines engagement rates

### `listen`

**Purpose**: Actively monitor groups for new messages in real-time.

**Description**: Continuously listens to specified groups and processes new messages as they arrive. Supports the same analysis features as `download_messages` but operates in real-time.

#### Options

| Option | Type | Required | Default | Description |
|--------|------|----------|---------|-------------|
| `--config` | string | ✅ Yes | None | Configuration file path |
| `--group_id` | string | ❌ No | '*' | Target group IDs (comma-separated) |
| `--ignore_media` | flag | ❌ No | False | Skip media file downloads |
| `--url-scraping` | flag | ❌ No | False | Enable URL extraction |
| `--forwarding-analysis` | flag | ❌ No | False | Enable forwarding analysis |
| `--reaction-analysis` | flag | ❌ No | False | Enable reaction analysis |

#### Usage Examples

```bash
# Listen to specific group
python3 -m TEx listen --config config.ini --group_id 123456789

# Listen with analysis features
python3 -m TEx listen --config config.ini --group_id 123456789 \
  --url-scraping --forwarding-analysis --reaction-analysis

# Listen to multiple groups
python3 -m TEx listen --config config.ini --group_id "123456789,987654321"
```

#### Real-time Features

- **Instant Processing**: Messages processed as they arrive
- **Live Analysis**: Real-time URL extraction and analysis
- **Continuous Monitoring**: 24/7 group monitoring capability
- **Event Logging**: Comprehensive logging of all activities

---

## 🔍 Analysis & Intelligence

### `analyze`

**Purpose**: Analyze already downloaded data for comprehensive insights.

**Description**: This command performs offline analysis on previously downloaded data. It can generate various types of analysis including URL analysis, forwarding patterns, reaction analysis, and comprehensive visual data analysis.

#### Options

| Option | Type | Required | Default | Description |
|--------|------|----------|---------|-------------|
| `--config` | string | ✅ Yes | None | Configuration file path |
| `--group_id` | string | ❌ No | '*' | Target group IDs (comma-separated) |
| `--url-analysis` | flag | ❌ No | False | Analyze URLs from stored messages |
| `--forwarding-analysis` | flag | ❌ No | False | Analyze forwarding relationships |
| `--reaction-analysis` | flag | ❌ No | False | Analyze reactions and engagement |
| `--visual-data` | flag | ❌ No | False | Generate comprehensive visual analysis |
| `--generate-visualizations` | flag | ❌ No | False | Create professional visualizations |

#### Usage Examples

```bash
# Basic URL analysis
python3 -m TEx analyze --config config.ini --group_id 123456789 --url-analysis

# Comprehensive analysis
python3 -m TEx analyze --config config.ini --group_id 123456789 \
  --url-analysis --forwarding-analysis --reaction-analysis

# Visual data analysis
python3 -m TEx analyze --config config.ini --group_id 123456789 --visual-data

# Generate professional visualizations
python3 -m TEx analyze --config config.ini --group_id 123456789 \
  --visual-data --generate-visualizations
```

#### Analysis Types

**URL Analysis** (`--url-analysis`):
- Extracts all URLs from message text
- Categorizes URLs by type
- Analyzes URL patterns and frequency
- Generates URL distribution reports

**Forwarding Analysis** (`--forwarding-analysis`):
- Identifies forwarded messages
- Tracks original sources
- Calculates forwarding metrics
- Builds forwarding networks

**Reaction Analysis** (`--reaction-analysis`):
- Analyzes message reactions
- Calculates engagement rates
- Identifies popular content
- Tracks reaction patterns

**Visual Data Analysis** (`--visual-data`):
- User interaction networks
- Selector extraction (phones, emails, etc.)
- GPS data from media
- Message pattern analysis
- Named entity recognition
- Ideological indicator analysis
- Threat assessment

**Professional Visualizations** (`--generate-visualizations`):
- PDF charts and graphs
- PNG social network diagrams
- Interactive HTML maps
- Comprehensive PDF reports

### `scrape_user_profiles`

**Purpose**: Extract user profile information including photos and bio.

**Description**: Scrapes detailed user profile information from Telegram, including profile pictures, bio, status, and other metadata. Supports scraping by user ID, username, or from group participants.

#### Options

| Option | Type | Required | Default | Description |
|--------|------|----------|---------|-------------|
| `--config` | string | ✅ Yes | None | Configuration file path |
| `--user_ids` | string | ❌ No | None | User IDs to scrape (comma-separated) |
| `--usernames` | string | ❌ No | None | Usernames to scrape (comma-separated) |
| `--group_ids` | string | ❌ No | None | Group IDs to scrape participants from |

#### Usage Examples

```bash
# Scrape specific user by ID
python3 -m TEx scrape_user_profiles --config config.ini --user_ids 123456789

# Scrape multiple users by ID
python3 -m TEx scrape_user_profiles --config config.ini --user_ids "123456789,987654321"

# Scrape by username
python3 -m TEx scrape_user_profiles --config config.ini --usernames "john_doe,jane_smith"

# Scrape all participants from a group
python3 -m TEx scrape_user_profiles --config config.ini --group_ids 123456789

# Scrape from multiple groups
python3 -m TEx scrape_user_profiles --config config.ini --group_ids "123456789,987654321"
```

#### Profile Information Extracted

- **Basic Info**: User ID, first name, last name, username
- **Profile Data**: Bio, about, status
- **Media**: Profile picture (with MD5 duplicate detection)
- **Metadata**: Last seen, contact status, mutual contacts
- **Version Control**: Track profile changes over time

---

## 📤 Export & Reporting

### `export_html`

**Purpose**: Export messages to professional HTML format.

**Description**: Creates beautiful HTML exports of Telegram messages with preserved formatting, media, and interactive features. Uses Jinja2 templates for customizable output.

#### Options

| Option | Type | Required | Default | Description |
|--------|------|----------|---------|-------------|
| `--config` | string | ✅ Yes | None | Configuration file path |
| `--group_id` | string | ❌ No | '*' | Target group IDs (comma-separated) |
| `--output_path` | string | ❌ No | './html_export/' | Output directory for HTML files |

#### Usage Examples

```bash
# Export specific group to HTML
python3 -m TEx export_html --config config.ini --group_id 123456789

# Export with custom output path
python3 -m TEx export_html --config config.ini --group_id 123456789 \
  --output_path ./reports/html/

# Export multiple groups
python3 -m TEx export_html --config config.ini --group_id "123456789,987654321"
```

#### HTML Export Features

- **Responsive Design**: Mobile-friendly layout
- **Dark Theme**: Professional dark color scheme
- **Message Formatting**: Preserves Telegram formatting (bold, italic, code, etc.)
- **Media Support**: Embeds images, videos, and documents
- **Search Functionality**: Full-text search within exported content
- **Navigation**: Easy navigation between messages
- **Exportable**: Can be shared or archived

### `export_text`

**Purpose**: Export messages using regex filters to text format.

**Description**: Exports filtered messages to plain text format with customizable filters and ordering options.

#### Options

| Option | Type | Required | Default | Description |
|--------|------|----------|---------|-------------|
| `--config` | string | ✅ Yes | None | Configuration file path |
| `--order_desc` | flag | ❌ No | False | Sort by date/time in descending order |
| `--regex` | string | ❌ No | None | Regex filter pattern |
| `--limit_days` | integer | ❌ No | 3650 | Limit to messages within specified days |
| `--report_folder` | string | ❌ No | 'reports' | Output folder for reports |
| `--group_id` | string | ❌ No | '*' | Target group IDs (comma-separated) |

#### Usage Examples

```bash
# Export all messages
python3 -m TEx export_text --config config.ini

# Export with regex filter
python3 -m TEx export_text --config config.ini --regex "keyword"

# Export recent messages only
python3 -m TEx export_text --config config.ini --limit_days 30

# Export with custom ordering
python3 -m TEx export_text --config config.ini --order_desc
```

### `export_file`

**Purpose**: Export files by MIME type with filtering options.

**Description**: Extracts and exports files from messages based on MIME type, filename filters, and other criteria.

#### Options

| Option | Type | Required | Default | Description |
|--------|------|----------|---------|-------------|
| `--config` | string | ✅ Yes | None | Configuration file path |
| `--mime_type` | string | ❌ No | None | MIME type to export |
| `--limit_days` | integer | ❌ No | 3650 | Limit to files within specified days |
| `--report_folder` | string | ❌ No | 'reports' | Output folder for reports |
| `--group_id` | string | ❌ No | '*' | Target group IDs (comma-separated) |
| `--filter` | string | ❌ No | '*' | Filter by filename parts |

#### Usage Examples

```bash
# Export all images
python3 -m TEx export_file --config config.ini --mime_type "image/*"

# Export specific file types
python3 -m TEx export_file --config config.ini --mime_type "application/pdf"

# Export with filename filter
python3 -m TEx export_file --config config.ini --mime_type "image/*" \
  --filter "screenshot,photo"

# Export recent files only
python3 -m TEx export_file --config config.ini --mime_type "video/*" \
  --limit_days 7
```

### `report`

**Purpose**: Generate comprehensive reports with all messages and media.

**Description**: Creates detailed reports including message content, media references, and metadata with various filtering and ordering options.

#### Options

| Option | Type | Required | Default | Description |
|--------|------|----------|---------|-------------|
| `--config` | string | ✅ Yes | None | Configuration file path |
| `--order_desc` | flag | ❌ No | False | Sort by date/time in descending order |
| `--filter` | string | ❌ No | None | Filter terms |
| `--limit_days` | integer | ❌ No | 3650 | Limit messages period in days |
| `--report_folder` | string | ❌ No | 'reports' | Report output folder |
| `--around_messages` | integer | ❌ No | 1 | Messages around filtered content |
| `--group_id` | string | ❌ No | '*' | Target group IDs (comma-separated) |
| `--suppress_repeating_messages` | flag | ❌ No | False | Suppress duplicate messages |

#### Usage Examples

```bash
# Generate basic report
python3 -m TEx report --config config.ini

# Report with filter
python3 -m TEx report --config config.ini --filter "important"

# Report with context
python3 -m TEx report --config config.ini --filter "keyword" \
  --around_messages 5

# Recent messages report
python3 -m TEx report --config config.ini --limit_days 30 --order_desc
```

### `stats`

**Purpose**: Display comprehensive statistics from groups, messages, and assets.

**Description**: Generates detailed statistics about the collected data including message counts, media statistics, user activity, and more.

#### Options

| Option | Type | Required | Default | Description |
|--------|------|----------|---------|-------------|
| `--config` | string | ✅ Yes | None | Configuration file path |
| `--report_folder` | string | ❌ No | 'reports' | Report output folder |
| `--limit_days` | integer | ❌ No | 3650 | Limit statistics period in days |

#### Usage Examples

```bash
# Generate basic statistics
python3 -m TEx stats --config config.ini

# Recent statistics
python3 -m TEx stats --config config.ini --limit_days 30

# Statistics with custom output
python3 -m TEx stats --config config.ini --report_folder ./stats/
```

#### Statistics Generated

- **Message Statistics**: Total messages, messages per day, average length
- **Media Statistics**: File counts, types, sizes
- **User Statistics**: Active users, message distribution
- **Group Statistics**: Group activity, member counts
- **Time Analysis**: Peak activity times, trends

### `sent_report_telegram`

**Purpose**: Send generated reports to Telegram users via username.

**Description**: Automatically sends generated reports to specified Telegram users, useful for automated reporting and sharing.

#### Options

| Option | Type | Required | Default | Description |
|--------|------|----------|---------|-------------|
| `--config` | string | ✅ Yes | None | Configuration file path |
| `--destination_username` | string | ✅ Yes | None | Target Telegram username |
| `--report_folder` | string | ❌ No | 'reports' | Report folder to send |
| `--title` | string | ✅ Yes | 'TEx Report @@now@@' | Report title |
| `--attachment_name` | string | ✅ Yes | 'report_@@now@@' | Attachment filename |

#### Usage Examples

```bash
# Send report to user
python3 -m TEx sent_report_telegram --config config.ini \
  --destination_username john_doe

# Send with custom title
python3 -m TEx sent_report_telegram --config config.ini \
  --destination_username john_doe --title "Daily Report"

# Send specific report folder
python3 -m TEx sent_report_telegram --config config.ini \
  --destination_username john_doe --report_folder ./daily_reports/
```

---

## 🧹 Maintenance

### `purge_old_data`

**Purpose**: Clean old messages, media, and other data.

**Description**: Removes old data based on age criteria to free up storage space and maintain database performance.

#### Options

| Option | Type | Required | Default | Description |
|--------|------|----------|---------|-------------|
| `--config` | string | ✅ Yes | None | Configuration file path |
| `--limit_days` | integer | ❌ No | 365 | Age limit in days |

#### Usage Examples

```bash
# Purge data older than 1 year
python3 -m TEx purge_old_data --config config.ini

# Purge data older than 30 days
python3 -m TEx purge_old_data --config config.ini --limit_days 30

# Purge data older than 7 days
python3 -m TEx purge_old_data --config config.ini --limit_days 7
```

#### Data Types Purged

- **Messages**: Old message records
- **Media Files**: Old media files and references
- **User Data**: Inactive user profiles
- **Temporary Files**: Cache and temporary data
- **Log Files**: Old log entries

### `purge_temp_files`

**Purpose**: Force delete all temporary files.

**Description**: Removes all temporary files created during operation, useful for cleaning up after processing or troubleshooting.

#### Options

| Option | Type | Required | Default | Description |
|--------|------|----------|---------|-------------|
| `--config` | string | ✅ Yes | None | Configuration file path |

#### Usage Examples

```bash
# Purge all temporary files
python3 -m TEx purge_temp_files --config config.ini
```

#### Temporary Files Removed

- **Download Cache**: Temporary download files
- **Processing Files**: Intermediate processing files
- **Session Files**: Temporary session data
- **Log Files**: Temporary log files
- **Cache Files**: Application cache

---

## 🔧 Advanced Usage Patterns

### Batch Processing

```bash
# Process multiple groups sequentially
for group_id in 123456789 987654321 555666777; do
    python3 -m TEx download_messages --config config.ini --group_id $group_id
    python3 -m TEx analyze --config config.ini --group_id $group_id --visual-data
done
```

### Automated Reporting

```bash
# Daily automated report generation
python3 -m TEx download_messages --config config.ini --group_id 123456789
python3 -m TEx analyze --config config.ini --group_id 123456789 --visual-data
python3 -m TEx report --config config.ini --limit_days 1
python3 -m TEx sent_report_telegram --config config.ini \
  --destination_username admin --title "Daily Report"
```

### Comprehensive Analysis

```bash
# Full analysis pipeline
python3 -m TEx download_messages --config config.ini --group_id 123456789 \
  --url-scraping --forwarding-analysis --reaction-analysis
python3 -m TEx analyze --config config.ini --group_id 123456789 \
  --visual-data --generate-visualizations
python3 -m TEx export_html --config config.ini --group_id 123456789
```

---

## 📊 Command Output Examples

### Successful Connection
```
2024-01-01 12:00:00 - INFO - [+] telegram_connection_manager.TelegramConnector
2024-01-01 12:00:01 - INFO - User Authorized on Telegram: True
```

### Group Loading
```
2024-01-01 12:00:00 - INFO - [+] telegram_groups_scrapper.TelegramGroupScrapper
2024-01-01 12:00:01 - INFO - Found 5 Groups in Database
2024-01-01 12:00:02 - INFO - Processing group: Example Group (123456789)
```

### Message Download
```
2024-01-01 12:00:00 - INFO - [+] enhanced_telegram_messages_scrapper.EnhancedTelegramMessagesScrapper
2024-01-01 12:00:01 - INFO - Starting to scrape group ID: 123456789
2024-01-01 12:00:05 - INFO - Found 1,234 messages for group 123456789
2024-01-01 12:00:10 - INFO - Processed 1,234 messages for group 123456789
```

### Analysis Results
```
2024-01-01 12:00:00 - INFO - [+] enhanced_data_analyzer.EnhancedDataAnalyzer
2024-01-01 12:00:01 - INFO - Analyzing group: 123456789
2024-01-01 12:00:02 - INFO - Found 45 URLs in messages
2024-01-01 12:00:03 - INFO - Identified 12 forwarding relationships
2024-01-01 12:00:04 - INFO - Analyzed 234 reactions
```

---

## 🚨 Common Issues and Solutions

### Authentication Issues
```bash
# Clear session and re-authenticate
rm session.session
python3 -m TEx connect --config config.ini
```

### Rate Limiting
```bash
# Use smaller limits and delays
python3 -m TEx download_messages --config config.ini --group_id 123456789 --limit 1000
```

### Memory Issues
```bash
# Use ignore_media for large groups
python3 -m TEx download_messages --config config.ini --group_id 123456789 --ignore_media
```

### Database Issues
```bash
# Clean old data
python3 -m TEx purge_old_data --config config.ini --limit_days 30
```

---

## 📚 Additional Resources

- **Configuration Guide**: [Configuration Documentation](../configuration/basic.md)
- **Troubleshooting**: [Troubleshooting Guide](../troubleshooting.md)
- **API Reference**: [API Documentation](../api/modules.md)
- **Examples**: [Usage Examples](basic-usage.md)

For more detailed information about specific commands and their advanced usage, refer to the individual command documentation pages.