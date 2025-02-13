# **🔹 Types of SQL JOINs & When to Use Them**

## **1️⃣ INNER JOIN (Most Common)**
✅ **Keeps only matching records from both tables**  
✅ **Use When**: You need data that exists in **both** tables  

```sql
SELECT *
FROM TableA
INNER JOIN TableB
    ON TableA.common_column = TableB.common_column;
```

### **Example Question:**  
**Find customers who have made at least one purchase.**  
- **TableA**: `Customers (customer_id, name)`  
- **TableB**: `Orders (order_id, customer_id, order_date)`  
- **Solution**:  
```sql
SELECT c.customer_id, c.name
FROM Customers c
INNER JOIN Orders o
    ON c.customer_id = o.customer_id;
```
✅ This ensures **only customers who made a purchase** are returned.  

---

## **2️⃣ LEFT JOIN (or LEFT OUTER JOIN)**
✅ **Keeps all records from the LEFT table & matching records from the RIGHT table**  
✅ **Use When**: You want **all records from TableA**, even if they have **no match** in TableB  

```sql
SELECT *
FROM TableA
LEFT JOIN TableB
    ON TableA.common_column = TableB.common_column;
```

### **Example Question:**  
**Find customers who have never placed an order.**  
- **TableA**: `Customers (customer_id, name)`  
- **TableB**: `Orders (order_id, customer_id, order_date)`  
- **Solution**:
```sql
SELECT c.customer_id, c.name
FROM Customers c
LEFT JOIN Orders o
    ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
```
✅ This returns customers **who have no matching order records** (i.e., `o.order_id IS NULL`).  

---

## **3️⃣ RIGHT JOIN (or RIGHT OUTER JOIN)**
✅ **Keeps all records from the RIGHT table & matching records from the LEFT table**  
✅ **Use When**: You need all records from TableB, even if there’s no match in TableA  

```sql
SELECT *
FROM TableA
RIGHT JOIN TableB
    ON TableA.common_column = TableB.common_column;
```

### **Example Question:**  
**Find all orders, including ones where we don’t have a matching customer record.**  
- **TableA**: `Customers (customer_id, name)`  
- **TableB**: `Orders (order_id, customer_id, order_date)`  
- **Solution**:
```sql
SELECT o.order_id, o.order_date, c.name
FROM Customers c
RIGHT JOIN Orders o
    ON c.customer_id = o.customer_id;
```
✅ This ensures **all orders are shown, even if no customer record exists**.  

---

## **4️⃣ FULL JOIN (or FULL OUTER JOIN)**
✅ **Keeps all records from both tables, even if there’s no match**  
✅ **Use When**: You need to **combine all data** from two tables, keeping unmatched records from both  

```sql
SELECT *
FROM TableA
FULL JOIN TableB
    ON TableA.common_column = TableB.common_column;
```

### **Example Question:**  
**List all employees and projects, even if some employees have no projects and some projects have no employees.**  
- **TableA**: `Employees (employee_id, name)`  
- **TableB**: `Projects (project_id, employee_id, project_name)`  
- **Solution**:
```sql
SELECT e.employee_id, e.name, p.project_name
FROM Employees e
FULL JOIN Projects p
    ON e.employee_id = p.employee_id;
```
✅ This ensures **all employees & projects are included, even if no match exists**.  

⛔ **Note:** Not all databases support `FULL JOIN` (e.g., MySQL doesn’t). You can simulate it with `LEFT JOIN` + `UNION` + `RIGHT JOIN`.  

---

## **5️⃣ SELF JOIN**
✅ **Joins a table to itself**  
✅ **Use When**: You need to **compare rows within the same table**  

```sql
SELECT a.*, b.*
FROM TableA a
JOIN TableA b
    ON a.some_column = b.some_column;
```

### **Example Question:**  
**Find employees who have the same manager.**  
- **Table**: `Employees (id, name, manager_id)`  
- **Solution**:
```sql
SELECT e1.name AS employee, e2.name AS manager
FROM Employees e1
JOIN Employees e2
    ON e1.manager_id = e2.id;
```
✅ This allows us to match an **employee’s manager to another employee’s ID**.  

---

## **6️⃣ CROSS JOIN (Cartesian Join)**
✅ **Matches every row of TableA with every row of TableB**  
✅ **Use When**: You need **all possible combinations** of two tables  

```sql
SELECT *
FROM TableA
CROSS JOIN TableB;
```

### **Example Question:**  
**Find all possible combinations of products and stores.**  
- **TableA**: `Products (product_id, product_name)`  
- **TableB**: `Stores (store_id, store_name)`  
- **Solution**:
```sql
SELECT p.product_name, s.store_name
FROM Products p
CROSS JOIN Stores s;
```
✅ This **generates every product-store combination**.  

---

# **📝 Summary Table: When to Use Which JOIN?**
| JOIN Type       | Keeps Non-Matching Rows? | Use Case |
|---------------|--------------------|---------|
| **INNER JOIN** | ❌ No  | Data that exists in **both** tables |
| **LEFT JOIN** | ✅ Yes (Left table) | Data from **Left table**, even if no match in Right table |
| **RIGHT JOIN** | ✅ Yes (Right table) | Data from **Right table**, even if no match in Left table |
| **FULL JOIN** | ✅ Yes (Both) | Combines all data from **both** tables |
| **SELF JOIN** | N/A | Compare rows within the **same table** |
| **CROSS JOIN** | ✅ Yes (All combinations) | Generate all possible pairings |

---

# **🎯 How to Approach JOIN Questions**
1️⃣ **Identify the main table**:  
   - What is the **primary table** you need to base the query on?  
   - E.g., in "customers who never made a purchase," the main table is `Customers`.  

2️⃣ **Decide how the second table connects**:  
   - Is every row expected to match? (**INNER JOIN**)  
   - Do we need unmatched rows? (**LEFT JOIN** / **FULL JOIN**)  

3️⃣ **Write the JOIN condition carefully**:  
   - Ensure you’re **joining on the correct column**.  
   - If dealing with **dates**, use `DATE_ADD()` or `DATEDIFF()`.  
   - If using **IDs**, ensure they are correctly linked.  

4️⃣ **Filter results with `WHERE` (if needed)**:  
   - Use `WHERE column IS NULL` to find missing data (e.g., customers with no transactions).  
   - Use `WHERE column > value` to filter specific conditions.  

---

# **🚀 Practice Makes Perfect!**
Want to test your JOIN skills? Try these LeetCode SQL questions:
1️⃣ **Customer Who Visited but Did Not Make Any Transactions** (LEFT JOIN)  
2️⃣ **Rising Temperature** (SELF JOIN with date comparison)  
3️⃣ **Employee Bonus** (LEFT JOIN with NULL filtering)  
4️⃣ **Department Highest Salary** (INNER JOIN with subquery)  
