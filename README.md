# Athas Builds

Repository này chứa các GitHub Actions workflows để build và release ứng dụng Athas từ repository gốc https://github.com/athasdev/athas.

## Workflows

### 1. Build (`build.yml`)

Workflow này chạy khi:

-   Được gọi từ workflow khác thông qua `workflow_call`
-   Chạy thủ công thông qua `workflow_dispatch`

**Chức năng:**

-   Build ứng dụng Tauri cho 3 platforms: **Windows**, **Ubuntu**, **macOS**
-   Setup Bun và Rust cho mỗi platform
-   Cache dependencies (Rust và Node modules)
-   Install system dependencies cần thiết cho từng OS
-   Disable Tauri updater để tạo build production
-   Build và upload artifacts cho từng platform

**Platforms được hỗ trợ:**

-   **Windows**: Tạo `.exe`, `.msi` installers và binary `athas-code.exe`
-   **Ubuntu**: Tạo `.AppImage`, `.deb` packages và binary `athas-code-linux`
-   **macOS**: Tạo `.dmg`, `.app` bundles và binary `athas-code-macos`

### 2. Release (`release.yml`)

Workflow này chạy khi:

-   Được gọi từ workflow khác thông qua `workflow_call`
-   Chạy thủ công thông qua `workflow_dispatch`

**Chức năng:**

-   Download tất cả build artifacts từ 3 platforms
-   Tạo GitHub Release với tag và description được chỉ định
-   Upload tất cả artifacts vào release để người dùng download

## Cách hoạt động

### Workflow Build:

1. **Clone repository Athas** từ `athasdev/athas`
2. **Setup environment** cho từng platform:
    - Windows: PowerShell, Rust target x86_64-pc-windows-msvc
    - Ubuntu: System dependencies (GTK, WebKit2GTK, etc.)
    - macOS: Không cần dependencies đặc biệt
3. **Build ứng dụng** sử dụng `bun run tauri build`
4. **Upload artifacts** với tên riêng cho từng platform

### Workflow Release:

1. **Download artifacts** từ tất cả platforms
2. **Tạo GitHub Release** với tag và description
3. **Upload tất cả files** vào release

## Cách sử dụng

### Để build ứng dụng:

1. **Chạy thủ công**:

    - Vào tab Actions trên GitHub
    - Chọn workflow "Build"
    - Click "Run workflow"
    - Nhập repository và ref (branch/tag) muốn build
    - Click "Run workflow"

2. **Gọi từ workflow khác**:
    ```yaml
    - uses: ./.github/workflows/build.yml
      with:
          repository: athasdev/athas
          ref: main
    ```

### Để tạo release:

1. **Chạy thủ công**:

    - Vào tab Actions trên GitHub
    - Chọn workflow "Release"
    - Click "Run workflow"
    - Nhập thông tin release (tag, description, pre-release)
    - Click "Run workflow"

2. **Gọi từ workflow khác**:
    ```yaml
    - uses: ./.github/workflows/release.yml
      with:
          release_tag: "v1.0.0"
          release_body: "Release version 1.0.0"
          is_prerelease: false
    ```

## Yêu cầu

-   Repository gốc https://github.com/athasdev/athas phải có:
    -   File `bun.lock` cho frontend dependencies
    -   Cấu hình Tauri trong `src-tauri/tauri.conf.json`
-   Repository này cần quyền `contents: write` để tạo release
-   Cần có quyền truy cập vào repository gốc

## Lưu ý kỹ thuật

-   **Dependencies được cache** để tăng tốc độ build
-   **Ubuntu cần cài đặt** system dependencies cho Tauri (GTK, WebKit2GTK)
-   **Tauri updater bị disable** để tạo build production
-   **Artifacts được tổ chức** theo platform để dễ quản lý
-   **Binary files có tên khác nhau** cho mỗi platform để tránh xung đột

## Ví dụ kết quả

Sau khi chạy workflow, bạn sẽ có:

-   **Windows**: `athas-code.exe`, `.msi`, `.exe` installers
-   **Ubuntu**: `athas-code-linux`, `.AppImage`, `.deb` packages
-   **macOS**: `athas-code-macos`, `.dmg`, `.app` bundles

Tất cả được upload vào GitHub Release để người dùng download version phù hợp với OS của họ.
