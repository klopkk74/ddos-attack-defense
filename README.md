# 🛡️ DDoS Attack & Defense Research

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python&logoColor=white)](https://www.python.org/)
[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Research](https://img.shields.io/badge/Type-Research-red)](#)

## 📌 Tổng quan

Dự án nghiên cứu chuyên sâu về **Tấn công từ chối dịch vụ phân tán (DDoS)** và các giải pháp phòng chống. Báo cáo hệ thống hóa kiến thức từ lý thuyết đến thực nghiệm, bao gồm mô phỏng tấn công bằng các công cụ phổ biến và triển khai các lớp phòng thủ đa tầng.

## 🎯 Mục tiêu

- 🔍 Hệ thống hóa lý thuyết về DDoS, Botnet và các kỹ thuật tấn công.
- ⚔️ Mô phỏng tấn công SYN Flood, UDP Flood, ICMP Flood, HTTP Flood.
- 🐍 Xây dựng mạng botnet đơn giản bằng Python (Py-Botnet).
- 🛡️ Triển khai giải pháp phòng chống: IPtables, Snort IDS, Packet Sniffer Python.

## ⚙️ Công nghệ sử dụng

**Tấn công**
- `Hping3` – Tạo gói tin TCP/IP tùy chỉnh cho SYN Flood, ICMP Flood
- `Slowhttptest` – Mô phỏng tấn công HTTP Flood (Slowloris, Slow POST)
- `Py-Botnet` – Mạng botnet Python với C&C Server và Client

**Phòng thủ**
- `IPtables` – Tường lửa Linux, lọc và chặn gói tin theo quy tắc
- `Snort IDS` – Hệ thống phát hiện xâm nhập mạng dựa trên rules

**Phân tích**
- `Wireshark` – Bắt và phân tích lưu lượng mạng
- `Packet Sniffer (Python)` – Công cụ phân tích lưu lượng, xác định nguồn tấn công

**Môi trường**
- Python 3.10+
- Ubuntu Linux / VMware Lab

## 📁 Cấu trúc dự án

```text
ddos-attack-defense/
├── README.md
├── LICENSE
├── .gitignore
├── src/
├── docs/
└── images/
    ├── architecture/
    ├── attack/
    │   ├── http-flood/
    │   ├── icmp-flood/
    │   ├── py-botnet/
    │   └── syn-flood/
    └── defense/
        ├── iptables/
        ├── python-sniffer/
        └── snort/

## 🚀 Hướng dẫn cài đặt & chạy

### 1. Yêu cầu hệ thống

- Ubuntu Linux (hoặc WSL trên Windows)
- Python 3.10+
- Quyền `sudo`

### 2. Cài đặt công cụ

```bash
# Cập nhật hệ thống
sudo apt update

# Cài Python và môi trường ảo
sudo apt install python3 python3-venv -y

# Cài thư viện Python cần thiết
pip install joblib pandas scikit-learn scapy

# Cài công cụ tấn công và phòng thủ
sudo apt install hping3 nmap wireshark snort iptables -y
```

### 3. Chạy Py-Botnet

**Terminal 1 - Server (C&C):**
```bash
python3 src/Server.py
```

**Terminal 2 - Client (Bot):**
```bash
python3 src/Client.py
```

**Trên Server, gửi lệnh tấn công:**
```text
attack <IP_đích> <port> <thời_gian> <số_luồng>
```

Ví dụ:
```text
attack 192.168.1.100 80 100 200
```

## 📸 Kết quả thực nghiệm

### 1. Server Py-Botnet điều khiển bot

<!-- Thêm ảnh: images/py-botnet/01-server-commands.png -->
![Server Py-Botnet](images/py-botnet/server-commands.png)

*Server hiển thị danh sách bot đã kết nối, các lệnh điều khiển (list, attack, kill) và lệnh tấn công UDP Flood được gửi đến mục tiêu.*

### 2. Tác động lên hệ thống mục tiêu

<!-- Thêm ảnh: images/py-botnet/02-attack-impact.png -->
![Tác động tấn công](images/py-botnet/after-attack.png)
*Sau khi tấn công, CPU tăng lên 34%, Memory chiếm 93%, tốc độ nhận lên tới 270 Mbps.*

### 3. Phân tích lưu lượng tấn công
<!-- Thêm ảnh: images/py-botnet/03-wireshark-traffic.png -->
![Wireshark bắt gói tin](images/py-botnet/wireshark-traffic.png)
*Wireshark ghi nhận hàng loạt gói tin UDP từ bot gửi đến cổng 80 của mục tiêu, mỗi gói có kích thước lớn (2048 bytes).*

## 🛡️ Giải pháp phòng chống

**IPtables**
- Chặn địa chỉ IP nguồn tấn công
- Giới hạn số lượng gói SYN/giây để ngăn SYN Flood
- Lọc giao thức ICMP và UDP
  
**Snort IDS**
- Phát hiện tấn công dựa trên rules
- Cảnh báo thời gian thực
  
**Packet Sniffer (Python)**
- Phân tích lưu lượng mạng
- Xác định nguồn và loại tấn công

## ⚠️ Disclaimer

> **Dự án này chỉ dành cho mục đích học tập và nghiên cứu.**  
> ❌ Không sử dụng để tấn công hệ thống thực tế.  
> ❌ Không sử dụng khi chưa có sự cho phép.  
> ✅ Chỉ thực nghiệm trong môi trường Lab hoặc mạng riêng.  
> ✅ Mục đích duy nhất là nâng cao kiến thức về an ninh mạng.

Việc sử dụng trái phép có thể vi phạm pháp luật. Người dùng tự chịu trách nhiệm về hành vi của mình.

## 👥 Tác giả

- **Nguyễn Trung Kiên** – AT200432
- **Nguyễn Văn Khánh** – AT200430
- **Vũ Trọng Khang** – AT200130
- **Nguyễn Công Khánh** – AT200131
- **Đinh Trí Đức** – AT200114

## 📄 Giấy phép

Dự án được phân phối dưới giấy phép MIT. Xem file [LICENSE](LICENSE) để biết thêm chi tiết.

---

*Hà Nội – 2026*
