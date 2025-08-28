# Giám sát Thời gian thực

Hướng dẫn sử dụng lệnh `listen` để giám sát tin nhắn thời gian thực từ các nhóm và kênh Telegram.

## Tổng quan

Lệnh `listen` cho phép bạn giám sát liên tục các tin nhắn mới từ Telegram theo thời gian thực. Đây là tính năng mạnh mẽ để theo dõi hoạt động của các nhóm và kênh mà không cần tải lại toàn bộ lịch sử tin nhắn.

## Cú pháp cơ bản

```bash
python -m TEx listen --config config.ini --group_id GROUP_ID
```

## Tham số bắt buộc

| Tham số | Mô tả | Ví dụ |
|---------|-------|-------|
| `--config` | Đường dẫn đến tệp cấu hình | `--config config.ini` |
| `--group_id` | ID của nhóm cần giám sát | `--group_id 123456789` |

## Tùy chọn

### Tùy chọn cơ bản

| Tùy chọn | Mô tả | Mặc định |
|----------|-------|----------|
| `--ignore_media` | Không tải media | Tải tất cả media |
| `--url-scraping` | Bật trích xuất URL | Tắt |
| `--forwarding-analysis` | Bật phân tích chuyển tiếp | Tắt |
| `--reaction-analysis` | Bật phân tích phản ứng | Tắt |

## Ví dụ sử dụng

### Giám sát cơ bản
```bash
# Giám sát tin nhắn mới từ một nhóm
python -m TEx listen --config config.ini --group_id 123456789
```

### Giám sát không bao gồm media
```bash
# Giám sát tin nhắn nhưng bỏ qua media
python -m TEx listen --config config.ini --group_id 123456789 --ignore_media
```

### Giám sát với phân tích
```bash
# Giám sát với phân tích URL và chuyển tiếp
python -m TEx listen --config config.ini --group_id 123456789 \
  --url-scraping \
  --forwarding-analysis \
  --reaction-analysis
```

### Giám sát nhiều nhóm
```bash
# Giám sát nhiều nhóm cùng lúc
python -m TEx listen --config config.ini --group_id "123456789,987654321,555666777"
```

## Cách thức hoạt động

### 1. Kết nối liên tục
- TEx duy trì kết nối liên tục với Telegram
- Lắng nghe các sự kiện tin nhắn mới
- Xử lý tin nhắn theo thời gian thực

### 2. Xử lý tin nhắn
- Tự động tải tin nhắn mới khi được gửi
- Lưu trữ vào cơ sở dữ liệu
- Áp dụng các phân tích được cấu hình

### 3. Phân tích thời gian thực
- Trích xuất URL từ tin nhắn mới
- Phân tích chuyển tiếp tin nhắn
- Theo dõi phản ứng và tương tác

## Cấu hình giám sát

### Cấu hình cơ bản

```ini
[CONFIGURATION]
# Thời gian chờ kết nối
timeout=30

# Mô hình thiết bị
device_model=Desktop
```

### Cấu hình media

```ini
[MEDIA.DOWNLOAD]
# Chính sách tải media mặc định
default=ALLOW

# Kích thước tối đa cho file media (byte)
max_download_size_bytes=256000000
```

### Cấu hình phân tích

```ini
[SELECTOR_PATTERNS]
# Mẫu regex cho URL
url_pattern=https?://(?:[-\w.])+(?:[:\d]+)?(?:/(?:[\w/_.])*(?:\?(?:[\w&=%.])*)?(?:#(?:[\w.])*)?)?

# Mẫu regex cho số điện thoại
phone_pattern=\b(?:\+?1[-.]?)?\(?([0-9]{3})\)?[-.]?([0-9]{3})[-.]?([0-9]{4})\b
```

## Theo dõi hoạt động

### Log thời gian thực
```
2025-08-28 15:30:45,123 - INFO - Bắt đầu giám sát nhóm 123456789
2025-08-28 15:30:45,456 - INFO - Kết nối thành công với Telegram
2025-08-28 15:30:45,789 - INFO - Đang lắng nghe tin nhắn mới...
2025-08-28 15:31:12,345 - INFO - Nhận tin nhắn mới từ User123 (ID: 987654321)
2025-08-28 15:31:12,456 - INFO - Đã lưu tin nhắn ID: 12345
2025-08-28 15:31:12,567 - INFO - Phát hiện URL: https://t.me/example
2025-08-28 15:31:12,678 - INFO - Tin nhắn được chuyển tiếp từ nhóm khác
```

### Thống kê giám sát
```
2025-08-28 16:00:00,000 - INFO - Thống kê giám sát (30 phút):
- Tin nhắn đã nhận: 45
- URL được phát hiện: 12
- Tin nhắn chuyển tiếp: 8
- Phản ứng: 23
- Media được tải: 15
```

## Dừng giám sát

