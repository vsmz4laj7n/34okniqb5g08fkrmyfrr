# Tải Tin nhắn

Hướng dẫn sử dụng lệnh `download_messages` để tải lịch sử tin nhắn từ các nhóm và kênh Telegram.

## Tổng quan

Lệnh `download_messages` cho phép bạn tải xuống tất cả tin nhắn từ một hoặc nhiều nhóm Telegram. TEx hỗ trợ nhiều tùy chọn để tùy chỉnh quá trình tải, bao gồm giới hạn số lượng, tải media, và các chiến lược tải khác nhau.

## Cú pháp cơ bản

```bash
python -m TEx download_messages --config config.ini --group_id GROUP_ID
```

## Tham số bắt buộc

| Tham số | Mô tả | Ví dụ |
|---------|-------|-------|
| `--config` | Đường dẫn đến tệp cấu hình | `--config config.ini` |
| `--group_id` | ID của nhóm cần tải tin nhắn | `--group_id 123456789` |

## Tùy chọn

### Tùy chọn cơ bản

| Tùy chọn | Mô tả | Mặc định |
|----------|-------|----------|
| `--limit` | Giới hạn số tin nhắn tải | Không giới hạn |
| `--ignore_media` | Không tải media | Tải tất cả media |
| `--latest` | Chỉ tải tin nhắn mới nhất | Tải tất cả tin nhắn |
| `--force-rescrape` | Tải lại tất cả tin nhắn | Chỉ tải tin nhắn mới |

### Tùy chọn nâng cao

| Tùy chọn | Mô tả | Mặc định |
|----------|-------|----------|
| `--refresh_profile_photos` | Làm mới ảnh đại diện | Không làm mới |
| `--scrape-new-only` | Chỉ tải tin nhắn mới | `True` |

## Ví dụ sử dụng

### Tải cơ bản
```bash
# Tải tất cả tin nhắn từ một nhóm
python -m TEx download_messages --config config.ini --group_id 123456789
```

### Tải với giới hạn
```bash
# Tải tối đa 1000 tin nhắn
python -m TEx download_messages --config config.ini --group_id 123456789 --limit 1000
```

### Tải tin nhắn mới nhất
```bash
# Chỉ tải tin nhắn mới nhất (từ lần tải cuối)
python -m TEx download_messages --config config.ini --group_id 123456789 --latest
```

### Tải không bao gồm media
```bash
# Tải tin nhắn nhưng bỏ qua media
python -m TEx download_messages --config config.ini --group_id 123456789 --ignore_media
```

### Tải lại tất cả tin nhắn
```bash
# Tải lại tất cả tin nhắn (bỏ qua cache)
python -m TEx download_messages --config config.ini --group_id 123456789 --force-rescrape
```

### Tải nhiều nhóm
```bash
# Tải từ nhiều nhóm cùng lúc
python -m TEx download_messages --config config.ini --group_id "123456789,987654321,555666777"
```

### Tải với làm mới ảnh đại diện
```bash
# Tải tin nhắn và làm mới ảnh đại diện người dùng
python -m TEx download_messages --config config.ini --group_id 123456789 --refresh_profile_photos
```

## Cấu hình tải

### Cấu hình trong config.ini

```ini
[DOWNLOAD_CONFIG]
# Giới hạn tin nhắn mặc định
default_message_limit=None

# Kích thước batch cho việc tải
batch_size=100

# Độ trễ giữa các batch (giây)
batch_delay=0.1

# Độ trễ giữa các tin nhắn (giây)
message_delay=0.05

# Khoảng thời gian cập nhật tiến trình
progress_update_interval=100

# Bật đếm tin nhắn nhanh
fast_counting_enabled=True

# Bật kiểm tra thay đổi trạng thái
status_check_enabled=True

# Kích thước batch kiểm tra trạng thái
status_check_batch_size=1000
```

### Cấu hình media

```ini
[MEDIA.DOWNLOAD]
# Chính sách tải media mặc định
default=ALLOW

# Kích thước tối đa cho file media (byte)
max_download_size_bytes=256000000
```

## Chiến lược tải

### 1. Tải mặc định (Default)
- Tải tất cả tin nhắn từ nhóm
- Bao gồm media và ảnh đại diện
- Sử dụng cache để tránh tải lại

