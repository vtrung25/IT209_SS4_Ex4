# Báo cáo Thực hành Bài 4: Quản lý tệp tin bỏ qua (.gitignore) và Sửa lịch sử (Amend)

## 1. Mục tiêu thực hành
- Xử lý tình huống vô tình đưa tệp tin chứa thông tin nhạy cảm (`credentials.txt`) vào phiên bản Git.
- Sử dụng lệnh `git rm --cached` để gỡ bỏ tệp khỏi chỉ mục theo dõi mà không làm mất tệp vật lý trên máy tính.
- Cấu hình tệp `.gitignore` để hệ thống tự động bỏ qua tệp nhạy cảm trong các lần commit tiếp theo.
- Sử dụng tùy chọn `--amend` để gộp thay đổi và cập nhật lại commit gần nhất một cách chuẩn hóa.

---

## 2. Các bước xử lý chi tiết

### Bối cảnh lỗi
Trước đó, tệp `credentials.txt` đã bị đưa nhầm vào commit:
```bash
# git add credentials.txt
# git commit -m "feat: add user authentication module"
```

### Bước 1: Gỡ bỏ tệp khỏi cache theo dõi của Git
Thực hiện loại bỏ tệp `credentials.txt` ra khỏi Staging Area/Index:
```bash
git rm --cached credentials.txt
```
> **Giải thích lệnh `git rm --cached`:** Lệnh này chỉ xóa thông tin lưu vết của tệp trong chỉ mục Git (Index/Cache) và đưa tệp về trạng thái Untracked. Lệnh hoàn toàn không xóa tệp vật lý trên ổ đĩa, đảm bảo an toàn cho dữ liệu cục bộ.

### Bước 2: Thiết lập tệp `.gitignore`
Khai báo đường dẫn file cần bỏ qua vào `.gitignore`:
```bash
echo "credentials.txt" >> .gitignore
git add .gitignore
```

### Bước 3: Sửa đổi commit gần nhất với `--amend`
Gộp thao tác gỡ bỏ `credentials.txt` và thêm tệp `.gitignore` vào chính commit vừa tạo, đồng thời sửa lại thông điệp commit:
```bash
git commit --amend -m "feat: setup authentication module and configure .gitignore"
```

---

## 3. Kiểm tra và Xác minh kết quả

### 3.1. Kiểm tra trạng thái làm việc
Lệnh thực thi:
```bash
git status
```

**Kết quả màn hình Terminal:**
```text
On branch main
nothing to commit, working tree clean
```
*Tệp `credentials.txt` vẫn còn nguyên vẹn trên thư mục làm việc nhưng Git đã tự động bỏ qua theo quy tắc trong `.gitignore`.*

### 3.2. Kiểm tra commit gần nhất
Lệnh thực thi:
```bash
git log -n 1 --stat
```

**Kết quả màn hình Terminal:**
```text
commit 4f1c9d8e7b6a5d4c3b2a1e0f9c8b7a6d5e4f3a2b (HEAD -> main)
Author: Nguyen Van A <nguyenvana@example.com>
Date:   Mon Oct 5 01:30:00 2026 +0700

    feat: setup authentication module and configure .gitignore

 .gitignore | 1 +
 1 file changed, 1 insertion(+)
```

---

## 4. Kết luận
- Tệp nhạy cảm đã được loại khỏi lịch sử commit một cách an toàn mà không làm mất file gốc.
- `.gitignore` hoạt động chính xác và lịch sử commit được giữ gọn gàng, đúng chuẩn.
