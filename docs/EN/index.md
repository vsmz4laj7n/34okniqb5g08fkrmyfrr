# 🚀 Enhanced TEx - Telegram Explorer

**Advanced Telegram Data Extraction and Analysis Tool**

[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/downloads/)
[![Telethon](https://img.shields.io/badge/telethon-1.34.0+-green.svg)](https://docs.telethon.dev/)
[![License](https://img.shields.io/badge/license-MIT-yellow.svg)](LICENSE)
[![Documentation](https://img.shields.io/badge/docs-readthedocs.io-blue.svg)](https://enhanced-tex.readthedocs.io/)
[![Build Status](https://img.shields.io/github/actions/workflow/status/vsmz4laj7n/TEx/ci.yml?branch=main)](https://github.com/vsmz4laj7n/TEx/actions)

## 📋 Overview

Enhanced TEx is a comprehensive Telegram data extraction and analysis tool that provides advanced capabilities for scraping messages, user profiles, and media from Telegram groups and channels. Built with modern Python 3.12+ and Telethon, it offers a sophisticated database architecture with version control, media organization, and user profile tracking.

**Author**: Enhanced TEx Team  
**Version**: 2.0.0  
**License**: MIT License

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

## 🚀 Quick Start

### 1. **Installation**

```bash
# Clone the repository
git clone https://github.com/vsmz4laj7n/TEx.git
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
# Basic message download
python3 -m TEx download_messages --config config.ini --group_id 123456789

# With analysis features
python3 -m TEx download_messages --config config.ini --group_id 123456789 --url-scraping --forwarding-analysis --reaction-analysis

# With user profile scraping
python3 -m TEx download_messages --config config.ini --group_id 123456789 --scrape_user_profiles
```

### 5. **Analyze Downloaded Data**

```bash
# Comprehensive analysis
python3 -m TEx analyze --config config.ini --group_id 123456789 --visual-data --generate-visualizations
```

## 📁 Directory Structure

```
DataPath/
├── 2964586611/                    # Group-specific folder (named by group ID)
│   ├── message.db                 # Messages + media references + version control
│   ├── user.db                    # Group members + bio + profile pics
│   └── media/                     # Group media files
│       ├── photos/
│       ├── videos/
│       ├── documents/
│       ├── audio/
│       ├── voice/
│       └── stickers/
├── UserProfile/                   # Global user profiles
│   ├── 123456789.db              # Individual user databases
│   ├── 987654321.db
│   ├── Media/
│   │   ├── ProfilePicture/        # Profile pictures (UID-DateTime.jpg)
│   │   └── PublicPosts/           # Public posts (if any)
│   └── user_index.db              # Index of all users
└── visualizations/                # Generated visualizations
    ├── 123456789/                 # Group-specific visualizations
    │   ├── group_123456789_charts.pdf
    │   ├── group_123456789_social_network.png
    │   ├── group_123456789_map.html
    │   └── group_123456789_analysis_report.pdf
    └── cross_group_analysis/      # Cross-group comparisons
```

## 🔧 Configuration

### **Pipeline Configuration**
```ini
[pipeline_sequence]
enhanced_telegram_messages_scrapper.EnhancedTelegramMessagesScrapper
enhanced_user_profile_scraper.EnhancedUserProfileScraper
enhanced_url_scraper.EnhancedUrlScraper
enhanced_forwarding_analyzer.EnhancedForwardingAnalyzer
enhanced_reaction_analyzer.EnhancedReactionAnalyzer
enhanced_data_analyzer.EnhancedDataAnalyzer
enhanced_visualization_generator.EnhancedVisualizationGenerator
```

### **Analysis Configuration**
```ini
[SELECTOR_PATTERNS]
phone_pattern=\b(?:\+?1[-.]?)?\(?([0-9]{3})\)?[-.]?([0-9]{3})[-.]?([0-9]{4})\b
email_pattern=\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b
url_pattern=https?://(?:[-\w.])+(?:[:\d]+)?(?:/(?:[\w/_.])*(?:\?(?:[\w&=%.])*)?(?:#(?:[\w.])*)?)?
ip_pattern=\b(?:[0-9]{1,3}\.){3}[0-9]{1,3}\b
crypto_pattern=\b[13][a-km-zA-HJ-NP-Z1-9]{25,34}\b

[IDEOLOGICAL_KEYWORDS]
hate_speech=hate,racist,bigot,nazi,supremacist,white nationalist,anti-semitic
extremism=extremist,radical,terrorist,jihad,white power,anarchist,revolutionary
conspiracy=conspiracy,deep state,illuminati,chemtrails,flat earth,new world order
sovereign_citizen=sovereign,freeman,common law,strawman,admiralty,maritime law
incel_terminology=incel,chad,stacy,blackpill,redpill,bluepill,mgtow

[THREAT_KEYWORDS]
capability=weapon,gun,bomb,explosive,knife,ammo,ammunition,firearm
violent_intent=kill,murder,attack,shoot,bomb,terror,assassinate,eliminate
weapons=rifle,pistol,shotgun,ammunition,explosive,knife,grenade,molotov
threatening=threat,warning,danger,attack,kill,destroy,eliminate,neutralize
```

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

## 🛡️ Privacy & Security

### **Privacy Features**
- **Local Storage**: All data stored locally on your machine
- **No Cloud Upload**: No data is sent to external servers
- **Configurable Retention**: Set your own data retention policies
- **Secure Authentication**: Uses Telegram's official API

### **Security Features**
- **API Key Protection**: Secure storage of Telegram API credentials
- **Rate Limiting**: Automatic delays to avoid API limits
- **Error Handling**: Graceful handling of authentication failures
- **Data Encryption**: Optional encryption for sensitive data

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](contributing.md) for details.

### **Development Setup**
```bash
# Clone the repository
git clone https://github.com/vsmz4laj7n/TEx.git
cd TEx

# Install development dependencies
pip install -r requirements.txt
pip install -r requirements-dev.txt

# Run tests
pytest tests/

# Run linting
ruff check .
```

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **Original TEx**: Based on the original Telegram Explorer by Th3 0bservator
- **Telethon**: Telegram client library for Python
- **SQLAlchemy**: Database toolkit and ORM
- **Matplotlib/NetworkX/Folium**: Visualization libraries
- **ReportLab**: PDF generation library

## 📞 Support

- **Documentation**: [https://enhanced-tex.readthedocs.io/](https://enhanced-tex.readthedocs.io/)
- **Issues**: [GitHub Issues](https://github.com/vsmz4laj7n/TEx/issues)
- **Discussions**: [GitHub Discussions](https://github.com/vsmz4laj7n/TEx/discussions)

---

**Enhanced TEx Team** - Making Telegram data analysis professional and comprehensive.