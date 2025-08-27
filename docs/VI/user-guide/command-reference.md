# 📋 Tham chiếu Lệnh

## Tổng quan

Tham chiếu lệnh toàn diện này cung cấp thông tin chi tiết về tất cả các lệnh Enhanced TEx, các tùy chọn và ví dụ sử dụng. Mỗi lệnh được thiết kế cho chức năng cụ thể trong hệ sinh thái Enhanced TEx.

## Phân loại Lệnh

### 🔐 Xác thực & Kết nối
- [`connect`](#connect) - Thiết lập kết nối Telegram API

### 📊 Quản lý Dữ liệu
- [`load_groups`](#load_groups) - Tải thông tin nhóm
- [`list_groups`](#list_groups) - Liệt kê các nhóm có sẵn
- [`download_messages`](#download_messages) - Tải xuống lịch sử tin nhắn
- [`listen`](#listen) - Giám sát tin nhắn thời gian thực

### 🔍 Phân tích & Tình báo
- [`analyze`](#analyze) - Phân tích dữ liệu đã tải xuống
- [`scrape_user_profiles`](#scrape_user_profiles) - Trích xuất hồ sơ người dùng

### 📤 Xuất & Báo cáo
- [`export_html`](#export_html) - Xuất ra định dạng HTML
- [`export_text`](#export_text) - Xuất ra định dạng văn bản
- [`export_file`](#export_file) - Xuất file theo MIME type
- [`report`](#report) - Tạo báo cáo toàn diện
- [`stats`](#stats) - Hiển thị thống kê
- [`sent_report_telegram`](#sent_report_telegram) - Gửi báo cáo qua Telegram

### 🧹 Bảo trì
- [`purge_old_data`](#purge_old_data) - Dọn dẹp dữ liệu cũ
- [`purge_temp_files`](#purge_temp_files) - Xóa file tạm thời

---

## 🔐 Xác thực & Kết nối

### `connect`

**Mục đích**: Thiết lập kết nối đến Telegram API và lưu trữ thông tin xác thực.

**Mô tả**: Lệnh này tạo kết nối an toàn đến máy chủ Telegram và lưu trữ phiên xác thực để sử dụng trong tương lai. Nó xử lý toàn bộ quy trình xác thực bao gồm xác minh SMS và xác thực hai yếu tố nếu được bật.

#### Tùy chọn

| Tùy chọn | Loại | Bắt buộc | Mặc định | Mô tả |
|----------|------|----------|----------|-------|
| `--config` | string | ✅ Có | None | Đường dẫn file cấu hình chứa thông tin API |

#### Ví dụ Sử dụng

```bash
# Kết nối cơ bản
python3 -m TEx connect --config config.ini

# Kết nối với debug output
python3 -m TEx connect --config config.ini --debug

# Buộc xác thực lại
python3 -m TEx connect --config config.ini --force
```

#### Yêu cầu Cấu hình

File `config.ini` phải chứa:
```ini
[CONFIGURATION]
api_id=YOUR_API_ID
api_hash=YOUR_API_HASH
phone_number=YOUR_PHONE_NUMBER
```

#### Quy trình Xác thực

1. **Xác thực API**: Xác thực thông tin API
2. **Kiểm tra Phiên**: Kiểm tra phiên hiện có
3. **Xác thực**: Yêu cầu mã SMS nếu cần
4. **2FA**: Xử lý xác thực hai yếu tố nếu được bật
5. **Lưu trữ Phiên**: Lưu phiên để sử dụng trong tương lai

---

## 📊 Quản lý Dữ liệu

### `load_groups`

**Mục đích**: Tải xuống và làm mới thông tin nhóm và danh sách thành viên.

**Mô tả**: Lệnh này lấy thông tin nhóm/kênh từ Telegram và lưu trữ trong cơ sở dữ liệu cục bộ. Nó có thể hoạt động ở chế độ fast-fetch để lấy thông tin nhóm nhanh hoặc chế độ đầy đủ cho danh sách thành viên hoàn chỉnh.

#### Tùy chọn

| Tùy chọn | Loại | Bắt buộc | Mặc định | Mô tả |
|----------|------|----------|----------|-------|
| `--config` | string | ✅ Có | None | Đường dẫn file cấu hình |
| `--refresh_profile_photos` | flag | ❌ Không | False | Buộc làm mới tất cả ảnh đại diện |
| `--fast-fetch` | flag | ❌ Không | False | Chế độ tải nhanh: chỉ thông tin nhóm, bỏ qua danh sách thành viên |

#### Ví dụ Sử dụng

```bash
# Tải tất cả nhóm với danh sách thành viên đầy đủ
python3 -m TEx load_groups --config config.ini

# Chế độ tải nhanh (chỉ thông tin nhóm)
python3 -m TEx load_groups --config config.ini --fast-fetch

# Làm mới với ảnh đại diện
python3 -m TEx load_groups --config config.ini --refresh_profile_photos

# Tải nhanh với làm mới ảnh đại diện
python3 -m TEx load_groups --config config.ini --fast-fetch --refresh_profile_photos
```

#### Chế độ Fast Fetch

Khi `--fast-fetch` được bật, chỉ những thông tin sau được lấy:
- ID Nhóm/Kênh
- Tiêu đề/Tên
- Tên người dùng
- Mô tả/Bio
- Số lượng thành viên
- Ngày tạo
- Access hash

Điều này giảm đáng kể thời gian xử lý cho các nhóm lớn.

### `list_groups`

**Mục đích**: Liệt kê tất cả nhóm đã tải xuống từ cơ sở dữ liệu hoặc API.

**Mô tả**: Hiển thị thông tin về các nhóm đã được tải vào cơ sở dữ liệu. Cũng có thể lấy dữ liệu mới trực tiếp từ Telegram API.

#### Tùy chọn

| Tùy chọn | Loại | Bắt buộc | Mặc định | Mô tả |
|----------|------|----------|----------|-------|
| `--config` | string | ✅ Có | None | Đường dẫn file cấu hình |
| `--list_from_api` | flag | ❌ Không | False | Liệt kê nhóm trực tiếp từ Telegram API |

#### Ví dụ Sử dụng

```bash
# Liệt kê nhóm từ cơ sở dữ liệu
python3 -m TEx list_groups --config config.ini

# Liệt kê nhóm từ API (dữ liệu mới)
python3 -m TEx list_groups --config config.ini --list_from_api

# Liệt kê chi tiết
python3 -m TEx list_groups --config config.ini --detailed
```

#### Định dạng Đầu ra

```
Group ID: 123456789
Title: Example Group
Username: @example_group
Members: 1,234
Description: This is an example group description
Created: 2023-01-01 12:00:00
```

### `download_messages`

**Mục đích**: Tải xuống lịch sử tin nhắn từ các nhóm được chỉ định.

**Mô tả**: Đây là lệnh cốt lõi để trích xuất dữ liệu tin nhắn từ các nhóm/kênh Telegram. Nó hỗ trợ các tính năng toàn diện bao gồm tải xuống media, trích xuất hồ sơ người dùng và các tùy chọn phân tích nâng cao.

#### Tùy chọn

| Tùy chọn | Loại | Bắt buộc | Mặc định | Mô tả |
|----------|------|----------|----------|-------|
| `--config` | string | ✅ Có | None | Đường dẫn file cấu hình |
| `--group_id` | string | ❌ Không | '*' | ID nhóm mục tiêu (phân cách bằng dấu phẩy) |
| `--ignore_media` | flag | ❌ Không | False | Bỏ qua tải xuống file media |
| `--refresh_profile_photos` | flag | ❌ Không | False | Buộc làm mới ảnh đại diện |
| `--limit` | integer | ❌ Không | 10000 | Số lượng tin nhắn tối đa để tải xuống |
| `--url-scraping` | flag | ❌ Không | False | Bật trích xuất URL |
| `--forwarding-analysis` | flag | ❌ Không | False | Bật phân tích chuyển tiếp |
| `--reaction-analysis` | flag | ❌ Không | False | Bật phân tích phản ứng |

#### Ví dụ Sử dụng

```bash
# Tải xuống tin nhắn từ nhóm cụ thể
python3 -m TEx download_messages --config config.ini --group_id 123456789

# Tải xuống với giới hạn
python3 -m TEx download_messages --config config.ini --group_id 123456789 --limit 5000

# Bỏ qua tải xuống media
python3 -m TEx download_messages --config config.ini --group_id 123456789 --ignore_media

# Tải xuống với tất cả tính năng phân tích
python3 -m TEx download_messages --config config.ini --group_id 123456789 \
  --url-scraping --forwarding-analysis --reaction-analysis

# Tải xuống từ nhiều nhóm
python3 -m TEx download_messages --config config.ini --group_id "123456789,987654321"
```

#### Tính năng Phân tích

**Trích xuất URL** (`--url-scraping`):
- Trích xuất tất cả liên kết `t.me` từ tin nhắn
- Phân loại URL (kênh, người dùng, bot, liên kết tham gia)
- Lưu trữ metadata URL và phân tích

**Phân tích Chuyển tiếp** (`--forwarding-analysis`):
- Theo dõi mẫu chuyển tiếp tin nhắn
- Xác định nguồn gốc
- Tính toán độ dài chuỗi chuyển tiếp
- Xây dựng mạng chuyển tiếp

**Phân tích Phản ứng** (`--reaction-analysis`):
- Trích xuất phản ứng tin nhắn
- Tính toán chỉ số tương tác
- Phân tích mẫu phản ứng
- Xác định tỷ lệ tương tác

### `listen`

**Mục đích**: Giám sát tích cực các nhóm để lấy tin nhắn mới theo thời gian thực.

**Mô tả**: Liên tục lắng nghe các nhóm được chỉ định và xử lý tin nhắn mới khi chúng đến. Hỗ trợ các tính năng phân tích giống như `download_messages` nhưng hoạt động theo thời gian thực.

#### Tùy chọn

| Tùy chọn | Loại | Bắt buộc | Mặc định | Mô tả |
|----------|------|----------|----------|-------|
| `--config` | string | ✅ Có | None | Đường dẫn file cấu hình |
| `--group_id` | string | ❌ Không | '*' | ID nhóm mục tiêu (phân cách bằng dấu phẩy) |
| `--ignore_media` | flag | ❌ Không | False | Bỏ qua tải xuống file media |
| `--url-scraping` | flag | ❌ Không | False | Bật trích xuất URL |
| `--forwarding-analysis` | flag | ❌ Không | False | Bật phân tích chuyển tiếp |
| `--reaction-analysis` | flag | ❌ Không | False | Bật phân tích phản ứng |

#### Ví dụ Sử dụng

```bash
# Lắng nghe nhóm cụ thể
python3 -m TEx listen --config config.ini --group_id 123456789

# Lắng nghe với tính năng phân tích
python3 -m TEx listen --config config.ini --group_id 123456789 \
  --url-scraping --forwarding-analysis --reaction-analysis

# Lắng nghe nhiều nhóm
python3 -m TEx listen --config config.ini --group_id "123456789,987654321"
```

#### Tính năng Thời gian thực

- **Xử lý Tức thì**: Tin nhắn được xử lý khi chúng đến
- **Phân tích Trực tiếp**: Trích xuất URL và phân tích theo thời gian thực
- **Giám sát Liên tục**: Khả năng giám sát nhóm 24/7
- **Ghi Log Sự kiện**: Ghi log toàn diện tất cả hoạt động

---

## 🔍 Phân tích & Tình báo

### `analyze`

**Mục đích**: Phân tích dữ liệu đã tải xuống để có cái nhìn toàn diện.

**Mô tả**: Lệnh này thực hiện phân tích offline trên dữ liệu đã tải xuống trước đó. Nó có thể tạo ra các loại phân tích khác nhau bao gồm phân tích URL, mẫu chuyển tiếp, phân tích phản ứng và phân tích dữ liệu trực quan toàn diện.

#### Tùy chọn

| Tùy chọn | Loại | Bắt buộc | Mặc định | Mô tả |
|----------|------|----------|----------|-------|
| `--config` | string | ✅ Có | None | Đường dẫn file cấu hình |
| `--group_id` | string | ❌ Không | '*' | ID nhóm mục tiêu (phân cách bằng dấu phẩy) |
| `--url-analysis` | flag | ❌ Không | False | Phân tích URL từ tin nhắn đã lưu |
| `--forwarding-analysis` | flag | ❌ Không | False | Phân tích mối quan hệ chuyển tiếp |
| `--reaction-analysis` | flag | ❌ Không | False | Phân tích phản ứng và tương tác |
| `--visual-data` | flag | ❌ Không | False | Tạo phân tích dữ liệu trực quan toàn diện |
| `--generate-visualizations` | flag | ❌ Không | False | Tạo trực quan hóa chuyên nghiệp |

#### Ví dụ Sử dụng

```bash
# Phân tích URL cơ bản
python3 -m TEx analyze --config config.ini --group_id 123456789 --url-analysis

# Phân tích toàn diện
python3 -m TEx analyze --config config.ini --group_id 123456789 \
  --url-analysis --forwarding-analysis --reaction-analysis

# Phân tích dữ liệu trực quan
python3 -m TEx analyze --config config.ini --group_id 123456789 --visual-data

# Tạo trực quan hóa chuyên nghiệp
python3 -m TEx analyze --config config.ini --group_id 123456789 \
  --visual-data --generate-visualizations
```

#### Loại Phân tích

**Phân tích URL** (`--url-analysis`):
- Trích xuất tất cả URL từ văn bản tin nhắn
- Phân loại URL theo loại
- Phân tích mẫu và tần suất URL
- Tạo báo cáo phân phối URL

**Phân tích Chuyển tiếp** (`--forwarding-analysis`):
- Xác định tin nhắn được chuyển tiếp
- Theo dõi nguồn gốc
- Tính toán chỉ số chuyển tiếp
- Xây dựng mạng chuyển tiếp

**Phân tích Phản ứng** (`--reaction-analysis`):
- Phân tích phản ứng tin nhắn
- Tính toán tỷ lệ tương tác
- Xác định nội dung phổ biến
- Theo dõi mẫu phản ứng

**Phân tích Dữ liệu Trực quan** (`--visual-data`):
- Mạng tương tác người dùng
- Trích xuất selector (điện thoại, email, v.v.)
- Dữ liệu GPS từ media
- Phân tích mẫu tin nhắn
- Nhận dạng thực thể có tên
- Phân tích chỉ số ý thức hệ
- Đánh giá mối đe dọa

**Trực quan hóa Chuyên nghiệp** (`--generate-visualizations`):
- Biểu đồ và đồ thị PDF
- Sơ đồ mạng xã hội PNG
- Bản đồ HTML tương tác
- Báo cáo PDF toàn diện

### `scrape_user_profiles`

**Mục đích**: Trích xuất thông tin hồ sơ người dùng bao gồm ảnh và bio.

**Mô tả**: Trích xuất thông tin hồ sơ người dùng chi tiết từ Telegram, bao gồm ảnh đại diện, bio, trạng thái và metadata khác. Hỗ trợ trích xuất theo ID người dùng, tên người dùng hoặc từ người tham gia nhóm.

#### Tùy chọn

| Tùy chọn | Loại | Bắt buộc | Mặc định | Mô tả |
|----------|------|----------|----------|-------|
| `--config` | string | ✅ Có | None | Đường dẫn file cấu hình |
| `--user_ids` | string | ❌ Không | None | ID người dùng để trích xuất (phân cách bằng dấu phẩy) |
| `--usernames` | string | ❌ Không | None | Tên người dùng để trích xuất (phân cách bằng dấu phẩy) |
| `--group_ids` | string | ❌ Không | None | ID nhóm để trích xuất người tham gia |

#### Ví dụ Sử dụng

```bash
# Trích xuất người dùng cụ thể theo ID
python3 -m TEx scrape_user_profiles --config config.ini --user_ids 123456789

# Trích xuất nhiều người dùng theo ID
python3 -m TEx scrape_user_profiles --config config.ini --user_ids "123456789,987654321"

# Trích xuất theo tên người dùng
python3 -m TEx scrape_user_profiles --config config.ini --usernames "john_doe,jane_smith"

# Trích xuất tất cả người tham gia từ một nhóm
python3 -m TEx scrape_user_profiles --config config.ini --group_ids 123456789

# Trích xuất từ nhiều nhóm
python3 -m TEx scrape_user_profiles --config config.ini --group_ids "123456789,987654321"
```

#### Thông tin Hồ sơ Được Trích xuất

- **Thông tin Cơ bản**: ID người dùng, tên, họ, tên người dùng
- **Dữ liệu Hồ sơ**: Bio, about, trạng thái
- **Media**: Ảnh đại diện (với phát hiện trùng lặp MD5)
- **Metadata**: Lần cuối online, trạng thái liên hệ, liên hệ chung
- **Kiểm soát Phiên bản**: Theo dõi thay đổi hồ sơ theo thời gian

---

## 📤 Xuất & Báo cáo

### `export_html`

**Mục đích**: Xuất tin nhắn ra định dạng HTML chuyên nghiệp.

**Mô tả**: Tạo ra các bản xuất HTML đẹp mắt của tin nhắn Telegram với định dạng được bảo toàn, media và tính năng tương tác. Sử dụng template Jinja2 cho đầu ra có thể tùy chỉnh.

#### Tùy chọn

| Tùy chọn | Loại | Bắt buộc | Mặc định | Mô tả |
|----------|------|----------|----------|-------|
| `--config` | string | ✅ Có | None | Đường dẫn file cấu hình |
| `--group_id` | string | ❌ Không | '*' | ID nhóm mục tiêu (phân cách bằng dấu phẩy) |
| `--output_path` | string | ❌ Không | './html_export/' | Thư mục đầu ra cho file HTML |

#### Ví dụ Sử dụng

```bash
# Xuất nhóm cụ thể ra HTML
python3 -m TEx export_html --config config.ini --group_id 123456789

# Xuất với đường dẫn đầu ra tùy chỉnh
python3 -m TEx export_html --config config.ini --group_id 123456789 \
  --output_path ./reports/html/

# Xuất nhiều nhóm
python3 -m TEx export_html --config config.ini --group_id "123456789,987654321"
```

#### Tính năng Xuất HTML

- **Thiết kế Responsive**: Bố cục thân thiện với mobile
- **Giao diện Tối**: Bảng màu tối chuyên nghiệp
- **Định dạng Tin nhắn**: Bảo toàn định dạng Telegram (đậm, nghiêng, code, v.v.)
- **Hỗ trợ Media**: Nhúng hình ảnh, video và tài liệu
- **Tính năng Tìm kiếm**: Tìm kiếm toàn văn trong nội dung đã xuất
- **Điều hướng**: Điều hướng dễ dàng giữa các tin nhắn
- **Có thể Xuất**: Có thể chia sẻ hoặc lưu trữ

### `export_text`

**Mục đích**: Xuất tin nhắn sử dụng bộ lọc regex ra định dạng văn bản.

**Mô tả**: Xuất tin nhắn đã lọc ra định dạng văn bản thuần với bộ lọc có thể tùy chỉnh và tùy chọn sắp xếp.

#### Tùy chọn

| Tùy chọn | Loại | Bắt buộc | Mặc định | Mô tả |
|----------|------|----------|----------|-------|
| `--config` | string | ✅ Có | None | Đường dẫn file cấu hình |
| `--order_desc` | flag | ❌ Không | False | Sắp xếp theo ngày/giờ giảm dần |
| `--regex` | string | ❌ Không | None | Mẫu bộ lọc regex |
| `--limit_days` | integer | ❌ Không | 3650 | Giới hạn tin nhắn trong số ngày được chỉ định |
| `--report_folder` | string | ❌ Không | 'reports' | Thư mục đầu ra cho báo cáo |
| `--group_id` | string | ❌ Không | '*' | ID nhóm mục tiêu (phân cách bằng dấu phẩy) |

#### Ví dụ Sử dụng

```bash
# Xuất tất cả tin nhắn
python3 -m TEx export_text --config config.ini

# Xuất với bộ lọc regex
python3 -m TEx export_text --config config.ini --regex "keyword"

# Chỉ xuất tin nhắn gần đây
python3 -m TEx export_text --config config.ini --limit_days 30

# Xuất với sắp xếp tùy chỉnh
python3 -m TEx export_text --config config.ini --order_desc
```

### `export_file`

**Mục đích**: Xuất file theo MIME type với tùy chọn lọc.

**Mô tả**: Trích xuất và xuất file từ tin nhắn dựa trên MIME type, bộ lọc tên file và tiêu chí khác.

#### Tùy chọn

| Tùy chọn | Loại | Bắt buộc | Mặc định | Mô tả |
|----------|------|----------|----------|-------|
| `--config` | string | ✅ Có | None | Đường dẫn file cấu hình |
| `--mime_type` | string | ❌ Không | None | MIME type để xuất |
| `--limit_days` | integer | ❌ Không | 3650 | Giới hạn file trong số ngày được chỉ định |
| `--report_folder` | string | ❌ Không | 'reports' | Thư mục đầu ra cho báo cáo |
| `--group_id` | string | ❌ Không | '*' | ID nhóm mục tiêu (phân cách bằng dấu phẩy) |
| `--filter` | string | ❌ Không | '*' | Lọc theo phần tên file |

#### Ví dụ Sử dụng

```bash
# Xuất tất cả hình ảnh
python3 -m TEx export_file --config config.ini --mime_type "image/*"

# Xuất loại file cụ thể
python3 -m TEx export_file --config config.ini --mime_type "application/pdf"

# Xuất với bộ lọc tên file
python3 -m TEx export_file --config config.ini --mime_type "image/*" \
  --filter "screenshot,photo"

# Chỉ xuất file gần đây
python3 -m TEx export_file --config config.ini --mime_type "video/*" \
  --limit_days 7
```

### `report`

**Mục đích**: Tạo báo cáo toàn diện với tất cả tin nhắn và media.

**Mô tả**: Tạo báo cáo chi tiết bao gồm nội dung tin nhắn, tham chiếu media và metadata với các tùy chọn lọc và sắp xếp khác nhau.

#### Tùy chọn

| Tùy chọn | Loại | Bắt buộc | Mặc định | Mô tả |
|----------|------|----------|----------|-------|
| `--config` | string | ✅ Có | None | Đường dẫn file cấu hình |
| `--order_desc` | flag | ❌ Không | False | Sắp xếp theo ngày/giờ giảm dần |
| `--filter` | string | ❌ Không | None | Từ khóa lọc |
| `--limit_days` | integer | ❌ Không | 3650 | Giới hạn thời gian tin nhắn theo ngày |
| `--report_folder` | string | ❌ Không | 'reports' | Thư mục đầu ra báo cáo |
| `--around_messages` | integer | ❌ Không | 1 | Tin nhắn xung quanh nội dung được lọc |
| `--group_id` | string | ❌ Không | '*' | ID nhóm mục tiêu (phân cách bằng dấu phẩy) |
| `--suppress_repeating_messages` | flag | ❌ Không | False | Loại bỏ tin nhắn trùng lặp |

#### Ví dụ Sử dụng

```bash
# Tạo báo cáo cơ bản
python3 -m TEx report --config config.ini

# Báo cáo với bộ lọc
python3 -m TEx report --config config.ini --filter "important"

# Báo cáo với ngữ cảnh
python3 -m TEx report --config config.ini --filter "keyword" \
  --around_messages 5

# Báo cáo tin nhắn gần đây
python3 -m TEx report --config config.ini --limit_days 30 --order_desc
```

### `stats`

**Mục đích**: Hiển thị thống kê toàn diện từ nhóm, tin nhắn và tài sản.

**Mô tả**: Tạo thống kê chi tiết về dữ liệu đã thu thập bao gồm số lượng tin nhắn, thống kê media, hoạt động người dùng và nhiều hơn nữa.

#### Tùy chọn

| Tùy chọn | Loại | Bắt buộc | Mặc định | Mô tả |
|----------|------|----------|----------|-------|
| `--config` | string | ✅ Có | None | Đường dẫn file cấu hình |
| `--report_folder` | string | ❌ Không | 'reports' | Thư mục đầu ra báo cáo |
| `--limit_days` | integer | ❌ Không | 3650 | Giới hạn thời gian thống kê theo ngày |

#### Ví dụ Sử dụng

```bash
# Tạo thống kê cơ bản
python3 -m TEx stats --config config.ini

# Thống kê gần đây
python3 -m TEx stats --config config.ini --limit_days 30

# Thống kê với đầu ra tùy chỉnh
python3 -m TEx stats --config config.ini --report_folder ./stats/
```

#### Thống kê Được Tạo

- **Thống kê Tin nhắn**: Tổng tin nhắn, tin nhắn mỗi ngày, độ dài trung bình
- **Thống kê Media**: Số lượng file, loại, kích thước
- **Thống kê Người dùng**: Người dùng hoạt động, phân phối tin nhắn
- **Thống kê Nhóm**: Hoạt động nhóm, số lượng thành viên
- **Phân tích Thời gian**: Thời gian hoạt động cao điểm, xu hướng

### `sent_report_telegram`

**Mục đích**: Gửi báo cáo đã tạo đến người dùng Telegram qua tên người dùng.

**Mô tả**: Tự động gửi báo cáo đã tạo đến người dùng Telegram được chỉ định, hữu ích cho báo cáo tự động và chia sẻ.

#### Tùy chọn

| Tùy chọn | Loại | Bắt buộc | Mặc định | Mô tả |
|----------|------|----------|----------|-------|
| `--config` | string | ✅ Có | None | Đường dẫn file cấu hình |
| `--destination_username` | string | ✅ Có | None | Tên người dùng Telegram mục tiêu |
| `--report_folder` | string | ❌ Không | 'reports' | Thư mục báo cáo để gửi |
| `--title` | string | ✅ Có | 'TEx Report @@now@@' | Tiêu đề báo cáo |
| `--attachment_name` | string | ✅ Có | 'report_@@now@@' | Tên file đính kèm |

#### Ví dụ Sử dụng

```bash
# Gửi báo cáo đến người dùng
python3 -m TEx sent_report_telegram --config config.ini \
  --destination_username john_doe

# Gửi với tiêu đề tùy chỉnh
python3 -m TEx sent_report_telegram --config config.ini \
  --destination_username john_doe --title "Báo cáo Hàng ngày"

# Gửi thư mục báo cáo cụ thể
python3 -m TEx sent_report_telegram --config config.ini \
  --destination_username john_doe --report_folder ./daily_reports/
```

---

## 🧹 Bảo trì

### `purge_old_data`

**Mục đích**: Dọn dẹp tin nhắn cũ, media và dữ liệu khác.

**Mô tả**: Xóa dữ liệu cũ dựa trên tiêu chí tuổi để giải phóng không gian lưu trữ và duy trì hiệu suất cơ sở dữ liệu.

#### Tùy chọn

| Tùy chọn | Loại | Bắt buộc | Mặc định | Mô tả |
|----------|------|----------|----------|-------|
| `--config` | string | ✅ Có | None | Đường dẫn file cấu hình |
| `--limit_days` | integer | ❌ Không | 365 | Giới hạn tuổi theo ngày |

#### Ví dụ Sử dụng

```bash
# Xóa dữ liệu cũ hơn 1 năm
python3 -m TEx purge_old_data --config config.ini

# Xóa dữ liệu cũ hơn 30 ngày
python3 -m TEx purge_old_data --config config.ini --limit_days 30

# Xóa dữ liệu cũ hơn 7 ngày
python3 -m TEx purge_old_data --config config.ini --limit_days 7
```

#### Loại Dữ liệu Được Xóa

- **Tin nhắn**: Bản ghi tin nhắn cũ
- **File Media**: File media cũ và tham chiếu
- **Dữ liệu Người dùng**: Hồ sơ người dùng không hoạt động
- **File Tạm thời**: Cache và dữ liệu tạm thời
- **File Log**: Mục log cũ

### `purge_temp_files`

**Mục đích**: Buộc xóa tất cả file tạm thời.

**Mô tả**: Xóa tất cả file tạm thời được tạo trong quá trình hoạt động, hữu ích để dọn dẹp sau khi xử lý hoặc khắc phục sự cố.

#### Tùy chọn

| Tùy chọn | Loại | Bắt buộc | Mặc định | Mô tả |
|----------|------|----------|----------|-------|
| `--config` | string | ✅ Có | None | Đường dẫn file cấu hình |

#### Ví dụ Sử dụng

```bash
# Xóa tất cả file tạm thời
python3 -m TEx purge_temp_files --config config.ini
```

#### File Tạm thời Được Xóa

- **Cache Tải xuống**: File tải xuống tạm thời
- **File Xử lý**: File xử lý trung gian
- **File Phiên**: Dữ liệu phiên tạm thời
- **File Log**: File log tạm thời
- **File Cache**: Cache ứng dụng

---

## 🔧 Mẫu Sử dụng Nâng cao

### Xử lý Hàng loạt

```bash
# Xử lý nhiều nhóm tuần tự
for group_id in 123456789 987654321 555666777; do
    python3 -m TEx download_messages --config config.ini --group_id $group_id
    python3 -m TEx analyze --config config.ini --group_id $group_id --visual-data
done
```

### Báo cáo Tự động

```bash
# Tạo báo cáo tự động hàng ngày
python3 -m TEx download_messages --config config.ini --group_id 123456789
python3 -m TEx analyze --config config.ini --group_id 123456789 --visual-data
python3 -m TEx report --config config.ini --limit_days 1
python3 -m TEx sent_report_telegram --config config.ini \
  --destination_username admin --title "Báo cáo Hàng ngày"
```

### Phân tích Toàn diện

```bash
# Pipeline phân tích đầy đủ
python3 -m TEx download_messages --config config.ini --group_id 123456789 \
  --url-scraping --forwarding-analysis --reaction-analysis
python3 -m TEx analyze --config config.ini --group_id 123456789 \
  --visual-data --generate-visualizations
python3 -m TEx export_html --config config.ini --group_id 123456789
```

---

## 📊 Ví dụ Đầu ra Lệnh

### Kết nối Thành công
```
2024-01-01 12:00:00 - INFO - [+] telegram_connection_manager.TelegramConnector
2024-01-01 12:00:01 - INFO - User Authorized on Telegram: True
```

### Tải Nhóm
```
2024-01-01 12:00:00 - INFO - [+] telegram_groups_scrapper.TelegramGroupScrapper
2024-01-01 12:00:01 - INFO - Found 5 Groups in Database
2024-01-01 12:00:02 - INFO - Processing group: Example Group (123456789)
```

### Tải xuống Tin nhắn
```
2024-01-01 12:00:00 - INFO - [+] enhanced_telegram_messages_scrapper.EnhancedTelegramMessagesScrapper
2024-01-01 12:00:01 - INFO - Starting to scrape group ID: 123456789
2024-01-01 12:00:05 - INFO - Found 1,234 messages for group 123456789
2024-01-01 12:00:10 - INFO - Processed 1,234 messages for group 123456789
```

### Kết quả Phân tích
```
2024-01-01 12:00:00 - INFO - [+] enhanced_data_analyzer.EnhancedDataAnalyzer
2024-01-01 12:00:01 - INFO - Analyzing group: 123456789
2024-01-01 12:00:02 - INFO - Found 45 URLs in messages
2024-01-01 12:00:03 - INFO - Identified 12 forwarding relationships
2024-01-01 12:00:04 - INFO - Analyzed 234 reactions
```

---

## 🚨 Vấn đề Thường gặp và Giải pháp

### Vấn đề Xác thực
```bash
# Xóa phiên và xác thực lại
rm session.session
python3 -m TEx connect --config config.ini
```

### Giới hạn Tốc độ
```bash
# Sử dụng giới hạn nhỏ hơn và độ trễ
python3 -m TEx download_messages --config config.ini --group_id 123456789 --limit 1000
```

### Vấn đề Bộ nhớ
```bash
# Sử dụng ignore_media cho nhóm lớn
python3 -m TEx download_messages --config config.ini --group_id 123456789 --ignore_media
```

### Vấn đề Cơ sở dữ liệu
```bash
# Dọn dẹp dữ liệu cũ
python3 -m TEx purge_old_data --config config.ini --limit_days 30
```

---

## 📚 Tài nguyên Bổ sung

- **Hướng dẫn Cấu hình**: [Tài liệu Cấu hình](../configuration/basic.md)
- **Khắc phục Sự cố**: [Hướng dẫn Khắc phục](../troubleshooting.md)
- **Tham chiếu API**: [Tài liệu API](../api/modules.md)
- **Ví dụ**: [Ví dụ Sử dụng](basic-usage.md)

Để biết thông tin chi tiết hơn về các lệnh cụ thể và cách sử dụng nâng cao, tham khảo các trang tài liệu lệnh riêng lẻ.