### Dừng an toàn
```bash
# Nhấn Ctrl+C để dừng giám sát an toàn
^C
2025-08-28 16:30:15,123 - INFO - Đang dừng giám sát...
2025-08-28 16:30:15,456 - INFO - Đã ngắt kết nối Telegram
2025-08-28 16:30:15,789 - INFO - Giám sát đã dừng thành công
```

### Dừng khẩn cấp
```bash
# Nhấn Ctrl+C hai lần để dừng khẩn cấp
^C
2025-08-28 16:30:20,123 - WARNING - Dừng khẩn cấp được yêu cầu
2025-08-28 16:30:20,456 - INFO - Đang dừng tất cả hoạt động...
```

## Xử lý lỗi

### Lỗi thường gặp

#### 1. Lỗi kết nối
```
2025-08-28 15:35:12,345 - ERROR - Lỗi kết nối Telegram: Connection timeout
2025-08-28 15:35:12,456 - INFO - Đang thử kết nối lại trong 30 giây...
2025-08-28 15:35:42,567 - INFO - Kết nối lại thành công
```

#### 2. Lỗi xác thực
```bash
# Xóa phiên và xác thực lại
rm -rf data/session/
python -m TEx connect --config config.ini
```

#### 3. Lỗi giới hạn tốc độ
```
2025-08-28 15:40:12,345 - WARNING - Đạt giới hạn tốc độ API
2025-08-28 15:40:12,456 - INFO - Đang chờ 60 giây trước khi tiếp tục...
```

### Khắc phục sự cố

#### Kiểm tra kết nối
```bash
# Kiểm tra kết nối Telegram
python -m TEx connect --config config.ini
```

#### Kiểm tra quyền truy cập
```bash
# Kiểm tra quyền truy cập nhóm
python -m TEx list_groups --config config.ini
```

#### Kiểm tra log
```bash
# Xem log chi tiết
tail -f tex.log
```

## Tối ưu hóa hiệu suất

### 1. Giám sát có chọn lọc
- Chỉ giám sát các nhóm quan trọng
- Sử dụng `--ignore_media` cho nhóm có nhiều media
- Tắt phân tích không cần thiết

### 2. Quản lý bộ nhớ
- Giám sát sử dụng bộ nhớ
- Định kỳ dừng và khởi động lại giám sát
- Sử dụng `purge_old_data` để dọn dẹp

### 3. Tối ưu hóa mạng
- Sử dụng kết nối internet ổn định
- Tránh giám sát quá nhiều nhóm cùng lúc
- Cấu hình timeout phù hợp

### 4. Lưu trữ dữ liệu
- Sử dụng SSD cho lưu trữ dữ liệu
- Định kỳ sao lưu cơ sở dữ liệu
- Giám sát dung lượng ổ đĩa

## Cấu hình nâng cao

### Giám sát tự động

#### Script khởi động
```bash
#!/bin/bash
# start_monitoring.sh

while true; do
    echo "Bắt đầu giám sát..."
    python -m TEx listen --config config.ini --group_id 123456789
    
    echo "Giám sát dừng, khởi động lại trong 30 giây..."
    sleep 30
done
```

#### Systemd service
```ini
[Unit]
Description=TEx Telegram Monitor
After=network.target

[Service]
Type=simple
User=tex
WorkingDirectory=/path/to/tex
ExecStart=/usr/bin/python3 -m TEx listen --config config.ini --group_id 123456789
Restart=always
RestartSec=30

[Install]
WantedBy=multi-user.target
```

### Giám sát với thông báo

#### Cấu hình Discord webhook
```ini
[NOTIFIER.DISCORD.MONITORING]
webhook=https://discord.com/api/webhooks/YOUR_WEBHOOK_URL
prevent_duplication_for_minutes=5
media_attachments_enabled=false
```

#### Cấu hình Elastic Search
```ini
[NOTIFIER.ELASTIC_SEARCH.MONITORING]
address=https://localhost:9200
api_key=YOUR_API_KEY
index_name=telegram_monitoring
pipeline_name=ent-search-generic-ingestion
```

## Lưu ý quan trọng

### Giới hạn API
- Telegram có giới hạn tốc độ cho API
- Không chạy nhiều instance giám sát cùng lúc
- Sử dụng độ trễ phù hợp

### Quyền riêng tư
- Chỉ giám sát các nhóm bạn có quyền truy cập
- Tôn trọng quyền riêng tư của người dùng
- Tuân thủ điều khoản dịch vụ của Telegram

### Bảo mật dữ liệu
- Dữ liệu được lưu trữ cục bộ
- Bảo vệ tệp cấu hình chứa thông tin đăng nhập
- Mã hóa dữ liệu nhạy cảm nếu cần

### Hiệu suất hệ thống
- Giám sát liên tục có thể tiêu tốn tài nguyên
- Định kỳ kiểm tra hiệu suất hệ thống
- Cân nhắc sử dụng máy chủ riêng cho giám sát dài hạn

### Sao lưu dữ liệu
- Định kỳ sao lưu cơ sở dữ liệu
- Lưu trữ log quan trọng
- Có kế hoạch khôi phục dữ liệu