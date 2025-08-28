# TEx Documentation - Tiếng Việt

Chào mừng bạn đến với tài liệu TEx (Enhanced Telegram Explorer) bằng tiếng Việt. Hướng dẫn toàn diện này bao gồm tất cả các khía cạnh sử dụng TEx để trích xuất, phân tích và giám sát dữ liệu Telegram.

## 🚀 Hướng dẫn nhanh

### 1. Cài đặt
```bash
# Cài đặt TEx
pip install TelegramExplorer

# Hoặc từ mã nguồn
git clone https://github.com/vsmz4laj7n/TEx.git
cd TEx
pip install -r requirements.txt
pip install -e .
```

### 2. Cấu hình
```bash
# Sao chép mẫu cấu hình
cp TEx/config.ini config.ini

# Chỉnh sửa với thông tin đăng nhập Telegram API của bạn
nano config.ini
```

### 3. Chạy lần đầu
```bash
# Kết nối Telegram
python -m TEx connect --config config.ini

# Tải danh sách nhóm
python -m TEx load_groups --config config.ini

# Tải tin nhắn
python -m TEx download_messages --config config.ini --group_id 123456789

# Phân tích dữ liệu
python -m TEx analyze --config config.ini --group_id 123456789 --visual-data
```

## 📖 Hướng dẫn sử dụng

### Lệnh cơ bản

#### Kết nối & Thiết lập
- **[Xác thực](authentication.md)** - Thiết lập thông tin đăng nhập Telegram API
- **[Cài đặt](installation.md)** - Hướng dẫn cài đặt hoàn chỉnh
- **[Cấu hình cơ bản](configuration/basic.md)** - Cài đặt cấu hình cần thiết

#### Thu thập dữ liệu
- **[Tải tin nhắn](user-guide/download_messages.md)** - Tải lịch sử tin nhắn từ các nhóm
- **[Giám sát thời gian thực](user-guide/listen.md)** - Giám sát tin nhắn liên tục
- **[Thu thập thông tin người dùng](user-guide/user_scraping.md)** - Trích xuất hồ sơ và dữ liệu người dùng
- **[Quản lý nhóm](user-guide/groups.md)** - Tải và quản lý các nhóm Telegram

#### Phân tích & Tình báo
- **[Phân tích dữ liệu](user-guide/analysis.md)** - Phân tích dữ liệu đã tải
- **[Tạo dữ liệu trực quan](user-guide/visual_data.md)** - Tạo biểu đồ và báo cáo
- **[Tính năng tình báo](user-guide/intelligence.md)** - Trích xuất PII và đánh giá mối đe dọa
- **[Phân tích mạng lưới](user-guide/networks.md)** - Mạng lưới tương tác người dùng

#### Xuất & Báo cáo
- **[Tạo báo cáo](user-guide/reports.md)** - Tạo báo cáo toàn diện
- **[Xuất văn bản](user-guide/export_text.md)** - Xuất dữ liệu văn bản đã lọc
- **[Xuất tệp](user-guide/export_file.md)** - Xuất tệp media
- **[Xuất HTML](user-guide/export_html.md)** - Xuất sang định dạng HTML
- **[Xuất Gephi](user-guide/gephi_export.md)** - Xuất dữ liệu mạng lưới để phân tích

#### Bảo trì
- **[Quản lý cơ sở dữ liệu](user-guide/database.md)** - Thao tác và bảo trì cơ sở dữ liệu
- **[Dọn dẹp dữ liệu](user-guide/cleanup.md)** - Xóa dữ liệu và tệp cũ
- **[Khắc phục sự cố](user-guide/troubleshooting.md)** - Các vấn đề thường gặp và giải pháp

## 🔧 Cấu hình

### Tệp cấu hình
- **[Cấu hình cơ bản](configuration/basic.md)** - Cài đặt cần thiết để bắt đầu
- **[Ví dụ cấu hình hoàn chỉnh](configuration/complete_configuration_file_example.md)** - Tham khảo config.ini đầy đủ
- **[Cấu hình tải media](configuration/media_download_configuration.md)** - Cài đặt xử lý media
- **[Cấu hình phân tích](configuration/analysis_configuration.md)** - Cài đặt phân tích và trực quan hóa

### Các phần cấu hình

#### Cấu hình cốt lõi
```ini
[CONFIGURATION]
api_id=YOUR_API_ID_HERE
api_hash=YOUR_API_HASH_HERE
phone_number=YOUR_PHONE_NUMBER_HERE
data_path=./data/
device_model=Desktop
timeout=30
```

