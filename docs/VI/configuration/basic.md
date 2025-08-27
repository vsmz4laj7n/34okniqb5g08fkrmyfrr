# ⚙️ Cấu hình Cơ bản

## Tổng quan

Enhanced TEx sử dụng file cấu hình INI để quản lý tất cả các thiết lập. File cấu hình chính thường được đặt tên là `config.ini` và chứa các phần khác nhau cho các tính năng khác nhau.

## Cấu trúc File Cấu hình

### File cấu hình mẫu

```ini
[CONFIGURATION]
# Thông tin đăng nhập Telegram API
api_id=YOUR_API_ID_HERE
api_hash=YOUR_API_HASH_HERE
phone_number=YOUR_PHONE_NUMBER_HERE

# Cài đặt cơ bản
data_path=./DataPath/
device_model=Desktop
timeout=30

# Cài đặt phân tích
enable_analysis=true
enable_visualizations=true

# Cài đặt bảo mật
encrypt_data=false
session_timeout=3600
```

## Các Phần Cấu hình

### [CONFIGURATION]

Phần chính chứa các cài đặt cơ bản:

```ini
[CONFIGURATION]
# API Credentials
api_id=12345678
api_hash=abcdef1234567890abcdef1234567890
phone_number=+84123456789

# Data Storage
data_path=./DataPath/
temp_path=./temp/

# Connection Settings
timeout=30
connection_retries=3
device_model=Desktop
system_version=Windows 10
app_version=2.0.0

# Analysis Settings
enable_analysis=true
enable_visualizations=true
enable_secret_chats=false

# Security Settings
encrypt_data=false
session_timeout=3600
require_authentication=true
```

### [PIPELINE]

Cấu hình pipeline xử lý:

```ini
[PIPELINE]
# Pipeline sequence
pipeline_sequence=telegram_connection_manager.TelegramConnector
                 telegram_groups_scrapper.TelegramGroupScrapper
                 telegram_groups_list.TelegramGroupList
                 enhanced_telegram_messages_scrapper.EnhancedTelegramMessagesScrapper
                 enhanced_user_profile_scraper.EnhancedUserProfileScraper
                 enhanced_url_scraper.EnhancedUrlScraper
                 enhanced_forwarding_analyzer.EnhancedForwardingAnalyzer
                 enhanced_reaction_analyzer.EnhancedReactionAnalyzer
                 enhanced_data_analyzer.EnhancedDataAnalyzer
                 enhanced_visualization_generator.EnhancedVisualizationGenerator
                 telegram_messages_listener.TelegramGroupMessageListener
                 telegram_report_generator.telegram_report_sent_telegram.TelegramReportSentViaTelegram
                 telegram_connection_manager.TelegramDisconnector

# Pre-pipeline sequence
pre_pipeline_sequence=database_handler.DatabaseHandler
                     temp_file_manager.TempFileManager
                     state_file_handler.LoadStateFileHandler
```

### [SELECTOR_PATTERNS]

Các mẫu regex để trích xuất thông tin:

```ini
[SELECTOR_PATTERNS]
# Mẫu regex cho số điện thoại
phone_pattern=\b(?:\+?1[-.]?)?\(?([0-9]{3})\)?[-.]?([0-9]{3})[-.]?([0-9]{4})\b

# Mẫu regex cho email
email_pattern=\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b

# Mẫu regex cho URL
url_pattern=https?://(?:[-\w.])+(?:[:\d]+)?(?:/(?:[\w/_.])*(?:\?(?:[\w&=%.])*)?(?:#(?:[\w.])*)?)?

# Mẫu regex cho IP address
ip_pattern=\b(?:[0-9]{1,3}\.){3}[0-9]{1,3}\b

# Mẫu regex cho địa chỉ crypto
crypto_pattern=\b[13][a-km-zA-HJ-NP-Z1-9]{25,34}\b
```

### [IDEOLOGICAL_KEYWORDS]

Từ khóa cho phân tích ý thức hệ:

```ini
[IDEOLOGICAL_KEYWORDS]
# Từ khóa ngôn ngữ thù địch
hate_speech=hate,racist,bigot,nazi,supremacist,white nationalist,anti-semitic

# Từ khóa cực đoan
extremism=extremist,radical,terrorist,jihad,white power,anarchist,revolutionary

# Từ khóa âm mưu
conspiracy=conspiracy,deep state,illuminati,chemtrails,flat earth,new world order

# Từ khóa sovereign citizen
sovereign_citizen=sovereign,freeman,common law,strawman,admiralty,maritime law

# Từ khóa incel
incel_terminology=incel,chad,stacy,blackpill,redpill,bluepill,mgtow
```

