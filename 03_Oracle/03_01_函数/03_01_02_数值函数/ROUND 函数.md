# Oracle 中的 ROUND 函数详解

ROUND 函数是 Oracle 数据库中用于**数值四舍五入**的函数，它可以对数字进行指定精度的舍入操作。

## 基本语法

```sql
ROUND(number [, decimal_places])
```

## 参数说明

| 参数 | 说明 |
|------|------|
| `number` | 要进行舍入的数值（可以是列、表达式或字面值） |
| `decimal_places` (可选) | 指定保留的小数位数，默认为 0 |

## 函数特点

1. **四舍五入规则**：遵循标准的四舍五入规则
2. **正负位数**：
   - 正数表示小数位数
   - 负数表示整数部分的舍入位
3. **NULL 处理**：如果输入为 NULL，返回 NULL
4. **日期支持**：也可用于日期类型的舍入

## 使用示例

### 1. 基本数值舍入

```sql
SELECT ROUND(123.4567) FROM dual;       -- 结果: 123 (默认舍入到整数)
SELECT ROUND(123.4567, 2) FROM dual;    -- 结果: 123.46
SELECT ROUND(123.4567, -1) FROM dual;   -- 结果: 120 (十位舍入)
```

### 2. 边界情况

```sql
SELECT ROUND(123.5) FROM dual;          -- 结果: 124
SELECT ROUND(124.5) FROM dual;          -- 结果: 125
SELECT ROUND(125.5) FROM dual;          -- 结果: 126
```

### 3. 与表列结合使用

```sql
SELECT product_name, 
       price,
       ROUND(price * 0.9, 2) AS discount_price
FROM products;
```

### 4. 日期舍入

```sql
SELECT ROUND(SYSDATE, 'YYYY') FROM dual;   -- 舍入到年
SELECT ROUND(SYSDATE, 'MM') FROM dual;     -- 舍入到月
SELECT ROUND(SYSDATE, 'DD') FROM dual;     -- 舍入到天
```

## 特殊注意事项

1. **银行家舍入**：Oracle 的 ROUND 使用"四舍六入五成双"规则
   - 当舍入位=5时，前一位是偶数则舍，奇数则入
   ```sql
   SELECT ROUND(125.5) FROM dual;  -- 结果: 126 (5前是奇数5，进位)
   SELECT ROUND(124.5) FROM dual;  -- 结果: 124 (5前是偶数4，舍去)
   ```

2. **与 TRUNC 对比**：
   ```sql
   SELECT ROUND(123.456, 2), TRUNC(123.456, 2) FROM dual;
   -- 结果: 123.46 | 123.45
   ```

3. **格式化输出**：
   ```sql
   SELECT TO_CHAR(ROUND(123.4567, 2), '9999.99') FROM dual;
   -- 结果: ' 123.46'
   ```

## 实际应用场景

1. **财务计算**：
   ```sql
   SELECT invoice_id,
          amount,
          ROUND(amount * tax_rate, 2) AS tax_amount
   FROM invoices;
   ```

2. **报表统计**：
   ```sql
   SELECT department_id,
          ROUND(AVG(salary), 0) AS avg_salary
   FROM employees
   GROUP BY department_id;
   ```

3. **价格处理**：
   ```sql
   UPDATE products
   SET price = ROUND(price * 1.05, 2); -- 涨价5%并保留两位小数
   ```

4. **日期分组**：
   ```sql
   SELECT ROUND(log_time, 'HH24'), 
          COUNT(*) 
   FROM system_logs
   GROUP BY ROUND(log_time, 'HH24');
   ```

5. **单位转换**：
   ```sql
   SELECT ROUND(kilometers * 0.621371, 1) AS miles 
   FROM distances;
   ```

## 性能提示

1. 在大量数据计算时，先过滤再应用 ROUND 更高效
2. 在索引列上使用 ROUND 会导致索引失效
3. 对于固定小数位需求，考虑使用 NUMBER(p,s) 类型定义列

ROUND 函数是数值处理中最常用的函数之一，特别适合财务、统计和科学计算等需要精确控制的场景。