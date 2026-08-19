# Reflection — Lab 19

**Tên:** _Đinh Thị Diễm Quỳnh_
**Cohort:** _A20_
**Path đã chạy:** _lite_

---

## Câu hỏi (≤ 200 chữ)

> Trên golden set 50 queries, mode nào thắng ở loại query nào (`exact` /
> `paraphrase` / `mixed`), và tại sao? Khi nào bạn **không** dùng hybrid
> (i.e. khi nào pure BM25 hoặc pure vector là lựa chọn đúng)?

Trên golden set 50 queries: keyword (BM25) đạt 77.8%, semantic (vector) đạt
73.2%, hybrid (RRF k=60) đạt 78.6% Precision@10 — hybrid thắng trung bình.
Theo loại query: `exact` (15 câu) BM25 và hybrid ngang nhau (96.7%) vì từ
khóa kỹ thuật xuất hiện verbatim trong doc nên BM25 đã đủ mạnh, semantic
yếu hơn (88.7%). `paraphrase` (15 câu) cả 3 mode đều yếu (24–33%) vì
embedding `bge-small-en` train tiếng Anh, không nắm tốt paraphrase tiếng
Việt. `mixed` (20 câu) hybrid thắng rõ nhất (100% so với 97–98.5%) vì kết
hợp được cả tín hiệu từ khóa lẫn ngữ nghĩa. Không nên dùng hybrid khi: (1)
corpus có thuật ngữ chuyên ngành cố định, query luôn chứa keyword chính
xác (exact-match dominant) — BM25 đơn giản, rẻ, đủ tốt; (2) latency budget
cực khắt khe và corpus nhỏ, gọi 2 retriever + fusion không đáng chi phí so
với mức tăng chất lượng nhỏ.

---

## Điều ngạc nhiên nhất khi làm lab này

Hybrid P99 latency thực đo được (~107ms) vượt xa ngưỡng rubric 50ms dù đã
warm-up 10 query trước khi đo — hoá ra bottleneck không phải cold-start mà
là tốc độ inference CPU-bound của ONNX runtime trên máy Windows này (mode
keyword không gọi embedding chỉ mất ~4ms P99, còn semantic/hybrid luôn
~90-110ms bất kể warm hay không). Cho thấy latency rubric phụ thuộc nhiều
vào phần cứng chạy thật, không chỉ vào thuật toán.

---

## Bonus challenge

- [ ] Đã làm bonus (xem `bonus/`)
- [ ] Pair work với: _<tên đồng đội nếu có>_
