# VNPT eRecon

**Tự động hoá đối soát tiền điện cơ sở hạ tầng VNPT với Điện lực (EVN) bằng AI.**

> Sản phẩm dự thi VNPT Hackathon "Build with AI" 2026 — Trung tâm Hạ Tầng, Viễn thông Phú Thọ.

---

## 1. Bài toán

Các cơ sở hạ tầng của VNPT (nhà trạm, điểm thiết bị…) dùng điện hàng tháng như hộ dân, mỗi điểm là một hợp đồng/hoá đơn riêng với Điện lực. Số lượng rất lớn (~1.500 điểm chỉ riêng Phú Thọ) và thanh toán đã chuyển sang **trích nợ tự động qua ngân hàng**.

Sau khi gom việc thanh toán về tập trung, nhân sự đối soát giảm mạnh (trước ~18 người theo 18 địa bàn, nay còn 3 người), trong khi khối lượng giữ nguyên. Mỗi hoá đơn xử lý thủ công đầy đủ (tải hoá đơn → đối chiếu → nhập báo cáo lên cổng nội bộ) tốn ~20–30 phút. Quy mô này vượt năng lực xử lý thủ công.

## 2. Giải pháp

eRecon tự động hoá **đối soát 3 chiều**:

```
Hoá đơn EVN   ⇄   Sao kê ngân hàng (auto-debit)   ⇄   Danh mục điểm điện VNPT
```

Khoá đối soát theo `(mã điểm đo, kỳ)`, phát hiện lệch số tiền / thiếu hoá đơn / thiếu giao dịch, rồi xuất báo cáo chuẩn để nộp lên cổng quản lý nội bộ.

## 3. Kiến trúc

| Khối | Vai trò |
|---|---|
| **Ingest** | Thu thập 3 nguồn: hoá đơn EVN, sao kê ngân hàng (Excel), danh mục điểm điện VNPT |
| **Extract** | Trích xuất dữ liệu hoá đơn bằng **vision-LLM** + schema kiểm chứng (self-validation), có cơ chế fallback |
| **Reconcile & Report** | Engine đối soát 3 chiều, đánh dấu sai lệch, xuất báo cáo Excel |
| **Export** | Kết xuất đúng định dạng cổng nội bộ để nộp |

Thiết kế theo Strategy pattern (`DocumentSource` / `Extractor` / `Reconciler`) để mở rộng sang đơn vị/nguồn hoá đơn khác.

## 4. Công nghệ

Python 3.12 · FastAPI · Vision-LLM · openpyxl · Docker / docker-compose · GitHub Actions (CI/CD) · pytest.

Cloud Native (container hoá, hướng SmartCloud) — đầy đủ pipeline CI/CD.

## 5. An toàn thông tin

Toàn bộ demo và kiểm thử **chỉ dùng dữ liệu giả lập / ẩn danh (mock / anonymized)**. Repo này **không chứa** dữ liệu khách hàng thật, thông tin tài khoản ngân hàng thật, hoá đơn thật hay bất kỳ thông tin xác thực nào.

## 6. Tính mới & nguồn gốc

Toàn bộ mã nguồn dự thi được **viết mới** trong repo này, trong khung thời gian của cuộc thi. Nhóm có tham khảo *cách tiếp cận* từ một số nguyên mẫu nội bộ trước đó (chỉ ở mức đặc tả luồng và định dạng đầu ra), **không sao chép mã nguồn**. Giá trị mới cốt lõi: trích xuất bằng vision-LLM và đối soát 3 chiều (đặc biệt là chiều sao kê ngân hàng auto-debit).

## 7. Trạng thái

🚧 Đang phát triển cho Vòng 2 Hackathon 2026.

## 8. Đội ngũ

Trung tâm Hạ Tầng — Viễn thông Phú Thọ (VNPT).

---

*License: TBD.*
