# Phân tích Dữ liệu

Hướng dẫn sử dụng lệnh `analyze` để phân tích dữ liệu đã tải từ Telegram và tạo các báo cáo chi tiết.

## Tổng quan

Lệnh `analyze` cho phép bạn thực hiện phân tích toàn diện trên dữ liệu đã tải, bao gồm:
- Phân tích URL và liên kết
- Phân tích chuyển tiếp tin nhắn
- Phân tích phản ứng và tương tác
- Tạo dữ liệu trực quan
- Trích xuất thông tin tình báo
- Đánh giá mối đe dọa

## Cú pháp cơ bản

```bash
python -m TEx analyze --config config.ini --group_id GROUP_ID
```

## Tham số bắt buộc

| Tham số | Mô tả | Ví dụ |
|---------|-------|-------|
| `--config` | Đường dẫn đến tệp cấu hình | `--config config.ini` |
| `--group_id` | ID của nhóm cần phân tích | `--group_id 123456789` |

## Tùy chọn phân tích

### Tùy chọn cơ bản

| Tùy chọn | Mô tả | Mặc định |
|----------|-------|----------|
| `--url-analysis` | Phân tích URL từ tin nhắn | Tắt |
| `--forwarding-analysis` | Phân tích chuyển tiếp tin nhắn | Tắt |
| `--reaction-analysis` | Phân tích phản ứng | Tắt |
| `--visual-data` | Tạo dữ liệu trực quan | Tắt |
| `--generate-visualizations` | Tạo biểu đồ và báo cáo | Tắt |

### Tùy chọn xuất

| Tùy chọn | Mô tả | Ví dụ |
|----------|-------|-------|
| `--gephi-format` | Xuất dữ liệu mạng cho Gephi | `GEXF`, `GraphML`, `CSV` |

## Ví dụ sử dụng

### Phân tích cơ bản
```bash
# Phân tích cơ bản dữ liệu đã tải
python -m TEx analyze --config config.ini --group_id 123456789
```

### Phân tích URL
```bash
# Phân tích và trích xuất URL từ tin nhắn
python -m TEx analyze --config config.ini --group_id 123456789 --url-analysis
```

### Phân tích chuyển tiếp
```bash
# Phân tích mạng lưới chuyển tiếp tin nhắn
python -m TEx analyze --config config.ini --group_id 123456789 --forwarding-analysis
```

### Phân tích phản ứng
```bash
# Phân tích phản ứng và tương tác người dùng
python -m TEx analyze --config config.ini --group_id 123456789 --reaction-analysis
```

### Phân tích toàn diện
```bash
# Thực hiện tất cả phân tích
python -m TEx analyze --config config.ini --group_id 123456789 \
  --url-analysis \
  --forwarding-analysis \
  --reaction-analysis \
  --visual-data \
  --generate-visualizations
```

### Xuất dữ liệu mạng
```bash
# Xuất dữ liệu mạng lưới cho Gephi
python -m TEx analyze --config config.ini --group_id 123456789 \
  --visual-data \
  --gephi-format GEXF
```

### Phân tích nhiều nhóm
```bash
# Phân tích nhiều nhóm cùng lúc
python -m TEx analyze --config config.ini --group_id "123456789,987654321"
```

## Các loại phân tích

### 1. Phân tích URL (URL Analysis)

#### Tính năng
- Trích xuất tất cả URL từ tin nhắn
- Phân loại URL theo loại (t.me, website, social media)
- Theo dõi sự lan truyền URL qua các nhóm
- Phân tích mẫu chia sẻ liên kết

#### Kết quả
- Danh sách tất cả URL được tìm thấy
- Thống kê loại URL
- Báo cáo URL phổ biến nhất
- Phân tích thời gian chia sẻ URL

### 2. Phân tích Chuyển tiếp (Forwarding Analysis)

#### Tính năng
- Theo dõi mạng lưới lan truyền tin nhắn
- Xác định nguồn gốc tin nhắn
- Phân tích hành vi chuyển tiếp
- Tạo sơ đồ mạng lưới chuyển tiếp

