[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=24112901&assignment_repo_type=AssignmentRepo)
# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** tamnd@bachmai.edu.vn
**Name:** Nguyen Duc Tam
**Student ID:** 2A202600946

---

## Mo ta

Bài Lab này tập trung vào việc xây dựng một đường ống dẫn dữ liệu ETL (Extract - Transform - Load) tự động, kết hợp với các kỹ năng kiểm định dữ liệu (Data Validation) và quan sát dữ liệu (Data Observability) nhằm bảo vệ tính toàn vẹn của mô hình/hệ thống AI Agent chống lại dữ liệu bị đầu độc hoặc dữ liệu rác (Garbage/Poisoned Data).

---

## Cach chay (How to Run)

### Prerequisites

Khởi tạo môi trường ảo và cài đặt các thư viện cần thiết:
```bash
# Tao moi truong ao
python -m venv venv

# Kich hoat moi truong ao (Windows)
.\venv\Scripts\activate

# Cai dat pandas va pytest
pip install pandas pytest
```

### Chay ETL Pipeline

Chạy lệnh dưới đây để trích xuất dữ liệu từ `raw_data.json`, làm sạch, biến đổi và lưu kết quả vào `processed_data.csv`:
```bash
python solution.py
```

### Chay Agent Simulation (Stress Test)

Để mô phỏng hành vi của AI Agent trước dữ liệu sạch so với dữ liệu rác, hãy chạy các lệnh sau:
```bash
# 1. Tao du lieu rac (garbage_data.csv)
python generate_garbage.py

# 2. Chay gia lap agent
python agent_simulation.py
```

### Chay Unit Tests

Chạy test suite tự động hóa để đánh giá bài làm:
```bash
pytest tests/test_autograder.py
```

---

## Cau truc thu muc

```
├── solution.py              # ETL Pipeline script
├── processed_data.csv       # Output cua pipeline (clean data)
├── garbage_data.csv         # File du lieu loi/rac (garbage data)
├── generate_garbage.py      # Script tao garbage data
├── agent_simulation.py      # Script gia lap AI Agent RAG
├── experiment_report.md     # Bao cao thi nghiem stress test
└── README.md                # File nay huong dan chi tiet
```

---

## Ket qua

- **Tổng số records đầu vào:** 5 records (từ file `raw_data.json`).
- **Số lượng records hợp lệ (Clean):** 3 records (đã được làm sạch và lưu trữ vào `processed_data.csv`).
- **Số lượng records bị loại bỏ (Dropped):** 2 records (do giá không hợp lệ `<= 0` hoặc trường danh mục `category` bị bỏ trống).
- **Các bước Transform đã thực hiện:**
  - Chuẩn hóa `category` thành định dạng Title Case.
  - Tính toán trường `discounted_price` giảm 10% so với giá trị gốc.
  - Thêm cột dấu thời gian `processed_at` biểu diễn thời điểm xử lý bản ghi.
- **Stress Test:** AI Agent đưa ra câu trả lời chính xác khi dùng dữ liệu sạch (`Laptop` trị giá `$1200`), nhưng bị đánh lừa đưa ra câu trả lời sai lệch, nguy hiểm khi dùng dữ liệu rác chứa outlier cực đoan (`Nuclear Reactor` trị giá `$999999`).
