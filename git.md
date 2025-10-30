# FPT Day 3 - 30/10/2025  
## <center>Git</center>

---

### I. Tổng quan
- **Git** là hệ thống quản lý phiên bản phân tán (*Distributed Version Control System – DVCS*), được **Linus Torvalds** (người tạo ra **Linux**) phát triển vào năm **2005**.  
  → Nó giúp lập trình viên theo dõi và quản lý các thay đổi trong mã nguồn của dự án theo thời gian.  
- Git cho phép nhiều người cùng làm việc trên cùng một dự án mà không ghi đè công việc của nhau.  
  → Mỗi lập trình viên có bản sao của toàn bộ dự án trên máy cá nhân, bao gồm toàn bộ lịch sử thay đổi.

---

### II. Các khái niệm quan trọng trong Git
- **Repository (repo):** một folder mà Git theo dõi dự án và lịch sử thay đổi.  
- **Remote repo:** Kho lưu trữ trên máy chủ (như GitHub, GitLab, Bitbucket…).  
- **Clone:** tạo một bản sao của một remote repo trên máy cá nhân.  
- **Stage:** Thông báo cho Git biết những thay đổi nào bạn muốn lưu lại trong lần commit tiếp theo.  
- **Commit:** Lưu lại một snapshot của những thay đổi đã được đưa vào stage.  
- **Branch:** Làm việc trên các phiên bản hoặc tính năng khác nhau của dự án cùng một lúc mà không ảnh hưởng đến nhánh chính.  
- **Merge:** Kết hợp các thay đổi từ các nhánh khác nhau lại với nhau.  
- **Pull:** Lấy các thay đổi mới nhất từ kho lưu trữ trên máy chủ về máy của bạn.  
- **Push:** Gửi các thay đổi của bạn từ máy cục bộ lên kho lưu trữ trên máy chủ.

---

### III. Làm việc với Git

#### 1. Khởi tạo Git
Khởi tạo Git trong một folder để biến nó thành một repo: `git init`
Git sẽ tạo một folder ẩn để lưu trữ các thay đổi.
Khi một file được thay đổi, thêm vào hoặc xóa đi, các thay đổi này sẽ được ghi nhận.
Để xem file nào đang được theo dõi (track), có thể dùng lệnh: `git status`

#### 2. Thêm file vào Stage
Chọn những file được thay đổi để thêm vào stage:

- Thêm file riêng lẻ: `git add <tên_file>`  
- Thêm toàn bộ các file bị thay đổi: `git add --all` hoặc `git add -A`  
- Loại file ra khỏi stage: `git restore --staged <tên_file>`

#### 3. Commit thay đổi
Các file trong stage được commit, Git sẽ lưu lại một snapshot vĩnh viễn của các file này.  
Khi commit cần để lại một message ngắn:

- Commit thông thường: `git commit -m "First release of Hello World!"`  
- Commit nhanh với các file đã được track (bỏ qua stage): `git commit -a -m "Quick update to README"`

#### 4. Xem lịch sử commit
Git cho phép xem toàn bộ lịch sử commit:

- Xem toàn bộ lịch sử commit: `git log`  
- Xem tóm tắt lịch sử commit: `git log --oneline`  
- Xem chi tiết một commit cụ thể: `git show <commit>`  
- Xem các thay đổi giữa working directory hiện tại và lần commit gần nhất (chưa được stage): `git diff`  
- Xem các thay đổi giữa các file được stage và lần commit gần nhất: `git diff --staged`

#### 5. Revert commit
Có thể quay về (revert) bất kỳ commit nào trong quá khứ:

- Quay về lần commit gần nhất: `git revert HEAD`  
- Quay về commit cụ thể: `git revert <commit>`  
- Bỏ qua commit message khi revert: `git revert --no-edit`

#### 6. Ghi chú
- Git **không lưu lại một bản copy riêng của từng file** mỗi khi commit, mà theo dõi sự thay đổi (diff) giữa các lần commit.  
- Hầu hết các hoạt động của Git xảy ra **trên máy cá nhân**.  
  Chỉ có **push** và **pull** yêu cầu tương tác với GitHub, GitLab, v.v.

### IV. Push và Pull

#### 1. Push
Sau khi commit trên local repo (máy cá nhân), chúng ta muốn cập nhật nội dung đó lên remote repo (GitHub, GitLab, …). Khi đó, ta sử dụng lệnh `git push`.

Các lệnh cơ bản:

- Push cơ bản: `git push`  
- Force push: nếu push bị từ chối vì một số lý do, có thể dùng force push — **lưu ý** rằng thao tác này sẽ **ghi đè** lên các thay đổi ở remote repo:  
  - `git push --force`  
  - Để an toàn hơn, có thể dùng: `git push --force-with-lease`

#### 2. Pull
Pull dùng để **cập nhật thay đổi từ remote repo về local repo**, giúp đồng bộ mã nguồn mới nhất.

Các lệnh cơ bản:

- Fetch: lấy các cập nhật mới từ remote repo về local, nhưng **chưa hợp nhất (merge)** vào branch hiện tại — dùng khi muốn xem hoặc kiểm tra thay đổi trước khi áp dụng.  
  - `git fetch`
- Merge: kết hợp branch hiện tại với một branch khác (thường sau khi fetch).  
  - `git merge <branch>`
- Pull: kết hợp hai bước fetch và merge trong một lệnh duy nhất.  
  - `git pull`