#### Kết quả
- Sơ đồ mạng lưới chuyển tiếp
- Thống kê tin nhắn được chuyển tiếp nhiều nhất
- Phân tích thời gian lan truyền
- Xác định người dùng có ảnh hưởng

### 3. Phân tích Phản ứng (Reaction Analysis)

#### Tính năng
- Phân tích phản ứng của người dùng
- Tính toán chỉ số tương tác
- Xác định nội dung phổ biến
- Theo dõi xu hướng phản ứng

#### Kết quả
- Thống kê phản ứng theo loại
- Xếp hạng tin nhắn theo tương tác
- Phân tích hành vi người dùng
- Báo cáo xu hướng tương tác

### 4. Dữ liệu Trực quan (Visual Data)

#### Tính năng
- Tạo mạng lưới tương tác người dùng
- Trích xuất thông tin PII (số điện thoại, email, IP)
- Trích xuất dữ liệu GPS từ hình ảnh
- Nhận dạng thực thể có tên (NER)
- Phân tích ý thức hệ và mối đe dọa

#### Kết quả
- Biểu đồ mạng lưới tương tác
- Báo cáo thông tin PII
- Bản đồ vị trí từ GPS
- Phân tích ý thức hệ
- Đánh giá mối đe dọa

## Cấu hình phân tích

### Cấu hình trích xuất bộ chọn

```ini
[SELECTOR_PATTERNS]
# Mẫu regex cho số điện thoại
phone_pattern=\b(?:\+?1[-.]?)?\(?([0-9]{3})\)?[-.]?([0-9]{3})[-.]?([0-9]{4})\b

# Mẫu regex cho email
email_pattern=\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b

# Mẫu regex cho URL
url_pattern=https?://(?:[-\w.])+(?:[:\d]+)?(?:/(?:[\w/_.])*(?:\?(?:[\w&=%.])*)?(?:#(?:[\w.])*)?)?

# Mẫu regex cho địa chỉ IP
ip_pattern=\b(?:[0-9]{1,3}\.){3}[0-9]{1,3}\b

# Mẫu regex cho địa chỉ tiền điện tử
crypto_pattern=\b[13][a-km-zA-HJ-NP-Z1-9]{25,34}\b
```

### Cấu hình từ khóa ý thức hệ

```ini
[IDEOLOGICAL_KEYWORDS]
# Từ khóa ngôn ngữ thù địch
hate_speech=hate,racist,bigot,nazi,supremacist,white nationalist,anti-semitic,phản động,chia rẽ dân tộc,phỉ báng

# Từ khóa cực đoan
extremism=extremist,radical,terrorist,jihad,white power,anarchist,revolutionary,phản cách mạng,phản quốc,kiêu gọi lật đổ

# Từ khóa âm mưu
conspiracy=conspiracy,deep state,illuminati,chemtrails,flat earth,new world order,diễn biến hòa bình,mưu đồ lật đổ,thuyết âm mưu
```

### Cấu hình từ khóa mối đe dọa

```ini
[THREAT_KEYWORDS]
# Từ khóa khả năng
capability=weapon,gun,bomb,explosive,knife,ammo,ammunition,firearm,vũ khí,súng,bom,thuốc nổ,dao,đạn dược

# Từ khóa ý định bạo lực
violent_intent=kill,murder,attack,shoot,bomb,terror,assassinate,eliminate,giết,gây bạo loạn,tấn công,khủng bố,ám sát

# Từ khóa vũ khí
weapons=rifle,pistol,shotgun,ammunition,explosive,knife,grenade,molotov,súng trường,súng ngắn,súng săn,đạn dược,thuốc nổ,dao,lựu đạn
```

## Kết quả phân tích

### Tệp đầu ra

#### 1. Báo cáo HTML
- `group_ANALYSIS_REPORT.html` - Báo cáo phân tích toàn diện
- Bao gồm biểu đồ, thống kê và phân tích chi tiết

