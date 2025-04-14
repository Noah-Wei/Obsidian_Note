# Oracle 中的 LENGTH 函数详解

LENGTH 是 Oracle 数据库中用于获取字符串长度的函数，它返回字符串中的字符数。

## 基本语法

```sql
LENGTH(string)
```

## 参数说明

- **string**: 要计算长度的字符串表达式，可以是字符串常量、列名或变量

## 使用示例

### 1. 基本用法

```sql
SELECT LENGTH('Oracle数据库') FROM dual;
-- 结果: 8 (中文字符每个算1个字符)
```

### 2. 结合表列使用

```sql
SELECT employee_name, LENGTH(employee_name) AS name_length
FROM employees;
```

### 3. 在 WHERE 子句中使用

```sql
SELECT * FROM products
WHERE LENGTH(product_name) > 20;
```

### 4. 与其他函数结合使用

```sql
SELECT SUBSTR(description, 1, LENGTH(description)/2) AS half_description
FROM product_details;
```

## 特殊注意事项

1. LENGTH 函数计算的是字符数，不是字节数
2. 对于空字符串('')，LENGTH 返回 NULL
3. 对于 NULL 值，LENGTH 返回 NULL

## 与 LENGTHB 的区别

Oracle 还提供了 LENGTHB 函数，它按字节而不是字符计算长度：

```sql
SELECT LENGTH('甲骨文') FROM dual;    -- 结果: 3 (字符数)
SELECT LENGTHB('甲骨文') FROM dual;   -- 结果可能是 6 或 9 (取决于字符编码，字节数)
```

## 处理 NULL 值

如果需要处理 NULL 值，可以使用 NVL 函数：

```sql
SELECT LENGTH(NVL(column_name, '')) FROM table_name;
```

## 实际应用场景

1. 验证输入数据的长度是否符合要求
2. 截取字符串时确定截取位置
3. 数据清洗时识别异常长度的记录
4. 格式化输出时计算填充空格的数量

LENGTH 函数是 Oracle SQL 中处理字符串时最基础也最常用的函数之一，经常与 SUBSTR、INSTR 等字符串函数配合使用。