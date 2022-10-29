# Kiến thức cơ bản về Oracle

## Một số câu lệnh điển hình

1. Kết nối cơ sở dữ liệu:

- Đăng nhập SQL : `SQLPLUS [{username[/password] [AtS {SYSOPER | SYSDBA | SYSASM}]`
- Kết nối hoặc đổi tài khoản: `conn username/pass@Database`
- Dọn dẹp dòng lệnh: `cle scr`
- Truy vấn thông tin của bảng: `Desc ten_bang`
- Chạy một file sql: `@<path>`
- Trạng thái listen: `lsnrctl status / help`
- Lấy tên instance: `select instance_name from v$instance;`
- Chuyển đổi Instance: `set oracle_sid=<name>` <Lưu ý: thoát sqlplus>
- Hiện người dùng hiện tại: `show user;`

## Một số bảng cơ bản

- Thông tin về tablespace: `dba_tablespaces`, `v$tablespace`
- Thông tin về datafiles: `dba_data_files`, `v$datafile`
- Thông tin về tempfiles:
  `dba_temp_files`, `v$tempfile`

## Một số khái niệm:

### Listener

- Lắng nghe yêu cầu kết nối , xây dựng kết nối đến oracle
- > CMD: `lsnrctl`
- ![ảnh minh họa](../Image/listener.png)

### Instance:

> Là thành phần liên kết giữa người dùng và thông tin trong Oracle Database. Một Oracle Instance chỉ được mở cho duy nhất một Oracle Database cần truy xuất.

### Database:

> Oracle Database dùng để lưu trữ và cung cấp thông tin cho người dùng. Việc quản lý dữ liệu trong Oracle Database thông qua cấu trúc lưu trữ logic và vật lý.

### Thành phần vật lý:

- < chỉ chứa dữ liệu của 1 db>
- Data file:
  - Lưu tất cả dữ liệu của duy nhất một database
  - Kích thước có thể tự động tăng theo kích thước database
- Redo Log file:
  - Lưu trữ thông tin thay đổi
- Control file:
  - chứa thông tin về cấu trúc database: tên, nơi lưu trữ của datafile, redo log file

### Tablespace

- Tablespace chỉ thuộc về một Database duy nhất.
- Một Tablespace bao gồm một hay nhiều tập tin vật lý dùng để lưu trữ dữ liệu, đó là Data File.
- Tablespace được tạo nên bởi sự kết hợp của một hay nhiều đơn vị lưu trữ Logic gọi là Segment, một Segment được chia thành nhiều Extent và trong Extent thì có nhiều Data Block liên tục nhau.
  - Data block:
    > Là đơn vị lưu trữ Logic nhỏ nhất được Oracle sử dụng trong việc đọc và ghi dữ liệu trong Oracle Database.
  - Extent
    > Tập hợp nhiều Data Block liên tiếp nhau sẽ tạo thành một đơn vị lưu trữ Logic lớn hơn gọi là Extent. Số lượng Data Block của một Extent tùy thuộc vào kích thước được chỉ định cho Extent khi tạo đối tượng Table.
  - Segment:
    > Segment là tập hợp một số Extent dùng để chứa toàn bộ thông tin của cấu trúc lưu trữ Logic bên trong Tablespace, như là Table.
- Câu lệnh:
  - Show đường dẫn hiện tại db:
    ` select file_name from dba_data_files where tablespace_name = 'USERS';`
  - Tạo:
    `create tablespace userdata datafile 'G:\APP\ORACLE\ORADATA\userdata1.dbf' size 1M;`
  - Thay đổi kích thước:  
    `alter database datafile 'G:\APP\ORACLE\ORADATA\userdata1.dbf' resize 2M;`
  - Thêm datafile
    `alter tablespace userdata add datafile 'G:\APP\ORACLE\ORADATA\userdata2.dbf' size 1M;`
  - Drop:
    `drop tablespace userdata including contents and datafiles;`
  - Thay đổi trạng thái:
    `alter tablespace userdata offline;`
  - Các bảng thông tin:
    - `select * from v$tablespace;`,
    - `select tablespace_name from dba_tablespaces;`
  - ![Ảnh minh họa](https://csc.edu.vn/data/images/tin-tuc/lap-trinh-csdl/kien-thuc-lap-trinh/gioi-thieu-oracle/oracel-4.png)

### Datafile

- `select file_name from dba_data_files;`
- `select name from v$datafile;`
- Tự động mở rộng datafile
  - `create tablespace userdata datafile 'G:\APP\ORACLE\ORADATA\userdata1.dbf' size 1M autoextend on next 1M maxsize 50M;`

### Segments

1. Các loại Segments

- User segments
- Temporary Segments
  > Khi một user thực hiện các lênh như CREATE INDEX, SELECT DISTINCT, và SELECT GROUP BY, Oracle sẽ cố gắng thực hiện công việc sắp xếp ngay trong bộ nhớ. Khi công việc sắp xếp cần đến nhiều không gian hơn, các kết quả này sẽ được ghi trực tiếp lên đĩa. Temporary segments sẽ được dùng đến trong trường hợp này.
- Undo Segments
  >     Undo segment được sử dụng trong transaction (giao dịch) để tạo các thay đổi trong database. Trước khi thay đổi các dữ liệu hay các index blocks, các giá trị cũ sẽ được lưu giữ vào undo segments. Việc làm này cho phép user có thể phục hồi lại các thay đổi.

### Extent

- Extent là đơn vị lưu trữ logic bao gồm các data block. Một segment bao gồm một hoặc nhiều extent.
- Một extent được cấp phát khi segment được:
  - Tạo ra
  - Mở rộng
  - Thay đổi
- Một extent bị thu hồi khi segment bị:
  - Xóa bỏ
  - Thay đổi
  - Cắt bớt

### Block

- Là đơn vị lưu trữ nhỏ nhất của Oracle database.
- Mỗi data block có kích thước bằng một số byte. Mặc định là 8 KB. Tham số DB_BLOCK_SIZE quy định kích thước này.
- Cấu trúc:
  ![Block](.\Image\Block.png)

### Câu lệnh

- ` select segment_name, tablespace_name from dba_segments where owner = 'SYS';`

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
