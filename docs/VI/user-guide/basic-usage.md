# 📖 Hướng dẫn Sử dụng Cơ bản

## Tổng quan

Hướng dẫn này sẽ hướng dẫn bạn cách sử dụng Enhanced TEx để thu thập và phân tích dữ liệu từ Telegram một cách cơ bản.

## Bắt đầu Nhanh

### 1. Cài đặt và Cấu hình

```bash
# Clone repository
git clone https://github.com/vsmz4laj7n/TEx.git
cd TEx

# Cài đặt dependencies
pip install -r requirements.txt

# Tạo file cấu hình
cp config.ini.example config.ini
# Chỉnh sửa config.ini với thông tin API của bạn
```

### 2. Kết nối Telegram

```bash
# Kết nối lần đầu
python3 -m TEx connect --config config.ini
```

### 3. Liệt kê Nhóm

```bash
# Liệt kê tất cả nhóm
python3 -m TEx list_groups --config config.ini

# Liệt kê với thông tin chi tiết
python3 -m TEx list_groups --config config.ini --detailed
```

### 4. Tải xuống Tin nhắn

```bash
# Tải xuống tin nhắn từ một nhóm
python3 -m TEx download_messages --config config.ini --group_id 123456789

# Tải xuống với giới hạn
python3 -m TEx download_messages --config config.ini --group_id 123456789 --limit 1000

# Tải xuống không bao gồm media
python3 -m TEx download_messages --config config.ini --group_id 123456789 --ignore-media
```

## Các Lệnh Cơ bản

### Kết nối và Xác thực

#### `connect`
Kết nối với Telegram API:

```bash
python3 -m TEx connect --config config.ini
```

**Tùy chọn:**
- `--config`: File cấu hình
- `--force`: Buộc xác thực lại
- `--debug`: Bật chế độ debug

#### `disconnect`
Ngắt kết nối:

```bash
python3 -m TEx disconnect --config config.ini
```

### Quản lý Nhóm

#### `load_groups`
Tải thông tin nhóm:

```bash
# Tải tất cả nhóm
python3 -m TEx load_groups --config config.ini

# Tải với fast-fetch (chỉ thông tin cơ bản)
python3 -m TEx load_groups --config config.ini --fast-fetch

# Tải nhóm cụ thể
python3 -m TEx load_groups --config config.ini --group_id 123456789
```

#### `list_groups`
Liệt kê nhóm:

```bash
# Liệt kê cơ bản
python3 -m TEx list_groups --config config.ini

# Liệt kê chi tiết
python3 -m TEx list_groups --config config.ini --detailed

# Liệt kê từ API
python3 -m TEx list_groups --config config.ini --list-from-api
```

### Thu thập Dữ liệu

#### `download_messages`
Tải xuống tin nhắn:

```bash
# Tải xuống cơ bản
python3 -m TEx download_messages --config config.ini --group_id 123456789

# Với giới hạn
python3 -m TEx download_messages --config config.ini --group_id 123456789 --limit 5000

# Không bao gồm media
python3 -m TEx download_messages --config config.ini --group_id 123456789 --ignore-media

# Với phân tích
python3 -m TEx download_messages --config config.ini --group_id 123456789 --url-scraping --forwarding-analysis --reaction-analysis
```

#### `listen`
Lắng nghe tin nhắn mới:

```bash
# Lắng nghe cơ bản
python3 -m TEx listen --config config.ini --group_id 123456789

# Lắng nghe với phân tích
python3 -m TEx listen --config config.ini --group_id 123456789 --url-scraping --forwarding-analysis --reaction-analysis
```

### Phân tích Dữ liệu

#### `analyze`
Phân tích dữ liệu đã tải xuống:

```bash
# Phân tích URL
python3 -m TEx analyze --config config.ini --group_id 123456789 --url-analysis

# Phân tích chuyển tiếp
python3 -m TEx analyze --config config.ini --group_id 123456789 --forwarding-analysis

# Phân tích phản ứng
python3 -m TEx analyze --config config.ini --group_id 123456789 --reaction-analysis

# Phân tích toàn diện
python3 -m TEx analyze --config config.ini --group_id 123456789 --visual-data --generate-visualizations
```

### Xuất Dữ liệu

#### `export_html`
Xuất tin nhắn ra HTML:

```bash
# Xuất cơ bản
python3 -m TEx export_html --config config.ini --group_id 123456789

# Xuất với đường dẫn tùy chỉnh
python3 -m TEx export_html --config config.ini --group_id 123456789 --output_path ./exports/
```

#### `export_text`
Xuất ra file text:

```bash
python3 -m TEx export_text --config config.ini --group_id 123456789
```

#### `export_file`
Xuất ra file:

```bash
python3 -m TEx export_file --config config.ini --group_id 123456789
```

## Ví dụ Thực tế

### Thu thập Dữ liệu từ Kênh Tin tức

```bash
# 1. Kết nối
python3 -m TEx connect --config config.ini

# 2. Tải thông tin nhóm
python3 -m TEx load_groups --config config.ini --fast-fetch

# 3. Liệt kê để tìm ID nhóm
python3 -m TEx list_groups --config config.ini

# 4. Tải xuống tin nhắn với phân tích
python3 -m TEx download_messages --config config.ini --group_id 123456789 --limit 10000 --url-scraping --forwarding-analysis --reaction-analysis

# 5. Phân tích dữ liệu
python3 -m TEx analyze --config config.ini --group_id 123456789 --visual-data --generate-visualizations

# 6. Xuất báo cáo
python3 -m TEx export_html --config config.ini --group_id 123456789 --output_path ./reports/
```

