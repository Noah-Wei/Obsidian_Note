# Oracle 中的 SUBSTR 函数详解

SUBSTR 是 Oracle 数据库中用于截取字符串的函数，它可以从指定位置开始提取字符串的一部分。

## 基本语法

```sql
SUBSTR(string, start_position [, length])
```

## 参数说明

- **string**: 要截取的源字符串
- **start_position**: 开始截取的位置（正整数表示从左向右，负整数表示从右向左）
- **length** (可选): 要截取的子字符串长度。如果省略，则从 start_position 开始截取到字符串末尾

## 使用示例

### 1. 基本用法

```sql
SELECT SUBSTR('Oracle Database', 8, 4) FROM dual;
-- 结果: 'Data'
```

### 2. 省略 length 参数

```sql
SELECT SUBSTR('Oracle Database', 8) FROM dual;
-- 结果: 'Database'
```

### 3. 使用负的 start_position

```sql
SELECT SUBSTR('Oracle Database', -8, 4) FROM dual;
-- 结果: 'Data' (从右向左数第8个字符开始)
```

### 4. 结合表列使用

```sql
SELECT employee_name, SUBSTR(employee_name, 1, 3) AS initials
FROM employees;
```

### 5. 处理边界情况

```sql
-- 当 start_position 超出字符串长度时返回空字符串
SELECT SUBSTR('Oracle', 10, 2) FROM dual;
-- 结果: ''

-- 当 length 超出剩余字符串长度时返回剩余部分
SELECT SUBSTR('Oracle', 3, 10) FROM dual;
-- 结果: 'acle'
```

## 特殊注意事项

1. Oracle 中字符串位置从 **1** 开始计数，不是从 0 开始
2. 如果 start_position 为 0，Oracle 会将其视为 1
3. 对于 CLOB 类型数据，可以使用 DBMS_LOB.SUBSTR 函数

## 与 SUBSTRB 的区别

Oracle 还提供了 SUBSTRB 函数，它按字节而不是字符进行截取，在处理多字节字符集时会有不同表现：

```sql
SELECT SUBSTR('甲骨文', 2, 1) FROM dual;  -- 结果: '骨' (字符)
SELECT SUBSTRB('甲骨文', 2, 1) FROM dual; -- 结果可能不完整 (字节)
```

SUBSTR 函数在数据清洗、报表生成和字符串处理中非常实用，是 Oracle SQL 中最常用的字符串函数之一。