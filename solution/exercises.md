# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi tăng temperature từ 0.0 lên 1.5, phản hồi thay đổi rõ về sự thật được chọn, cách diễn đạt và ngôn ngữ sử dụng. Ở temperature thấp, câu trả lời có xu hướng trực tiếp và ổn định hơn; ở mức cao, phản hồi đa dạng và khó dự đoán hơn, thậm chí có thể pha tiếng Anh. Tuy nhiên, temperature cao không đồng nghĩa với chất lượng cao hơn vì câu trả lời có thể kém nhất quán.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ đặt temperature khoảng 0.2–0.3 cho chatbot hỗ trợ khách hàng. Mức thấp giúp câu trả lời nhất quán, chính xác và bám sát chính sách hơn, đồng thời giảm nguy cơ chatbot sáng tạo ra thông tin không có thật.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Mỗi ngày có 10.000 × 3 × 350 = 10.500.000 output token. Theo bảng giá của lab, GPT-4o có giá output $0,010/1K token, còn GPT-4o-mini là $0,0006/1K token, nên GPT-4o đắt hơn khoảng 16,7 lần (khoảng $105/ngày so với $6,30/ngày, chỉ tính output). GPT-4o xứng đáng cho các tình huống cần suy luận và độ chính xác cao, ví dụ tư vấn chuyên môn hoặc phân tích tài liệu quan trọng; nên dùng mini cho các tác vụ đơn giản, số lượng lớn như trả lời FAQ hoặc phân loại câu hỏi.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Với persona giáo viên tiểu học, không tạo được câu trả lời giải thích cho trẻ 8 tuổi. Với persona tài chính dùng từ ngữ chuyên sâu hơn

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Đoạn văn có 104 từ. Hàm count_tokens() trả về 118 token, trong khi ước lượng theo công thức số từ / 0,75 là khoảng 138,67 token. Hai kết quả chênh lệch khoảng 14,9%; điều này cho thấy công thức ước lượng theo số từ không hoàn toàn chính xác. Với tiếng Việt, tokenizer có thể chia các từ có dấu và các đơn vị âm tiết theo cách khác với đếm từ thông thường, nên số token thực tế có thể khác đáng kể.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất khi phản hồi dài hoặc người dùng cần cảm giác hệ thống đang xử lý ngay, ví dụ chatbot hỗ trợ, trợ lý viết nội dung hoặc giải thích tài liệu. Người dùng có thể bắt đầu đọc câu trả lời ngay thay vì chờ toàn bộ nội dung được tạo xong, nên trải nghiệm nhanh và tự nhiên hơn. Non-streaming phù hợp hơn khi chương trình cần nhận toàn bộ kết quả trước khi xử lý, chẳng hạn kiểm tra định dạng JSON, lưu vào cơ sở dữ liệu hoặc hiển thị một kết quả ngắn.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp giảm tải cho API đang quá tải vì mỗi lần lỗi, client chờ lâu hơn trước khi thử lại: ví dụ 0,5 giây rồi 1 giây. So với luôn chờ 1 giây, cách này tránh gửi request retry liên tục khi server chưa kịp phục hồi. Nếu hàng nghìn client cùng retry với một delay cố định, chúng có thể cùng gửi request trở lại một lúc, tạo thêm một đợt quá tải mới và làm lỗi kéo dài hơn.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Tôi chọn persona: “Bạn là trợ lý học tập AI thân thiện. Luôn trả lời ngắn gọn bằng tiếng Việt, dùng ví dụ đơn giản.” Cụm “trợ lý học tập AI thân thiện” giúp phản hồi có giọng điệu hỗ trợ và phù hợp với người mới học. Yêu cầu “bằng tiếng Việt” giúp câu trả lời dễ đọc, còn “dùng ví dụ đơn giản” khiến các khái niệm như blockchain được giải thích bằng tình huống gần gũi.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất là trợ lý chỉ giữ tối đa 3 lượt hội thoại gần nhất, tức 6 message, nên có thể quên thông tin quan trọng ở đầu cuộc trò chuyện dài. Một cải thiện cụ thể là tạo bản tóm tắt lịch sử cũ trước khi cắt history: khi số message vượt giới hạn, gửi các message cũ cho model để tóm tắt ngắn, rồi lưu summary đó như một system message cùng với 3 lượt mới nhất. Cách này giữ được ngữ cảnh quan trọng nhưng vẫn kiểm soát số token và chi phí.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
