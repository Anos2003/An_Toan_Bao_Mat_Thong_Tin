# Gửi Báo Cáo Tài Chính Có Nén Dữ Liệu

## 📌 Mô tả
Dự án triển khai một hệ thống **truyền file bảo mật** giữa hai thực thể `Sender` và `Receiver`. File báo cáo tài chính (`finance.txt`) được **nén, mã hóa, ký số và kiểm tra toàn vẹn** trước khi gửi đi.

Sử dụng các kỹ thuật:
- **AES (GCM)**: Mã hóa dữ liệu file (đối xứng)
- **RSA (1024-bit)**: Mã hóa khóa AES và ký metadata (bất đối xứng)
- **SHA-512**: Tạo hàm băm để kiểm tra tính toàn vẹn của gói tin
- **JSON**: Đóng gói dữ liệu để truyền qua mạng
- **Zlib**: Nén dữ liệu

---

## ⚙️ Cấu trúc hệ thống

Sender.py → Mã hóa và gửi gói tin
Receiver.py → Nhận, giải mã, kiểm tra toàn vẹn và lưu file
Generate_keys.py → Tạo các cặp khóa RSA cho sender/receiver
packet.json → Gói tin chứa dữ liệu mã hóa, chữ ký, khóa và metadata

markdown
Sao chép
Chỉnh sửa

---

## 🛡️ Quy trình hoạt động

1. **Sender:**
   - Đọc nội dung `Input/finance.txt`
   - Tạo khóa AES ngẫu nhiên (16 byte) và nonce (12 byte)
   - Nén dữ liệu và mã hóa bằng AES-GCM
   - Mã hóa khóa AES bằng public key của Receiver (RSA)
   - Ký metadata bằng private key của Sender (RSA + SHA-512)
   - Tạo hash SHA-512 của nonce + ciphertext + tag
   - Đóng gói tất cả vào `packet.json`

2. **Receiver:**
   - Kiểm tra handshake `"Hello!"`
   - Tải `packet.json`
   - Kiểm tra hash toàn vẹn
   - Xác minh chữ ký metadata
   - Giải mã khóa AES bằng private key của Receiver
   - Giải mã AES-GCM và giải nén
   - Kiểm tra timestamp
   - Lưu vào `uploads/received_finance.txt`

---

## 📁 Yêu cầu thư viện

Cài bằng pip:

```bash
pip install pycryptodome
▶️ Hướng dẫn chạy chương trình
bash
Sao chép
Chỉnh sửa
# 1. Tạo khóa RSA
python Generate_keys.py

# 2. Chạy Sender
python Sender.py

# 3. Chạy Receiver (ngay sau Sender, tránh chênh timestamp quá 60s)
python Receiver.py
✅ Tính năng nổi bật
🔐 Bảo mật kép: Kết hợp RSA và AES

🧾 Đảm bảo toàn vẹn: SHA-512

📦 Gói tin đơn giản, dễ xử lý: JSON

📉 Dữ liệu được nén trước khi mã hóa

⚠️ Giới hạn
RSA chỉ dùng khóa 1024-bit (nên nâng lên 2048+ trong thực tế)

Không có xác thực danh tính người gửi (chỉ ký metadata)

Dễ bị replay attack nếu không kiểm soát tốt timestamp

🧑‍💻 Tác giả
Nhóm 10 – Lớp CNTT 16-04
Trường Đại học Đại Nam

less
Sao chép
Chỉnh sửa

---

📦 **Cách sử dụng:**  
Tạo file `README.md` trong repo của bạn, dán nội dung trên vào.

Nếu bạn muốn mình **tạo thêm hình minh họa quy trình**, hoặc viết bản README bằng **tiếng Việt hoàn toàn*
