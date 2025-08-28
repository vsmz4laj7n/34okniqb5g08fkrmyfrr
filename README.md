# GG - GhostGram

[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/downloads/)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![PyPI version](https://badge.fury.io/py/GhostGram.svg)](https://badge.fury.io/py/GhostGram)
[![Documentation](https://readthedocs.org/projects/ghostgram/badge/?version=latest)](https://ghostgram.readthedocs.io/)

**GhostGram (GG)** is a comprehensive Python-based tool for extracting, analyzing, and monitoring Telegram data. Built with modern Python 3.12+ and advanced libraries, GhostGram provides powerful capabilities for OSINT, data analysis, and intelligence gathering from Telegram channels and groups.

## 🚀 Key Features

### 📊 **Data Extraction & Monitoring**
- **Message Download**: Download complete message history from Telegram channels and groups
- **Real-time Monitoring**: Continuous listening to live messages and updates
- **Media Download**: Automatic download of images, videos, documents, and other media files
- **User Profile Scraping**: Extract user profiles, photos, and biographical information
- **Group Management**: Load, list, and manage Telegram groups and channels

### 🔍 **Advanced Analysis**
- **URL Analysis**: Extract and categorize t.me links and other URLs from messages
- **Forwarding Analysis**: Track message propagation networks and forwarding chains
- **Reaction Analysis**: Analyze user engagement through reactions and interactions
- **Visual Data Generation**: Create comprehensive visualizations and network graphs
- **Selector Extraction**: Extract phone numbers, emails, IPs, crypto addresses, and other PII
- **GPS Data Extraction**: Extract location data from image EXIF metadata
- **Named Entity Recognition**: Identify persons, organizations, locations, and dates

### 🎯 **Intelligence Features**
- **Ideological Analysis**: Detect hate speech, extremism, conspiracy theories, and other ideological indicators
- **Threat Assessment**: Identify capability indicators, violent intent, and security threats
- **User Interaction Networks**: Map user relationships and interaction patterns
- **Message Pattern Analysis**: Analyze posting patterns, frequency, and activity trends
- **Cross-group Analysis**: Compare data across multiple channels and groups

### 📈 **Reporting & Export**
- **HTML Reports**: Generate comprehensive HTML reports with embedded visualizations
- **Text Exports**: Export filtered data in various text formats
- **File Exports**: Export media files by type and content
- **Gephi Integration**: Export network data in GEXF, GraphML, and CSV formats
- **Statistics Generation**: Generate detailed statistics and analytics reports
- **Telegram Integration**: Send reports directly to Telegram users

### 🛠️ **Technical Features**
- **Modular Architecture**: Plugin-based system with extensible modules
- **Database Management**: SQLite-based storage with WAL mode for real-time updates
- **Configuration Management**: Comprehensive configuration system with validation
- **Error Handling**: Robust error handling and recovery mechanisms
- **Performance Optimization**: Fast message counting, batch processing, and caching
- **Multi-language Support**: English and Vietnamese keyword detection

## 📋 Requirements

### **System Requirements**
- **Python**: 3.12 or higher
- **Operating System**: Linux, macOS, Windows
- **Memory**: Minimum 4GB RAM (8GB+ recommended for large datasets)
- **Storage**: SSD recommended for better database performance
- **Network**: Stable internet connection for Telegram API access

### **Dependencies**
- **Telethon**: Telegram client library
- **SQLAlchemy**: Database ORM
- **Jinja2**: Template engine for reports
- **Matplotlib**: Data visualization
- **NetworkX**: Network analysis and graph generation
- **Pandas**: Data manipulation and analysis
- **ReportLab**: PDF generation
- **Folium**: Interactive map generation
- **Pillow**: Image processing and EXIF extraction

## 🛠️ Installation

### **Method 1: Using pip (Recommended)**
```bash
pip install GhostGram
```

### **Method 2: From Source**
```bash
# Clone the repository
git clone https://github.com/vsmz4laj7n/GhostGram.git
cd GhostGram

# Install dependencies
pip install -r requirements.txt

# Install the package
pip install -e .
```

### **Method 3: Using Poetry**
```bash
# Clone the repository
git clone https://github.com/vsmz4laj7n/GhostGram.git
cd GhostGram

# Install using Poetry
poetry install
poetry run python -m GG --help
```

## ⚙️ Configuration

### **1. Get Telegram API Credentials**
1. Visit [https://my.telegram.org/apps](https://my.telegram.org/apps)
2. Create a new application
3. Note your `api_id` and `api_hash`

### **2. Create Configuration File**
Copy the example configuration:
```bash
cp TEx/config.ini config.ini
```

### **3. Update Configuration**
Edit `config.ini` with your credentials:
```ini
[CONFIGURATION]
api_id=YOUR_API_ID_HERE
api_hash=YOUR_API_HASH_HERE
phone_number=YOUR_PHONE_NUMBER_HERE
data_path=./data/
device_model=Desktop
timeout=30
```

## 🚀 Quick Start

### **1. First-time Setup**
```bash
# Connect to Telegram and authenticate
python -m GG connect --config config.ini
```

### **2. Load Groups**
```bash
# Download and refresh groups list
python -m GG load_groups --config config.ini
```

### **3. Download Messages**
```bash
# Download messages from specific group
python -m GG download_messages --config config.ini --group_id 123456789

# Download with media
python -m GG download_messages --config config.ini --group_id 123456789

# Download only latest messages
python -m GG download_messages --config config.ini --group_id 123456789 --latest
```

### **4. Analyze Data**
```bash
# Run comprehensive analysis
python -m GG analyze --config config.ini --group_id 123456789 --visual-data

# Generate visualizations
python -m GG analyze --config config.ini --group_id 123456789 --generate-visualizations

# Export to Gephi format
python -m GG analyze --config config.ini --group_id 123456789 --visual-data --gephi-format GEXF
```

### **5. Monitor Live**
```bash
# Start continuous monitoring
python -m GG listen --config config.ini --group_id 123456789
```

## 📖 Usage Guide

### **Available Commands**

| Command | Description | Usage |
|---------|-------------|-------|
| `connect` | Create Telegram connection and store authentication | `python -m GG connect --config config.ini` |
| `load_groups` | Download and refresh groups and members list | `python -m GG load_groups --config config.ini` |
| `download_messages` | Download message history | `python -m GG download_messages --config config.ini --group_id 123456789` |
| `listen` | Actively listen to all chats (continuous monitoring) | `python -m GG listen --config config.ini --group_id 123456789` |
| `analyze` | Analyze downloaded data | `python -m GG analyze --config config.ini --group_id 123456789 --visual-data` |
| `report` | Generate reports with messages and media | `python -m GG report --config config.ini --group_id 123456789` |
| `export_text` | Export messages using regex filters | `python -m GG export_text --config config.ini --group_id 123456789` |
| `export_file` | Export files by mime type | `python -m GG export_file --config config.ini --group_id 123456789` |
| `export_html` | Export messages to HTML format | `python -m GG export_html --config config.ini --group_id 123456789` |
| `scrape_user_profiles` | Scrape user profiles with photos and bio | `python -m GG scrape_user_profiles --config config.ini --user_ids 123456789` |
| `user_scrape` | Scrape user profiles with real-time database updates | `python -m GG user_scrape --config config.ini --group-ids 123456789` |
| `list_groups` | List all downloaded groups | `python -m GG list_groups --config config.ini` |
| `stats` | Show statistics from phone number groups | `python -m GG stats --config config.ini` |
| `purge_old_data` | Purge old messages and media | `python -m GG purge_old_data --config config.ini` |
| `fix_database` | Fix database inconsistencies and sync user data | `python -m GG fix_database --config config.ini` |

### **Download Messages Options**
```bash
# Basic download
python -m GG download_messages --config config.ini --group_id 123456789

# Download with options
python -m GG download_messages \
  --config config.ini \
  --group_id 123456789 \
  --limit 10000 \
  --ignore_media \
  --latest \
  --force-rescrape
```

### **Analysis Options**
```bash
# Comprehensive analysis
python -m GG analyze \
  --config config.ini \
  --group_id 123456789 \
  --visual-data \
  --generate-visualizations \
  --url-analysis \
  --forwarding-analysis \
  --reaction-analysis \
  --gephi-format GEXF
```

### **Export Options**
```bash
# Export with filters
python -m GG export_text \
  --config config.ini \
  --group_id 123456789 \
  --filter "keyword" \
  --limit_days 30 \
  --desc

# Export specific file types
python -m GG export_file \
  --config config.ini \
  --group_id 123456789 \
  --mimetype "image/jpeg" \
  --limit_days 7
```

## 🔧 Configuration Options

### **Download Configuration**
```ini
[DOWNLOAD_CONFIG]
# Message limit (None for all messages)
default_message_limit=None
# Batch size for downloading
batch_size=100
# Delay between batches (seconds)
batch_delay=0.1
# Delay between messages (seconds)
message_delay=0.05
# Progress update interval
progress_update_interval=100
# Enable fast message counting
fast_counting_enabled=True
# Enable status change checking
status_check_enabled=True
# Status check batch size
status_check_batch_size=1000
```

### **Analysis Configuration**
```ini
[SELECTOR_PATTERNS]
# Regex patterns for extracting selectors
phone_pattern=\b(?:\+?1[-.]?)?\(?([0-9]{3})\)?[-.]?([0-9]{3})[-.]?([0-9]{4})\b
email_pattern=\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b
url_pattern=https?://(?:[-\w.])+(?:[:\d]+)?(?:/(?:[\w/_.])*(?:\?(?:[\w&=%.])*)?(?:#(?:[\w.])*)?)?
ip_pattern=\b(?:[0-9]{1,3}\.){3}[0-9]{1,3}\b
crypto_pattern=\b[13][a-km-zA-HJ-NP-Z1-9]{25,34}\b

[IDEOLOGICAL_KEYWORDS]
# Keywords for ideological analysis
hate_speech=hate,racist,bigot,nazi,supremacist,white nationalist,anti-semitic
extremism=extremist,radical,terrorist,jihad,white power,anarchist,revolutionary
conspiracy=conspiracy,deep state,illuminati,chemtrails,flat earth,new world order

[THREAT_KEYWORDS]
# Keywords for threat assessment
capability=weapon,gun,bomb,explosive,knife,ammo,ammunition,firearm
violent_intent=kill,murder,attack,shoot,bomb,terror,assassinate,eliminate
weapons=rifle,pistol,shotgun,ammunition,explosive,knife,grenade,molotov
```

## 📊 Output Formats

### **Visual Data Generation**
- **PDF Charts**: Statistical charts and graphs
- **PNG Social Graphs**: User interaction networks
- **HTML Maps**: Interactive maps with GPS data
- **PDF Reports**: Comprehensive analysis reports
- **JSON Data**: Structured data for further processing

### **Gephi Export Formats**
- **GEXF**: Graph Exchange XML Format
- **GraphML**: Graph Markup Language
- **CSV**: Comma-separated values (nodes and edges)

### **Report Formats**
- **HTML**: Interactive web reports
- **Text**: Plain text exports
- **CSV**: Structured data exports
- **JSON**: Machine-readable data

## 🏗️ Architecture

### **Pipeline System**
TEx uses a modular pipeline system with the following stages:

1. **Pre-Pipeline**: Initialization, configuration, database setup
2. **Core Pipeline**: Connection, group loading, basic functionality
3. **Command-Specific Pipelines**:
   - **Download Pipeline**: Message and media downloading
   - **Listen Pipeline**: Real-time monitoring
   - **Analysis Pipeline**: Data analysis and visualization
   - **Report Pipeline**: Report generation and export
4. **Post-Pipeline**: Cleanup and resource management

### **Database Structure**
- **Group-specific databases**: `message.db`, `user.db` for each group
- **Global user databases**: `user_index.db`, `user_profiles.db`
- **Enhanced schema**: User flags, message metadata, interaction tracking

### **Module System**
- **Core modules**: Connection, database, configuration management
- **Scraping modules**: Message, user, media extraction
- **Analysis modules**: URL, forwarding, reaction, visual data analysis
- **Export modules**: Report generation, file export, data formatting

## 🔒 Security & Privacy

### **Data Protection**
- **Local storage**: All data stored locally on your machine
- **No cloud uploads**: No data is sent to external servers
- **Configurable retention**: Control data retention periods
- **Secure authentication**: Telegram API authentication only

### **Privacy Features**
- **User consent**: Respects Telegram's terms of service
- **Rate limiting**: Built-in rate limiting to avoid API restrictions
- **Error handling**: Graceful handling of access restrictions
- **Logging control**: Configurable logging levels

## 🐛 Troubleshooting

### **Common Issues**

**Authentication Problems**
```bash
# Clear session and re-authenticate
rm -rf data/session/
python -m GG connect --config config.ini
```

**Database Issues**
```bash
# Fix database inconsistencies
python -m GG fix_database --config config.ini
```

**Memory Issues**
```bash
# Reduce batch size in config.ini
batch_size=50
```

**Rate Limiting**
```bash
# Increase delays in config.ini
batch_delay=0.5
message_delay=0.1
```

### **Logging**
Enable debug logging by modifying `logging.conf`:
```ini
[logger_TelegramExplorer]
level=DEBUG
```

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

### **Development Setup**
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

### **Code Style**
- **Python**: Follow PEP 8 guidelines
- **Type hints**: Use type annotations
- **Documentation**: Docstrings for all functions
- **Testing**: Unit tests for new features

## 📄 License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **Original TEx**: Based on the original Telegram Explorer by Th3 0bservator
- **Telethon**: Telegram client library
- **SQLAlchemy**: Database ORM
- **Matplotlib**: Data visualization
- **NetworkX**: Network analysis

## 📞 Support

- **Documentation**: [https://enhanced-tex.readthedocs.io/](https://enhanced-tex.readthedocs.io/)
- **Issues**: [GitHub Issues](https://github.com/vsmz4laj7n/TEx/issues)
- **Discussions**: [GitHub Discussions](https://github.com/vsmz4laj7n/TEx/discussions)

## 📈 Project Status

| Feature | Status | Version |
|---------|--------|---------|
| Message Download | ✅ Complete | 2.0.0 |
| Real-time Monitoring | ✅ Complete | 2.0.0 |
| Media Download | ✅ Complete | 2.0.0 |
| User Profile Scraping | ✅ Complete | 2.0.0 |
| URL Analysis | ✅ Complete | 2.0.0 |
| Forwarding Analysis | ✅ Complete | 2.0.0 |
| Reaction Analysis | ✅ Complete | 2.0.0 |
| Visual Data Generation | ✅ Complete | 2.0.0 |
| Intelligence Analysis | ✅ Complete | 2.0.0 |
| Gephi Export | ✅ Complete | 2.0.0 |
| Database Management | ✅ Complete | 2.0.0 |
| Multi-language Support | ✅ Complete | 2.0.0 |

---

**GS.NO10** - Building the future of Telegram data analysis and intelligence gathering.
