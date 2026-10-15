# ScopeWeaver Web UI Navigation Map & Feature Guide

Tài liệu bản đồ tổng quan về cấu trúc, tính năng và hướng dẫn sử dụng giao diện người dùng Web UI trên nền tảng **ScopeWeaver** (`http://localhost:8787`).

---

## 🗺️ 1. Bản đồ Cấu trúc Giao diện (Navigation Map)

```
ScopeWeaver Console (http://localhost:8787)
├── 📊 Dashboard (/dashboard)
├── 💬 Global Chat (/chat)
├── 🎯 Functions (Quản lý Kiểm thử)
│   ├── Tasks (/function/tasks)
│   │   └── Task Detail (/function/tasks/detail?id=...) [10 Workspace Tabs]
│   ├── Findings (/function/findings) [3 View Modes]
│   ├── Traffic (/function/traffic)
│   ├── Commands (/function/commands)
│   ├── LLM Records (/function/llm-records)
│   ├── Assets / Scope (/function/assets)
│   ├── Sync (/function/sync)
│   └── Workspace (/function/workspace)
└── ⚙️ System Settings (Cấu hình Nền tảng)
    ├── LLM Profiles (/system/llm)
    ├── Agents Config (/system/agents)
    ├── MCP Servers (/system/mcp)
    ├── Skills (/system/skills)
    ├── Tools (/system/tools)
    ├── Notifications (/system/notify)
    ├── Intercept Rules (/system/intercept)
    ├── Logs (/system/logs)
    └── Settings (/system/settings)
```

---

## 2. Chi tiết Nhóm Tính năng Kiểm thử (Functions)

### 🎯 Tasks & Task Detail (`/function/tasks`)
- **Trang Danh sách Tasks**:
  - Xem danh dạng Bảng (Table) hoặc Kanban.
  - Quản lý theo Nhóm (`Categories`), Mẫu (`Templates`), và Thao tác hàng loạt (Start / Pause / Archive / Delete).
- **Trang Chi tiết Task (10 Tabs Workspace)**:
  1. **Session**: Khung chat tương tác trực tiếp với `MainAgent` (gửi hint/chỉ đạo cho AI).
  2. **Overview**: Tổng quan tiến độ, mục tiêu, thời gian chạy, token usage của task.
  3. **Exploration paths**: Sơ đồ mạng cây (Graph) biểu diễn các bước AI thực thi (`begin` ➔ `goal` ➔ `intent` ➔ `fact` ➔ `finding`).
  4. **Broadcast board**: Nhật ký suy luận thời gian thực của `planner` và `worker`.
  5. **Findings**: Danh sách các lỗ hổng phát hiện riêng cho Task này.
  6. **Retests**: Quản lý các lượt tái kiểm thử lỗ hổng.
  7. **Assets**: Danh sách tài sản/mục tiêu đính kèm Task.
  8. **Coverage**: Mức độ phủ quét kiểm thử.
  9. **Intercept**: Quy tắc can thiệp Proxy dành riêng cho Task.
  10. **Report**: Báo cáo tổng hợp kết quả kiểm thử định dạng Markdown.

---

### 🐛 Findings Hub (`/function/findings`)
Quản lý tập trung toàn bộ lỗ hổng hệ thống phát hiện được với **3 chế độ xem**:
1. **Flat Table**: Bảng phẳng danh sách lỗ hổng.
2. **Grouped**: Nhóm lỗ hổng theo từng Task.
3. **Asset Tree**: Cây tài sản bên trái, danh sách lỗ hổng tương ứng ở bên phải.
- **Tính năng trọng tâm**:
  - **`Deepen`**: Yêu cầu AI đào sâu thêm về một lỗ hổng cụ thể.
  - **`Retest`**: Tái kiểm thử xác minh lỗ hổng đã được khắc phục.
  - **Phân loại**: Trạng thái (`Pending`, `In Progress`, `Confirmed`, `Resolved`, `False Positive`, `Ignored`).

---

### 📡 Traffic Inspector (`/function/traffic`)
- Giao diện soi lưu lượng HTTP/HTTPS đi qua MITM Proxy (`go-mitmproxy` / cổng `8788`).
- Hỗ trợ xem chi tiết Request/Response Header & Body, lọc theo Host/Status/Method.
- Nút **`Link to Finding`**: Đính kèm lưu lượng HTTP cụ thể làm PoC/Bằng chứng cho lỗ hổng.

---

### 📜 Commands Audit & LLM Records
- **Commands (`/function/commands`)**: Kiểm toán toàn bộ các lệnh Bash / System command do AI Worker thực thi.
- **LLM Records (`/function/llm-records`)**: Nhật ký chi tiết mọi lượt gọi API Gemini/OpenAI (Prompt thô, response, token, độ trễ).

---

### 🌐 Assets & Scope (`/function/assets`)
- Quản lý cây phạm vi công ty/mục tiêu (Company ➔ Root Domain / IP / CIDR / URL).
- Kiểm soát ranh giới an toàn (In-Scope vs Out-of-Scope).

---

## 3. Chi tiết Nhóm Cấu hình Hệ thống (System)

- **LLM Profiles (`/system/llm`)**: Thêm/Sửa API Keys (Gemini, OpenAI, Anthropic), cấu hình chuỗi dự phòng (Failover chain) và Rate Limit.
- **Agents Config (`/system/agents`)**: Tùy chỉnh System Prompt cho các Agent (`Planner`, `Worker`, `MainAgent`, `Report-Writer`).
- **MCP Servers (`/system/mcp`)**: Kết nối các MCP server mở rộng (vd Playwright Browser MCP).
- **Skills (`/system/skills`) & Tools (`/system/tools`)**: Quản lý script tùy chỉnh và cấp quyền gọi công cụ cho AI.
- **Notifications (`/system/notify`)**: Gửi thông báo lỗ hổng qua Webhook, Telegram, Email.
- **Logs (`/system/logs`)**: Stream log hệ thống backend thời gian thực.
- **Settings (`/system/settings`)**: Cài đặt ngôn ngữ giao diện (English / Korean / Chinese) và cập nhật 1-click.
