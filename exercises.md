# Ngày 1 — Bài Tập & Phản Ánh

## Nền Tảng LLM API | Phiếu Thực Hành

**Thời lượng:** 1:30 giờ  
**Cấu trúc:** Lập trình cốt lõi (60 phút) → Bài tập mở rộng (30 phút)

---

## Phần 1 — Lập Trình Cốt Lõi (0:00–1:00)

Chạy các ví dụ trong Google Colab tại: https://colab.research.google.com/drive/172zCiXpLr1FEXMRCAbmZoqTrKiSkUERm?usp=sharing

Triển khai tất cả TODO trong `template.py`. Chạy `pytest tests/` để kiểm tra tiến độ.

**Điểm kiểm tra:** Sau khi hoàn thành 4 nhiệm vụ, chạy:

```bash
python template.py
```

Bạn sẽ thấy output so sánh phản hồi của GPT-4o và GPT-4o-mini.

---

## Phần 2 — Bài Tập Mở Rộng (1:00–1:30)

### Bài tập 2.1 — Độ Nhạy Của Temperature

Gọi `call_openai` với các giá trị temperature 0.0, 0.5, 1.0 và 1.5 sử dụng prompt **"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
Khi tăng dần temperature từ 0.0 lên 1.5, câu trả lời dịch chuyển rõ rệt từ trạng thái nhất quán, chính xác sang sáng tạo và cuối cùng là hỗn loạn. Ở mức 0.0 và 0.5, mô hình đưa ra các sự thật quen thuộc (như xuất khẩu cà phê, hang Sơn Đoòng) với câu từ logic, gãy gọn; nhưng từ mức 1.0 đến 1.5, từ ngữ bắt đầu trở nên bay bổng, ít phổ biến hơn và ở mức cao nhất (1.5) có thể xuất hiện hiện tượng "ảo tưởng" (hallucination) hoặc hành văn rời rạc.
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
Em sẽ đặt temperature từ 0.0 đến 0.2. Bởi vì chatbot hỗ trợ khách hàng đòi hỏi sự chính xác tuyệt đối, nhất quán và đáng tin cậy khi cung cấp thông tin về chính sách, giá cả hoặc quy trình; việc đặt temperature thấp sẽ hạn chế tối đa việc AI "bịa" ra thông tin sai sự thật làm ảnh hưởng đến trải nghiệm của khách hàng.

---

### Bài tập 2.2 — Đánh Đổi Chi Phí

Xem xét kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người thực hiện 3 lần gọi API, mỗi lần trung bình ~350 token.

**Ước tính xem GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này:**
Dựa trên bảng giá PRICING_1M_TOKENS trong file code:Giá GPT-4o: Input $5.00 / Output $20.00Giá GPT-4o-mini: Input $0.150 / Output $0.600Tỷ lệ chênh lệch giá cho cả Token Input và Output đều là: $\frac{5.00}{0.150} = \frac{20.00}{0.600} \approx 33.33$. Do đó, với cùng một lượng workload và phân bổ token như nhau, GPT-4o sẽ đắt hơn GPT-4o-mini khoảng 33.3 lần.
**Mô tả một trường hợp mà chi phí cao hơn của GPT-4o là xứng đáng, và một trường hợp GPT-4o-mini là lựa chọn tốt hơn:**
Trường hợp GPT-4o xứng đáng: Khi hệ thống cần xử lý các tác vụ phức tạp, đòi hỏi tư duy logic cao, lập luận đa bước hoặc phân tích dữ liệu chuyên sâu (ví dụ: Trợ lý phân tích tài chính doanh nghiệp, chấm điểm code tự động, hoặc biên dịch tài liệu pháp lý chuyên ngành).

## Trường hợp GPT-4o-mini tốt hơn: Khi hệ thống thực hiện các tác vụ lặp đi lặp lại có cấu trúc đơn giản, yêu cầu tốc độ phản hồi nhanh và tối ưu chi phí với lượng người dùng lớn (ví dụ: Chatbot phân loại ý định khách hàng - Intent Classification, tóm tắt đoạn hội thoại ngắn, hoặc trích xuất thông tin cơ bản từ email).

### Bài tập 2.3 — Trải Nghiệm Người Dùng với Streaming

**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì non-streaming lại phù hợp hơn?** (1 đoạn văn)
Streaming quan trọng nhất trong các ứng dụng trò chuyện trực tiếp (Chatbot giao tiếp, Trợ lý ảo AI) nơi người dùng cần nhìn thấy phản hồi ngay lập tức để giảm "thời gian chờ đợi nhận thức" (perceived latency), tạo cảm giác mượt mà giống như đang nhắn tin với người thật dù câu trả lời dài. Ngược lại, Non-streaming lại phù hợp hơn trong các tác vụ xử lý ngầm (background jobs) hoặc tích hợp hệ thống (API-to-API), nơi ứng dụng cần nhận toàn bộ kết quả hoàn chỉnh dưới dạng cấu trúc (như JSON) để xử lý logic tiếp theo trước khi hiển thị cho người dùng, ví dụ như tự động tạo báo cáo, phân tích sắc thái văn bản (Sentiment Analysis), hoặc trích xuất dữ liệu hàng loạt.

## Danh Sách Kiểm Tra Nộp Bài

- [ ] Tất cả tests pass: `pytest tests/ -v`
- [ ] `call_openai` đã triển khai và kiểm thử
- [ ] `call_openai_mini` đã triển khai và kiểm thử
- [ ] `compare_models` đã triển khai và kiểm thử
- [ ] `streaming_chatbot` đã triển khai và kiểm thử
- [ ] `retry_with_backoff` đã triển khai và kiểm thử
- [ ] `batch_compare` đã triển khai và kiểm thử
- [ ] `format_comparison_table` đã triển khai và kiểm thử
- [ ] `exercises.md` đã điền đầy đủ
- [ ] Sao chép bài làm vào folder `solution` và đặt tên theo quy định
