# K3 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Ở cả bốn mức temperature, mô hình đều trả lời đúng nội dung vì đây là một câu hỏi mang tính khái niệm, nên sự khác biệt chủ yếu nằm ở cách diễn đạt. Ở temperature = 0.0, mô hình trả lời ổn định và gần như giống nhau qua nhiều lần chạy. Khi tăng temperature lên 0.5, 1.0 và 1.5, câu trả lời trở nên đa dạng và sáng tạo hơn, nhưng kém nhất quán và lan man hơn.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ chọn temperature = 0.2–0.3 cho chatbot hỗ trợ khách hàng vì mức này giúp câu trả lời ổn định, nhất quán và chính xác, giảm nguy cơ tạo ra thông tin sai hoặc trả lời khác nhau cho cùng một câu hỏi. Đồng thời, phản hồi vẫn đủ tự nhiên để mang lại trải nghiệm tốt cho người dùng.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Theo bảng giá OpenAI, GPT-4o có chi phí output khoảng 10 lần GPT-4o-mini (10 USD so với 1 USD trên mỗi 1 triệu token). Với 10.000 người dùng × 3 lượt gọi × 350 token đầu ra ≈ 10,5 triệu token/ngày, GPT-4o sẽ tốn khoảng 105 USD/ngày, trong khi GPT-4o-mini chỉ khoảng 10,5 USD/ngày. GPT-4o phù hợp với các tác vụ yêu cầu lập luận phức tạp hoặc độ chính xác cao, còn GPT-4o-mini phù hợp cho chatbot FAQ, hỗ trợ khách hàng hoặc các tác vụ hội thoại thông thường.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Hai phản hồi khác nhau rõ rệt về cách diễn đạt và mức độ chi tiết. Persona giáo viên tiểu học sử dụng từ ngữ đơn giản, câu ngắn và ví dụ gần gũi để trẻ em dễ hiểu, trong khi persona chuyên gia tài chính dùng nhiều thuật ngữ chuyên môn và giải thích sâu hơn về cơ chế hoạt động của blockchain. Điều này cho thấy system prompt định hướng phong cách, mức độ chuyên sâu và đối tượng mà mô hình hướng tới, mặc dù câu hỏi của người dùng hoàn toàn giống nhau.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Số token do tiktoken đếm thường chênh lệch khoảng 10–20% so với cách ước lượng bằng số từ / 0.75, tùy nội dung. tiktoken phản ánh đúng cách mô hình mã hóa văn bản, còn công thức ước lượng chỉ mang tính gần đúng. Tiếng Việt thường tiêu tốn nhiều token hơn tiếng Anh vì có nhiều từ ghép, dấu thanh và ký tự Unicode, khiến bộ tokenizer phải chia nhỏ thành nhiều token hơn.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming hữu ích khi mô hình tạo câu trả lời dài hoặc mất vài giây để xử lý, vì người dùng có thể đọc nội dung ngay khi nó được sinh ra thay vì phải chờ toàn bộ phản hồi hoàn tất. Điều này giúp giảm cảm giác chờ đợi và cải thiện trải nghiệm sử dụng. Ngược lại, non-streaming phù hợp với các tác vụ trả lời ngắn, cần nhận kết quả đầy đủ trước khi hiển thị hoặc khi xử lý dữ liệu theo lô.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp giảm áp lực lên máy chủ khi API bị quá tải bằng cách tăng dần thời gian chờ giữa các lần thử lại. So với việc luôn chờ một khoảng thời gian cố định, cách này giảm khả năng nhiều client cùng gửi lại yêu cầu vào đúng một thời điểm. Nếu hàng nghìn client đều retry sau đúng 1 giây, chúng có thể tạo ra các đợt truy cập đồng thời liên tiếp, khiến máy chủ tiếp tục quá tải.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Persona: Trợ lý AI chuyên về Machine Learning và LLM.
System prompt: "Bạn là trợ lý AI chuyên về Machine Learning và LLM. Hãy trả lời bằng tiếng Việt, giải thích rõ ràng, có cấu trúc, sử dụng thuật ngữ chính xác và ưu tiên ví dụ thực tế khi cần." Tôi yêu cầu "trả lời bằng tiếng Việt" để đảm bảo câu trả lời thống nhất với người dùng và dễ tiếp cận hơn. Cụm "ưu tiên ví dụ thực tế" giúp các khái niệm kỹ thuật trở nên trực quan, dễ hiểu và dễ áp dụng trong học tập cũng như thực tế.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất là chatbot chỉ lưu tối đa 3 lượt hội thoại, nên dễ quên ngữ cảnh khi cuộc trò chuyện kéo dài. Một cải thiện phù hợp là bổ sung bộ nhớ dài hạn (long-term memory) hoặc RAG, trong đó các thông tin quan trọng của cuộc hội thoại được lưu trữ và truy xuất khi cần. Điều này giúp chatbot duy trì ngữ cảnh tốt hơn mà không phải gửi toàn bộ lịch sử trong mỗi lần gọi API

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn README