#### Cấu hình tải
```ini
[DOWNLOAD_CONFIG]
default_message_limit=None
batch_size=100
batch_delay=0.1
message_delay=0.05
progress_update_interval=100
fast_counting_enabled=True
status_check_enabled=True
status_check_batch_size=1000
```

#### Cấu hình phân tích
```ini
[SELECTOR_PATTERNS]
phone_pattern=\b(?:\+?1[-.]?)?\(?([0-9]{3})\)?[-.]?([0-9]{3})[-.]?([0-9]{4})\b
email_pattern=\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b
url_pattern=https?://(?:[-\w.])+(?:[:\d]+)?(?:/(?:[\w/_.])*(?:\?(?:[\w&=%.])*)?(?:#(?:[\w.])*)?)?
ip_pattern=\b(?:[0-9]{1,3}\.){3}[0-9]{1,3}\b
crypto_pattern=\b[13][a-km-zA-HJ-NP-Z1-9]{25,34}\b
```

## 🔍 Tính năng nâng cao

### Các module phân tích

#### Phân tích URL
- Trích xuất và phân loại liên kết t.me và các URL khác
- Theo dõi sự lan truyền URL qua các nhóm
- Phân tích mẫu chia sẻ liên kết

#### Phân tích chuyển tiếp
- Theo dõi mạng lưới lan truyền tin nhắn
- Xác định nguồn gốc và mẫu lan truyền
- Phân tích hành vi và thời gian chuyển tiếp

#### Phân tích phản ứng
- Phân tích mức độ tương tác người dùng qua phản ứng
- Xác định nội dung và người dùng phổ biến
- Theo dõi xu hướng phản ứng theo thời gian

#### Trích xuất bộ chọn
- Trích xuất số điện thoại, email, IP
- Xác định địa chỉ tiền điện tử
- Trích xuất URL và liên kết mạng xã hội

#### Trích xuất dữ liệu GPS
- Trích xuất dữ liệu vị trí từ metadata EXIF của hình ảnh
- Tạo bản đồ tương tác với vị trí người dùng
- Tạo báo cáo phân tích dựa trên vị trí

#### Nhận dạng thực thể có tên
- Xác định tên người và bí danh
- Trích xuất tên tổ chức
- Phát hiện địa điểm địa lý và ngày tháng

### Tính năng tình báo

#### Phân tích ý thức hệ
- Phát hiện ngôn ngữ thù địch và thuật ngữ cực đoan
- Xác định chỉ số lý thuyết âm mưu
- Phân tích thuật ngữ công dân có chủ quyền
- Hỗ trợ đa ngôn ngữ (Tiếng Anh và Tiếng Việt)

#### Đánh giá mối đe dọa
- Xác định chỉ số khả năng
- Phát hiện ý định bạo lực và mối đe dọa
- Phân tích mẫu mối đe dọa dựa trên khoảng cách
- Chấm điểm rủi ro và ưu tiên

#### Mạng lưới tương tác người dùng
- Lập bản đồ mối quan hệ và tương tác người dùng
- Xác định người có ảnh hưởng chính và cộng đồng
- Phân tích cấu trúc và động lực cộng đồng
- Xuất sang định dạng tương thích Gephi

#### Phân tích mẫu tin nhắn
- Phân tích mẫu đăng bài và tần suất
- Theo dõi hoạt động người dùng theo thời gian
- Xác định mẫu hành vi và xu hướng
- Tạo bản đồ nhiệt hoạt động

## 📊 Định dạng đầu ra

### Tạo dữ liệu trực quan
- **Biểu đồ PDF**: Biểu đồ và đồ thị thống kê
- **Đồ thị mạng xã hội PNG**: Mạng lưới tương tác người dùng
- **Bản đồ HTML**: Bản đồ tương tác với dữ liệu GPS
- **Báo cáo PDF**: Báo cáo phân tích toàn diện
- **Dữ liệu JSON**: Dữ liệu có cấu trúc để xử lý thêm

### Định dạng xuất Gephi
- **GEXF**: Định dạng XML trao đổi đồ thị
- **GraphML**: Ngôn ngữ đánh dấu đồ thị
- **CSV**: Giá trị phân tách bằng dấu phẩy (nút và cạnh)

### Định dạng báo cáo
- **HTML**: Báo cáo web tương tác với biểu đồ nhúng
- **Văn bản**: Xuất văn bản thuần với tùy chọn lọc
- **CSV**: Xuất dữ liệu có cấu trúc để phân tích
- **JSON**: Dữ liệu có thể đọc bằng máy để tự động hóa

## 🏗️ Kiến trúc

### Hệ thống pipeline
TEx sử dụng hệ thống pipeline mô-đun với các giai đoạn sau:

