# Network Design and Simulation for a Critical Large Hospital 🏥

Bài tập lớn thiết kế, cấu hình và mô phỏng hệ thống mạng toàn diện cho một Bệnh viện Chuyên khoa quy mô lớn, bao gồm 1 cơ sở chính (Main Site tại TP.HCM) và 2 cơ sở phụ trợ (Auxiliary Sites tại DBP và BHTQ). Hệ thống được thiết kế để giải quyết bài toán xử lý dữ liệu y tế khổng lồ (HIS, RIS-PACS, LIS) và yêu cầu hoạt động ổn định 24/7.

## 🎯 Mục tiêu dự án

* Xây dựng kiến trúc mạng phân cấp, đảm bảo tính sẵn sàng cao (High Availability), bảo mật nghiêm ngặt và khả năng mở rộng.
* Thiết kế và quy hoạch IP, VLAN, triển khai các công nghệ kết nối LAN/WAN, định tuyến OSPF và cấu hình hệ thống tường lửa (Firewall), DMZ, VPN.
* Mô phỏng và kiểm thử toàn diện bằng Cisco Packet Tracer để xác minh tính đúng đắn của thiết kế.

## 🏗️ Kiến trúc Hệ thống

Bài tập áp dụng mô hình **Thiết kế mạng phân cấp** tiêu chuẩn của Cisco nhằm tối ưu hiệu suất, bảo mật và khả năng quản lý:

* **Lớp lõi:** "Xương sống" tốc độ cực cao (10GbE/40GbE) của mạng, chịu trách nhiệm chuyển tiếp gói tin nhanh nhất có thể, kết nối các khối Distribution và Data Center với khả năng chịu lỗi cao.
* **Lớp phân phối:** "Bộ não" của mạng LAN, đóng vai trò ranh giới định tuyến giữa các VLAN, thực thi danh sách kiểm soát truy cập (ACLs), lọc lưu lượng và cung cấp dự phòng cho lớp Access.
* **Lớp truy cập:** Điểm kết nối trực tiếp cho các thiết bị đầu cuối (máy trạm bác sĩ/y tá, máy X-quang, MRI, điện thoại IP, Access Points) và thực thi các chính sách bảo mật cơ bản.

## 🧪 Kế hoạch Kiểm thử 

Hệ thống đã được kiểm thử nghiêm ngặt thông qua 10 kịch bản nhằm xác thực khả năng kết nối và tính hiệu quả của các chính sách bảo mật. Toàn bộ các yêu cầu của bài toán đều được **đáp ứng thành công**.

### 1. Kiểm thử Kết nối

Đảm bảo luồng dữ liệu thông suốt và định tuyến chính xác giữa các mạng và cơ sở:

| Mã KB | Mô tả kịch bản | Công cụ | Mục đích xác thực |
| --- | --- | --- | --- |
| **TC-01** | Ping giữa 2 PC cùng VLAN 10 (KhamBenh) | `ping` | Hoạt động Lớp 2 (switching) và gán VLAN. |
| **TC-02** | Ping giữa PC VLAN 10 và PC VLAN 20 | `ping` | Hoạt động Lớp 3 (Inter-VLAN Routing) trên Core Switch (SVI). |
| **TC-03** | Traceroute từ PC VLAN 10 đến PC VLAN 20 | `traceroute` | Đường đi gói tin qua Default Gateway (Core Switch). |
| **TC-04** | Ping từ PC Main Site đến Server DBP Site | `ping` | Kết nối WAN và định tuyến động (OSPF). |
| **TC-05** | Traceroute từ PC Main Site đến Server DBP | `traceroute` | Đường đi WAN qua các Router biên. |
| **TC-06** | PC nội bộ (VLAN 10) truy cập Web Server DMZ | Web Browser | Tường lửa cho phép truy cập dịch vụ từ Internal vào DMZ. |
| **TC-07** | PC nội bộ (VLAN 10) truy cập Internet | Web Browser | Cấu hình NAT/PAT và chính sách ra Internet. |

### 2. Kiểm thử Bảo mật

Xác minh nguyên tắc *Deny all, allow by exception*, đảm bảo hệ thống chặn đứng các truy cập trái phép:

| Mã KB | Mô tả kịch bản | Kết quả | Mục đích xác thực |
| --- | --- | --- | --- |
| **TC-08** | Ping từ PC VLAN Khách (VLAN 99) đến PC VLAN Nhân viên (VLAN 10) | ❌ THẤT BẠI *(Denied)* | ACL/Firewall cách ly hoàn toàn mạng Khách với mạng nội bộ. |
| **TC-09** | Ping từ Server vùng DMZ đến PC VLAN Nhân viên (VLAN 10) | ❌ THẤT BẠI *(Denied)* | Tường lửa (stateful) ngăn chặn DMZ chủ động kết nối ngược vào Internal. |
| **TC-10** | Ping từ máy chủ Internet đến PC nội bộ (VLAN 10) | ❌ THẤT BẠI *(Denied)* | Firewall/NAT ngăn chặn hoàn toàn rủi ro xâm nhập từ bên ngoài. |

## 📂 Cấu trúc Repository

* `BTL2.pkt`: File Source code mô phỏng trên Cisco Packet Tracer.
* `ASS2_MMT_HK251.pdf`: Báo cáo kỹ thuật chi tiết (Bao gồm quy hoạch IP/VLAN, tính toán băng thông và phân tích thiết bị).
