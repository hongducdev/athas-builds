# Athas Builds

Repository này chứa các GitHub Actions workflows để build và release ứng dụng Athas từ repository gốc https://github.com/athasdev/athas.

## Workflows

### 1. Build and Test (`build.yml`)

Workflow này chạy tự động khi:

-   Push code lên branch `master`
-   Tạo Pull Request vào branch `master`
-   Chạy thủ công thông qua `workflow_dispatch`

**Chức năng:**

-   Setup Bun và Rust
-   Cache dependencies
-   Install system dependencies
-   Install project dependencies
-   Build frontend và Tauri app

### 2. Release (`release.yml`)

Workflow này chạy khi:

-   Push tag với format `v*` (ví dụ: `v1.0.0`)
-   Chạy thủ công thông qua `workflow_dispatch`

**Chức năng:**

-   Build cho nhiều platform: macOS (ARM64, x86_64), Ubuntu, Windows
-   Tạo release với assets cho từng platform
-   Sử dụng Tauri Action để build và publish

## Cách sử dụng

### Để build và test:

1. Push code lên branch `master`
2. Workflow sẽ tự động chạy
3. Kiểm tra kết quả trong tab Actions của GitHub

### Để tạo release:

1. Tạo tag mới: `git tag v1.0.0`
2. Push tag: `git push origin v1.0.0`
3. Workflow release sẽ tự động chạy
4. Tạo release với assets cho tất cả platforms

### Chạy thủ công:

1. Vào tab Actions trên GitHub
2. Chọn workflow muốn chạy
3. Click "Run workflow"
4. Chọn branch và click "Run workflow"

## Yêu cầu

-   Repository gốc https://github.com/athasdev/athas phải có file `bun.lock`
-   Repository gốc phải có cấu hình Tauri
-   Cần có quyền write contents để tạo release
-   Cần có quyền truy cập vào repository gốc (GITHUB_TOKEN)

## Lưu ý

-   Workflow release chỉ chạy khi có tag mới
-   Các dependencies được cache để tăng tốc độ build
-   Ubuntu cần cài đặt thêm system dependencies cho Tauri
-   macOS build cho cả ARM64 và x86_64 architectures
