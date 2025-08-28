# Cấu hình Phân tích

TEx cung cấp các tính năng phân tích nâng cao có thể được cấu hình thông qua tệp `config.ini`. Dưới đây là hướng dẫn chi tiết về các tùy chọn cấu hình phân tích.

## Cấu hình Trích xuất Bộ chọn (Selector Patterns)

### Regex Patterns cho PII
```ini
[SELECTOR_PATTERNS]
# Mẫu regex để trích xuất số điện thoại
phone_pattern=\b(?:\+?1[-.]?)?\(?([0-9]{3})\)?[-.]?([0-9]{3})[-.]?([0-9]{4})\b

# Mẫu regex để trích xuất email
email_pattern=\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b

# Mẫu regex để trích xuất URL
url_pattern=https?://(?:[-\w.])+(?:[:\d]+)?(?:/(?:[\w/_.])*(?:\?(?:[\w&=%.])*)?(?:#(?:[\w.])*)?)?

# Mẫu regex để trích xuất địa chỉ IP
ip_pattern=\b(?:[0-9]{1,3}\.){3}[0-9]{1,3}\b

# Mẫu regex để trích xuất địa chỉ tiền điện tử
crypto_pattern=\b[13][a-km-zA-HJ-NP-Z1-9]{25,34}\b
```

### Regex Patterns cho Nhận dạng Thực thể (Named Entity Recognition)
```ini
[ENTITY_PATTERNS]
# Mẫu regex để nhận dạng tên người
person_pattern=\b[A-Z][a-z]+ [A-Z][a-z]+\b

# Mẫu regex để nhận dạng tổ chức
organization_pattern=\b[A-Z][A-Z\s&]+(?:Inc|Corp|LLC|Ltd|Company|Organization|Group)\b

# Mẫu regex để nhận dạng địa điểm
location_pattern=\b[A-Z][a-z]+(?: [A-Z][a-z]+)*,?\s+[A-Z]{2}\b

# Mẫu regex để nhận dạng ngày tháng
date_pattern=\b\d{1,2}[/-]\d{1,2}[/-]\d{2,4}\b
```

## Cấu hình Từ khóa Ý thức hệ

### Phân tích Ý thức hệ (Ideological Analysis)
```ini
[IDEOLOGICAL_KEYWORDS]
# Từ khóa ngôn ngữ thù địch (hate speech)
hate_speech=hate,racist,bigot,nazi,supremacist,white nationalist,anti-semitic,phản động,chia rẽ dân tộc,phỉ báng

# Từ khóa cực đoan (extremism)
extremism=extremist,radical,terrorist,jihad,white power,anarchist,revolutionary,phản cách mạng,phản quốc,kiêu gọi lật đổ

# Từ khóa âm mưu (conspiracy)
conspiracy=conspiracy,deep state,illuminati,chemtrails,flat earth,new world order,diễn biến hòa bình,mưu đồ lật đổ,thuyết âm mưu

# Từ khóa công dân có chủ quyền (sovereign citizen)
sovereign_citizen=sovereign,freeman,common law,strawman,admiralty,maritime law,tự do cá nhân,pháp luật tự nhiên

# Từ khóa thuật ngữ incel
incel_terminology=incel,chad,stacy,blackpill,redpill,bluepill,mgtow,độc thân bất đắc dĩ,ghét phụ nữ

# Từ khóa chống chính phủ
anti_government=anti-government,anti-state,anti-communist,phản đối chính phủ,phản đối Đảng,phản kháng chế độ

# Từ khóa an ninh văn hóa
cultural_security=cultural erosion,westernization,globalization threat,tha hóa văn hóa,ảnh hưởng phương Tây,bảo vệ văn hóa dân tộc
```

## Cấu hình Từ khóa Mối đe dọa

