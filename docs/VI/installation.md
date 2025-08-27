# 📦 Hướng dẫn Cài đặt

## Yêu cầu Hệ thống

### **Hệ điều hành**
- **Linux**: Ubuntu 20.04+, Debian 11+, CentOS 8+, RHEL 8+
- **macOS**: macOS 10.15+ (Catalina trở lên)
- **Windows**: Windows 10+ (64-bit)

### **Yêu cầu Python**
- **Python**: 3.12 trở lên
- **pip**: Phiên bản mới nhất
- **Git**: Để clone repository

### **Yêu cầu Phần cứng**
- **RAM**: Tối thiểu 4GB, Khuyến nghị 8GB+
- **Ổ cứng**: Tối thiểu 10GB dung lượng trống
- **CPU**: Khuyến nghị bộ xử lý đa nhân

## Phương pháp Cài đặt

### **Phương pháp 1: Clone từ GitHub (Khuyến nghị)**

```bash
# Clone repository
git clone https://github.com/vsmz4laj7n/TEx.git
cd TEx

# Tạo môi trường ảo (khuyến nghị)
python3 -m venv venv
source venv/bin/activate  # Trên Windows: venv\Scripts\activate

# Cài đặt dependencies
pip install -r requirements.txt

# Cài đặt dependencies phát triển (tùy chọn)
pip install -r requirements-dev.txt
```

### **Phương pháp 2: Sử dụng pip (Thay thế)**

```bash
# Cài đặt trực tiếp từ GitHub
pip install git+https://github.com/vsmz4laj7n/TEx.git

# Hoặc cài đặt ở chế độ phát triển
pip install -e git+https://github.com/vsmz4laj7n/TEx.git#egg=enhanced-tex
```

### **Phương pháp 3: Sử dụng Poetry (Nâng cao)**

```bash
# Cài đặt Poetry nếu chưa có
curl -sSL https://install.python-poetry.org | python3 -

# Clone và cài đặt
git clone https://github.com/vsmz4laj7n/TEx.git
cd TEx
poetry install
poetry shell
```

## Dependencies

### **Dependencies Chính**
- **Telethon**: Thư viện client Telegram
- **SQLAlchemy**: Database ORM
- **aiosqlite**: Hỗ trợ SQLite bất đồng bộ
- **Jinja2**: Template engine
- **pandas**: Xử lý dữ liệu
- **matplotlib**: Vẽ biểu đồ và trực quan hóa
- **reportlab**: Tạo PDF
- **networkx**: Phân tích mạng
- **folium**: Bản đồ tương tác
- **Pillow**: Xử lý ảnh

### **Dependencies Phát triển**
- **pytest**: Framework testing
- **ruff**: Kiểm tra code
- **mypy**: Kiểm tra kiểu dữ liệu
- **tox**: Tự động hóa test
- **coverage**: Độ bao phủ code

## Thiết lập Telegram API

### **1. Lấy Thông tin đăng nhập Telegram API**

