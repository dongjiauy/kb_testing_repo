# SQL 学习笔记 (For PMs)
**目标**: 不再求研发导数据，自己动手丰衣足食。

## 1. 基础查询
```sql
SELECT user_id, last_login 
FROM users 
WHERE status = 'active' 
LIMIT 10;
```

## 2. 关联表 (Join) - 难点！
- `INNER JOIN`: 只返回两个表都有的匹配行。
- `LEFT JOIN`: 返回左表所有行，右表没有的显示 NULL。
*场景*: 查所有用户最近一次下单时间。用 LEFT JOIN `orders` 表。

## 3. 聚合函数
- `COUNT(DISTINCT user_id)`: 算 UV。
- `AVG(order_amount)`: 算客单价。

*Flag*: 下周数据周报，我要自己写 SQL 跑出来！惊艳一下大家。

---

## 相关笔记

- [[04_Excel_Data_Export_DAU]] — SQL 学习的直接应用场景：自己跑 DAU 数据
- [[14_Weekly_Report_W12]] — 周报中提到：SQL 学习笔记准备下周分享
