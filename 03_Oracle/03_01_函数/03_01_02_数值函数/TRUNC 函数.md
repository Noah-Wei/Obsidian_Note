# Oracle 中的 TRUNC 函数详解

TRUNC 函数是 Oracle 数据库中用于**截断数值或日期**的函数，它可以将数值截断到指定的小数位，或将日期截断到指定的时间单位。

## 基本语法

### 对于数值的截断
```sql
TRUNC(number [, decimal_places])
```

### 对于日期的截断
```sql
TRUNC(date [, format])
```

## 参数说明

### 数值截断参数
| 参数 | 说明 |
|------|------|
| `number` | 要进行截断的数值 |
| `decimal_places` (可选) | 指定保留的小数位数，默认为 0 |

### 日期截断参数
| 参数 | 说明 |
|------|------|
| `date` | 要进行截断的日期 |
| `format` (可选) | 指定截断的精度单位 |

## 数值截断示例

### 1. 基本数值截断
```sql
SELECT TRUNC(123.4567) FROM dual;       -- 结果: 123 (默认截断到整数)
SELECT TRUNC(123.4567, 2) FROM dual;    -- 结果: 123.45
SELECT TRUNC(123.4567, -1) FROM dual;   -- 结果: 120 (十位截断)
```

### 2. 与 ROUND 对比
```sql
SELECT TRUNC(123.499), ROUND(123.499) FROM dual;  -- 结果: 123 | 123
SELECT TRUNC(123.5), ROUND(123.5) FROM dual;     -- 结果: 123 | 124
```

### 3. 财务计算应用
```sql
SELECT account_id,
       balance,
       TRUNC(balance * 0.033, 2) AS monthly_interest
FROM accounts;
```

## 日期截断示例

### 1. 基本日期截断
```sql
SELECT TRUNC(SYSDATE) FROM dual;           -- 结果: 当天00:00:00
SELECT TRUNC(SYSDATE, 'MM') FROM dual;     -- 结果: 当月1日
SELECT TRUNC(SYSDATE, 'YYYY') FROM dual;   -- 结果: 当年1月1日
```

### 2. 常用格式参数
| 格式参数 | 说明 |
|----------|------|
| 'DD' | 截断到天 (默认) |
| 'MM' | 截断到月 |
| 'YYYY' | 截断到年 |
| 'Q' | 截断到季度 |
| 'HH24' | 截断到小时 |
| 'MI' | 截断到分钟 |

### 3. 日期范围查询
```sql
SELECT *
FROM orders
WHERE order_date BETWEEN TRUNC(SYSDATE, 'MM') 
                     AND LAST_DAY(TRUNC(SYSDATE, 'MM'));
-- 查询本月所有订单
```

## 特殊注意事项

1. **NULL 处理**：如果输入为 NULL，返回 NULL
2. **与 ROUND 区别**：TRUNC 是直接截断，不进行四舍五入
3. **负数处理**：对于负数，TRUNC 向零方向截断
   ```sql
   SELECT TRUNC(-123.456, 1) FROM dual;  -- 结果: -123.4
   ```
4. **性能优势**：TRUNC 通常比 TO_CHAR/TO_DATE 组合更高效

## 实际应用场景

1. **财务周期统计**：
   ```sql
   SELECT TRUNC(transaction_date, 'MM') AS month,
          SUM(amount) AS total
   FROM transactions
   GROUP BY TRUNC(transaction_date, 'MM');
   ```

2. **年龄计算**：
   ```sql
   SELECT employee_name,
          TRUNC(MONTHS_BETWEEN(SYSDATE, birth_date)/12) AS age
   FROM employees;
   ```

3. **数据分组**：
   ```sql
   SELECT TRUNC(log_time, 'HH24') AS hour,
          COUNT(*) AS log_count
   FROM system_logs
   GROUP BY TRUNC(log_time, 'HH24');
   ```

4. **报表生成**：
   ```sql
   SELECT product_id,
          TRUNC(AVG(rating), 1) AS avg_rating
   FROM product_reviews
   GROUP BY product_id;
   ```

TRUNC 函数在需要精确控制数值精度或日期粒度时非常有用，特别是在财务计算、时间序列分析和数据汇总等场景中。