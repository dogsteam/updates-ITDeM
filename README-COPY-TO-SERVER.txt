ITDEM WINDOWS SERVER — V0.1.4

=== LẦN ĐẦU CÀI (VPS mới / chưa có data) ===
1. Copy cả thư mục release này lên VPS (ví dụ C:\ITDeM\) — web app đã NHÚNG SẴN trong ITDeMServer.exe, không cần thư mục public\.
2. Cài PostgreSQL trên VPS. Tạo database rỗng tên itdem:
      psql -U postgres -c "CREATE DATABASE itdem"
3. IMPORT DEMO DATA (để chạy data demo như local):
      psql -U postgres -d itdem -f itdem-init.sql
   File gồm schema + demo data. Server thấy schema sẵn sẽ BỎ QUA migration,
   giữ nguyên demo data. Sau này muốn clear demo: truncate bảng, không đụng schema.
4. Sửa ITDeMServer.env: thay THAY_MAT_KHAU_POSTGRES bằng mật khẩu PostgreSQL thật.
5. Đổi ADMIN_PASSWORD trong ITDeMServer.env (điền sẵn admin123 — dùng cho /admin Basic Auth).
6. Chạy ITDeMServer.exe → mở http://SERVER_IP:1982/ → trang đăng nhập.

=== ĐĂNG NHẬP ===
- User thường + Admin: đăng nhập bằng FORM tại http://IP:1982/
- Trang quản trị server (backup, Firebase): http://IP:1982/admin (HTTP Basic — dùng ADMIN_USERNAME/ADMIN_PASSWORD trong .env)

=== DEMO ACCOUNTS (password = username) ===
admin        → ADMIN (toàn quyền)
truongkhoa   → CNTT_APPROVER (trưởng phòng CNTT)
thukho       → CNTT_STAFF (thủ kho)
kythuat1     → CNTT_STAFF (kỹ thuật)
kettoankho   → CNTT_STAFF (kế toán kho)
kettoantc    → VIEW_KETOAN
vattu        → VIEW_VATTU
bgd          → VIEW_BGD
nhanvienkhoa → KHOA_USER (khoa khám bệnh)

=== TÀI LIỆU TRONG PHẦN MỀM ===
- Menu "Tài liệu quy trình" (hoặc /#/docs): 10 quy trình QT-01…QT-11 dạng chữ + sơ đồ, in PDF được
- File HDSD-ITDeM-Nhap-kho-tu-NCC.pdf: hướng dẫn chi tiết nhập kho

=== CẬP NHẬT PHIÊN BẢN SAU (KHÔNG ĐÈ DATA) — GIỐNG ShipTuDoServer ===
1. Tắt ITDeMServer.exe đang chạy.
2. CHỈ copy đè ITDeMServer.exe mới.
   KHÔNG đè: ITDeMServer.env (chứa encryption key — mất = mất data mã hóa),
   storage\, itdem-server.log, CSDL PostgreSQL.
3. Chạy lại EXE. Server tự backup CSDL trước migration, chạy migration
   TĂNG DẦN trong transaction — không replace DB cũ.

=== THƯ MỤC public\ (WEB APP — BẮT BUỘC) ===
Thiếu thư mục này, http://IP:1982/ sẽ redirect sang /admin thay vì mở web app.
Cập nhật version: luôn copy đè public\ kèm theo EXE mới.

=== FILE/TỰ ĐỘNG TẠO — PHẢI GIỮ LẠI ===
- ITDeMServer.env
- storage\
- itdem-server.log
- CSDL PostgreSQL (database itdem)