1. **Pre-Pipeline**: Khởi tạo, cấu hình, thiết lập cơ sở dữ liệu
2. **Core Pipeline**: Kết nối, tải nhóm, chức năng cơ bản
3. **Command-Specific Pipelines**:
   - **Download Pipeline**: Tải tin nhắn và media
   - **Listen Pipeline**: Giám sát thời gian thực
   - **Analysis Pipeline**: Phân tích dữ liệu và trực quan hóa
   - **Report Pipeline**: Tạo báo cáo và xuất
4. **Post-Pipeline**: Dọn dẹp và quản lý tài nguyên

### Cấu trúc cơ sở dữ liệu
- **Cơ sở dữ liệu theo nhóm**: `message.db`, `user.db` cho mỗi nhóm
- **Cơ sở dữ liệu người dùng toàn cục**: `user_index.db`, `user_profiles.db`
- **Schema nâng cao**: Cờ người dùng, metadata tin nhắn, theo dõi tương tác
- **Chế độ WAL**: Cập nhật cơ sở dữ liệu thời gian thực với tính đồng thời cải thiện

### Hệ thống module
- **Core modules**: Kết nối, cơ sở dữ liệu, quản lý cấu hình
- **Scraping modules**: Trích xuất tin nhắn, người dùng, media
- **Analysis modules**: Phân tích URL, chuyển tiếp, phản ứng, dữ liệu trực quan
- **Export modules**: Tạo báo cáo, xuất tệp, định dạng dữ liệu

## 🔒 Bảo mật & Quyền riêng tư

### Bảo vệ dữ liệu
- **Lưu trữ cục bộ**: Tất cả dữ liệu được lưu trữ cục bộ trên máy của bạn
- **Không tải lên đám mây**: Không có dữ liệu nào được gửi đến máy chủ bên ngoài
- **Lưu trữ có thể cấu hình**: Kiểm soát thời gian lưu trữ dữ liệu
- **Xác thực an toàn**: Chỉ xác thực Telegram API

### Tính năng quyền riêng tư
- **Sự đồng ý của người dùng**: Tôn trọng điều khoản dịch vụ của Telegram
- **Giới hạn tốc độ**: Giới hạn tốc độ tích hợp để tránh hạn chế API
- **Xử lý lỗi**: Xử lý nhẹ nhàng các hạn chế truy cập
- **Kiểm soát ghi log**: Mức độ ghi log có thể cấu hình

## 🐛 Khắc phục sự cố

### Các vấn đề thường gặp

#### Vấn đề xác thực
```bash
# Xóa phiên và xác thực lại
rm -rf data/session/
python -m TEx connect --config config.ini
```

#### Vấn đề cơ sở dữ liệu
```bash
# Sửa lỗi không nhất quán cơ sở dữ liệu
python -m TEx fix_database --config config.ini
```

#### Vấn đề bộ nhớ
```bash
# Giảm kích thước batch trong config.ini
batch_size=50
```

#### Giới hạn tốc độ
```bash
# Tăng độ trễ trong config.ini
batch_delay=0.5
message_delay=0.1
```

### Tối ưu hóa hiệu suất
- **Xử lý batch**: Cấu hình kích thước batch phù hợp
- **Quản lý bộ nhớ**: Theo dõi sử dụng bộ nhớ cho bộ dữ liệu lớn
- **Tối ưu hóa cơ sở dữ liệu**: Sử dụng lưu trữ SSD để hiệu suất tốt hơn
- **Tối ưu hóa mạng**: Cấu hình độ trễ và timeout phù hợp

## 📋 Tham khảo lệnh

### Các lệnh có sẵn

| Lệnh | Mô tả | Cách sử dụng |
|------|-------|--------------|
| `connect` | Tạo kết nối Telegram và lưu xác thực | `python -m TEx connect --config config.ini` |
| `load_groups` | Tải và làm mới danh sách nhóm và thành viên | `python -m TEx load_groups --config config.ini` |
| `download_messages` | Tải lịch sử tin nhắn | `python -m TEx download_messages --config config.ini --group_id 123456789` |
| `listen` | Lắng nghe tích cực tất cả các cuộc trò chuyện (giám sát liên tục) | `python -m TEx listen --config config.ini --group_id 123456789` |
| `analyze` | Phân tích dữ liệu đã tải | `python -m TEx analyze --config config.ini --group_id 123456789 --visual-data` |
| `report` | Tạo báo cáo với tin nhắn và media | `python -m TEx report --config config.ini --group_id 123456789` |
| `export_text` | Xuất tin nhắn sử dụng bộ lọc regex | `python -m TEx export_text --config config.ini --group_id 123456789` |
| `export_file` | Xuất tệp theo loại mime | `python -m TEx export_file --config config.ini --group_id 123456789` |
| `export_html` | Xuất tin nhắn sang định dạng HTML | `python -m TEx export_html --config config.ini --group_id 123456789` |
| `scrape_user_profiles` | Thu thập hồ sơ người dùng với ảnh và tiểu sử | `python -m TEx scrape_user_profiles --config config.ini --user_ids 123456789` |
| `user_scrape` | Thu thập hồ sơ người dùng với cập nhật cơ sở dữ liệu thời gian thực | `python -m TEx user_scrape --config config.ini --group-ids 123456789` |
| `list_groups` | Liệt kê tất cả các nhóm đã tải | `python -m TEx list_groups --config config.ini` |
| `stats` | Hiển thị thống kê từ các nhóm số điện thoại | `python -m TEx stats --config config.ini` |
| `purge_old_data` | Xóa tin nhắn và media cũ | `python -m TEx purge_old_data --config config.ini` |
| `fix_database` | Sửa lỗi không nhất quán cơ sở dữ liệu và đồng bộ dữ liệu người dùng | `python -m TEx fix_database --config config.ini` |

