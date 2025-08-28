# TEx Documentation - English

Welcome to the TEx (Enhanced Telegram Explorer) English documentation. This comprehensive guide covers all aspects of using TEx for Telegram data extraction, analysis, and monitoring.

## 🚀 Quick Start

### 1. Installation
```bash
# Install TEx
pip install TelegramExplorer

# Or from source
git clone https://github.com/vsmz4laj7n/TEx.git
cd TEx
pip install -r requirements.txt
pip install -e .
```

### 2. Configuration
```bash
# Copy configuration template
cp TEx/config.ini config.ini

# Edit with your Telegram API credentials
nano config.ini
```

### 3. First Run
```bash
# Connect to Telegram
python -m TEx connect --config config.ini

# Load groups
python -m TEx load_groups --config config.ini

# Download messages
python -m TEx download_messages --config config.ini --group_id 123456789

# Analyze data
python -m TEx analyze --config config.ini --group_id 123456789 --visual-data
```

## 📖 User Guide

### Core Commands

#### Connection & Setup
- **[Authentication](authentication.md)** - Setting up Telegram API credentials
- **[Installation](installation.md)** - Complete installation guide
- **[Basic Configuration](configuration/basic.md)** - Essential configuration settings

#### Data Collection
- **[Download Messages](user-guide/download_messages.md)** - Download message history from groups
- **[Real-time Monitoring](user-guide/listen.md)** - Continuous message monitoring
- **[User Profile Scraping](user-guide/user_scraping.md)** - Extract user profiles and data
- **[Group Management](user-guide/groups.md)** - Load and manage Telegram groups

#### Analysis & Intelligence
- **[Data Analysis](user-guide/analysis.md)** - Analyze downloaded data
- **[Visual Data Generation](user-guide/visual_data.md)** - Create visualizations and reports
- **[Intelligence Features](user-guide/intelligence.md)** - PII extraction and threat assessment
- **[Network Analysis](user-guide/networks.md)** - User interaction networks

#### Export & Reporting
- **[Report Generation](user-guide/reports.md)** - Generate comprehensive reports
- **[Text Export](user-guide/export_text.md)** - Export filtered text data
- **[File Export](user-guide/export_file.md)** - Export media files
- **[HTML Export](user-guide/export_html.md)** - Export to HTML format
- **[Gephi Export](user-guide/gephi_export.md)** - Export network data for analysis

#### Maintenance
- **[Database Management](user-guide/database.md)** - Database operations and maintenance
- **[Data Cleanup](user-guide/cleanup.md)** - Purge old data and files
- **[Troubleshooting](user-guide/troubleshooting.md)** - Common issues and solutions

## 🔧 Configuration

### Configuration Files
- **[Basic Configuration](configuration/basic.md)** - Essential settings for getting started
- **[Complete Configuration Example](configuration/complete_configuration_file_example.md)** - Full config.ini reference
- **[Media Download Configuration](configuration/media_download_configuration.md)** - Media handling settings
- **[Analysis Configuration](configuration/analysis_configuration.md)** - Analysis and visualization settings

### Configuration Sections

#### Core Configuration
```ini
[CONFIGURATION]
api_id=YOUR_API_ID_HERE
api_hash=YOUR_API_HASH_HERE
phone_number=YOUR_PHONE_NUMBER_HERE
data_path=./data/
device_model=Desktop
timeout=30
```

#### Download Configuration
```ini
[DOWNLOAD_CONFIG]
default_message_limit=None
batch_size=100
batch_delay=0.1
message_delay=0.05
progress_update_interval=100
fast_counting_enabled=True
status_check_enabled=True
status_check_batch_size=1000
```

#### Analysis Configuration
```ini
[SELECTOR_PATTERNS]
phone_pattern=\b(?:\+?1[-.]?)?\(?([0-9]{3})\)?[-.]?([0-9]{3})[-.]?([0-9]{4})\b
email_pattern=\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b
url_pattern=https?://(?:[-\w.])+(?:[:\d]+)?(?:/(?:[\w/_.])*(?:\?(?:[\w&=%.])*)?(?:#(?:[\w.])*)?)?
ip_pattern=\b(?:[0-9]{1,3}\.){3}[0-9]{1,3}\b
crypto_pattern=\b[13][a-km-zA-HJ-NP-Z1-9]{25,34}\b
```

## 🔍 Advanced Features

### Analysis Modules

#### URL Analysis
- Extract and categorize t.me links and other URLs
- Track URL propagation across groups
- Analyze link sharing patterns

