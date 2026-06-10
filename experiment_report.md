# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** 2A202600946
**Name:** Nguyen Duc Tam
**Date:** 2026-06-10

---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | Agent: Based on my data, the best choice is Laptop at $1200. | 10 | The agent correctly identified the Laptop as the highest-priced electronics item. |
| Garbage Data (`garbage_data.csv`) | Agent: Based on my data, the best choice is Nuclear Reactor at $999999. | 2 | The agent selected a dangerous outlier (Nuclear Reactor) which is not a normal electronic product. |

---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

Agent đã trả về câu trả lời sai vì dữ liệu rác (Garbage Data) không được qua bộ lọc kiểm tra tính chính xác và độ tin cậy của dữ liệu trước khi đưa vào mô hình (Knowledge Base của Agent). 

Cụ thể, các vấn đề dữ liệu trong file:
- **Duplicate IDs (Trùng ID):** Việc trùng lặp ID làm nhiễu loạn dữ liệu và gây nhầm lẫn khi truy xuất hoặc liên kết thông tin.
- **Wrong Data Types (Sai kiểu dữ liệu):** Nhập giá trị chuỗi văn bản như `ten dollars` thay vì số thực có thể gây lỗi hệ thống hoặc khiến thuật toán tìm giá lớn nhất `idxmax()` bỏ qua hoặc so sánh sai.
- **Extreme Outliers (Điểm dị biệt cực đoan):** Thiết bị `Nuclear Reactor` với mức giá `999999` là một outlier nghiêm trọng, vượt xa các thiết bị điện tử gia dụng thông thường. Nếu không lọc bỏ outlier, agent sẽ bị đánh lừa bởi giá trị lớn này.
- **Null Values (Giá trị rỗng):** Bản ghi chứa `None`/`Null` có thể gây crash logic lập trình hoặc dẫn tới việc agent phán đoán sai lệch.

Nếu không thực hiện chuẩn hóa và làm sạch (Validation), hệ thống AI Agent (như RAG) sẽ đọc thông tin sai lệch này và đưa ra câu trả lời không chính xác, thậm chí gây nguy hại.

---

## 3. Ket luan

**Quality Data > Quality Prompt?**

Đồng ý. Cho dù ta có viết Prompt tốt và tối ưu đến đâu đi chăng nữa, nếu dữ liệu nguồn (Knowledge Base) đầu vào bị sai lệch, chứa thông tin nhiễu, outlier hoặc lỗi kiểu dữ liệu, thì câu trả lời của AI Agent vẫn sẽ sai lệch. Bản chất của AI RAG là tìm kiếm thông tin có sẵn trong dữ liệu để trả lời, do đó chất lượng dữ liệu (Data Quality) luôn là yếu tố cốt lõi quyết định độ tin cậy của hệ thống.

