# 🤐 Chat Bí mật

## Tổng quan

Enhanced TEx hỗ trợ thu thập dữ liệu từ các cuộc trò chuyện bí mật (Secret Chats) của Telegram. Chat bí mật cung cấp mã hóa end-to-end và tự hủy tin nhắn, đòi hỏi xử lý đặc biệt.

## Đặc điểm Chat Bí mật

### Tính năng Bảo mật
- **Mã hóa End-to-End**: Chỉ người tham gia có thể đọc tin nhắn
- **Tự hủy**: Tin nhắn tự động xóa sau thời gian định sẵn
- **Không đồng bộ**: Không lưu trữ trên máy chủ Telegram
- **Không chuyển tiếp**: Không thể chuyển tiếp tin nhắn

### Hạn chế Kỹ thuật
- **Chỉ hoạt động**: Chỉ có thể thu thập khi chat đang hoạt động
- **Không lưu trữ**: Không có lịch sử tin nhắn cũ
- **Yêu cầu quyền**: Cần quyền truy cập trực tiếp

## Cấu hình Chat Bí mật

### Bật Hỗ trợ Chat Bí mật

```ini
[CONFIGURATION]
enable_secret_chats=true
secret_chat_timeout=300
auto_accept_secret_chats=false
```

### Cài đặt Timeout

```ini
[CONFIGURATION]
# Timeout cho chat bí mật (giây)
secret_chat_timeout=300

# Tự động chấp nhận chat bí mật
auto_accept_secret_chats=false
```

## Sử dụng Chat Bí mật

### Thu thập Tin nhắn

```bash
# Thu thập từ chat bí mật cụ thể
python3 -m TEx download_messages --config config.ini --secret_chat_id CHAT_ID

# Thu thập tất cả chat bí mật
python3 -m TEx download_messages --config config.ini --all_secret_chats
```

### Lắng nghe Chat Bí mật

```bash
# Lắng nghe chat bí mật
python3 -m TEx listen --config config.ini --secret_chat_id CHAT_ID

# Lắng nghe tất cả chat bí mật
python3 -m TEx listen --config config.ini --all_secret_chats
```

## Xử lý Dữ liệu Chat Bí mật

### Lưu trữ An toàn

```ini
[SECRET_CHATS]
# Mã hóa dữ liệu chat bí mật
encrypt_data=true
encryption_key=YOUR_ENCRYPTION_KEY

# Tự động xóa sau thời gian
auto_delete_after=86400  # 24 giờ
```

### Backup và Khôi phục

```bash
# Backup dữ liệu chat bí mật
python3 -m TEx backup_secret_chats --config config.ini --output backup.db

# Khôi phục dữ liệu
python3 -m TEx restore_secret_chats --config config.ini --input backup.db
```

## Bảo mật và Quyền riêng tư

### Best Practices

1. **Mã hóa dữ liệu**: Luôn mã hóa dữ liệu chat bí mật
2. **Tự động xóa**: Thiết lập tự động xóa dữ liệu
3. **Quyền truy cập**: Hạn chế quyền truy cập
4. **Audit trail**: Ghi lại tất cả hoạt động

### Cấu hình Bảo mật

```ini
[SECURITY]
# Mã hóa dữ liệu nhạy cảm
encrypt_sensitive_data=true
encryption_algorithm=AES256

# Kiểm soát truy cập
require_authentication=true
session_timeout=3600

# Audit logging
enable_audit_log=true
audit_log_level=INFO
```

## Monitoring và Logging

### Log Files

```bash
# Xem logs chat bí mật
tail -f logs/secret_chats.log

# Xem audit logs
tail -f logs/audit.log
```

### Alerting

```ini
[ALERTS]
# Cảnh báo khi có chat bí mật mới
alert_new_secret_chat=true
alert_email=admin@example.com

# Cảnh báo khi có tin nhắn nhạy cảm
alert_sensitive_content=true
sensitive_keywords=keyword1,keyword2
```

## Troubleshooting

### Common Issues

1. **Chat không hoạt động**: Chat bí mật có thể đã bị hủy
2. **Timeout**: Tăng thời gian timeout
3. **Quyền truy cập**: Kiểm tra quyền truy cập
4. **Mã hóa**: Kiểm tra cấu hình mã hóa

### Debug Mode

```bash
# Bật debug mode
python3 -m TEx download_messages --config config.ini --debug --secret_chat_id CHAT_ID
```

## Compliance và Legal

### Tuân thủ Pháp luật

- **GDPR**: Tuân thủ quy định bảo vệ dữ liệu
- **Local Laws**: Tuân thủ luật pháp địa phương
- **Consent**: Đảm bảo có sự đồng ý hợp lệ

### Documentation

```bash
# Tạo báo cáo tuân thủ
python3 -m TEx compliance_report --config config.ini --output compliance.pdf
```

## Advanced Features

### Machine Learning Analysis

```bash
# Phân tích nội dung với ML
python3 -m TEx analyze_secret_chats --config config.ini --ml_analysis
```

### Pattern Detection

```bash
# Phát hiện mẫu hành vi
python3 -m TEx detect_patterns --config config.ini --secret_chats_only
```

### Threat Assessment

```bash
# Đánh giá mối đe dọa
python3 -m TEx threat_assessment --config config.ini --include_secret_chats
```

## Support

### Documentation

- **User Guide**: [Hướng dẫn sử dụng](user-guide/secret-chats.md)
- **API Reference**: [Tham chiếu API](api/secret-chats.md)
- **Troubleshooting**: [Xử lý sự cố](troubleshooting.md)

### Community

- **GitHub Issues**: [Báo cáo vấn đề](https://github.com/vsmz4laj7n/TEx/issues)
- **Discussions**: [Thảo luận](https://github.com/vsmz4laj7n/TEx/discussions)
- **Discord**: [Tham gia Discord](https://discord.gg/enhanced-tex)