#### Forwarding Analysis
- Track message propagation networks
- Identify original sources and spread patterns
- Analyze forwarding behavior and timing

#### Reaction Analysis
- Analyze user engagement through reactions
- Identify popular content and users
- Track reaction trends over time

#### Selector Extraction
- Extract phone numbers, emails, IPs
- Identify cryptocurrency addresses
- Extract URLs and social media links

#### GPS Data Extraction
- Extract location data from image EXIF metadata
- Create interactive maps with user locations
- Generate location-based analysis reports

#### Named Entity Recognition
- Identify person names and aliases
- Extract organization names
- Detect geographic locations and dates

### Intelligence Features

#### Ideological Analysis
- Detect hate speech and extremist terminology
- Identify conspiracy theory indicators
- Analyze sovereign citizen terminology
- Multi-language support (English and Vietnamese)

#### Threat Assessment
- Identify capability indicators
- Detect violent intent and threats
- Analyze proximity-based threat patterns
- Risk scoring and prioritization

#### User Interaction Networks
- Map user relationships and interactions
- Identify key influencers and communities
- Analyze community structure and dynamics
- Export to Gephi-compatible formats

#### Message Pattern Analysis
- Analyze posting patterns and frequency
- Track user activity over time
- Identify behavioral patterns and trends
- Generate activity heatmaps

## 📊 Output Formats

### Visual Data Generation
- **PDF Charts**: Statistical charts and graphs
- **PNG Social Graphs**: User interaction networks
- **HTML Maps**: Interactive maps with GPS data
- **PDF Reports**: Comprehensive analysis reports
- **JSON Data**: Structured data for further processing

### Gephi Export Formats
- **GEXF**: Graph Exchange XML Format
- **GraphML**: Graph Markup Language
- **CSV**: Comma-separated values (nodes and edges)

### Report Formats
- **HTML**: Interactive web reports with embedded visualizations
- **Text**: Plain text exports with filtering options
- **CSV**: Structured data exports for analysis
- **JSON**: Machine-readable data for automation

## 🏗️ Architecture

### Pipeline System
TEx uses a modular pipeline system with the following stages:

1. **Pre-Pipeline**: Initialization, configuration, database setup
2. **Core Pipeline**: Connection, group loading, basic functionality
3. **Command-Specific Pipelines**:
   - **Download Pipeline**: Message and media downloading
   - **Listen Pipeline**: Real-time monitoring
   - **Analysis Pipeline**: Data analysis and visualization
   - **Report Pipeline**: Report generation and export
4. **Post-Pipeline**: Cleanup and resource management

### Database Structure
- **Group-specific databases**: `message.db`, `user.db` for each group
- **Global user databases**: `user_index.db`, `user_profiles.db`
- **Enhanced schema**: User flags, message metadata, interaction tracking
- **WAL mode**: Real-time database updates with improved concurrency

### Module System
- **Core modules**: Connection, database, configuration management
- **Scraping modules**: Message, user, media extraction
- **Analysis modules**: URL, forwarding, reaction, visual data analysis
- **Export modules**: Report generation, file export, data formatting

## 🔒 Security & Privacy

### Data Protection
- **Local storage**: All data stored locally on your machine
- **No cloud uploads**: No data is sent to external servers
- **Configurable retention**: Control data retention periods
- **Secure authentication**: Telegram API authentication only

### Privacy Features
- **User consent**: Respects Telegram's terms of service
- **Rate limiting**: Built-in rate limiting to avoid API restrictions
- **Error handling**: Graceful handling of access restrictions
- **Logging control**: Configurable logging levels

## 🐛 Troubleshooting

### Common Issues

#### Authentication Problems
```bash
# Clear session and re-authenticate
rm -rf data/session/
python -m TEx connect --config config.ini
```

#### Database Issues
```bash
# Fix database inconsistencies
python -m TEx fix_database --config config.ini
```

#### Memory Issues
```bash
# Reduce batch size in config.ini
batch_size=50
```

#### Rate Limiting
```bash
# Increase delays in config.ini
batch_delay=0.5
message_delay=0.1
```

### Performance Optimization
- **Batch processing**: Configure appropriate batch sizes
- **Memory management**: Monitor memory usage for large datasets
- **Database optimization**: Use SSD storage for better performance
- **Network optimization**: Configure appropriate delays and timeouts

## 📋 Command Reference

### Available Commands

