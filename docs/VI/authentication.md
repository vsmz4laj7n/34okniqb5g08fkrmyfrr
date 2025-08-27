# 🔐 Xác thực

## Tổng quan

Enhanced TEx sử dụng API chính thức của Telegram để xác thực và kết nối với các máy chủ Telegram. Quá trình xác thực được thực hiện thông qua thư viện Telethon, cung cấp giao diện an toàn và đáng tin cậy cho API Telegram.

## Thiết lập API Credentials

### 1. Lấy API Credentials

1. **Truy cập Telegram API**: Đi đến [https://my.telegram.org/apps](https://my.telegram.org/apps)
2. **Đăng nhập**: Sử dụng số điện thoại của bạn để đăng nhập
3. **Tạo ứng dụng**: Tạo một ứng dụng mới
4. **Ghi chú thông tin**: Lưu lại `api_id` và `api_hash`

### 2. Cấu hình Credentials

Thêm thông tin đăng nhập vào file `config.ini`:

```ini
[CONFIGURATION]
api_id=YOUR_API_ID_HERE
api_hash=YOUR_API_HASH_HERE
phone_number=YOUR_PHONE_NUMBER_HERE
```

## Quá trình Xác thực

### 1. Kết nối lần đầu

```bash
python3 -m TEx connect --config config.ini
```

### 2. Xác thực qua SMS

- Nhập mã xác thực được gửi qua SMS
- Hoặc sử dụng mã xác thực từ ứng dụng Telegram

### 3. Xác thực hai yếu tố (nếu có)

- Nhập mật khẩu hai yếu tố nếu đã bật
- Lưu session để tránh xác thực lại

## Quản lý Session

### Lưu Session

Session được tự động lưu trong file `session` để tránh xác thực lại:

```
session.session
```

### Xóa Session

Để xóa session và xác thực lại:

```bash
rm session.session
python3 -m TEx connect --config config.ini
```

## Bảo mật

### Bảo vệ Credentials

- **Không chia sẻ**: Không chia sẻ `api_id` và `api_hash`
- **File cấu hình**: Bảo vệ file `config.ini`
- **Session file**: Bảo vệ file session

### Best Practices

- Sử dụng môi trường ảo
- Cập nhật thường xuyên
- Kiểm tra logs để phát hiện hoạt động bất thường

## Xử lý Sự cố

### Lỗi Xác thực

```bash
# Kiểm tra cấu hình
python3 -c "
import configparser
config = configparser.ConfigParser()
config.read('config.ini')
print(f'API ID: {config[\"CONFIGURATION\"][\"api_id\"]}')
print(f'API Hash: {config[\"CONFIGURATION\"][\"api_hash\"]}')
print(f'Phone: {config[\"CONFIGURATION\"][\"phone_number\"]}')
"
```

### Lỗi Kết nối

- Kiểm tra kết nối internet
- Kiểm tra firewall
- Thử lại sau vài phút

### Lỗi Rate Limiting

- Đợi vài phút trước khi thử lại
- Giảm tần suất yêu cầu
- Sử dụng proxy nếu cần

## Cấu hình Nâng cao

### Proxy Support

```ini
[CONFIGURATION]
proxy_type=socks5
proxy_host=127.0.0.1
proxy_port=1080
proxy_username=user
proxy_password=pass
```

### Timeout Settings

```ini
[CONFIGURATION]
timeout=30
connection_retries=3
```

### Device Model

```ini
[CONFIGURATION]
device_model=Desktop
system_version=Windows 10
app_version=2.0.0
```

## Monitoring

### Log Files

Kiểm tra logs để theo dõi hoạt động xác thực:

```bash
tail -f logs/tex.log
```

### Session Status

Kiểm tra trạng thái session:

```bash
python3 -m TEx status --config config.ini
```

## Troubleshooting

### Common Issues

1. **Invalid API Credentials**: Kiểm tra lại `api_id` và `api_hash`
2. **Phone Number Format**: Sử dụng định dạng quốc tế (+84...)
3. **Session Expired**: Xóa session và xác thực lại
4. **Rate Limiting**: Đợi và thử lại sau

### Support

Nếu gặp vấn đề:

- **GitHub Issues**: [Tạo issue](https://github.com/vsmz4laj7n/TEx/issues)
- **Documentation**: [Đọc docs](https://enhanced-tex.readthedocs.io/)
- **Discussions**: [Tham gia thảo luận](https://github.com/vsmz4laj7n/TEx/discussions)