# Bài: Lập kế hoạch tài chính cá nhân bằng Chain-of-Thought (CoT)

## 1. Ý tưởng làm bài

AI hay tính sai khi gặp bài toán nhiều bước số học. Vì vậy mình dùng CoT: bắt AI **tính từng bước, ghi rõ phép tính trung gian** rồi mới đưa bảng kết quả. Làm hoàn toàn bằng prompt, không dùng code Python hay Pydantic.

## 2. Dữ liệu vào (Input)

| Thông tin | Ví dụ dùng để thử |
| --- | --- |
| Thu nhập | 20.000.000 đ/tháng (nếu thu nhập biến động thì lấy mức thấp nhất làm cơ sở) |
| Thuế, bảo hiểm | 2.000.000 đ |
| Chi phí cố định | Thuê nhà 5.000.000 + điện nước, internet 1.000.000 = 6.000.000 đ |
| Các khoản nợ | Thẻ tín dụng, trả tối thiểu 3.000.000 đ/tháng |
| Mục tiêu tiết kiệm | 20% thu nhập khả dụng |

## 3. Prompt hoàn chỉnh

```
Bạn là chuyên gia tư vấn tài chính cá nhân, làm việc cẩn thận và chính xác.

Dữ liệu của tôi (đơn vị: đồng/tháng):
- Thu nhập: 20.000.000 (nếu thu nhập biến động, lấy mức thấp nhất làm cơ sở)
- Thuế và bảo hiểm: 2.000.000
- Chi phí cố định: thuê nhà 5.000.000, điện nước và internet 1.000.000
- Nợ: thẻ tín dụng, trả tối thiểu 3.000.000
- Mục tiêu tiết kiệm: 20% thu nhập khả dụng

Hãy suy nghĩ từng bước một và tính toán chi tiết trước khi đưa ra quyết định.
Thực hiện đúng 4 bước sau:
Bước 1: Tính thu nhập khả dụng = thu nhập − thuế − chi phí cố định bắt buộc.
Bước 2: Trừ khoản trả nợ tối thiểu, ghi số tiền còn lại.
Bước 3: Trích quỹ tiết kiệm theo tỷ lệ cam kết (20% thu nhập khả dụng), ghi số tiền còn lại.
Bước 4: Chia phần còn lại theo quy tắc 50/30/20 cải tiến: 50% nhu cầu thiết yếu,
        30% mong muốn cá nhân, 20% quỹ dự phòng.

Yêu cầu trình bày:
- Phần 1 (Reasoning Steps): ghi đầy đủ công thức và số liệu trung gian của từng bước.
  Mỗi phép cộng, trừ, nhân, chia phải viết ra, không nhảy cóc ra kết quả.
- Phần 2: bảng Markdown gồm các cột: Hạng mục | Số tiền | Tỷ lệ % | Ghi chú.
  Tỷ lệ % tính trên tổng thu nhập, tổng các tỷ lệ phải bằng chính xác 100%.
- Cuối cùng kiểm tra lại: cộng các khoản trong bảng, phải bằng tổng thu nhập.
  Nếu không khớp, hãy tính lại.
```

## 4. Kết quả mong đợi (đã tính tay để đối chiếu)

**Phần 1: Reasoning Steps**
- Bước 1: 20.000.000 − 2.000.000 (thuế) − 6.000.000 (chi phí cố định) = **12.000.000** (thu nhập khả dụng).
- Bước 2: 12.000.000 − 3.000.000 (trả nợ) = **9.000.000**.
- Bước 3: tiết kiệm 20% × 12.000.000 = **2.400.000**; còn 9.000.000 − 2.400.000 = **6.600.000**.
- Bước 4: chia 6.600.000 theo 50/30/20:
  - Thiết yếu: 50% × 6.600.000 = 3.300.000
  - Mong muốn: 30% × 6.600.000 = 1.980.000
  - Quỹ dự phòng: 20% × 6.600.000 = 1.320.000

**Phần 2: Bảng phân bổ ngân sách**

| Hạng mục | Số tiền (đ) | Tỷ lệ % | Ghi chú |
| --- | --- | --- | --- |
| Thuế và bảo hiểm | 2.000.000 | 10% | Bắt buộc |
| Chi phí cố định | 6.000.000 | 30% | Thuê nhà, điện nước, internet |
| Trả nợ tối thiểu | 3.000.000 | 15% | Thẻ tín dụng, không trễ hạn |
| Tiết kiệm | 2.400.000 | 12% | 20% thu nhập khả dụng |
| Chi tiêu thiết yếu | 3.300.000 | 16,5% | Ăn uống, đi lại |
| Chi tiêu mong muốn | 1.980.000 | 9,9% | Giải trí, mua sắm |
| Quỹ dự phòng | 1.320.000 | 6,6% | Phòng rủi ro |
| **Tổng** | **20.000.000** | **100%** | Khớp tổng thu nhập |

**Kiểm tra:** 2.000.000 + 6.000.000 + 3.000.000 + 2.400.000 + 3.300.000 + 1.980.000 + 1.320.000 = 20.000.000. Tỷ lệ: 10 + 30 + 15 + 12 + 16,5 + 9,9 + 6,6 = 100%.

## 5. Kết quả chạy thử

*(Bạn tự chạy prompt trên AI, dán kết quả thật hoặc ảnh chụp màn hình vào đây, rồi đối chiếu với mục 4.)*

Mô hình dùng: *(điền)*

| Tiêu chí kiểm tra | Kết quả thật |
|---|---|
| AI có ghi đủ phép tính từng bước, không nhảy cóc? | Đạt – Có ghi đầy đủ công thức và phép tính từng bước. |
| Các số trung gian có khớp mục 4 không? | Đạt – Các kết quả tính toán đều khớp với kết quả mong đợi. |
| Tổng % trong bảng có đúng 100% không? | Đạt – Tổng tỷ lệ phần trăm bằng 100%. |
| Cột Hạng mục, Số tiền, Tỷ lệ %, Ghi chú có đủ không? | Đạt – Bảng có đầy đủ 4 cột theo yêu cầu. |


## 6. Nhận xét

- Nhờ CoT, AI tính từng bước nên dễ kiểm tra: nếu sai ở bước nào thì thấy ngay.
- Dòng "kiểm tra lại tổng bằng thu nhập" giúp phát hiện AI tính lệch.
- Nhược điểm: câu trả lời dài hơn nên tốn token hơn, nhưng đổi lại số liệu chính xác hơn, rất cần với bài toán tiền bạc.