| Command | Description | Usage |
|---------|-------------|-------|
| `connect` | Create Telegram connection and store authentication | `python -m TEx connect --config config.ini` |
| `load_groups` | Download and refresh groups and members list | `python -m TEx load_groups --config config.ini` |
| `download_messages` | Download message history | `python -m TEx download_messages --config config.ini --group_id 123456789` |
| `listen` | Actively listen to all chats (continuous monitoring) | `python -m TEx listen --config config.ini --group_id 123456789` |
| `analyze` | Analyze downloaded data | `python -m TEx analyze --config config.ini --group_id 123456789 --visual-data` |
| `report` | Generate reports with messages and media | `python -m TEx report --config config.ini --group_id 123456789` |
| `export_text` | Export messages using regex filters | `python -m TEx export_text --config config.ini --group_id 123456789` |
| `export_file` | Export files by mime type | `python -m TEx export_file --config config.ini --group_id 123456789` |
| `export_html` | Export messages to HTML format | `python -m TEx export_html --config config.ini --group_id 123456789` |
| `scrape_user_profiles` | Scrape user profiles with photos and bio | `python -m TEx scrape_user_profiles --config config.ini --user_ids 123456789` |
| `user_scrape` | Scrape user profiles with real-time database updates | `python -m TEx user_scrape --config config.ini --group-ids 123456789` |
| `list_groups` | List all downloaded groups | `python -m TEx list_groups --config config.ini` |
| `stats` | Show statistics from phone number groups | `python -m TEx stats --config config.ini` |
| `purge_old_data` | Purge old messages and media | `python -m TEx purge_old_data --config config.ini` |
| `fix_database` | Fix database inconsistencies and sync user data | `python -m TEx fix_database --config config.ini` |

### Command Options

#### Download Messages Options
```bash
# Basic download
python -m TEx download_messages --config config.ini --group_id 123456789

# Download with options
python -m TEx download_messages \
  --config config.ini \
  --group_id 123456789 \
  --limit 10000 \
  --ignore_media \
  --latest \
  --force-rescrape
```

#### Analysis Options
```bash
# Comprehensive analysis
python -m TEx analyze \
  --config config.ini \
  --group_id 123456789 \
  --visual-data \
  --generate-visualizations \
  --url-analysis \
  --forwarding-analysis \
  --reaction-analysis \
  --gephi-format GEXF
```

#### Export Options
```bash
# Export with filters
python -m TEx export_text \
  --config config.ini \
  --group_id 123456789 \
  --filter "keyword" \
  --limit_days 30 \
  --desc

# Export specific file types
python -m TEx export_file \
  --config config.ini \
  --group_id 123456789 \
  --mimetype "image/jpeg" \
  --limit_days 7
```

## 📈 Examples & Tutorials

### Basic Tutorials
- **[First Analysis](tutorials/first_analysis.md)** - Complete first analysis workflow
- **[Real-time Monitoring](tutorials/monitoring.md)** - Setting up continuous monitoring
- **[User Analysis](tutorials/user_analysis.md)** - Analyzing user behavior
- **[Network Analysis](tutorials/network_analysis.md)** - Creating interaction networks

### Advanced Tutorials
- **[Cross-group Analysis](tutorials/cross_group.md)** - Analyzing multiple groups
- **[Threat Assessment](tutorials/threat_assessment.md)** - Conducting threat analysis
- **[Custom Analysis](tutorials/custom_analysis.md)** - Creating custom analysis workflows
- **[Report Automation](tutorials/automation.md)** - Automating report generation

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](../CONTRIBUTING.md) for details.

### Development Setup
```bash
# Clone repository
git clone https://github.com/vsmz4laj7n/TEx.git
cd TEx

# Install development dependencies
poetry install

# Run tests
poetry run pytest

# Run linting
poetry run ruff check .
```

### Code Style
- **Python**: Follow PEP 8 guidelines
- **Type hints**: Use type annotations
- **Documentation**: Docstrings for all functions
- **Testing**: Unit tests for new features

## 📞 Support

- **Documentation**: [https://enhanced-tex.readthedocs.io/](https://enhanced-tex.readthedocs.io/)
- **Issues**: [GitHub Issues](https://github.com/vsmz4laj7n/TEx/issues)
- **Discussions**: [GitHub Discussions](https://github.com/vsmz4laj7n/TEx/discussions)

## 📄 License

This project is licensed under the Apache License 2.0 - see the [LICENSE](../../LICENSE) file for details.

---

**Enhanced TEx Team** - Building the future of Telegram data analysis and intelligence gathering.