### Giám sát Nhóm Liên tục

```bash
# 1. Lắng nghe tin nhắn mới
python3 -m TEx listen --config config.ini --group_id 123456789 --url-scraping --forwarding-analysis --reaction-analysis

# 2. Trong terminal khác, kiểm tra dữ liệu
python3 -m TEx analyze --config config.ini --group_id 123456789 --visual-data
```

### Thu thập Hồ sơ Người dùng

```bash
# Tải xuống tin nhắn và thu thập hồ sơ người dùng
python3 -m TEx download_messages --config config.ini --group_id 123456789 --scrape-user-profiles

# Thu thập hồ sơ người dùng cụ thể
python3 -m TEx scrape-user-profiles --config config.ini --user_ids 123456789,987654321
```

## Cấu trúc Dữ liệu

### Thư mục Dữ liệu

```
DataPath/
├── 123456789/                    # ID nhóm
│   ├── message.db                # Cơ sở dữ liệu tin nhắn
│   ├── user.db                   # Cơ sở dữ liệu người dùng
│   └── media/                    # File phương tiện
│       ├── photos/
│       ├── videos/
│       ├── documents/
│       └── audio/
├── UserProfile/                  # Hồ sơ người dùng toàn cục
│   ├── 123456789.db
│   ├── Media/
│   │   ├── ProfilePicture/
│   │   └── PublicPosts/
│   └── user_index.db
└── visualizations/               # Trực quan hóa
    └── 123456789/
        ├── group_123456789_charts.pdf
        ├── group_123456789_social_network.png
        ├── group_123456789_map.html
        └── group_123456789_analysis_report.pdf
```

### File Cấu hình

```ini
[CONFIGURATION]
api_id=YOUR_API_ID
api_hash=YOUR_API_HASH
phone_number=YOUR_PHONE_NUMBER
data_path=./DataPath/
timeout=30

[SELECTOR_PATTERNS]
phone_pattern=\b(?:\+?1[-.]?)?\(?([0-9]{3})\)?[-.]?([0-9]{3})[-.]?([0-9]{4})\b
email_pattern=\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b
url_pattern=https?://(?:[-\w.])+(?:[:\d]+)?(?:/(?:[\w/_.])*(?:\?(?:[\w&=%.])*)?(?:#(?:[\w.])*)?)?

[IDEOLOGICAL_KEYWORDS]
hate_speech=hate,racist,bigot,nazi,supremacist
extremism=extremist,radical,terrorist,jihad
conspiracy=conspiracy,deep state,illuminati

[THREAT_KEYWORDS]
capability=weapon,gun,bomb,explosive,knife
violent_intent=kill,murder,attack,shoot,bomb
weapons=rifle,pistol,shotgun,ammunition
threatening=threat,warning,danger,attack
```

## Best Practices

### Hiệu suất

1. **Sử dụng fast-fetch**: Cho danh sách nhóm lớn
2. **Giới hạn tin nhắn**: Sử dụng `--limit` để tránh quá tải
3. **Bỏ qua media**: Sử dụng `--ignore-media` nếu không cần
4. **Phân tích offline**: Sử dụng `analyze` thay vì phân tích real-time

### Bảo mật

1. **Bảo vệ credentials**: Không chia sẻ file config.ini
2. **Session management**: Xóa session khi không sử dụng
3. **Rate limiting**: Tôn trọng giới hạn API
4. **Data encryption**: Mã hóa dữ liệu nhạy cảm

### Maintenance

1. **Regular backups**: Sao lưu dữ liệu thường xuyên
2. **Cleanup**: Xóa dữ liệu cũ định kỳ
3. **Updates**: Cập nhật thường xuyên
4. **Monitoring**: Theo dõi logs và hiệu suất

## Troubleshooting

### Common Issues

1. **Connection failed**: Kiểm tra API credentials và kết nối internet
2. **Rate limited**: Đợi và thử lại sau
3. **Permission denied**: Kiểm tra quyền truy cập file
4. **Memory error**: Giảm giới hạn tin nhắn

### Debug Mode

```bash
# Bật debug mode
python3 -m TEx --debug --config config.ini download_messages --group_id 123456789
```

### Log Files

```bash
# Xem logs
tail -f logs/tex.log

# Xem logs lỗi
tail -f logs/error.log
```

## Support

### Documentation

- **Advanced Usage**: [Sử dụng nâng cao](advanced-usage.md)
- **Analysis Features**: [Tính năng phân tích](analysis-features.md)
- **Visualization**: [Trực quan hóa](visualization.md)
- **Configuration**: [Cấu hình](../configuration/basic.md)

### Community

- **GitHub Issues**: [Báo cáo vấn đề](https://github.com/vsmz4laj7n/TEx/issues)
- **Discussions**: [Thảo luận](https://github.com/vsmz4laj7n/TEx/discussions)
- **Discord**: [Tham gia Discord](https://discord.gg/enhanced-tex)