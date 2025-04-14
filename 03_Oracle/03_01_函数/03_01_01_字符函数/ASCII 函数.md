# Oracle 中的 ASCII 函数详解

ASCII 函数是 Oracle 数据库中用于获取字符 ASCII 码值的函数，它返回字符串中第一个字符的 ASCII 码值。

## 基本语法

```sql
ASCII(char)
```

## 参数说明

- **char**: 要获取 ASCII 码的字符或字符串（如果输入是字符串，只返回第一个字符的 ASCII 码）

## 使用示例

### 1. 基本用法

```sql
SELECT ASCII('A') FROM dual;
-- 结果: 65 (大写字母A的ASCII码)

SELECT ASCII('a') FROM dual;
-- 结果: 97 (小写字母a的ASCII码)

SELECT ASCII('1') FROM dual;
-- 结果: 49 (数字1的ASCII码)
```

### 2. 处理字符串

```sql
SELECT ASCII('Oracle') FROM dual;
-- 结果: 79 (只返回第一个字符'O'的ASCII码)
```

### 3. 结合表列使用

```sql
SELECT customer_name, ASCII(customer_name) AS first_char_code
FROM customers;
```

### 4. 特殊字符处理

```sql
SELECT ASCII(' ') FROM dual;    -- 空格: 32
SELECT ASCII('@') FROM dual;    -- @符号: 64
SELECT ASCII(CHR(9)) FROM dual; -- 制表符: 9
```

## 相关函数

### CHR 函数（与 ASCII 相反）

```sql
SELECT CHR(65) FROM dual; -- 结果: 'A'
```

### 组合使用示例

```sql
SELECT ASCII('A'), CHR(65) FROM dual;
-- 结果: 65 | A
```

## 特殊注意事项

1. 对于多字节字符（如中文），ASCII 函数可能返回不可预期的结果
2. 如果输入是空字符串，返回 NULL
3. 如果输入是 NULL，返回 NULL
4. 扩展 ASCII 字符（128-255）也能正确返回

## 实际应用场景

1. 字符分类和排序
2. 数据验证（检查特定字符）
3. 加密和编码转换
4. 特殊字符处理

```sql
-- 示例：查找以大写字母开头的产品
SELECT product_name 
FROM products
WHERE ASCII(product_name) BETWEEN 65 AND 90;

-- 示例：检查是否以字母开头
SELECT CASE 
         WHEN ASCII(column_name) BETWEEN 65 AND 90 
           OR ASCII(column_name) BETWEEN 97 AND 122 
         THEN '字母开头'
         ELSE '非字母开头'
       END AS check_result
FROM table_name;
```

ASCII 函数虽然简单，但在字符处理和验证中非常有用，特别是与 CHR 函数配合使用时，可以实现多种字符转换功能。