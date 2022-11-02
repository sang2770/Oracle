## Quản lý User

- User:

  > là một tài khoản trong cơ sở dữ liệu Oracle, sau khi được khởi tạo và gán quyền bằng lệnh CREATE USER thì tài khoản này được phép đăng nhập và sở hữu một schema trong cơ sở dữ liệu

- Tạo mới:

  - Đăng nhập user có quyền tạo
  - Xác định các tablespace trong đó user lưu trữ các object.
  - Quyết định hạn mức cho mỗi tablespace.
  - Chọn default tablespace và temporary tablespace.
  - Tạo user.
  - Gán quyền truy nhập cho user vừa tạo lập
  - Cú pháp:
    > `CREATE USER user IDENTIFIED BY password [DEFAULT TABLESPACE tablespace] [TEMPORARY TABLESPACE tablespace] [ QUOTA {integer [K|M] | UNLIMITED } ON tablespace [QUOTA {integer [K|M] | UNLIMITED} ON tablespace]...] [PASSWORD EXPIRE] [ACCOUNT{LOCK|UNLOCK }]`
    > Nếu không có mệnh đề default tablespace, thì default tablespace của user chính là default tablespace của database được quy định khi tạo CSDL
  - VD:
    > `CREATE USER student IDENTIFIED BY soccer DEFAULT TABLESPACE data TEMPORARY TABLESPACE temp QUOTA 15M ON data QUOTA 10M ON users PASSWORD EXPIRE;`

- Thay đổi hạn mức của user trên tablespaces

  - VD: `ALTER USER student QUOTA 0 ON USERS; `
    > Sau khi thay đổi hạn mức của user trên tablespace là 0, các đối tượng thuộc sở hữu user đó vẫn còn trong các tablespace, nhưng không thể cấp phát thêm một không gian mới để lưu trữ.. Ví dụ, nếu một Bảng Đó là 10MB tồn tại trong USERS tablespace, và sau đó tiến hành thay đổi hạn mức của user student trên tablespace USERS là 0, thì khi đó không thể cấp phát thêm vùng trống cho user trên USERS.

- Thay đổi mật khẩu:

  - Cú pháp: `ALTER USER user IDENTIFIED BY newpassword [REPLACE oldpassword]; `
    > Mệnh đề REPLACE oldpassword: Dành cho user A khi đăng nhập thành công và thay đổi mật khẩu của chính mình.

- Mở khóa/ Khóa:
  - Cú pháp: `ALTER USER user ACCOUNT lock/unlock;`
- Xóa User:
  - Cú pháp: `DROP USER <TEN_USER>`
  - Xóa những user đang kết nối:
    - `SELECT s.sid, s.serial#, s.status, p.spid FROM v$session s, v$process p WHERE s.username = 'TEST'`
    - `ALTER SYSTEM KILL SESSION '<SID>, <SERIAL>'`
    - `DROP USER ___`
- Bảng thông tin user:

  - DBA_USERS
  - DBA_TS_QUOTAS
  - USER_TS_QUOTAS

- Schema:
  > là 1 tập hợp các đối tượng trong cơ sở dữ liệu Oracle được quản lý bởi 1 user nào đó, các đối tượng của schema có thể là table, view, stored procedures, index, sequence… Schema được tự động tạo cùng với user khi thực thi lệnh CREATE USER.

> Mối quan hệ giữa User và Schema là quan hệ 1 – 1, một User chỉ quản lý 1 Schema, và cũng chỉ có 1 Schema được khởi tạo khi thực thi lệnh CREATE USER
