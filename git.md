# 30/10/2025  
## Git

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
Khởi tạo Git trong một folder để biến nó thành một repo:
```bash
git init
```