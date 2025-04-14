# Oracle 中的 LPAD 函数和 RPAD 函数详解

LPAD 函数是 Oracle 数据库中用于**左侧填充字符串**的函数，它可以在字符串的左侧填充指定的字符，直到字符串达到指定的长度。

RPAD 函数是 Oracle 数据库中用于**右侧填充字符串**的函数，它与 LPAD 函数相对应，可以在字符串的右侧填充指定的字符，直到字符串达到指定的长度。

**没有PAD()函数，不同于TRIM、LTRIM 和 RTRIM 函数**

## 基本语法

```sql
LPAD(string, target_length [, pad_string])

RPAD(string, target_length [, pad_string])
```

## 参数说明

| 参数 | 说明 |
|------|------|
| `string` | 原始字符串（可以是列名、变量或字符串常量） |
| `target_length` | 填充后的总长度（数值类型） |
| `pad_string` (可选) | 用于填充的字符串，默认为单个空格 |

## 使用示例

### 1. 基本填充（使用默认空格）

```sql
SELECT LPAD('Oracle', 10) FROM dual;
-- 结果: '    Oracle' (左侧填充4个空格，总长度10)

SELECT RPAD('Oracle', 10) FROM dual;
-- 结果: 'Oracle    ' (右侧填充4个空格，总长度10)
```

### 2. 使用指定填充字符

```sql
SELECT LPAD('123', 6, '0') FROM dual;
-- 结果: '000123' (左侧填充3个0，总长度6)

SELECT RPAD('123', 6, '0') FROM dual;
-- 结果: '123000' (右侧填充3个0，总长度6)
```

### 3. 填充多字符模式

```sql
SELECT LPAD('ID', 7, '*.') FROM dual;
-- 结果: '*.*.ID' (循环使用'*.'填充)

SELECT RPAD('ID', 7, '*.') FROM dual;
-- 结果: 'ID*.*.*' (循环使用'*.'填充)
```

### 4. 结合表列使用

```sql
SELECT employee_name, LPAD(salary, 10, ' ') AS padded_salary
FROM employees;

SELECT employee_name, RPAD(job_title, 20, '.') AS formatted_title
FROM employees;
```

### 5. 处理超长字符串

```sql
SELECT LPAD('Oracle Database', 5, '*') FROM dual;
-- 结果: 'Oracl' (如果原始字符串已长于目标长度，会被截断)

SELECT RPAD('Oracle Database', 5, '*') FROM dual;
-- 结果: 'Oracl' (如果原始字符串已长于目标长度，会被截断)
```

## 特殊注意事项

1. **截断行为**：如果原始字符串长度 > target_length，LPAD 会从右侧截断字符串，RPAD 会从右侧截断字符串
2. **NULL 处理**：如果 string 为 NULL，结果返回 NULL
3. **多字节字符**：对于多字节字符集，长度计算可能不同
4. **pad_string 为空**：如果 pad_string 为空字符串('')，返回原字符串

## LPAD 与 RPAD 的区别

LPAD 是左侧填充，RPAD 是右侧填充：

```sql
SELECT LPAD('X', 3, 'Y') FROM dual;  -- 结果: 'YYX'
SELECT RPAD('X', 3, 'Y') FROM dual;  -- 结果: 'XYY'
```

## LPAD 的实际应用场景

1. **数字格式化**（前导零）：
   ```sql
   SELECT LPAD(month, 2, '0') AS formatted_month FROM sales;
   ```

2. **固定长度显示**：
   ```sql
   SELECT LPAD(product_code, 8, ' ') || product_name 
   FROM products;
   ```

3. **生成特定格式代码**：
   ```sql
   SELECT 'ORD' || LPAD(order_id, 6, '0') AS order_number
   FROM orders;
   ```

4. **报表对齐**：
   ```sql
   SELECT LPAD(department_name, 20, '.') || employee_count
   FROM department_stats;
   ```

5. **数据脱敏**：
   ```sql
   SELECT LPAD(SUBSTR(credit_card, -4), 16, '*') AS masked_card
   FROM payments;
   ```

LPAD 函数在数据格式化、报表生成和字符串处理中非常实用，是 Oracle SQL 中常用的字符串函数之一。

## RPAD 的实际应用场景

1. **固定长度显示**：

   ```sql
   SELECT employee_name || RPAD(' ', 5) || salary 
   FROM employees;
   ```

2. **生成特定格式文本**：

   ```sql
   SELECT RPAD('Item:', 10) || product_name AS product_label
   FROM products;
   ```

3. **报表对齐**：

   ```sql
   SELECT employee_name || RPAD(':', 15-LENGTH(employee_name), '.') || salary
   FROM employees;
   ```

4. **数据脱敏**：

   ```sql
   SELECT RPAD(SUBSTR(phone, 1, 3), 10, '*') AS masked_phone
   FROM customers;
   ```

5. **生成分隔线**：

   ```sql
   SELECT RPAD('-', 20, '-') FROM dual;
   -- 结果: '--------------------'
   ```

## RPAD 的高级用法

1. **动态填充**：

   ```sql
   SELECT RPAD('Section', column_width-LENGTH('Section'), '*') 
   FROM table_settings;
   ```

2. **与 TRIM 结合使用**：

   ```sql
   SELECT TRIM(RPAD(' Text ', 10)) FROM dual;
   ```

3. **生成固定宽度文件格式**：

   ```sql
   SELECT RPAD(employee_id, 8) || 
          RPAD(employee_name, 30) || 
          RPAD(TO_CHAR(salary, '99999'), 7)
   FROM employees;
   ```

RPAD 函数在数据格式化、报表生成和固定宽度文本处理中非常实用，是 Oracle SQL 中常用的字符串操作函数之一。