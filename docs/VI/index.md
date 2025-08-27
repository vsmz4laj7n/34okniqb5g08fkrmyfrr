# 🚀 Enhanced TEx - Telegram Explorer

**Công cụ Trích xuất và Phân tích Dữ liệu Telegram Nâng cao**

[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/downloads/)
[![Telethon](https://img.shields.io/badge/telethon-1.34.0+-green.svg)](https://docs.telethon.dev/)
[![License](https://img.shields.io/badge/license-MIT-yellow.svg)](LICENSE)
[![Documentation](https://img.shields.io/badge/docs-readthedocs.io-blue.svg)](https://enhanced-tex.readthedocs.io/)
[![Build Status](https://img.shields.io/github/actions/workflow/status/vsmz4laj7n/TEx/ci.yml?branch=main)](https://github.com/vsmz4laj7n/TEx/actions)

## 📋 Tổng quan

Enhanced TEx là một công cụ trích xuất và phân tích dữ liệu Telegram toàn diện, cung cấp khả năng nâng cao để thu thập tin nhắn, hồ sơ người dùng và phương tiện từ các nhóm và kênh Telegram. Được xây dựng với Python 3.12+ hiện đại và Telethon, nó cung cấp kiến trúc cơ sở dữ liệu tinh vi với kiểm soát phiên bản, tổ chức phương tiện và theo dõi hồ sơ người dùng.

**Tác giả**: Enhanced TEx Team  
**Phiên bản**: 2.0.0  
**Giấy phép**: MIT License

## ✨ Tính năng chính

### 🏗️ **Kiến trúc Cơ sở dữ liệu Nâng cao**
- **Cơ sở dữ liệu riêng cho từng nhóm**: Mỗi nhóm có `message.db` và `user.db` riêng
- **Tổ chức phương tiện**: Tự động tổ chức theo loại (ảnh, video, tài liệu, âm thanh)
- **Kiểm soát phiên bản**: Theo dõi chỉnh sửa tin nhắn và thay đổi hồ sơ người dùng
- **Thiết kế có thể mở rộng**: Hiệu suất tốt hơn với bộ dữ liệu lớn

### 👤 **Thu thập Hồ sơ Người dùng Nâng cao**
- **Ảnh đại diện**: Tự động tải xuống với tên file có timestamp
- **Bio & Giới thiệu**: Trích xuất thông tin hồ sơ hoàn chỉnh
- **Theo dõi trạng thái**: Trạng thái online/offline và lần cuối hoạt động
- **Lịch sử phiên bản**: Theo dõi thay đổi hồ sơ theo thời gian
- **Thông tin liên hệ**: Số điện thoại và trạng thái liên hệ chung
- **Phát hiện Bot**: Xác định và đánh dấu tài khoản bot

### 📊 **Quản lý Tin nhắn Nâng cao**
- **Kiểm soát phiên bản tin nhắn**: Theo dõi tất cả phiên bản của tin nhắn đã chỉnh sửa
- **Theo dõi xóa**: Đánh dấu tin nhắn đã xóa mà không xóa dữ liệu
- **Hỗ trợ phương tiện**: Tự động tải xuống với trích xuất metadata
- **Bảo toàn định dạng**: Định dạng Telegram (đậm, nghiêng, code, spoiler, quote)
- **Chuỗi trả lời**: Hỗ trợ chuỗi hội thoại hoàn chỉnh

### 🔍 **Tính năng Phân tích Nâng cao**
- **Thu thập URL**: Trích xuất tất cả URL t.me từ tin nhắn với phân loại
- **Phân tích chuyển tiếp**: Theo dõi mạng lưới lan truyền tin nhắn qua các kênh
- **Phân tích phản ứng**: Tính toán chỉ số tương tác và mẫu phản ứng
- **Mạng lưới tương tác người dùng**: Trực quan hóa mối quan hệ và mẫu giao tiếp
- **Trích xuất thông tin tình báo**: Trích xuất số điện thoại, email, IP và địa chỉ crypto
- **Trích xuất dữ liệu GPS**: Trích xuất dữ liệu vị trí từ file phương tiện
- **Nhận dạng thực thể có tên**: Xác định người, tổ chức và địa điểm
- **Phân tích ý thức hệ**: Phát hiện ngôn ngữ thù địch, cực đoan và chỉ số âm mưu
- **Đánh giá mối đe dọa**: Tự động chấm điểm mối đe dọa và đánh giá rủi ro

### 📈 **Trực quan hóa Chuyên nghiệp**
- **Biểu đồ & Đồ thị**: Báo cáo PDF với matplotlib và reportlab
- **Mạng xã hội**: Trực quan hóa PNG sử dụng networkx và matplotlib
- **Bản đồ tương tác**: Bản đồ HTML với folium cho dữ liệu vị trí
- **Báo cáo văn bản**: Báo cáo PDF toàn diện với phát hiện chi tiết

### 🔄 **Di chuyển & Tương thích**
- **Di chuyển tự động**: Chuyển đổi liền mạch từ kiến trúc cũ sang mới
- **Tương thích ngược**: Hoạt động với cấu hình hiện có
- **Bảo toàn dữ liệu**: Tất cả dữ liệu hiện có được bảo toàn trong quá trình di chuyển

## 🚀 Bắt đầu nhanh

### 1. **Cài đặt**

```bash
# Clone repository
git clone https://github.com/vsmz4laj7n/TEx.git
cd TEx

# Cài đặt dependencies
pip install -r requirements.txt
```

### 2. **Cấu hình**

Tạo hoặc cập nhật file `config.ini`:

```ini
[CONFIGURATION]
# Lấy từ https://my.telegram.org/apps
api_id=YOUR_API_ID_HERE
api_hash=YOUR_API_HASH_HERE
phone_number=YOUR_PHONE_NUMBER_HERE
data_path=./DataPath/
device_model=Desktop
timeout=30
```

### 3. **Kết nối Telegram**

```bash
python3 -m TEx connect --config config.ini
```

### 4. **Tải xuống tin nhắn với tính năng nâng cao**

```bash
# Tải xuống tin nhắn cơ bản
python3 -m TEx download_messages --config config.ini --group_id 123456789

# Với tính năng phân tích
python3 -m TEx download_messages --config config.ini --group_id 123456789 --url-scraping --forwarding-analysis --reaction-analysis

# Với thu thập hồ sơ người dùng
python3 -m TEx download_messages --config config.ini --group_id 123456789 --scrape_user_profiles
```

### 5. **Phân tích dữ liệu đã tải xuống**

```bash
# Phân tích toàn diện
python3 -m TEx analyze --config config.ini --group_id 123456789 --visual-data --generate-visualizations
```

## 📁 Cấu trúc thư mục

```
DataPath/
├── 2964586611/                    # Thư mục riêng cho nhóm (đặt tên theo ID nhóm)
│   ├── message.db                 # Tin nhắn + tham chiếu phương tiện + kiểm soát phiên bản
│   ├── user.db                    # Thành viên nhóm + bio + ảnh đại diện
│   └── media/                     # File phương tiện của nhóm
│       ├── photos/
│       ├── videos/
│       ├── documents/
│       ├── audio/
│       ├── voice/
│       └── stickers/
├── UserProfile/                   # Hồ sơ người dùng toàn cục
│   ├── 123456789.db              # Cơ sở dữ liệu người dùng cá nhân
│   ├── 987654321.db
│   ├── Media/
│   │   ├── ProfilePicture/        # Ảnh đại diện (UID-DateTime.jpg)
│   │   └── PublicPosts/           # Bài đăng công khai (nếu có)
│   └── user_index.db              # Chỉ mục tất cả người dùng
└── visualizations/                # Trực quan hóa được tạo
    ├── 123456789/                 # Trực quan hóa riêng cho nhóm
    │   ├── group_123456789_charts.pdf
    │   ├── group_123456789_social_network.png
    │   ├── group_123456789_map.html
    │   └── group_123456789_analysis_report.pdf
    └── cross_group_analysis/      # So sánh đa nhóm
```

## 🔧 Cấu hình

### **Cấu hình Pipeline**
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

### **Cấu hình Phân tích**
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

## 📊 Mô hình Cơ sở dữ liệu

### **Mô hình Tin nhắn Nâng cao**
```python
class TelegramMessageOrmEntity:
    # Khóa chính
    id: int
    group_id: int
    
    # Nội dung
    message: str
    text_entities: str  # Thực thể định dạng JSON
    raw: str
    
    # Phương tiện
    has_media: bool
    media_type: str
    media_path: str
    
    # Kiểm soát phiên bản
    version: int
    is_edited: bool
    is_deleted: bool
    edit_date: datetime
    delete_date: datetime
    message_status: str  # active, edited, deleted
```

### **Mô hình Người dùng Nâng cao**
```python
class TelegramUserOrmEntity:
    # Thông tin cơ bản
    id: int
    first_name: str
    last_name: str
    username: str
    phone_number: str
    
    # Hồ sơ
    bio: str
    about: str
    has_profile_photo: bool
    profile_photo_path: str
    profile_photo_date: datetime
    
    # Trạng thái
    status: str
    was_online: datetime
    
    # Kiểm soát phiên bản
    profile_version: int
    last_updated: datetime
```

## 🛡️ Quyền riêng tư & Bảo mật

### **Tính năng Quyền riêng tư**
- **Lưu trữ cục bộ**: Tất cả dữ liệu được lưu trữ cục bộ trên máy của bạn
- **Không tải lên đám mây**: Không có dữ liệu nào được gửi đến máy chủ bên ngoài
- **Giữ lại có thể cấu hình**: Thiết lập chính sách giữ lại dữ liệu của riêng bạn
- **Xác thực an toàn**: Sử dụng API chính thức của Telegram

### **Tính năng Bảo mật**
- **Bảo vệ khóa API**: Lưu trữ an toàn thông tin đăng nhập API Telegram
- **Giới hạn tốc độ**: Tự động trì hoãn để tránh giới hạn API
- **Xử lý lỗi**: Xử lý nhẹ nhàng các lỗi xác thực
- **Mã hóa dữ liệu**: Mã hóa tùy chọn cho dữ liệu nhạy cảm

## 🤝 Đóng góp

Chúng tôi hoan nghênh sự đóng góp! Vui lòng xem [Hướng dẫn Đóng góp](contributing.md) để biết chi tiết.

### **Thiết lập Phát triển**
```bash
# Clone repository
git clone https://github.com/vsmz4laj7n/TEx.git
cd TEx

# Cài đặt dependencies phát triển
pip install -r requirements.txt
pip install -r requirements-dev.txt

# Chạy tests
pytest tests/

# Chạy linting
ruff check .
```

## 📄 Giấy phép

Dự án này được cấp phép theo MIT License - xem file [LICENSE](LICENSE) để biết chi tiết.

## 🙏 Lời cảm ơn

- **TEx gốc**: Dựa trên Telegram Explorer gốc của Th3 0bservator
- **Telethon**: Thư viện client Telegram cho Python
- **SQLAlchemy**: Bộ công cụ cơ sở dữ liệu và ORM
- **Matplotlib/NetworkX/Folium**: Thư viện trực quan hóa
- **ReportLab**: Thư viện tạo PDF

## 📞 Hỗ trợ

- **Tài liệu**: [https://enhanced-tex.readthedocs.io/](https://enhanced-tex.readthedocs.io/)
- **Vấn đề**: [GitHub Issues](https://github.com/vsmz4laj7n/TEx/issues)
- **Thảo luận**: [GitHub Discussions](https://github.com/vsmz4laj7n/TEx/discussions)

---

**Enhanced TEx Team** - Làm cho phân tích dữ liệu Telegram trở nên chuyên nghiệp và toàn diện.