#### 2. Dữ liệu JSON
- `group_analysis_data.json` - Dữ liệu phân tích có cấu trúc
- `group_user_interaction_network.json` - Dữ liệu mạng lưới tương tác

#### 3. Biểu đồ và hình ảnh
- `group_charts.pdf` - Biểu đồ thống kê
- `group_social_network.png` - Sơ đồ mạng lưới xã hội
- `group_map.html` - Bản đồ tương tác

#### 4. Dữ liệu Gephi
- `group_network.gexf` - Định dạng GEXF cho Gephi
- `group_network.graphml` - Định dạng GraphML cho Gephi
- `group_nodes.csv` và `group_edges.csv` - Dữ liệu CSV

### Nội dung báo cáo

#### Thống kê tổng quan
- Tổng số tin nhắn phân tích
- Số lượng người dùng tham gia
- Thời gian phân tích
- Các loại media được tìm thấy

#### Phân tích URL
- Danh sách URL phổ biến nhất
- Phân loại URL theo loại
- Thống kê chia sẻ URL theo thời gian
- Phân tích mẫu chia sẻ

#### Phân tích chuyển tiếp
- Sơ đồ mạng lưới chuyển tiếp
- Tin nhắn được chuyển tiếp nhiều nhất
- Người dùng có ảnh hưởng cao
- Thời gian lan truyền trung bình

#### Phân tích phản ứng
- Thống kê phản ứng theo loại
- Tin nhắn có tương tác cao nhất
- Hành vi phản ứng của người dùng
- Xu hướng tương tác theo thời gian

#### Thông tin tình báo
- Danh sách PII được trích xuất
- Vị trí GPS từ hình ảnh
- Thực thể có tên được nhận dạng
- Phân tích ý thức hệ
- Đánh giá mối đe dọa

## Xử lý lỗi

### Lỗi thường gặp

#### 1. Không có dữ liệu để phân tích
```bash
# Kiểm tra xem đã tải tin nhắn chưa
python -m TEx list_groups --config config.ini
```

#### 2. Lỗi bộ nhớ
```bash
# Giảm kích thước batch trong config.ini
batch_size=50
```

#### 3. Lỗi cơ sở dữ liệu
```bash
# Sửa lỗi cơ sở dữ liệu
python -m TEx fix_database --config config.ini
```

### Khắc phục sự cố

#### Kiểm tra dữ liệu
```bash
# Kiểm tra số lượng tin nhắn đã tải
python -m TEx stats --config config.ini
```

#### Kiểm tra cấu hình
```bash
# Kiểm tra tệp cấu hình
cat config.ini | grep -E "(SELECTOR_PATTERNS|IDEOLOGICAL_KEYWORDS|THREAT_KEYWORDS)"
```

## Tối ưu hóa hiệu suất

### 1. Phân tích theo batch
- Phân tích từng nhóm riêng biệt
- Sử dụng giới hạn tin nhắn cho nhóm lớn

### 2. Tối ưu hóa regex
- Sử dụng regex hiệu quả
- Tránh regex phức tạp không cần thiết

### 3. Quản lý bộ nhớ
- Giám sát sử dụng bộ nhớ
- Đóng các ứng dụng không cần thiết

### 4. Lưu trữ kết quả
- Sử dụng SSD cho lưu trữ kết quả
- Sao lưu kết quả phân tích quan trọng

## Lưu ý quan trọng

### Quyền riêng tư
- Chỉ phân tích dữ liệu bạn có quyền truy cập
- Tôn trọng quyền riêng tư của người dùng
- Không chia sẻ thông tin PII với bên thứ ba

### Độ chính xác
- Kết quả phân tích có thể có false positives
- Kiểm tra và xác minh kết quả quan trọng
- Cập nhật mẫu regex và từ khóa khi cần

### Bảo mật
- Bảo vệ kết quả phân tích
- Mã hóa dữ liệu nhạy cảm
- Xóa dữ liệu không cần thiết