# ScopeWeaver - Hướng dẫn Sử dụng Chi tiết (User Guide)

Tài liệu hướng dẫn sử dụng từng bước toàn bộ các tính năng trên nền tảng **ScopeWeaver** (`http://localhost:8787`).

---

## 📖 MỤC LỤC

1. [Khởi động Hệ thống bằng Docker](#1-khởi-động-hệ-thống-bằng-docker)
2. [Cấu hình LLM Profile (Gemini / OpenAI / Anthropic)](#2-cấu-hình-llm-profile)
3. [Tạo Mục tiêu & Phạm vi An toàn (Assets & Scope)](#3-tạo-mục-tiêu--phạm-vi-an-toàn)
4. [Tạo và Quản lý Tác vụ Kiểm thử (Tasks)](#4-tạo-và-quản-lý-tác-vụ-kiểm-thử)
5. [Theo dõi & Điều khiển Agent (Interactive Workspace)](#5-theo-dõi--điều-khiển-agent)
6. [Quản lý Lỗ hổng & Tái kiểm thử (Findings & Retest)](#6-quản-lý-lỗ-hổng--tái-kiểm-thử)
7. [Soi Traffic qua MITM Proxy (Traffic Inspector)](#7-soi-traffic-qua-mitm-proxy)
8. [Xuất Báo cáo (Reporting)](#8-xuất-báo-cáo)

---

## 1. Khởi động Hệ thống bằng Docker

### Bước 1: Chuẩn bị file `.env`
Sao chép file cấu hình mẫu `.env.example` thành `.env` tại thư mục gốc:

```bash
cp .env.example .env
```

Nội dung cấu hình mẫu hỗ trợ **Google Gemini API**:

```env
SCOPEWEAVER_LANGUAGE=en
POSTGRES_USER=artex
POSTGRES_PASSWORD=change-me-please
POSTGRES_DB=artex

# Cấu hình Gemini API (OpenAI-compatible format)
ARTEX_LLM_PROVIDER=openai
ARTEX_LLM_BASE_URL=https://generativelanguage.googleapis.com/v1beta/openai
ARTEX_LLM_MODEL=gemini-2.5-flash
OPENAI_API_KEY=your_gemini_api_key_here
```

### Bước 2: Chạy Docker Compose
Khởi chạy container PostgreSQL và ScopeWeaver backend/frontend:

```bash
docker compose up -d --build
```

### Bước 3: Truy cập Giao diện
Mở trình duyệt web và truy cập địa chỉ: **[http://localhost:8787](http://localhost:8787)**.

---

## 2. Cấu hình LLM Profile

Bạn có thể thêm và quản lý nhiều nhà cung cấp AI trực tiếp trên Web UI mà không cần khởi động lại ứng dụng.

### Các bước cấu hình trên UI:
1. Nhấp vào mục **`LLM`** (biểu tượng 🧠 `Brain`) ở menu bên trái.
2. Nhấp vào nút **`+ Add Profile`** ở góc trên bên phải.
3. Điền các thông số:
   - **Name**: `Gemini 2.5 Flash`
   - **Format**: Chọn **`OpenAI (Chat Completions)`**
   - **Model**: `gemini-2.5-flash`
   - **Base URL**: `https://generativelanguage.googleapis.com/v1beta/openai`
   - **API Key**: Điền Gemini API Key của bạn.
4. Bấm **`Test Connection`** để kiểm tra kết nối.
5. Bấm **`Save`** để lưu lại.

---

## 3. Tạo Mục tiêu & Phạm vi An toàn

Để đảm bảo AI chỉ kiểm thử đúng các mục tiêu cho phép:

1. Vào menu **`Assets`** trên thanh bên trái.
2. Chọn hoặc thêm mới một **Company** (ví dụ: `Internal Web Lab`).
3. Nhấp **`Add Asset`** để thêm phạm vi:
   - **Type**: Chọn `URL`, `Root Domain`, `IP`, hoặc `CIDR`.
   - **Value**: Nhập địa chỉ target, ví dụ: `http://host.docker.internal:9999` (dùng `host.docker.internal` khi target chạy trên máy host/Docker).
4. Thiết lập trạng thái `In Scope` để cấp phép kiểm thử.

---

## 4. Tạo và Quản lý Tác vụ Kiểm thử

1. Vào menu **`Tasks`** -> chọn **`Create Task`**.
2. Điền thông tin Task:
   - **Title**: Tên ngắn gọn của tác vụ (ví dụ: `Kiểm thử Web Lab 9999`).
   - **Goal / Instructions**: Mô tả mục tiêu rà soát cho AI (ví dụ: *"Rà soát các ô nhập liệu, danh mục tin tức và chức năng đăng nhập để tìm lỗ hổng an ninh."*).
   - **Target Asset Scope**: Chọn Asset mục tiêu đã tạo ở Bước 3.
3. Nhấn **`Create & Start`** để kích hoạt Agent.

---

## 5. Theo dõi & Điều khiển Agent

Khi truy cập vào chi tiết một Task (`Tasks` -> chọn Task ID), bạn có **10 Tabs Workspace** để theo dõi và chỉ đạo:

- 💬 **Tab `Session`**: Trò chuyện thời gian thực với Agent. Bạn có thể gửi hướng dẫn thêm (vd: `"Use the LFI vulnerability to read the source code and identify findings."`).
- 📊 **Tab `Overview`**: Xem thống kê thời gian chạy, số lượt gọi công cụ và token usage.
- 🕸️ **Tab `Exploration paths`**: Xem đồ thị hình cây các bước suy luận (`Goal` ➔ `Intent` ➔ `Fact` ➔ `Finding`).
- 📜 **Tab `Broadcast board`**: Xem dòng suy luận chi tiết của Planner và Worker theo thời gian thực.

---

## 6. Quản lý Lỗ hổng & Tái kiểm thử

1. Vào menu **`Findings`** ở thanh menu bên trái.
2. Bạn sẽ thấy danh sách các lỗ hổng tìm được (ví dụ: LFI, SQLi, XSS).
3. **Xem Chi tiết**: Nhấp vào lỗ hổng để xem mô tả, PoC HTTP Request/Response thu thập được.
4. **Đào sâu (`Deepen`)**: Nhấp nút **`Deepen`** nếu muốn chỉ định AI nâng cấp chuỗi tấn công (ví dụ: leo thang từ LFI sang RCE).
5. **Tái kiểm thử (`Retest`)**: Nhấp nút **`Retest`** sau khi lập trình viên đã sửa lỗi để AI xác nhận lại.

---

## 7. Soi Traffic qua MITM Proxy

1. ScopeWeaver tích hợp sẵn Proxy lắng nghe tại cổng `127.0.0.1:8788`.
2. Vào menu **`Traffic`** trên sidebar để xem danh sách toàn bộ lưu lượng HTTP/HTTPS thu thập được.
3. Nhấp vào bất kỳ lượt request/response nào để xem đầy đủ Header & Body.
4. Nhấp nút **`Link to Finding`** nếu muốn đính kèm lưu lượng này làm bằng chứng cho một lỗ hổng cụ thể.

---

## 8. Xuất Báo cáo

1. Vào chi tiết Task -> chọn Tab **`Report`**.
2. Nhấn **`Generate Report`** để AI tổng hợp toàn bộ kết quả kiểm thử.
3. Báo cáo được định dạng bằng Markdown chuẩn bao gồm: Tổng quan phạm vi, Danh sách lỗ hổng xác nhận, Bằng chứng PoC và Khuyến nghị khắc phục.
4. Nhấn **`Export / Download`** để lưu file báo cáo.
