# 📦 Installation Guide

## System Requirements

### **Operating Systems**
- **Linux**: Ubuntu 20.04+, Debian 11+, CentOS 8+, RHEL 8+
- **macOS**: macOS 10.15+ (Catalina or later)
- **Windows**: Windows 10+ (64-bit)

### **Python Requirements**
- **Python**: 3.12 or higher
- **pip**: Latest version
- **Git**: For cloning the repository

### **Hardware Requirements**
- **RAM**: Minimum 4GB, Recommended 8GB+
- **Storage**: Minimum 10GB free space
- **CPU**: Multi-core processor recommended

## Installation Methods

### **Method 1: Clone from GitHub (Recommended)**

```bash
# Clone the repository
git clone https://github.com/vsmz4laj7n/TEx.git
cd TEx

# Create virtual environment (recommended)
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Install development dependencies (optional)
pip install -r requirements-dev.txt
```

### **Method 2: Using pip (Alternative)**

```bash
# Install directly from GitHub
pip install git+https://github.com/vsmz4laj7n/TEx.git

# Or install in development mode
pip install -e git+https://github.com/vsmz4laj7n/TEx.git#egg=enhanced-tex
```

### **Method 3: Using Poetry (Advanced)**

```bash
# Install Poetry if not already installed
curl -sSL https://install.python-poetry.org | python3 -

# Clone and install
git clone https://github.com/vsmz4laj7n/TEx.git
cd TEx
poetry install
poetry shell
```

## Dependencies

### **Core Dependencies**
- **Telethon**: Telegram client library
- **SQLAlchemy**: Database ORM
- **aiosqlite**: Async SQLite support
- **Jinja2**: Template engine
- **pandas**: Data manipulation
- **matplotlib**: Plotting and visualization
- **reportlab**: PDF generation
- **networkx**: Network analysis
- **folium**: Interactive maps
- **Pillow**: Image processing

### **Development Dependencies**
- **pytest**: Testing framework
- **ruff**: Code linting
- **mypy**: Type checking
- **tox**: Test automation
- **coverage**: Code coverage

## Telegram API Setup

### **1. Get Telegram API Credentials**

