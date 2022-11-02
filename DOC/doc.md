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
