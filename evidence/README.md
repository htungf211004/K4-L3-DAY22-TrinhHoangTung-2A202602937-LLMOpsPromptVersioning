# Phân tích kết quả Prompt V1 và V2

## Khác biệt thiết kế

- **V1** dùng giọng thân thiện, yêu cầu trả lời ngắn gọn 2–4 câu và chỉ dựa trên context; nếu context không có thông tin thì nói rõ là không biết.
- **V2** đặt vai trò chuyên gia phân tích, yêu cầu xác định facts liên quan và trình bày có tổ chức trong 3–5 câu; không suy đoán ngoài context.

Cả hai phiên bản đều nhận cùng câu hỏi và context RAG. Theo cấu hình trong mã nguồn, đánh giá dùng 50 cặp QA, cùng retriever lấy tối đa 3 tài liệu và cùng cấu hình LLM/embeddings.

## Kết quả RAGAS

| Chỉ số | V1 | V2 | Nhận xét |
|---|---:|---:|---|
| Faithfulness | **0.9562** | 0.9434 | V1 cao hơn 0.0128 |
| Answer relevancy | **0.9145** | 0.8915 | V1 cao hơn 0.0230 |
| Context recall | 1.0000 | 1.0000 | Hòa |
| Context precision | **0.9450** | 0.9417 | V1 cao hơn 0.0033 |

Cả hai prompt đều đạt ngưỡng faithfulness 0.8; báo cáo cũng đánh dấu `target_met: true`. V1 nhỉnh hơn ở 3/4 chỉ số, trong khi khả năng bao phủ thông tin từ context bằng nhau. Một cách giải thích hợp lý là chỉ dẫn ngắn gọn và “không biết khi thiếu dữ liệu” của V1 giúp câu trả lời bám sát context; đây là diễn giải phù hợp với kết quả, không phải quan hệ nhân quả đã được chứng minh.

## Kết luận

Trong lần chạy được ghi lại, V1 là lựa chọn tốt hơn một chút nếu ưu tiên câu trả lời ngắn, liên quan và trung thành với tài liệu. V2 phù hợp hơn khi cần câu trả lời có cấu trúc và giọng phân tích chuyên gia. Chênh lệch điểm tương đối nhỏ; báo cáo không kèm độ biến thiên hay kiểm định thống kê, vì vậy chưa đủ cơ sở kết luận V1 luôn tốt hơn trên dữ liệu hoặc model khác.

Log A/B ghi nhận 50 truy vấn được định tuyến tất định theo `request_id` (19 câu vào V1, 31 câu vào V2). Kết quả định lượng trong bảng lấy từ `03_ragas_report.json`, vốn đánh giá riêng cả hai prompt trên bộ 50 cặp QA.