### 2. Tải tin nhắn mới nhất (Latest Only)
- Chỉ tải tin nhắn chưa có trong cơ sở dữ liệu
- Sử dụng ID tin nhắn cao nhất để xác định tin nhắn mới
- Hiệu quả cho việc cập nhật thường xuyên

### 3. Tải lại tất cả (Force Rescrape)
- Bỏ qua cache và tải lại tất cả tin nhắn
- Hữu ích khi cần cập nhật dữ liệu đã bị lỗi
- Có thể mất nhiều thời gian hơn

### 4. Tải có giới hạn (Limited)
- Tải số lượng tin nhắn cụ thể
- Hữu ích cho việc kiểm tra hoặc phân tích mẫu
- Có thể chỉ định giới hạn theo thời gian

## Theo dõi tiến trình

Trong quá trình tải, TEx sẽ hiển thị thông tin tiến trình:

```
2025-08-28 10:30:15,123 - INFO - Bắt đầu tải tin nhắn cho nhóm 123456789
2025-08-28 10:30:15,456 - INFO - Đếm nhanh: 50,000 tin nhắn tổng cộng, 1,234 tin nhắn mới
2025-08-28 10:30:15,789 - INFO - Tải batch 1/50 (100 tin nhắn)
2025-08-28 10:30:16,012 - INFO - Tiến trình: 100/50,000 tin nhắn đã xử lý (49,900 còn lại)
...
2025-08-28 10:35:20,456 - INFO - Hoàn thành tải: 50,000/50,000 tin nhắn cho nhóm 123456789
```

## Xử lý lỗi

### Lỗi thường gặp

#### 1. Lỗi xác thực
```bash
# Xóa phiên và xác thực lại
rm -rf data/session/
python -m TEx connect --config config.ini
```

#### 2. Lỗi giới hạn tốc độ
```bash
# Tăng độ trễ trong config.ini
batch_delay=0.5
message_delay=0.1
```

#### 3. Lỗi bộ nhớ
```bash
# Giảm kích thước batch
batch_size=50
```

#### 4. Lỗi cơ sở dữ liệu
```bash
# Sửa lỗi cơ sở dữ liệu
python -m TEx fix_database --config config.ini
```

### Khắc phục sự cố

#### Kiểm tra kết nối
```bash
# Kiểm tra kết nối Telegram
python -m TEx connect --config config.ini
```

#### Kiểm tra quyền truy cập
```bash
# Liệt kê các nhóm có sẵn
python -m TEx list_groups --config config.ini
```

#### Kiểm tra dung lượng ổ đĩa
```bash
# Kiểm tra dung lượng trống
df -h data/
```

## Tối ưu hóa hiệu suất

### 1. Điều chỉnh batch size
- **Tăng** `batch_size` để tải nhanh hơn (có thể gây lỗi bộ nhớ)
- **Giảm** `batch_size` để ổn định hơn (chậm hơn)

### 2. Điều chỉnh độ trễ
- **Tăng** `batch_delay` và `message_delay` để tránh giới hạn tốc độ
- **Giảm** để tải nhanh hơn (có thể bị chặn)

### 3. Sử dụng SSD
- Lưu trữ dữ liệu trên SSD để cải thiện hiệu suất I/O
- Đặc biệt quan trọng cho các nhóm có nhiều media

### 4. Tối ưu hóa bộ nhớ
- Giám sát sử dụng bộ nhớ trong quá trình tải
- Điều chỉnh `batch_size` phù hợp với RAM có sẵn

## Lưu ý quan trọng

### Giới hạn API
- Telegram có giới hạn tốc độ cho API
- Sử dụng độ trễ phù hợp để tránh bị chặn
- Không chạy nhiều instance cùng lúc

### Dung lượng lưu trữ
- Media có thể chiếm nhiều dung lượng
- Kiểm tra dung lượng trống trước khi tải
- Sử dụng `--ignore_media` nếu không cần media

### Quyền riêng tư
- Chỉ tải từ các nhóm bạn có quyền truy cập
- Tôn trọng quyền riêng tư của người dùng
- Tuân thủ điều khoản dịch vụ của Telegram

### Bảo mật dữ liệu
- Dữ liệu được lưu trữ cục bộ
- Không chia sẻ dữ liệu với bên thứ ba
- Bảo vệ tệp cấu hình chứa thông tin đăng nhập