# Bài tập:

- Tạo tablespace và datafile
  > `create tablespace tbs01 datafile 'G:\APP\ORACLE\ORADATA\data01.dbf' size 1M;`
- Tạo tablespace và datafile tự động mở rộng
  > `create tablespace tbs02 datafile 'G:\APP\ORACLE\ORADATA\data02.dbf' size 500K, 'G:\APP\ORACLE\ORADATA\data03.dbf' size 1M AUTOEXTEND ON;`
- Danh sách datafile của tablespace
  > `select file_name from dba_data_files where tablespace_name = 'USERS';`
- Tạo User với hạn mức trên datafile:
  > `CREATE USER A IDENTIFIED BY A DEFAULT TABLESPACE tbs01 QUOTA 15M ON tbs01 QUOTA 15M ON tbs02;`
- Tạo User:
  > `CREATE USER B IDENTIFIED BY B DEFAULT TABLESPACE tbs02 QUOTA 15M ON tbs02;`
- gán quyền đăng nhập và tạo bảng:
  > `grant create session to A/B;` > `grant create table to A/B;`
- Lấy tablespace theo người dùng
  - > `conn / as sysdba;`
  - > `select username,default_tablespace from dba_users where username = 'A';`
- Tạo bảng:
  - > `create table tableA1 (ID number, name nvarchar2(50)) tablespace tbs01;`
  - > `create table tableA2 (ID number) tablespace tbs01;`
- Insert dữ liệu:
  - > `conn A/A;`
  - > `insert into tableA1 values(1, 'Sang Dzai');`
  - > `insert into tableA2 values(1);`
- Lấy tablespace theo tài khoản đăng nhập hiện tại:
  - > `SELECT TABLESPACE_NAME,CONTENTS FROM USER_TABLESPACES;`
- Lấy danh sách bảng trong tablespace:
  - > `select owner, segment_type, segment_name from dba_segments where tablespace_name = 'TBS01' order by 1,2,3;`
- Lấy danh sách bảng theo người dùng:
  - > `SELECT TABLE_NAME FROM ALL_TABLES WHERE OWNER = 'A';`