### [THREAT_KEYWORDS]

Từ khóa cho đánh giá mối đe dọa:

```ini
[THREAT_KEYWORDS]
# Từ khóa khả năng
capability=weapon,gun,bomb,explosive,knife,ammo,ammunition,firearm

# Từ khóa ý định bạo lực
violent_intent=kill,murder,attack,shoot,bomb,terror,assassinate,eliminate

# Từ khóa vũ khí
weapons=rifle,pistol,shotgun,ammunition,explosive,knife,grenade,molotov

# Từ khóa đe dọa
threatening=threat,warning,danger,attack,kill,destroy,eliminate,neutralize
```

## Cài đặt Nâng cao

### Proxy Configuration

```ini
[PROXY]
# Cài đặt proxy
enable_proxy=true
proxy_type=socks5
proxy_host=127.0.0.1
proxy_port=1080
proxy_username=user
proxy_password=pass
```

### Database Configuration

```ini
[DATABASE]
# Cài đặt cơ sở dữ liệu
database_type=sqlite
database_path=./DataPath/
backup_enabled=true
backup_interval=86400
encryption_enabled=false
```

### Logging Configuration

```ini
[LOGGING]
# Cài đặt logging
log_level=INFO
log_file=./logs/tex.log
log_format=%(asctime)s - %(name)s - %(levelname)s - %(message)s
log_rotation=daily
log_retention=30
```

## Validation và Testing

### Kiểm tra Cấu hình

```bash
# Kiểm tra cú pháp cấu hình
python3 -m TEx validate_config --config config.ini

# Kiểm tra kết nối
python3 -m TEx test_connection --config config.ini
```

### Validation Script

```python
import configparser
import os

def validate_config(config_path):
    config = configparser.ConfigParser()
    config.read(config_path)
    
    # Kiểm tra phần CONFIGURATION
    required_fields = ['api_id', 'api_hash', 'phone_number']
    for field in required_fields:
        if not config.has_option('CONFIGURATION', field):
            raise ValueError(f"Missing required field: {field}")
    
    # Kiểm tra đường dẫn
    data_path = config.get('CONFIGURATION', 'data_path')
    if not os.path.exists(data_path):
        os.makedirs(data_path)
    
    return True
```

## Best Practices

### Bảo mật

1. **Không commit credentials**: Thêm `config.ini` vào `.gitignore`
2. **Sử dụng biến môi trường**: Cho thông tin nhạy cảm
3. **Mã hóa file cấu hình**: Cho dữ liệu nhạy cảm
4. **Phân quyền file**: Chỉ cho phép owner đọc

### Performance

1. **Tối ưu timeout**: Điều chỉnh theo mạng
2. **Cấu hình cache**: Cho dữ liệu thường xuyên sử dụng
3. **Parallel processing**: Cho xử lý lớn
4. **Memory management**: Giới hạn sử dụng RAM

### Maintenance

1. **Backup cấu hình**: Sao lưu thường xuyên
2. **Version control**: Theo dõi thay đổi cấu hình
3. **Documentation**: Ghi chú thay đổi
4. **Testing**: Kiểm tra sau mỗi thay đổi

## Troubleshooting

### Common Issues

1. **Invalid API credentials**: Kiểm tra lại api_id và api_hash
2. **Permission denied**: Kiểm tra quyền truy cập file
3. **Path not found**: Tạo thư mục nếu chưa tồn tại
4. **Syntax error**: Kiểm tra cú pháp INI

### Debug Mode

```bash
# Bật debug mode
python3 -m TEx --debug --config config.ini
```

## Support

### Documentation

- **Advanced Configuration**: [Cấu hình nâng cao](advanced.md)
- **Media Download**: [Cấu hình tải media](media_download_configuration.md)
- **Proxy Setup**: [Cấu hình proxy](proxy.md)
- **OCR Configuration**: [Cấu hình OCR](ocr.md)

### Community

- **GitHub Issues**: [Báo cáo vấn đề](https://github.com/vsmz4laj7n/TEx/issues)
- **Discussions**: [Thảo luận](https://github.com/vsmz4laj7n/TEx/discussions)
- **Discord**: [Tham gia Discord](https://discord.gg/enhanced-tex)