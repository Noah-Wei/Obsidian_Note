# Docker安装Oracle11G



> OS users:
>
> - root/install
> - oracle/install
>
> DB users:
>
> - SYS/5208
> - system/5208
> - ODBA/5208

- 数据源

```
docker pull yycx/oracle11

Port: 1521 SID: orcl
```

- 创建容器

```
docker run -d -p 1521:1521 --name oracle11g yycx/oracle11
```

- 进入容器

```
docker exec -it oracle11g bash
```

- 切换oracle用户

```
su - oracle
```

- 最后使用 sqlplus 连接：

```
sqlplus / as sysdba
```

##### 1、解锁scott账户

```sql
SQL> alter user scott account unlock;
User altered.
SQL> commit;
Commit complete.
SQL> conn scott/tiger
ERROR:
ORA-28001: the password has expired
Changing password for scott
New password:    设置为了 scott
Retype new password:
Password changed
Connected.
SQL>
```

##### 2、修改管理员密码

```sql
alter user sys identified by 123456;

alter user system identified by 123456;

alter user scott identified by 123456;
```

##### 3、设置密码过期时间为永不过期

```
Alter PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;
```

##### 4、设置密码登录尝试次数为不受限

```
alter profile default limit failed_login_attempts unlimited;
```

##### 5、创建表空间

```sql
CREATE SMALLFILE TABLESPACE "ODB" DATAFILE '/opt/oracle/app/oradata/orcl/ODB1' SIZE 500M AUTOEXTEND ON NEXT 500M MAXSIZE UNLIMITED LOGGING EXTENT MANAGEMENT LOCAL SEGMENT SPACE MANAGEMENT AUTO;

ALTER TABLESPACE "ODB" ADD DATAFILE '/opt/oracle/app/oradata/orcl/ODB2' SIZE 500M AUTOEXTEND ON NEXT 500M MAXSIZE UNLIMITED;

--同样设置loong表空间以及用户
```

##### 6、创建用户和密码

```
create user ODBA identified by 5208 default tablespace ODB;
```

##### 7、添加dba权限

```
grant connect,resource,dba to ODBA;
```