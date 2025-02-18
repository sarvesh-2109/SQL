# Database Design and Normalization Notes

## 1. First Normal Form (1NF)
- Ensures atomicity: Each column contains only indivisible values.
- Eliminates duplicate columns in a table.
- Uses a unique identifier (Primary Key).
- Example:
  ```sql
  CREATE TABLE Student (
      student_id INT PRIMARY KEY,
      name VARCHAR(100),
      subjects VARCHAR(100)  -- Not 1NF if multiple values are stored in one column
  );
  ```
  **To normalize:** Create a separate table for subjects.

## 2. Second Normal Form (2NF)
- Achieved by removing partial dependencies.
- A table is in 2NF if:
  1. It is in 1NF.
  2. No partial dependency exists (non-key columns must depend on the entire primary key).
- Example:
  ```sql
  CREATE TABLE Student_Subjects (
      student_id INT,
      subject VARCHAR(100),
      PRIMARY KEY (student_id, subject)
  );
  ```

## 3. Third Normal Form (3NF)
- Removes transitive dependencies.
- A table is in 3NF if:
  1. It is in 2NF.
  2. Non-key columns are not dependent on other non-key columns.
- Example:
  ```sql
  CREATE TABLE Student_Details (
      student_id INT PRIMARY KEY,
      name VARCHAR(100),
      department_id INT
  );

  CREATE TABLE Department (
      department_id INT PRIMARY KEY,
      department_name VARCHAR(100)
  );
  ```

## 4. Boyce-Codd Normal Form (BCNF)
- Stricter than 3NF: Every determinant must be a candidate key.
- Eliminates overlapping candidate keys.
- Example:
  ```sql
  CREATE TABLE Course_Allocation (
      professor_id INT,
      course_id INT,
      PRIMARY KEY (professor_id, course_id)
  );
  ```

## 5. Identifying and Resolving Update Anomalies
- Update anomalies occur when redundant data requires multiple updates.
- Normalization helps minimize them.
- Example:
  ```sql
  UPDATE Student SET department_name = 'CS' WHERE department_id = 1;
  ```
  **Solution:** Store department_name in a separate table.

## 6. Many-to-Many Relationships (Junction Table)
- Requires an intermediate table.
- Example:
  ```sql
  CREATE TABLE Student_Course (
      student_id INT,
      course_id INT,
      PRIMARY KEY (student_id, course_id),
      FOREIGN KEY (student_id) REFERENCES Student(student_id),
      FOREIGN KEY (course_id) REFERENCES Course(course_id)
  );
  ```

## 7. Denormalization for Performance
- Merges tables to reduce joins and improve query speed.
- Example:
  ```sql
  CREATE TABLE Orders_Details (
      order_id INT,
      customer_name VARCHAR(100),
      total_amount DECIMAL(10,2)
  );
  ```

## 8. Surrogate Key for Entity Identification
- Uses auto-increment ID instead of natural keys.
- Example:
  ```sql
  CREATE TABLE Employee (
      employee_id INT AUTO_INCREMENT PRIMARY KEY,
      name VARCHAR(100)
  );
  ```

## 9. Insertion Anomalies
- Occurs when certain data cannot be inserted without other data.
- Example: Cannot insert a new student without enrolling them in a course.
- **Solution:** Separate data into different tables.

## 10. Star Schema for Data Warehousing
- Central fact table connected to dimension tables.
- Example:
  ```sql
  CREATE TABLE Sales (
      sale_id INT PRIMARY KEY,
      date_id INT,
      customer_id INT,
      amount DECIMAL(10,2)
  );
  ```

## 11. Deletion Anomalies
- Occurs when deleting a record removes useful data.
- Example:
  ```sql
  DELETE FROM Course WHERE professor_id = 3;  -- Deletes professor data too!
  ```
  **Solution:** Store professors separately.

## 12. Composite Key for Multi-Attribute Relationships
- Combines multiple columns as a primary key.
- Example:
  ```sql
  CREATE TABLE Employee_Project (
      employee_id INT,
      project_id INT,
      PRIMARY KEY (employee_id, project_id)
  );
  ```

## 13. Functional Dependencies
- If `A → B`, then B is functionally dependent on A.
- Example:
  ```sql
  CREATE TABLE Employee (
      employee_id INT PRIMARY KEY,
      department_id INT
  );
  ```

## 14. Recursive Relationship for Hierarchical Data
- Stores parent-child relationships.
- Example:
  ```sql
  CREATE TABLE Employee (
      employee_id INT PRIMARY KEY,
      manager_id INT REFERENCES Employee(employee_id)
  );
  ```

## 15. Weak Entity for Dependent Data
- No primary key without an associated strong entity.
- Example:
  ```sql
  CREATE TABLE Dependent (
      dependent_id INT,
      employee_id INT,
      PRIMARY KEY (dependent_id, employee_id)
  );
  ```

## 16. Lookup Table for Categorical Data
- Stores predefined values for consistency.
- Example:
  ```sql
  CREATE TABLE Status (
      status_id INT PRIMARY KEY,
      status_name VARCHAR(50)
  );
  ```

## 17. History Table for Auditing Changes
- Tracks updates to records.
- Example:
  ```sql
  CREATE TABLE Employee_History (
      history_id INT AUTO_INCREMENT PRIMARY KEY,
      employee_id INT,
      change_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
  );
  ```

## 18. Partitioned Table for Large Datasets
- Improves performance by splitting data into partitions.
- Example:
  ```sql
  CREATE TABLE Sales_Partitioned (
      sale_id INT,
      sale_date DATE
  ) PARTITION BY RANGE (YEAR(sale_date));
  ```

## 19. Polymorphic Association for Flexible Relationships
- Allows one table to relate to multiple other tables.
- Example:
  ```sql
  CREATE TABLE Comments (
      comment_id INT PRIMARY KEY,
      entity_type VARCHAR(50),
      entity_id INT
  );
  ```

## 20. Temporal Table for Time-Based Tracking
- Stores historical records with timestamps.
- Example:
  ```sql
  CREATE TABLE Employee_Temporal (
      employee_id INT,
      start_date DATE,
      end_date DATE
  );
  ```


This Markdown file is ready to be stored in your GitHub repository. Let me know if you need any modifications! 🚀
