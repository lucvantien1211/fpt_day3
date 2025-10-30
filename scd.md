# FPT Day 3 - 30/10/2025
## Slowly Changing Dimension (SCD)

### I. Tổng quan
Trong Data Warehouse, **Dimension Table** lưu trữ các thông tin mô tả (metadata) của dữ liệu, ví dụ: khách hàng, sản phẩm, nhân viên, khu vực, v.v.

Theo thời gian, các thông tin này **có thể thay đổi** — ví dụ:
- Khách hàng đổi địa chỉ
- Nhân viên đổi phòng ban
- Sản phẩm đổi tên hoặc giá

Vấn đề đặt ra: **khi thông tin thay đổi, ta nên lưu lại lịch sử cũ hay chỉ cập nhật giá trị mới?**  
Cách xử lý các thay đổi này được gọi là **Slowly Changing Dimension (SCD)** (các dimension chậm thay đổi).

---

### II. Mục tiêu của SCD
- Giữ lại **lịch sử thay đổi** của dữ liệu nếu cần.
- Đảm bảo **tính toàn vẹn dữ liệu phân tích theo thời gian**.
- Giúp truy vấn dữ liệu trong các thời điểm khác nhau (ví dụ: “Doanh số theo khu vực *tại thời điểm đó*”).

---

### III. Các loại Slowly Changing Dimension

#### 1. **SCD Type 0 – Fixed Dimension**
- Không bao giờ thay đổi sau khi đã được nạp (load) vào warehouse.
- Dữ liệu được coi là “cố định” — nếu có thay đổi, bỏ qua.
- Thường dùng cho dữ liệu lịch sử hoặc dữ liệu không thay đổi (ví dụ: mã quốc gia ISO, giới tính).

**Ví dụ:**  
Khách hàng có `gender = 'Male'`. Dù người đó thay đổi giới tính thật ngoài đời, hệ thống vẫn giữ nguyên `'Male'`.

---

#### 2. **SCD Type 1 – Overwrite (Không lưu lịch sử)**
- Khi có thay đổi, giá trị **cũ bị ghi đè** bằng giá trị mới.
- Lịch sử cũ **không được lưu lại**.

**Ví dụ:**
| CustomerID | Name   | City     |
|-------------|--------|----------|
| 001         | An     | Hanoi    |

→ Sau khi khách hàng chuyển sang Đà Nẵng:

| CustomerID | Name   | City     |
|-------------|--------|----------|
| 001         | An     | Danang   |

Kết quả: chỉ còn giá trị mới (“Danang”), không còn biết trước đó là “Hanoi”.

---

#### 3. **SCD Type 2 – Add New Row (Lưu toàn bộ lịch sử)**
- Khi có thay đổi, **thêm một dòng mới** với khóa mới (ví dụ: `CustomerKey` tự tăng).  
- Giữ lại bản ghi cũ để lưu lịch sử.

**Ví dụ:**
| CustomerKey | CustomerID | Name | City   | StartDate  | EndDate    | IsCurrent |
|--------------|-------------|------|--------|-------------|-------------|------------|
| 1001         | 001         | An   | Hanoi  | 2021-01-01  | 2023-06-30  | N          |
| 1002         | 001         | An   | Danang | 2023-07-01  | NULL        | Y          |

→ Khi khách hàng chuyển sang “Danang”, ta **thêm dòng mới** và đánh dấu bản ghi cũ là không còn hiệu lực (`IsCurrent = N`).

✅ Ưu điểm:
- Giữ toàn bộ lịch sử.
- Dễ truy vấn theo thời gian.

⚠️ Nhược điểm:
- Dữ liệu phình to nhanh.
- Cần xử lý logic khi join (chỉ chọn bản ghi hiện tại hoặc theo thời điểm cụ thể).

---

#### 4. **SCD Type 3 – Add New Column (Lưu 1 lần thay đổi gần nhất)**
- Thêm cột mới để lưu giá trị cũ.
- Chỉ lưu được **một lần thay đổi** (giá trị hiện tại + giá trị trước đó).

**Ví dụ:**
| CustomerID | Name | CurrentCity | PreviousCity |
|-------------|------|-------------|---------------|
| 001         | An   | Danang      | Hanoi         |

✅ Ưu điểm:
- Truy vấn nhanh, ít dòng.

⚠️ Nhược điểm:
- Không lưu được nhiều hơn một lần thay đổi.

---

#### 5. **SCD Type 4 – History Table (Tách bảng lịch sử riêng)**
- Bảng chính lưu dữ liệu hiện tại.
- Bảng phụ (history table) lưu toàn bộ lịch sử thay đổi.

**Ví dụ:**

**Bảng chính (Current):**
| CustomerID | Name | City   |
|-------------|------|--------|
| 001         | An   | Danang |

**Bảng lịch sử (History):**
| CustomerID | Name | City   | ChangeDate |
|-------------|------|--------|-------------|
| 001         | An   | Hanoi  | 2023-06-30  |

✅ Ưu điểm:
- Gọn gàng, tách biệt dữ liệu hiện tại và lịch sử.  

⚠️ Nhược điểm:
- Phức tạp hơn khi truy vấn kết hợp 2 bảng.

---

#### 6. **SCD Type 6 – Hybrid (Kết hợp Type 1 + Type 2 + Type 3)**
- Kết hợp ưu điểm của Type 1, 2, và 3.
- Vừa ghi đè giá trị hiện tại (Type 1), vừa lưu lịch sử theo dòng (Type 2), và giữ giá trị trước đó (Type 3).

**Ví dụ:**
| CustomerKey | CustomerID | Name | CurrentCity | PreviousCity | StartDate  | EndDate    | IsCurrent |
|--------------|-------------|------|--------------|---------------|-------------|-------------|------------|
| 1001         | 001         | An   | Hanoi        | NULL          | 2021-01-01  | 2023-06-30  | N          |
| 1002         | 001         | An   | Danang       | Hanoi         | 2023-07-01  | NULL        | Y          |

✅ Ưu điểm:
- Có thể truy vết lịch sử và so sánh giá trị cũ – mới dễ dàng.  

⚠️ Nhược điểm:
- Thiết kế phức tạp, tốn tài nguyên khi ETL.