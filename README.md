# Báo cáo Bài 1 (Session 5): Khôi phục commit đã mất bằng Git Reflog

## 1. Tình huống lỗi
Tôi đã tạo một commit quan trọng có nội dung "Them tinh nang quan trong", nhưng sau đó vô tình chạy lệnh:
```bash
git reset --hard HEAD~1
```
Lệnh này đã lùi con trỏ HEAD về 1 bước và xóa sạch thư mục làm việc. Lúc này, khi chạy `git log --oneline`, commit "Them tinh nang quan trong" đã hoàn toàn biến mất khỏi lịch sử theo dõi thông thường.

## 2. Các bước tra cứu và khôi phục
Do commit đã không còn trong `git log`, tôi cần dùng đến nhật ký tham chiếu của Git (Reflog) – nơi lưu lại mọi sự dịch chuyển của con trỏ HEAD.

### Bước 2.1: Chạy lệnh `git reflog`
```bash
git reflog
```
**Kết quả thu được:**
```text
af4015b HEAD@{0}: reset: moving to HEAD~1
809a45e HEAD@{1}: commit: Them tinh nang quan trong
af4015b HEAD@{2}: commit (initial): Initial setup
```
Từ kết quả trên, tôi xác định được mã hash của commit vừa bị mất là `809a45e`.

### Bước 2.2: Khôi phục bằng lệnh reset
Tôi sử dụng lệnh reset --hard trỏ tới đúng mã hash vừa tìm được để đưa lịch sử và thư mục làm việc quay lại trạng thái đó:
```bash
git reset --hard 809a45e
```

## 3. Kết quả kiểm tra
Sau khi chạy lệnh khôi phục, thông báo trả về:
```text
HEAD is now at 809a45e Them tinh nang quan trong
```
Tôi chạy lại `git log --oneline` để kiểm chứng và thấy commit đã quay trở lại:
```text
809a45e Them tinh nang quan trong
af4015b Initial setup
```
File `feature.txt` cùng mã nguồn bên trong cũng đã xuất hiện lại hoàn toàn bình thường trong thư mục làm việc.