### Đánh giá Mối đe dọa (Threat Assessment)
```ini
[THREAT_KEYWORDS]
# Từ khóa khả năng (capability)
capability=weapon,gun,bomb,explosive,knife,ammo,ammunition,firearm,vũ khí,súng,bom,thuốc nổ,dao,đạn dược

# Từ khóa ý định bạo lực (violent intent)
violent_intent=kill,murder,attack,shoot,bomb,terror,assassinate,eliminate,giết,gây bạo loạn,tấn công,khủng bố,ám sát

# Từ khóa vũ khí (weapons)
weapons=rifle,pistol,shotgun,ammunition,explosive,knife,grenade,molotov,súng trường,súng ngắn,súng săn,đạn dược,thuốc nổ,dao,lựu đạn

# Từ khóa đe dọa (threatening)
threatening=threat,warning,danger,attack,kill,destroy,eliminate,neutralize,đe dọa,cảnh báo,nguy hiểm,tấn công,phá hủy

# Từ khóa an ninh hàng hải
maritime_security=south china sea,maritime dispute,gray zone tactics,biển đông,tranh chấp biển,chiến thuật vùng xám

# Từ khóa mối đe dọa mạng
cyber_threat=cyberattack,hack,cyber espionage,tấn công mạng,xâm nhập mạng,gián điệp mạng

# Từ khóa an ninh phi truyền thống
non_traditional_security=climate change,pandemic,resource scarcity,biến đổi khí hậu,dịch bệnh,thiếu hụt tài nguyên
```

## Cấu hình Tải và Phân tích

### Cấu hình Tải (Download Configuration)
```ini
[DOWNLOAD_CONFIG]
# Giới hạn tin nhắn mặc định (None cho tất cả tin nhắn)
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

## Tùy chỉnh Mẫu Regex

### Thêm Mẫu Tùy chỉnh
Bạn có thể thêm các mẫu regex tùy chỉnh vào các phần tương ứng:

```ini
[SELECTOR_PATTERNS]
# Thêm mẫu tùy chỉnh cho số thẻ tín dụng
credit_card_pattern=\b(?:4[0-9]{12}(?:[0-9]{3})?|5[1-5][0-9]{14}|3[47][0-9]{13}|3[0-9]{13}|6(?:011|5[0-9]{2})[0-9]{12})\b

# Thêm mẫu tùy chỉnh cho địa chỉ Bitcoin
bitcoin_pattern=\b[13][a-km-zA-HJ-NP-Z1-9]{25,34}\b

# Thêm mẫu tùy chỉnh cho số căn cước
id_card_pattern=\b[0-9]{9,12}\b
```

### Thêm Từ khóa Tùy chỉnh
```ini
[IDEOLOGICAL_KEYWORDS]
# Thêm từ khóa tùy chỉnh cho phân tích ý thức hệ
custom_ideology=your_custom_keyword1,your_custom_keyword2

[THREAT_KEYWORDS]
# Thêm từ khóa tùy chỉnh cho đánh giá mối đe dọa
custom_threat=your_threat_keyword1,your_threat_keyword2
```

## Lưu ý Quan trọng

### Hiệu suất
- **Regex Patterns**: Sử dụng regex hiệu quả để tránh làm chậm quá trình phân tích
- **Batch Processing**: Điều chỉnh `batch_size` phù hợp với tài nguyên hệ thống
- **Memory Usage**: Theo dõi sử dụng bộ nhớ khi xử lý bộ dữ liệu lớn

### Độ chính xác
- **False Positives**: Kiểm tra và tinh chỉnh các mẫu regex để giảm false positives
- **Context Analysis**: Cân nhắc ngữ cảnh khi phân tích từ khóa
- **Multi-language Support**: Hỗ trợ tiếng Anh và tiếng Việt

### Bảo mật
- **Data Privacy**: Tất cả dữ liệu được xử lý cục bộ
- **No Cloud Upload**: Không có dữ liệu nào được gửi đến máy chủ bên ngoài
- **Configurable Retention**: Kiểm soát thời gian lưu trữ dữ liệu