1. Visit [https://my.telegram.org/apps](https://my.telegram.org/apps)
2. Log in with your phone number
3. Create a new application
4. Note down your `api_id` and `api_hash`

### **2. Create Configuration File**

Create a `config.ini` file in your project directory:

```ini
[CONFIGURATION]
# Telegram API credentials
api_id=YOUR_API_ID_HERE
api_hash=YOUR_API_HASH_HERE
phone_number=YOUR_PHONE_NUMBER_HERE

# Data storage
data_path=./DataPath/
device_model=Desktop
timeout=30

# Analysis settings
enable_analysis=true
enable_visualizations=true
```

### **3. Test Connection**

```bash
# Test your configuration
python3 -m TEx connect --config config.ini
```

## Post-Installation Setup

### **1. Verify Installation**

```bash
# Check if TEx is properly installed
python3 -m TEx --help

# Check version
python3 -m TEx --version
```

### **2. Initialize Database**

```bash
# Initialize the database structure
python3 -m TEx init --config config.ini
```

### **3. Test Basic Functionality**

```bash
# List available groups
python3 -m TEx list_groups --config config.ini

# Test message download (replace with actual group ID)
python3 -m TEx download_messages --config config.ini --group_id YOUR_GROUP_ID
```

## Troubleshooting

### **Common Issues**

#### **Python Version Issues**
```bash
# Check Python version
python3 --version

# If version is below 3.12, upgrade Python
# Ubuntu/Debian
sudo apt update
sudo apt install python3.12 python3.12-venv python3.12-pip

# macOS
brew install python@3.12

# Windows
# Download from https://www.python.org/downloads/
```

#### **Permission Issues**
```bash
# Fix permission issues on Linux/macOS
chmod +x TEx/__main__.py
chmod +x setup.py

# On Windows, run as Administrator if needed
```

#### **Dependency Installation Issues**
```bash
# Upgrade pip
python3 -m pip install --upgrade pip

# Install with user flag
pip install --user -r requirements.txt

# On Windows, use
pip install --user --break-system-packages -r requirements.txt
```

#### **Telegram API Issues**
```bash
# Verify API credentials
python3 -c "
import configparser
config = configparser.ConfigParser()
config.read('config.ini')
print(f'API ID: {config[\"CONFIGURATION\"][\"api_id\"]}')
print(f'API Hash: {config[\"CONFIGURATION\"][\"api_hash\"]}')
print(f'Phone: {config[\"CONFIGURATION\"][\"phone_number\"]}')
"
```

### **Environment-Specific Issues**

#### **Linux Issues**
```bash
# Install system dependencies
sudo apt update
sudo apt install python3-dev python3-pip python3-venv
sudo apt install libffi-dev libssl-dev

# For visualization dependencies
sudo apt install libfreetype6-dev libpng-dev
```

#### **macOS Issues**
```bash
# Install Xcode command line tools
xcode-select --install

# Install Homebrew if not installed
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install dependencies
brew install python@3.12
brew install freetype png
```

#### **Windows Issues**
```bash
# Install Visual C++ Build Tools
# Download from: https://visualstudio.microsoft.com/visual-cpp-build-tools/

# Use Windows Subsystem for Linux (WSL) for better compatibility
wsl --install
```

## Development Setup

### **1. Clone Development Repository**

```bash
git clone https://github.com/vsmz4laj7n/TEx.git
cd TEx
git checkout develop  # or your feature branch
```

### **2. Install Development Dependencies**

```bash
# Create virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install all dependencies
pip install -r requirements.txt
pip install -r requirements-dev.txt
```

### **3. Setup Pre-commit Hooks**

```bash
# Install pre-commit
pip install pre-commit

# Setup hooks
pre-commit install
```

### **4. Run Tests**

```bash
# Run all tests
pytest tests/

# Run with coverage
pytest --cov=TEx tests/

# Run specific test file
pytest tests/test_enhanced_db_manager.py
```

### **5. Code Quality Checks**

```bash
# Run linting
ruff check .

# Run type checking
mypy TEx/

# Run security checks
bandit -r TEx/
```

## Docker Installation (Alternative)

### **1. Build Docker Image**

```bash
# Build the image
docker build -t enhanced-tex .

# Run container
docker run -it --rm enhanced-tex python3 -m TEx --help
```

### **2. Docker Compose Setup**

Create `docker-compose.yml`:

```yaml
version: '3.8'
services:
  enhanced-tex:
    build: .
    volumes:
      - ./DataPath:/app/DataPath
      - ./config.ini:/app/config.ini
    environment:
      - PYTHONPATH=/app
    command: python3 -m TEx connect --config config.ini
```

Run with:
```bash
docker-compose up
```

## Verification

### **1. Basic Functionality Test**

```bash
# Test connection
python3 -m TEx connect --config config.ini

# Test group listing
python3 -m TEx list_groups --config config.ini

# Test message download
python3 -m TEx download_messages --config config.ini --group_id TEST_GROUP_ID
```

### **2. Advanced Features Test**

```bash
# Test analysis features
python3 -m TEx download_messages --config config.ini --group_id TEST_GROUP_ID --url-scraping

# Test visualization
python3 -m TEx analyze --config config.ini --group_id TEST_GROUP_ID --generate-visualizations
```

### **3. Database Verification**

```bash
# Check database structure
ls -la DataPath/
ls -la DataPath/*/  # Check group-specific databases
```

## Next Steps

After successful installation:

1. **Read the [Configuration Guide](configuration.md)**
2. **Follow the [Quick Start Guide](quickstart.md)**
3. **Explore [Advanced Features](advanced-features.md)**
4. **Check [Troubleshooting](troubleshooting.md)** if you encounter issues

## Support

If you encounter installation issues:

- **GitHub Issues**: [Create an issue](https://github.com/vsmz4laj7n/TEx/issues)
- **Documentation**: [Read the docs](https://enhanced-tex.readthedocs.io/)
- **Discussions**: [Join discussions](https://github.com/vsmz4laj7n/TEx/discussions)