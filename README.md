
# BBS-Based Image Steganography

Đây là một hệ thống giấu tin trong ảnh sử dụng thuật toán sinh số ngẫu nhiên Blum Blum Shub (BBS) kết hợp với kỹ thuật LSB (Least Significant Bit).

## Cấu trúc hệ thống

Hệ thống gồm 3 bước chính được thực hiện thông qua các script:

---

## 1. `bbs_step1_prepare.py` – Chuẩn bị dữ liệu

### 🎯 Mục đích:
- Chuẩn bị dữ liệu cơ bản và tạo các tham số cho thuật toán BBS.

### ⚙️ Quy trình:
- Đọc ảnh gốc và chuyển thành danh sách các pixel.
- Đọc thông điệp cần giấu từ file văn bản.
- Tạo các tham số BBS:
  - Hai số nguyên tố Blum `p` và `q` (dạng 4k+3).
  - Tính `n = p × q`.
  - Chọn hạt giống `s` sao cho nguyên tố cùng nhau với `n`.
- Lưu dữ liệu trung gian:
  - `bbs_data.json` – chứa các tham số BBS và metadata.
  - `bbs_pixels.bin` – chứa danh sách pixel ảnh.
- Hiển thị thông tin ảnh, thông điệp và tham số.

---

## 2. `bbs_step2_convert.py` – Chuyển đổi thông điệp & tạo dãy bit ngẫu nhiên

### 🎯 Mục đích:
- Chuyển đổi thông điệp thành nhị phân và tạo dãy bit ngẫu nhiên xác định vị trí giấu tin.

### ⚙️ Quy trình:
- Đọc dữ liệu từ `bbs_data.json` và `bbs_pixels.bin`.
- Chuyển thông điệp thành chuỗi nhị phân (mỗi ký tự → 8 bit).
- Sử dụng BBS để tạo dãy bit ngẫu nhiên:
  - Mỗi 3 bit → 1 vị trí bit giấu thông điệp.
  - Tổng số bit cần tạo = số bit thông điệp × 3.
- Kiểm tra khả năng chứa thông điệp của ảnh.
- Lưu dữ liệu:
  - `bbs_binary.json` – chứa thông điệp nhị phân và vị trí pixel đã chọn.
- Hiển thị thông tin nhị phân, dãy BBS và vị trí giấu.

---

## 3. `bbs_step3_embed.py` – Giấu thông điệp vào ảnh

### 🎯 Mục đích:
- Giấu thông điệp vào ảnh bằng thuật toán BBS + LSB.

### ⚙️ Quy trình:
- Đọc dữ liệu từ `bbs_binary.json` và `bbs_pixels.bin`.
- Nhúng header (128 pixel đầu):
  - 32 pixel đầu: độ dài thông điệp (32 bit).
  - 32 tiếp theo: tham số `p` (32 bit).
  - 32 tiếp theo: tham số `q` (32 bit).
  - 32 cuối: hạt giống `s` (32 bit).
- Sử dụng dãy bit BBS để xác định vị trí nhúng:
  - Mỗi 3 bit xác định một pixel & kênh màu.
  - Nhúng bit vào bit thấp nhất (LSB).
- Xuất ảnh đã giấu dưới dạng `.png` để tránh nén.
- Lưu thông tin vào:
  - `bbs_output.json` – chứa log quá trình giấu.
- **So sánh với phương pháp giấu liên tiếp (không dùng BBS):**
  - Giấu độ dài vào 32 pixel đầu.
  - Giấu thông điệp vào các pixel liên tục từ pixel thứ 128 trở đi.

---

## 📁 File tạo ra

| Tên file             | Nội dung                                       |
|----------------------|------------------------------------------------|
| `bbs_data.json`      | Tham số thuật toán BBS và metadata             |
| `bbs_pixels.bin`     | Danh sách pixel từ ảnh gốc                     |
| `bbs_binary.json`    | Thông điệp nhị phân và dãy vị trí từ BBS       |
| `bbs_output.json`    | Kết quả quá trình nhúng                        |
| `output_bbs.png`     | Ảnh đã giấu tin dùng thuật toán BBS           |
| `output_direct.png`  | Ảnh đã giấu tin bằng phương pháp trực tiếp    |

---

## 📌 Yêu cầu hệ thống

- Python 3.x
- Pillow
- NumPy

---

## 📞 Liên hệ

Nếu có câu hỏi, vui lòng liên hệ qua GitHub hoặc email của người phát triển.