### Tùy chọn lệnh

#### Tùy chọn tải tin nhắn
```bash
# Tải cơ bản
python -m TEx download_messages --config config.ini --group_id 123456789

# Tải với tùy chọn
python -m TEx download_messages \
  --config config.ini \
  --group_id 123456789 \
  --limit 10000 \
  --ignore_media \
  --latest \
  --force-rescrape
```

#### Tùy chọn phân tích
```bash
# Phân tích toàn diện
python -m TEx analyze \
  --config config.ini \
  --group_id 123456789 \
  --visual-data \
  --generate-visualizations \
  --url-analysis \
  --forwarding-analysis \
  --reaction-analysis \
  --gephi-format GEXF
```

#### Tùy chọn xuất
```bash
# Xuất với bộ lọc
python -m TEx export_text \
  --config config.ini \
  --group_id 123456789 \
  --filter "từ khóa" \
  --limit_days 30 \
  --desc

# Xuất loại tệp cụ thể
python -m TEx export_file \
  --config config.ini \
  --group_id 123456789 \
  --mimetype "image/jpeg" \
  --limit_days 7
```

## 📈 Ví dụ & Hướng dẫn

### Hướng dẫn cơ bản
- **[Phân tích đầu tiên](tutorials/first_analysis.md)** - Quy trình phân tích đầu tiên hoàn chỉnh
- **[Giám sát thời gian thực](tutorials/monitoring.md)** - Thiết lập giám sát liên tục
- **[Phân tích người dùng](tutorials/user_analysis.md)** - Phân tích hành vi người dùng
- **[Phân tích mạng lưới](tutorials/network_analysis.md)** - Tạo mạng lưới tương tác

### Hướng dẫn nâng cao
- **[Phân tích đa nhóm](tutorials/cross_group.md)** - Phân tích nhiều nhóm
- **[Đánh giá mối đe dọa](tutorials/threat_assessment.md)** - Tiến hành phân tích mối đe dọa
- **[Phân tích tùy chỉnh](tutorials/custom_analysis.md)** - Tạo quy trình phân tích tùy chỉnh
- **[Tự động hóa báo cáo](tutorials/automation.md)** - Tự động hóa tạo báo cáo

## 🤝 Đóng góp

Chúng tôi hoan nghênh sự đóng góp! Vui lòng xem [Hướng dẫn đóng góp](../CONTRIBUTING.md) để biết chi tiết.

### Thiết lập phát triển
```bash
# Clone repository
git clone https://github.com/vsmz4laj7n/TEx.git
cd TEx

# Cài đặt dependencies phát triển
poetry install

# Chạy tests
poetry run pytest

# Chạy linting
poetry run ruff check .
```

### Phong cách code
- **Python**: Tuân theo hướng dẫn PEP 8
- **Type hints**: Sử dụng chú thích kiểu
- **Tài liệu**: Docstrings cho tất cả các hàm
- **Testing**: Unit tests cho tính năng mới

## 📞 Hỗ trợ

- **Tài liệu**: [https://enhanced-tex.readthedocs.io/](https://enhanced-tex.readthedocs.io/)
- **Vấn đề**: [GitHub Issues](https://github.com/vsmz4laj7n/TEx/issues)
- **Thảo luận**: [GitHub Discussions](https://github.com/vsmz4laj7n/TEx/discussions)

## 📄 Giấy phép

Dự án này được cấp phép theo Apache License 2.0 - xem tệp [LICENSE](../../LICENSE) để biết chi tiết.

---

**Enhanced TEx Team** - Xây dựng tương lai của phân tích dữ liệu và thu thập tình báo Telegram.