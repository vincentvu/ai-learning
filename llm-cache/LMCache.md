LMCache – Biến KV Cache thành một lớp hạ tầng riêng cho LLM Inference
Khi chạy các mô hình AI lớn, một trong những tài nguyên quan trọng nhất là KV Cache. Đây là dữ liệu được tạo ra trong quá trình xử lý prompt và thường được lưu tạm trong VRAM để tăng tốc inference.
Tuy nhiên, trong hầu hết hệ thống hiện nay, KV Cache chỉ tồn tại trong thời gian ngắn. Khi request kết thúc hoặc server gặp sự cố, toàn bộ cache sẽ biến mất và phải tính toán lại từ đầu.
🔧 LMCache được xây dựng để giải quyết vấn đề này.
Thay vì xem KV Cache là dữ liệu tạm thời, LMCache biến nó thành một lớp quản lý độc lập có thể lưu trữ, tái sử dụng, theo dõi và chia sẻ giữa nhiều inference engines khác nhau.
Điều này giúp giảm đáng kể TTFT (Time-To-First-Token), tăng throughput và đặc biệt hiệu quả với các workload có context dài như:
• Multi-turn Conversation
• RAG Systems
• AI Agents
• Long-context Reasoning
• Knowledge-intensive Applications
📦 Một trong những điểm đáng chú ý nhất là LMCache hoạt động độc lập với inference engine.
LMCache chạy dưới dạng một daemon riêng, tách biệt hoàn toàn khỏi engine phục vụ model. Nếu engine bị crash hoặc restart, KV Cache vẫn được giữ nguyên và có thể tiếp tục sử dụng.
Cách tiếp cận này giúp giảm lượng prefill computation phải thực hiện lại nhiều lần.
🚀 LMCache hỗ trợ cơ chế tiered KV Cache storage.
KV Cache có thể được chuyển từ GPU sang nhiều tầng lưu trữ khác nhau:
• CPU RAM
• SSD Local Storage
• Redis / Valkey
• S3-compatible Storage
• Mooncake
• InfiniStore
• NIXL
• GDS
Nhờ đó hệ thống có thể lưu lượng cache lớn hơn rất nhiều so với giới hạn VRAM của GPU.
📊 Ngoài việc lưu trữ cache, LMCache còn cung cấp hệ thống observability dành riêng cho KV Cache.
Developer có thể theo dõi:
• Prefix Cache Hit Rate
• Token-level Cache Metrics
• Request-level Performance
• User Usage Statistics
• Health Monitoring
• System Diagnostics
Điều này giúp việc vận hành các cụm inference quy mô lớn trở nên dễ kiểm soát hơn.
🧠 Một tính năng đáng chú ý khác là Non-prefix KV Reuse.
Thông thường cache chỉ được tái sử dụng khi phần đầu của prompt giống nhau.
LMCache mở rộng khả năng này bằng cách cho phép tái sử dụng KV Cache ở nhiều vị trí khác nhau trong prompt thông qua kỹ thuật CacheBlend, giúp giảm thêm lượng tính toán phải thực hiện lại.
🔄 Framework này cũng hỗ trợ PD Disaggregation và KV Transfer giữa các worker thông qua:
• NVLink
• RDMA
• TCP
Điều này đặc biệt hữu ích cho các hệ thống inference phân tán và các AI serving cluster quy mô lớn.
🌐 LMCache được thiết kế theo hướng vendor-neutral.
Framework có thể tích hợp với nhiều serving engines, hardware vendors, storage systems và cloud infrastructure khác nhau mà không bị phụ thuộc vào một hệ sinh thái duy nhất.
📌 Thông điệp chính của LMCache khá đơn giản:
Thay vì để KV Cache chỉ tồn tại trong vài giây rồi bị xóa, hãy biến nó thành một lớp dữ liệu có thể lưu trữ, chia sẻ và tái sử dụng trong toàn bộ hệ thống AI.
Khi context ngày càng dài và AI Agents ngày càng phổ biến, KV Cache đang dần trở thành một phần quan trọng của hạ tầng LLM Inference thay vì chỉ là dữ liệu tạm thời trong GPU memory.
