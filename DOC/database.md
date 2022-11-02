# Database:

> Oracle Database dùng để lưu trữ và cung cấp thông tin cho người dùng. Việc quản lý dữ liệu trong Oracle Database thông qua cấu trúc lưu trữ logic và vật lý.

# Thành phần vật lý:

- > chỉ chứa dữ liệu của 1 db
- Data file:
  - Lưu tất cả dữ liệu của duy nhất một database
  - Kích thước có thể tự động tăng theo kích thước database
- Redo Log file:
  - Lưu trữ thông tin thay đổi
- Control file:
  - chứa thông tin về cấu trúc database: tên, nơi lưu trữ của datafile, redo log file

# Tạo CSDL

- Để tạo một CSDL mới, bạn cần phải có các điều kiện sau:
  - Một account đủ quyền tạo CSDL.
  - Bộ nhớ đủ để khởi động một instance.
  - Đĩa đủ dung lượng cho CSDL đã lên kế hoạch.

## Công cụ: DBCA

## Dòng lệnh

- Xóa DB : mynewdb
  - > `set oracle_sid=mynewdb`
  - > `sqlplus / as sysdba`
  - > `shutdown immediate;`
  - > `startup mount exclusive restrict;`
  - > `drop database;`
  - > `Quit`;
  - > `sc delete oracleservicemynewdb`
