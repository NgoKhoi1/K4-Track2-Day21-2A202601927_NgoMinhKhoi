# Báo Cáo Lab Day 21 - CI/CD cho AI Systems

<!--
HƯỚNG DẪN - đọc rồi XÓA TOÀN BỘ các khối chú thích này sau khi điền xong:

  - Giới hạn: KHÔNG QUÁ 1 TRANG A4, tương đương khoảng 450 - 550 từ nội dung.
  - Chỉ điền vào các chỗ ___ và các ô trong bảng. Không thêm mục mới.
  - Viết bằng câu hoàn chỉnh, không gạch đầu dòng cụt lủn.
  - Kiểm tra độ dài sau khi đã xóa hết chú thích:
        wc -w nop-bai/bao-cao.md
    và xem trước bản in bằng cách mở file trên GitHub rồi Ctrl+P / Cmd+P.
-->

| | |
|---|---|
| Họ và tên | Ngô Minh Khôi |
| MSSV | 2A202601927 |
| Lớp / Khóa | K4 |
| Repo GitHub | https://github.com/NgoKhoi1/K4-Track2-Day21-2A202601927_NgoMinhKhoi |
| Ngày nộp | ___ |

---

## 1. Bộ Siêu Tham Số Đã Chọn và Lý Do

| Lần chạy | n_estimators | learning_rate | max_depth | f1_score | accuracy |
|---|---|---|---|---|---|
| 1 | 100 | 0.1 | 3 | 0.7109 | 0.878 |
| 2 | 50 | 0.05 | 2 | 0.6051 | 0.846 |
| 3 | 200 | 0.1 | 5 | 0.7149 | 0.874 |
| 4 | 200 | 0.2 | 3 | 0.7032 | 0.870 |

**Bộ siêu tham số đã chọn:** `n_estimators=200`, `learning_rate=0.1`, `max_depth=5`.

**Lý do:** Chọn bộ này vì nó có f1_score cao nhất trong 4 lần chạy (0.7149), dù accuracy (0.874) thấp hơn một chút so với lần chạy 1 (0.878) - tuy nhiên: lần có accuracy cao nhất không phải lần có f1 cao nhất, vì accuracy cao đó đến từ việc model thiên về đoán "thu nhập thấp" (lớp đa số) nhiều hơn, bỏ sót một số ca thu nhập cao. Về đánh đổi n_estimators/learning_rate: giảm cả hai cùng lúc (lần 2: 50 cây, lr=0.05) khiến model học chưa tới, f1 rớt hẳn xuống dưới ngưỡng 0.65. Tăng learning_rate lên 0.2 (lần 4) cũng không cải thiện, có thể model học hơi vội nên kém ổn định hơn so với lr=0.1 kết hợp cây sâu hơn (max_depth=5).

---

## 2. Vì Sao Ngưỡng Chất Lượng Đặt Trên F1 Chứ Không Phải Accuracy

Tập Adult Income mất cân bằng khá rõ: chỉ 24.8% mẫu thuộc nhóm thu nhập >50K, còn lại 75.2% là thu nhập thấp. Với tỷ lệ này, một model "lười biếng" chỉ luôn đoán "thu nhập thấp" cho mọi input vẫn đạt accuracy tới 0.752, dù nó chẳng học được gì và không bắt được một ca thu nhập cao nào (f1 = 0). Vậy nên accuracy ở bài toán này có thể rất dễ gây hiểu lầm - số cao không đồng nghĩa model tốt. F1-score của lớp dương thì đo trực tiếp việc model vừa đoán đúng (precision) vừa đoán đủ (recall) các ca thu nhập cao, nên phản ánh chất lượng thật sự chính xác hơn nhiều. Đây cũng là lý do khi tính f1_score không dùng `average="weighted"` hay `"macro"`, vì hai cách này gộp cả lớp đa số vào phép tính, kéo điểm lên cao giả tạo và làm mất hẳn ý nghĩa của ngưỡng chặn 0.65.

---

## 3. Khó Khăn Gặp Phải và Cách Giải Quyết

<!-- Nêu 2 - 3 khó khăn thật, mỗi ô một câu ngắn. -->

| Khó khăn | Nguyên nhân | Cách giải quyết |
|---|---|---|
| ___ | ___ | ___ |
| ___ | ___ | ___ |
| ___ | ___ | ___ |

---

## 4. So Sánh Bước 2 và Bước 3 (bắt buộc, 2 - 3 câu)

<!-- Lấy số liệu từ bảng ở mục 3.6 của tasks/buoc-3.md. -->

| | f1_score | accuracy |
|---|---|---|
| Bước 2 (chỉ `train_batch1`) | ___ | ___ |
| Bước 3 (thêm `train_batch2`) | ___ | ___ |

**Nhận xét:** ___

<!--
Một câu trả lời trung thực kiểu "f1 giảm 0,01 vì dữ liệu mới cùng phân phối, không mang
thêm thông tin mới" được đánh giá cao hơn kết luận sai rằng thêm dữ liệu luôn tốt hơn.
-->

---

## 5. Phần Bonus Đã Thực Hiện (nếu có)

<!-- Xóa cả mục 5 nếu không làm bonus. Mỗi bonus tối đa 1 dòng. -->

- [ ] Bonus 1 - Tracking MLflow từ xa với DagsHub: ___
- [ ] Bonus 2 - Điều chỉnh ngưỡng quyết định: ___
- [ ] Bonus 3 - Báo cáo precision / recall tự động: ___
- [ ] Bonus 4 - Hoàn trả về phiên bản trước: ___
- [ ] Bonus 5 - Cảnh báo lệch lạc dữ liệu: ___
