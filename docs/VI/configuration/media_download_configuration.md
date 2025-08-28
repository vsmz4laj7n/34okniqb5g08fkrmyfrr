# Cấu hình Tải Media

Bạn có thể tùy chỉnh (bật hoàn toàn, tắt hoặc bật có chọn lọc) việc tải media bằng cách chỉ định các cài đặt này trong tệp cấu hình.

**Bật / Tắt Hành vi Tải Media Mặc định**
```ini
[MEDIA.DOWNLOAD]
default=ALLOW
max_download_size_bytes=256000000
```

* **default** > Bắt buộc - Thiết lập hành vi mặc định. Bật (ALLOW) hoặc Tắt (DISALLOW)
* **max_download_size_bytes** > Tùy chọn - Kích thước tải tối đa cho tất cả media tính bằng byte
     * Mặc định: 256000000

**Cài đặt Theo Từng Loại Media**
Sử dụng *MEDIA.DOWNLOAD.<content-type>* để chỉ định cài đặt cho từng loại nội dung cụ thể.
```ini
[MEDIA.DOWNLOAD.<content-type>]
enabled=ALLOW
max_download_size_bytes=256000000
groups=*
```

* **enabled** > Bắt buộc - Bật/Tắt tải loại nội dung này. Bật (ALLOW) hoặc Tắt (DISALLOW)
* **max_download_size_bytes** > Tùy chọn - Kích thước tải tối đa cho loại nội dung này
    * Mặc định: 256000000
* **groups** > Tùy chọn - Nếu có, chỉ tải tin nhắn từ các ID nhóm được chỉ định. Phân tách bằng dấu phẩy. Để tất cả nhóm, sử dụng *
    * Mặc định: *