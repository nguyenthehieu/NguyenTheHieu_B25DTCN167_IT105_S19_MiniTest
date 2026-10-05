

![][image1]

## **CHƯƠNG 1\. INTRODUCTION**

### **1.1. Mục đích**

Đặc tả yêu cầu cho phân hệ **Đặt hàng nhanh**, làm cơ sở cho thiết kế giao diện, cơ sở dữ liệu và kiểm thử.

### **1.2. Phạm vi**

Khách hàng xem sản phẩm, nhập số lượng, đặt hàng và nhận phản hồi. Nhân viên xem và xác nhận đơn hàng.

# **CHƯƠNG 2\. OVERALL DESCRIPTION**

### **2.1. Actor**

* **Khách hàng:** Đặt hàng.  
* **Nhân viên xử lý đơn:** Xác nhận đơn.

### **2.2. Quy tắc nghiệp vụ**

* Một khách hàng có thể có nhiều đơn hàng.  
* Một đơn hàng có thể có nhiều sản phẩm.  
* Số lượng phải \> 0\.  
* Đơn mới có trạng thái **Chờ xử lý**.  
* Nhân viên xác nhận → **Đã xác nhận**.  
* Sau khi đặt hàng phải có thông báo thành công hoặc lỗi.

**CHƯƠNG 3\. SPECIFIC REQUIREMENTS**

## **3.1. Chức năng sản phẩm — Use Case "Đặt hàng"**

**Tiền điều kiện:** Khách hàng đang ở danh sách sản phẩm.

**Luồng chính:**

1. Xem danh sách sản phẩm.  
2. Chọn sản phẩm và nhập số lượng.  
3. Nhấn **Xác nhận đặt hàng**.  
4. Hệ thống kiểm tra và tạo đơn ở trạng thái **Chờ xử lý**.  
5. Hiển thị thông báo thành công.

**Ngoại lệ 4a:** Số lượng rỗng hoặc ≤ 0 → thông báo lỗi và không tạo đơn.

## **3.2. Giao diện người dùng**

| Use Case | UI Element |
| ----- | ----- |
| Xem sản phẩm | Danh sách sản phẩm |
| Nhập số lượng | Input |
| Xác nhận | Button |
| Thành công | Thông báo Success |
| Lỗi | Thông báo Error |

**Wireframe:** https\://www\.figma.com/design/Yl14Tw4MTW0a0bOMrY5XUd/Untitled?node-id=0-1\&t=sVL2Wkq7oWdWZliZ-1

**3.3. Yêu cầu chức năng**

**REQ-01:** Sau khi khách hàng nhấn **“Xác nhận đặt hàng”**, hệ thống phải hiển thị thông báo kết quả trong vòng **2 giây**.

→ Có thể đo bằng đơn vị **giây**, không còn từ cảm tính “nhanh chóng”, “mượt mà” → đạt **Verifiable**.

## **3.4. Yêu cầu cơ sở dữ liệu — ERD**

### **Các Entity**

**CUSTOMER**

* customerId (PK)  
* customerName

**PRODUCT**

* productId (PK)  
* productName  
* unitPrice

**ORDER**

* orderId (PK)  
* customerId (FK)  
* status

**ORDER\_DETAIL**

* orderId (PK, FK)  
* productId (PK, FK)  
* quantity

### **Quan hệ**

CUSTOMER 1 ─── N ORDER  
ORDER    1 ─── N ORDER\_DETAIL  
PRODUCT  1 ─── N ORDER\_DETAIL

ORDER và PRODUCT có quan hệ **N:N**, được xử lý bằng ORDER\_DETAIL.

## **3.5. Rà soát chuẩn hóa**

Bảng nháp có:

productName1, productName2

→ **Vi phạm 1NF** vì có nhóm thuộc tính lặp.

### **Cách sửa**

Tách sản phẩm trong đơn thành bảng ORDER\_DETAIL:

| orderId | productId | quantity |
| ----- | ----- | ----- |
| ORD-01 | PRD-01 | 2 |
| ORD-01 | PRD-02 | 1 |

[image1]: <data:image/png;base64,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>