1. Truy cập [https://my.telegram.org/apps](https://my.telegram.org/apps)
2. Đăng nhập bằng số điện thoại
3. Tạo ứng dụng mới
4. Ghi chú `api_id` và `api_hash`

### **2. Tạo File Cấu hình**

Tạo file `config.ini` trong thư mục dự án:

```ini
[CONFIGURATION]
# Thông tin đăng nhập Telegram API
api_id=YOUR_API_ID_HERE
api_hash=YOUR_API_HASH_HERE
phone_number=YOUR_PHONE_NUMBER_HERE

# Lưu trữ dữ liệu
data_path=./DataPath/
device_model=Desktop
timeout=30

# Cài đặt phân tích
enable_analysis=true
enable_visualizations=true
```

### **3. Kiểm tra Kết nối**

```bash
# Kiểm tra cấu hình
python3 -m TEx connect --config config.ini
```

## Thiết lập Sau Cài đặt

### **1. Xác minh Cài đặt**

```bash
# Kiểm tra TEx đã được cài đặt đúng
python3 -m TEx --help

# Kiểm tra phiên bản
python3 -m TEx --version
```

### **2. Khởi tạo Cơ sở dữ liệu**

```bash
# Khởi tạo cấu trúc cơ sở dữ liệu
python3 -m TEx init --config config.ini
```

### **3. Kiểm tra Chức năng Cơ bản**

```bash
# Liệt kê các nhóm có sẵn
python3 -m TEx list_groups --config config.ini

# Kiểm tra tải xuống tin nhắn (thay thế bằng ID nhóm thực)
python3 -m TEx download_messages --config config.ini --group_id YOUR_GROUP_ID
```

## Xử lý Sự cố

### **Sự cố Thường gặp**

#### **Sự cố Phiên bản Python**
```bash
# Kiểm tra phiên bản Python
python3 --version

# Nếu phiên bản dưới 3.12, nâng cấp Python
# Ubuntu/Debian
sudo apt update
sudo apt install python3.12 python3.12-venv python3.12-pip

# macOS
brew install python@3.12

# Windows
# Tải từ https://www.python.org/downloads/
```

#### **Sự cố Quyền truy cập**
```bash
# Sửa sự cố quyền truy cập trên Linux/macOS
chmod +x TEx/__main__.py
chmod +x setup.py

# Trên Windows, chạy với quyền Administrator nếu cần
```

#### **Sự cố Cài đặt Dependencies**
```bash
# Nâng cấp pip
python3 -m pip install --upgrade pip

# Cài đặt với flag user
pip install --user -r requirements.txt

# Trên Windows, sử dụng
pip install --user --break-system-packages -r requirements.txt
```

#### **Sự cố Telegram API**
```bash
# Xác minh thông tin đăng nhập API
python3 -c "
import configparser
config = configparser.ConfigParser()
config.read('config.ini')
print(f'API ID: {config[\"CONFIGURATION\"][\"api_id\"]}')
print(f'API Hash: {config[\"CONFIGURATION\"][\"api_hash\"]}')
print(f'Phone: {config[\"CONFIGURATION\"][\"phone_number\"]}')
"
```

### **Sự cố Theo Môi trường**

#### **Sự cố Linux**
```bash
# Cài đặt system dependencies
sudo apt update
sudo apt install python3-dev python3-pip python3-venv
sudo apt install libffi-dev libssl-dev

# Cho visualization dependencies
sudo apt install libfreetype6-dev libpng-dev
```

#### **Sự cố macOS**
```bash
# Cài đặt Xcode command line tools
xcode-select --install

# Cài đặt Homebrew nếu chưa có
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Cài đặt dependencies
brew install python@3.12
brew install freetype png
```

#### **Sự cố Windows**
```bash
# Cài đặt Visual C++ Build Tools
# Tải từ: https://visualstudio.microsoft.com/visual-cpp-build-tools/

# Sử dụng Windows Subsystem for Linux (WSL) để tương thích tốt hơn
wsl --install
```

## Thiết lập Phát triển

### **1. Clone Repository Phát triển**

```bash
git clone https://github.com/vsmz4laj7n/TEx.git
cd TEx
git checkout develop  # hoặc nhánh tính năng của bạn
```

### **2. Cài đặt Dependencies Phát triển**

```bash
# Tạo môi trường ảo
python3 -m venv venv
source venv/bin/activate  # Trên Windows: venv\Scripts\activate

# Cài đặt tất cả dependencies
pip install -r requirements.txt
pip install -r requirements-dev.txt
```

### **3. Thiết lập Pre-commit Hooks**

```bash
# Cài đặt pre-commit
pip install pre-commit

# Thiết lập hooks
pre-commit install
```

### **4. Chạy Tests**

```bash
# Chạy tất cả tests
pytest tests/

# Chạy với coverage
pytest --cov=TEx tests/

# Chạy file test cụ thể
pytest tests/test_enhanced_db_manager.py
```

### **5. Kiểm tra Chất lượng Code**

```bash
# Chạy linting
ruff check .

# Chạy type checking
mypy TEx/

# Chạy security checks
bandit -r TEx/
```

## Cài đặt Docker (Thay thế)

### **1. Build Docker Image**

```bash
# Build image
docker build -t enhanced-tex .

# Chạy container
docker run -it --rm enhanced-tex python3 -m TEx --help
```

### **2. Thiết lập Docker Compose**

Tạo `docker-compose.yml`:

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

Chạy với:
```bash
docker-compose up
```

## Xác minh

### **1. Kiểm tra Chức năng Cơ bản**

```bash
# Kiểm tra kết nối
python3 -m TEx connect --config config.ini

# Kiểm tra liệt kê nhóm
python3 -m TEx list_groups --config config.ini

# Kiểm tra tải xuống tin nhắn
python3 -m TEx download_messages --config config.ini --group_id TEST_GROUP_ID
```

### **2. Kiểm tra Tính năng Nâng cao**

```bash
# Kiểm tra tính năng phân tích
python3 -m TEx download_messages --config config.ini --group_id TEST_GROUP_ID --url-scraping

# Kiểm tra trực quan hóa
python3 -m TEx analyze --config config.ini --group_id TEST_GROUP_ID --generate-visualizations
```

### **3. Xác minh Cơ sở dữ liệu**

```bash
# Kiểm tra cấu trúc cơ sở dữ liệu
ls -la DataPath/
ls -la DataPath/*/  # Kiểm tra cơ sở dữ liệu riêng cho nhóm
```

## Bước Tiếp theo

Sau khi cài đặt thành công:

1. **Đọc [Hướng dẫn Cấu hình](configuration.md)**
2. **Làm theo [Hướng dẫn Bắt đầu Nhanh](quickstart.md)**
3. **Khám phá [Tính năng Nâng cao](advanced-features.md)**
4. **Kiểm tra [Xử lý Sự cố](troubleshooting.md)** nếu gặp vấn đề

## Hỗ trợ

Nếu gặp sự cố cài đặt:

- **GitHub Issues**: [Tạo issue](https://github.com/vsmz4laj7n/TEx/issues)
- **Tài liệu**: [Đọc docs](https://enhanced-tex.readthedocs.io/)
- **Thảo luận**: [Tham gia thảo luận](https://github.com/vsmz4laj7n/TEx/discussions)