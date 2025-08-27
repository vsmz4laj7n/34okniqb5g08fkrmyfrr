# 🚀 Enhanced TEx - Telegram Explorer

**Advanced Telegram Data Extraction and Analysis Tool**

[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/downloads/)
[![Telethon](https://img.shields.io/badge/telethon-1.34.0+-green.svg)](https://docs.telethon.dev/)
[![License](https://img.shields.io/badge/license-MIT-yellow.svg)](LICENSE)

## 📋 Overview

Enhanced TEx is a comprehensive Telegram data extraction and analysis tool that provides advanced capabilities for scraping messages, user profiles, and media from Telegram groups and channels. Built with modern Python 3.12+ and Telethon, it offers a sophisticated database architecture with version control, media organization, and user profile tracking.

## ✨ Key Features

### 🏗️ **Enhanced Database Architecture**
- **Group-Specific Databases**: Each group has its own `message.db` and `user.db`
- **Media Organization**: Automatic organization by type (photos, videos, documents, audio)
- **Version Control**: Track message edits and user profile changes
- **Scalable Design**: Better performance with large datasets

### 👤 **Advanced User Profile Scraping**
- **Profile Pictures**: Automatic download with timestamp naming
- **Bio & About**: Complete profile information extraction
- **Status Tracking**: Online/offline status and last seen
- **Version History**: Track profile changes over time
- **Contact Information**: Phone numbers and mutual contact status
- **Bot Detection**: Identify and flag bot accounts

### 📊 **Enhanced Message Management**
- **Message Version Control**: Track all versions of edited messages
- **Delete Tracking**: Mark deleted messages without removing data
- **Media Support**: Automatic download with metadata extraction
- **Formatting Preservation**: Telegram formatting (bold, italic, code, spoiler, quote)
- **Reply Threading**: Complete conversation threading support

### 🔍 **Advanced Analysis Features**
- **URL Scraping**: Extract all t.me URLs from messages with categorization
- **Forwarding Analysis**: Track message propagation networks across channels
- **Reaction Analysis**: Calculate engagement metrics and reaction patterns
- **User Interaction Networks**: Visualize user relationships and communication patterns
- **Intelligence Extraction**: Extract phone numbers, emails, IPs, and crypto addresses
- **GPS Data Extraction**: Extract location data from media files
- **Named Entity Recognition**: Identify persons, organizations, and locations
- **Ideological Analysis**: Detect hate speech, extremism, and conspiracy indicators
- **Threat Assessment**: Automated threat scoring and risk assessment

### 📈 **Professional Visualizations**
- **Graphs & Charts**: PDF reports with matplotlib and reportlab
- **Social Networks**: PNG visualizations using networkx and matplotlib
- **Interactive Maps**: HTML maps with folium for location data
- **Text Reports**: Comprehensive PDF reports with detailed findings

### 🔄 **Migration & Compatibility**
- **Automatic Migration**: Seamless transition from old to new architecture
- **Backward Compatibility**: Works with existing configurations
- **Data Preservation**: All existing data is preserved during migration

## 📁 Directory Structure

```
DataPath/
├── 1842894456/                    # Group-specific folder (named by group ID)
│   ├── message.db                 # Messages + media references + version control
│   ├── user.db                    # Group members + bio + profile pics
│   └── media/                     # Group media files
│       ├── photos/
│       ├── videos/
│       ├── documents/
│       ├── audio/
│       ├── voice/
│       └── stickers/
└── UserProfile/                   # Global user profiles
    ├── 608391654.db              # Individual user databases
    ├── 608391654.db
    ├── Media/
    │   ├── ProfilePicture/        # Profile pictures (UID-DateTime.jpg)
    │   └── PublicPosts/           # Public posts (if any)
    └── user_index.db              # Index of all users
```

## 🚀 Quick Start

### 1. **Installation**

```bash
# Clone the repository
git clone https://github.com/example/TEx.git
cd TEx

# Install dependencies
pip install -r requirements.txt
```

### 2. **Configuration**

Create or update your `config.ini` file:

```ini
[CONFIGURATION]
# Get these from https://my.telegram.org/apps
api_id=YOUR_API_ID_HERE
api_hash=YOUR_API_HASH_HERE
phone_number=YOUR_PHONE_NUMBER_HERE
data_path=./DataPath/
device_model=Desktop
timeout=30
```

### 3. **Connect to Telegram**

```bash
python3 -m TEx connect --config config.ini
```

### 4. **Download Messages with Enhanced Features**

```bash
# Download messages from a specific group
python3 -m TEx download_messages --config config.ini --group_id 7219437906 --limit 1000

# Download messages from multiple groups
python3 -m TEx download_messages --config config.ini --group_id "6705413239,1234567890" --limit 500
```

### 5. **Scrape User Profiles**

```bash
# Scrape specific users
python3 -m TEx scrape_user_profiles --config config.ini --user_ids "123456789,021621012"

# Scrape by usernames
python3 -m TEx scrape_user_profiles --config config.ini --usernames "john_doe,jane_smith"

# Scrape all participants from groups
python3 -m TEx scrape_user_profiles --config config.ini --group_ids "6919952449,1234567890"
```

## 🛠️ Advanced Usage

### 📥 **Enhanced Message Download**

```bash
# Download with custom limits and offsets
python3 -m TEx download_messages \
    --config config.ini \
    --group_id 2964586611 \
    --limit 1000 \
    --offset_id 0

# Download with media
python3 -m TEx download_messages \
    --config config.ini \
    --group_id 2964586611 \
    --limit 500
```

### 👥 **User Profile Scraping**

```bash
# Comprehensive user profile scraping
python3 -m TEx scrape_user_profiles \
    --config config.ini \
    --user_ids "123456789,987654321" \
    --usernames "john_doe,jane_smith" \
    --group_ids "0420212189,1234567890"
```

### 📊 **Export and Analysis**

```bash
# Export to HTML with enhanced formatting
python3 -m TEx export_html \
    --config config.ini \
    --group_id 0420212189 \
    --output_path ./html_export/

# Generate reports
python3 -m TEx report --config config.ini --group_id 2964586611

# View statistics
python3 -m TEx stats --config config.ini
```

## 🔄 Migration from Old Architecture

### **Automatic Migration**

```bash
# Migrate from old to new architecture
python3 TEx/database/migration_to_enhanced_architecture.py \
    --old_data_path /path/to/old/data \
    --new_data_path /path/to/new/data \
    --backup
```

### **Migration Features**
- **Data Preservation**: All existing data is preserved
- **Automatic Backup**: Optional backup before migration
- **Progress Tracking**: Detailed logging of migration progress
- **Error Recovery**: Graceful handling of migration errors

## 📊 Database Models

### **Enhanced Message Model**
```python
class TelegramMessageOrmEntity:
    # Primary keys
    id: int
    group_id: int
    
    # Content
    message: str
    text_entities: str  # JSON formatting entities
    raw: str
    
    # Media
    has_media: bool
    media_type: str
    media_path: str
    
    # Version control
    version: int
    is_edited: bool
    is_deleted: bool
    edit_date: datetime
    delete_date: datetime
    message_status: str  # active, edited, deleted
```

### **Enhanced User Model**
```python
class TelegramUserOrmEntity:
    # Basic info
    id: int
    first_name: str
    last_name: str
    username: str
    phone_number: str
    
    # Profile
    bio: str
    about: str
    has_profile_photo: bool
    profile_photo_path: str
    profile_photo_date: datetime
    
    # Status
    status: str
    was_online: datetime
    
    # Version control
    profile_version: int
    last_updated: datetime
```

## 🔧 Configuration

### **Pipeline Configuration**
```ini
[pipeline_sequence]
enhanced_telegram_messages_scrapper.EnhancedTelegramMessagesScrapper
enhanced_user_profile_scraper.EnhancedUserProfileScraper
```

### **Enhanced Arguments**
- `--user_ids`: Comma-separated user IDs for scraping
- `--usernames`: Comma-separated usernames for scraping
- `--group_ids`: Comma-separated group IDs for participant scraping
- `--limit`: Message download limit
- `--offset_id`: Message offset for pagination

## 📈 Performance Features

### **Database Performance**
- **Separate Databases**: Reduced query complexity
- **Indexed Fields**: Faster searches and lookups
- **Connection Pooling**: Better resource management
- **Batch Operations**: Efficient bulk data operations

### **Scraping Performance**
- **Rate Limiting**: Automatic delays to avoid API limits
- **Parallel Processing**: Concurrent user profile scraping
- **Caching**: Avoid re-scraping recent profiles
- **Error Recovery**: Graceful handling of API errors

## 🔍 Advanced Analysis Features

### **Real-Time Analysis (download_messages & listen)**
```bash
# URL Scraping
python3 -m TEx download_messages --config config.ini --group_id 123456789 --url-scraping

# Forwarding Analysis
python3 -m TEx download_messages --config config.ini --group_id 123456789 --forwarding-analysis

# Reaction Analysis
python3 -m TEx download_messages --config config.ini --group_id 123456789 --reaction-analysis

# All Analysis Types
python3 -m TEx download_messages --config config.ini --group_id 123456789 --url-scraping --forwarding-analysis --reaction-analysis
```

### **Offline Analysis (analyze command)**
```bash
# Analyze stored data for URLs
python3 -m TEx analyze --config config.ini --group_id 123456789 --url-analysis

# Analyze forwarding relationships
python3 -m TEx analyze --config config.ini --group_id 123456789 --forwarding-analysis

# Analyze reactions and engagement
python3 -m TEx analyze --config config.ini --group_id 123456789 --reaction-analysis

# Comprehensive visual data analysis
python3 -m TEx analyze --config config.ini --group_id 123456789 --visual-data

# Generate professional visualizations
python3 -m TEx analyze --config config.ini --group_id 123456789 --generate-visualizations
```

### **Analysis Capabilities**

#### **URL Analysis**
- **t.me URL Extraction**: Scrapes all t.me URLs from messages
- **URL Categorization**: Automatically categorizes as channels, users, bots, or join links
- **Dual Extraction Methods**: Uses both message entities and regex pattern matching
- **Analytics**: Provides insights on URL patterns and frequency

#### **Forwarding Analysis**
- **Forwarding Network Analysis**: Tracks message propagation across channels
- **Source Tracking**: Identifies original sources of forwarded messages
- **Cross-Channel Analysis**: Compares forwarding activity between channels
- **Network Mapping**: Creates forwarding relationship networks
- **Engagement Metrics**: Calculates forwarding-based engagement rates

#### **Reaction Analysis**
- **Reaction Pattern Analysis**: Tracks all reaction types and frequencies
- **Engagement Rate Calculation**: Based on reactions, forwards, and comments
- **Cross-Channel Comparison**: Compares engagement metrics between channels
- **Participant-Based Metrics**: Calculates engagement rates using participant counts
- **View-Based Metrics**: Calculates engagement rates using post views

#### **Visual Data Analysis**
- **User Interaction Networks**: Identify possible user associates via interaction network maps
- **Selector/Intel Extraction**: Extract phone numbers, emails, IPs, and crypto addresses
- **GPS Data Extraction**: Extract location data from media files with EXIF analysis
- **User Message Patterns**: Analyze posting patterns over time for pattern of life analysis
- **Named Entity Recognition**: Identify persons, organizations, and locations in messages
- **Ideological Analysis**: Detect hate speech, extremism, conspiracy, and other indicators
- **Threat Assessment**: Automated threat scoring and risk assessment

### **User Analytics**
- **Profile Changes**: Track username, bio, photo changes
- **Activity Patterns**: Message frequency and timing
- **Media Usage**: Types and frequency of media sharing
- **Group Participation**: Cross-group user analysis

### **Message Analytics**
- **Edit Patterns**: Frequency and types of message edits
- **Media Analysis**: Popular media types and sizes
- **Formatting Usage**: Most common formatting styles
- **Reply Patterns**: Message threading and conversations

## 📈 Professional Visualizations

### **Visualization Types**

#### **Graphs & Charts → PDFs (matplotlib + reportlab)**
- **Message Activity Charts**: Time-based message frequency analysis
- **User Engagement Charts**: Reaction and engagement rate distributions
- **URL Distribution Charts**: Pie charts showing URL type breakdowns
- **Threat Assessment Charts**: Bar charts of threat indicators by category
- **Comprehensive PDF Reports**: Professional reports with embedded charts

#### **Social Graphs → PNG (networkx + matplotlib)**
- **User Interaction Networks**: Visualize user relationships and communication patterns
- **Forwarding Networks**: Show message propagation across channels
- **Community Detection**: Identify user clusters and communities
- **Influence Mapping**: Highlight key influencers and their connections
- **High-Resolution Output**: 300 DPI PNG files suitable for presentations

#### **Maps → Interactive HTML + CSV (folium)**
- **GPS Data Visualization**: Interactive maps showing location data from media
- **Location Entity Mapping**: Map locations mentioned in messages
- **Heat Maps**: Show activity density by geographic location
- **Interactive Features**: Clickable markers with detailed information
- **CSV Export**: Location data for external mapping tools

#### **Text Reports → PDFs (reportlab)**
- **Executive Summaries**: High-level analysis overviews
- **Detailed Findings**: Comprehensive analysis with citations
- **Threat Assessments**: Professional threat evaluation reports
- **Intelligence Reports**: Structured intelligence findings
- **Professional Formatting**: Corporate-ready PDF reports

### **Visualization Output Structure**
```
DataPath/visualizations/
├── 123456789/                    # Group-specific visualizations
│   ├── group_123456789_charts.pdf           # Charts and graphs PDF
│   ├── group_123456789_social_network.png   # Social network visualization
│   ├── group_123456789_map.html             # Interactive map
│   ├── group_123456789_locations.csv        # Location data CSV
│   └── group_123456789_analysis_report.pdf  # Comprehensive text report
└── cross_group_analysis/         # Cross-group comparison visualizations
```

### **Configuration for Analysis**

#### **Selector Patterns (config.ini)**
```ini
[SELECTOR_PATTERNS]
# Regex patterns for extracting selectors from messages
phone_pattern=\b(?:\+?1[-.]?)?\(?([0-9]{3})\)?[-.]?([0-9]{3})[-.]?([0-9]{4})\b
email_pattern=\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b
url_pattern=https?://(?:[-\w.])+(?:[:\d]+)?(?:/(?:[\w/_.])*(?:\?(?:[\w&=%.])*)?(?:#(?:[\w.])*)?)?
ip_pattern=\b(?:[0-9]{1,3}\.){3}[0-9]{1,3}\b
crypto_pattern=\b[13][a-km-zA-HJ-NP-Z1-9]{25,34}\b
```

#### **Entity Patterns (config.ini)**
```ini
[ENTITY_PATTERNS]
# Regex patterns for named entity recognition
person_pattern=\b[A-Z][a-z]+ [A-Z][a-z]+\b
organization_pattern=\b[A-Z][A-Z\s&]+(?:Inc|Corp|LLC|Ltd|Company|Organization|Group)\b
location_pattern=\b[A-Z][a-z]+(?: [A-Z][a-z]+)*,?\s+[A-Z]{2}\b
date_pattern=\b\d{1,2}[/-]\d{1,2}[/-]\d{2,4}\b
```

#### **Ideological Keywords (config.ini)**
```ini
[IDEOLOGICAL_KEYWORDS]
# Keywords for ideological indicator analysis (comma-separated)
hate_speech=hate,racist,bigot,nazi,supremacist,white nationalist,anti-semitic
extremism=extremist,radical,terrorist,jihad,white power,anarchist,revolutionary
conspiracy=conspiracy,deep state,illuminati,chemtrails,flat earth,new world order
sovereign_citizen=sovereign,freeman,common law,strawman,admiralty,maritime law
incel_terminology=incel,chad,stacy,blackpill,redpill,bluepill,mgtow
```

#### **Threat Keywords (config.ini)**
```ini
[THREAT_KEYWORDS]
# Keywords for threat assessment (comma-separated)
capability=weapon,gun,bomb,explosive,knife,ammo,ammunition,firearm
violent_intent=kill,murder,attack,shoot,bomb,terror,assassinate,eliminate
weapons=rifle,pistol,shotgun,ammunition,explosive,knife,grenade,molotov
threatening=threat,warning,danger,attack,kill,destroy,eliminate,neutralize
```

## 🛡️ Privacy & Security

### **Privacy Features**
- **Local Storage**: All data stored locally
- **No Cloud Sync**: Complete data control
- **Encrypted Sessions**: Secure Telegram authentication
- **Access Control**: File system permissions

### **Data Management**
- **Automatic Cleanup**: Remove old temporary files
- **Storage Optimization**: Efficient file organization
- **Backup Support**: Easy data backup and restore
- **Migration Tools**: Safe data architecture transitions

## 🐛 Troubleshooting

### **Common Issues**

#### **Migration Errors**
```bash
# Check database integrity
sqlite3 DataPath/data_local.db "PRAGMA integrity_check;"

# Backup and retry migration
python3 TEx/database/migration_to_enhanced_architecture.py \
    --old_data_path /path/to/old/data \
    --new_data_path /path/to/new/data \
    --backup
```

#### **Scraping Issues**
```bash
# Check API limits
python3 -m TEx scrape_user_profiles --config config.ini --user_ids "123456789"

# Reduce batch size for large groups
python3 -m TEx scrape_user_profiles --config config.ini --group_ids "2964586611"
```

#### **Media Download Issues**
```bash
# Check disk space
df -h DataPath/

# Verify file permissions
ls -la DataPath/2964586611/media/
```

## 📚 API Reference

### **Enhanced Database Manager**
```python
from TEx.database.enhanced_db_manager import EnhancedDbManager

# Initialize
db_manager = EnhancedDbManager("/path/to/data")

# Get group database
group_db = db_manager.get_group_database(2964586611)

# Get user profile database
profile_db = db_manager.get_user_profile_db()
```

### **User Profile Scraper**
```python
from TEx.modules.enhanced_user_profile_scraper import EnhancedUserProfileScraper

# Initialize
scraper = EnhancedUserProfileScraper()

# Scrape single user
await scraper._scrape_single_user_profile(client, user_id)

# Scrape group participants
await scraper._scrape_group_participants(client, group_id)
```

### **Message Scraper**
```python
from TEx.modules.enhanced_telegram_messages_scrapper import EnhancedTelegramMessagesScrapper

# Initialize
scraper = EnhancedTelegramMessagesScrapper()

# Process message with media
await scraper._process_message(client, message, group_id)

# Handle message edits
await scraper.handle_message_edit(client, edited_message, group_id)
```

## 🎯 Future Enhancements

### **Planned Features**
- **Real-time Monitoring**: Live message and profile updates
- **Advanced Analytics**: Machine learning insights
- **Export Formats**: JSON, CSV, Excel exports
- **Web Interface**: Browser-based data exploration
- **API Integration**: REST API for external tools

### **Performance Optimizations**
- **Database Sharding**: Horizontal scaling for large datasets
- **Caching Layer**: Redis integration for faster queries
- **Background Processing**: Async task queues
- **Compression**: Efficient storage for media files

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guidelines](CONTRIBUTING.md) for details.

### **Development Setup**
```bash
# Clone the repository
git clone https://github.com/example/TEx.git
cd TEx

# Install development dependencies
pip install -r requirements-dev.txt

# Run tests
pytest tests/

# Run linting
ruff check .
```

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Support

For questions, issues, or feature requests:
- **GitHub Issues**: [Report bugs and request features](https://github.com/vsmz4laj7n/TEx/issues)
- **Documentation**: Check this README and inline code comments
- **Migration Help**: Use the migration script with `--backup` flag

## 🙏 Acknowledgments

- **Telethon**: The powerful Telegram client library
- **SQLAlchemy**: The SQL toolkit and ORM
- **Jinja2**: The template engine for HTML exports
- **Original TEx Team**: For the foundation of this project

---

**Enhanced TEx** - Advanced Telegram Data Extraction and Analysis Tool

*Built with ❤️ for the Telegram community*
