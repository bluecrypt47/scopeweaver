# ScopeWeaver (ARTEX) - Repository Overview

Nền tảng tự khởi tạo và vận hành nghiên cứu an ninh mạng / kiểm thử xâm nhập hỗ trợ bởi AI (Autonomous AI penetration-testing system).

---

## 1. Kiến trúc Tổng quan

ScopeWeaver (`github.com/Autumn-27/artex`) được xây dựng theo mô hình **Monolith mô-đun hóa (Modular Monolith)** bằng Go. Dự án tích hợp backend daemon, engine lập kế hoạch và thực thi agent, HTTP MITM Proxy bắt giữ lưu lượng traffic, và giao diện người dùng Web UI (Next.js / shadcn/ui) được nhúng trực tiếp vào binary duy nhất thông qua `go:embed`.

```
                        +---------------------------------------+
                        |  Web UI (Embedded Next.js / shadcn)   |
                        +---------------------------------------+
                                           | HTTP REST / SSE / WS
                                           v
+-----------------------+       +---------------------------------------+
| External Agent        | ----> |         Server & API Layer            |
| Adapters (Claude Code)|       |             (server/)                 |
+-----------------------+       +---------------------------------------+
                                    |                       |
                                    v                       v
                        +-----------------------+   +-------------------+
                        |     Agent Engine      |   |   Database Layer  |
                        |       (agent/)        |   |      (db/)        |
                        +-----------------------+   +-------------------+
                          /                   \
                         v                     v
            +-----------------------+   +-----------------------+
            | LLM Pool & Failover   |   |    Tools & Skills     |
            | (llmpool/ & llmrec/)  |   |   (tools/ & skills/)  |
            +-----------------------+   +-----------------------+

+-----------------------+       +---------------------------------------+
|   MITM Traffic Proxy  | ----> |          Traffic Logging              |
|      (intercept/)     |       |             (traffic/)                |
+-----------------------+       +---------------------------------------+
```

---

## 2. Các Thành phần chính (Components & Entry Points)

- **CLI Entry Point (`cmd/artex/main.go`)**: Quản lý khởi động server, cờ lệnh (`-addr`, `-data`, `-proxy`), cấu hình hệ thống (`config/`), và cơ chế tự động nâng cấp (`selfupdate/`).
- **HTTP Server & API Layer (`server/`)**: Cung cấp các REST API cho tác vụ, tài nguyên, phát hiện (findings), người dùng và cấu hình LLM. Quản lý việc truyền dữ liệu thời gian thực qua SSE và WebSocket.
- **Agent Execution Engine (`agent/`)**:
  - `Planner` (`agent/planner.go`): Lập kế hoạch và lộ trình xử lý tác vụ.
  - `Worker` (`agent/worker.go`): Gọi các công cụ (tools), thực thi lệnh và thu thập kết quả.
  - `Assembly` (`agent/assembly.go`): Tích hợp công cụ, prompts override, skills và MCP.
  - `ChatAgent` & `MainAgent`: Xử lý luồng hội thoại và các lượt chạy tác vụ chính.
- **Traffic Interception Proxy (`intercept/`)**: Proxy MITM dựa trên `go-mitmproxy`, hỗ trợ bắt giữ traffic HTTP/HTTPS, áp dụng bộ lọc DSL và ghi nhận dữ liệu lưu lượng.
- **External Adapters (`adapters/agent/`)**: Adapter kết nối giao tiếp giữa các AI CLI bên ngoài (Claude Code, Codex, Pi) và ScopeWeaver API.
- **LLM Provider Management (`llmpool/`, `llmrec/`)**: Quản lý danh sách nhà cung cấp LLM, chuỗi dự phòng (failover chain), và ghi nhận chi phí/lượt sử dụng token.

---

## 3. Quản lý Dữ liệu & Lưu trữ (Data Stores)

1. **PostgreSQL (`db/schema.sql`)**: Lưu trữ dữ liệu quan hệ chính của ứng dụng:
   - Tác vụ (`tasks`), Loại tác vụ (`task_categories`), Bản mẫu (`task_templates`)
   - Công ty & Phạm vi tài sản (`companies`, `assets`, `company_scope`)
   - Cấu hình Agent & Prompts (`agents`, `agent_prompts`)
   - Phát hiện an ninh & Bằng chứng (`findings`, `finding_assets`)
   - Nhật ký sử dụng LLM (`llm_usage_logs`, `llm_profiles`)
2. **SQLite (`modernc.org/sqlite`)**: Dùng cho dữ liệu tần suất cao hoặc lưu trữ phân tán ngắn hạn:
   - Lịch sử hội thoại & transcript
   - Lưu lượng traffic bắt được
   - Minh chứng bằng chứng (evidence files)

---

## 4. Luồng Xử lý Chính (Key Workflows)

### Luồng Thực thi Tác vụ (Task Execution Workflow)
1. Yêu cầu tạo tác vụ được gửi đến REST API `POST /api/tasks` (`server/server.go`).
2. Tác vụ được kích hoạt qua `Engine.StartTask` (`server/engine.go`).
3. Engine gán bộ đôi `Planner` và `Worker` phù hợp cho tác vụ.
4. `Planner` sinh lộ trình công việc; `Worker` thực thi chuỗi công cụ trong `tools/`.
5. Kết quả quan sát và lỗ hổng được ghi nhận tự động vào database qua `finding_recorder.go`.
6. Trạng thái và tiến độ được broadcast liên tục về giao diện Web UI.

### Luồng Ghi nhận Traffic (Traffic Interception Workflow)
1. Lưu lượng HTTP/HTTPS đi qua MITM proxy (`intercept/`).
2. Proxy kiểm tra quy tắc DSL (`db/asset_dsl.go`).
3. Dữ liệu request/response được ghi vào file SQLite (`traffic/`).
4. Các domain/endpoint thuộc phạm vi được cấp phép sẽ tự động được thêm vào bảng tài sản (`db/assets.go`).

---

## 5. Các Thư viện Phụ thuộc Chính

- **CSDL**: `github.com/jackc/pgx/v5` (PostgreSQL), `modernc.org/sqlite` (SQLite).
- **Proxy**: `github.com/lqqyt2423/go-mitmproxy`.
- **LLM/Tools Framework**: `github.com/Autumn-27/norma`.
- **Auth**: `github.com/golang-jwt/jwt/v5`.
- **DNS**: `github.com/miekg/dns`, `github.com/projectdiscovery/dnsx`.
