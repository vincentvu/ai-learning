🧠 codebase-memory-mcp: Biến toàn bộ codebase thành Knowledge Graph để AI hiểu dự án nhanh hơn
codebase-memory-mcp là một MCP Server dành cho AI Coding Agent. Công cụ này không phải là một LLM, mà đóng vai trò như một backend phân tích code.
Thay vì để AI phải đọc từng file, tìm kiếm bằng grep hoặc khám phá dự án từng bước, codebase-memory-mcp sẽ quét toàn bộ codebase và xây dựng thành một Knowledge Graph chứa:
• Function
• Class
• Call Graph
• HTTP Route
• Service Dependency
• Quan hệ giữa các thành phần trong hệ thống
Sau khi index hoàn tất, AI Agent như Claude Code, Codex, Gemini CLI hoặc OpenCode có thể truy vấn trực tiếp vào đồ thị này để hiểu cấu trúc dự án.
⚡ Hiệu suất
• Index Linux Kernel với khoảng 28 triệu dòng code và 75.000 file trong khoảng 3 phút.
• Thời gian truy vấn dưới 1ms.
• Giảm tới khoảng 99% token so với cách đọc file thủ công.
• Pipeline được tối ưu RAM bằng SQLite In-Memory và LZ4 Compression.
• Tự động giải phóng bộ nhớ sau khi hoàn thành quá trình index.
🏗️ Kiến trúc
Hệ thống sử dụng hai lớp phân tích:
1️⃣ Tree-sitter
• Phân tích AST.
• Hỗ trợ 158 ngôn ngữ lập trình.
• Hoạt động trên toàn bộ codebase.
2️⃣ Hybrid LSP
• Được phát triển bằng C.
• Không cần chạy Language Server riêng.
• Hỗ trợ khả năng tương tự Go to Definition trong IDE.
• Phân tích kiểu dữ liệu và quan hệ ngữ nghĩa sâu hơn.
🔍 Các tính năng nổi bật
📊 Code Graph Analysis
• Tổng quan kiến trúc hệ thống.
• Phát hiện Functional Cluster bằng Louvain Algorithm.
• Phân tích phạm vi ảnh hưởng của Git Diff.
• Phát hiện Dead Code.
• Truy vấn bằng Cypher Query.
🔎 Search Engine
• Semantic Search bằng Embedding tích hợp sẵn.
• Không cần API Key.
• Full-text Search với BM25 và FTS5.
• Structural Search theo Label hoặc Regex.
• Graph-aware Grep.
🔗 Cross-Service Analysis
• Tự động liên kết HTTP Route với nơi gọi API.
• Phát hiện gRPC, GraphQL và tRPC.
• Theo dõi Pub/Sub Channel như Socket.IO hoặc EventEmitter.
🌌 Multi-Repository Support
• Kết nối nhiều repository trong cùng một Knowledge Graph.
• Hỗ trợ Cross-Repo Dependency Analysis.
• Có giao diện 3D Visualization cho hệ thống nhiều repository.
☁️ Infrastructure Analysis
Ngoài source code, công cụ còn hiểu:
• Dockerfile
• Kubernetes Manifest
• Kustomize Overlay
Tất cả đều được biểu diễn dưới dạng node trong Knowledge Graph.
🛠️ MCP Tools
Hệ thống cung cấp 14 MCP Tool bao gồm:
• Index Codebase
• Search
• Trace Call Path
• ADR Management
• Runtime Trace Ingestion
• Và nhiều công cụ phân tích khác
Ngoài ra, team có thể chia sẻ Graph Artifact đã index sẵn để các thành viên khác sử dụng mà không cần index lại từ đầu.
💻 Hỗ trợ nền tảng
• Hỗ trợ tới 158 ngôn ngữ (qua tree-sitter), được đánh giá hiệu quả ở 3 mức: Xuất sắc, Tốt, Khá (một số như OCaml, Haskell ở mức thấp hơn).
• Phát hành dưới dạng một file binary tĩnh duy nhất, không cần Docker, không cần API key, chạy được trên macOS, Linux, Windows.
• Tự động phát hiện và cấu hình cho 11 coding agent khác nhau (Claude Code, Codex CLI, Gemini CLI, Zed, OpenCode, Aider, VS Code...).
• Có sẵn trên nhiều kênh phân phối: npm, PyPI, Homebrew, Scoop, Winget, AUR...
🔒 Bảo mật
Toàn bộ quá trình xử lý diễn ra trên máy người dùng.
• Không gửi source code ra bên ngoài.
• Mỗi bản phát hành đều được quét bởi hơn 70 Antivirus Engine.
• Ký số bằng Sigstore và Cosign.
• Hỗ trợ SLSA Level 3 Provenance.
• Công bố SHA-256 Checksum.
• Kiểm tra bảo mật bằng CodeQL.
Đặc biệt, dự án không có Runtime Dependency. Toàn bộ thư viện được nhúng sẵn trong quá trình build.
Dự án cũng có Research Paper đánh giá trên 31 repository thực tế.
Kết quả cho thấy:
• Độ chính xác trả lời khoảng 83%
• Giảm khoảng 10 lần số token sử dụng
• Giảm khoảng 2,1 lần số lượt gọi tool
codebase-memory-mcp là một hướng tiếp cận đáng chú ý cho AI-assisted Software Engineering, khi thay thế việc khám phá codebase theo từng file bằng một Knowledge Graph có thể truy vấn trực tiếp.
