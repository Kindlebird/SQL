# HR Analytics with SQL
HR Analytics with SQL

Collection of **SQL queries for HR analytics**: turnover, terminations, hires, age, seniority, contracts, headcount (HC), and more.

---

## Table of Contents

- [Staff Turnover](#staff-turnover)
- [Number of hires](#number-of-hires)
- [Ages](#ages)
- [Seniority in the company](#seniority-in-the-company)
- [Contracts](#contracts)
- [Headcount (HC)](#headcount-hc)
- [Update table data](#update-table-data)
- [Working hours reduction](#working-hours-reduction)

---

## Staff Turnover

### Number of terminations

```sql
SELECT
  Company,
  Termination_reason,
  Department,
  COUNT(*)
FROM employees2025.employees0225
WHERE Termination_date != ''
GROUP BY Company, Termination_reason, Department;
```

---

### Termination types

### Create `Termination_type` column

```sql
ALTER TABLE employees2025.employees0125
ADD COLUMN Termination_type VARCHAR(50);
```

### Classify as Voluntary Termination (VT)

```sql
UPDATE employees2025.employees0125
SET Termination_type = 'VT'
WHERE Termination_reason IN (
  'Voluntary or mandatory leave of absence',
  'Resignation',
  'Probation termination at employee request',
  'Childcare leave',
  'Voluntary termination'
);
```

### Classify as Involuntary Termination (IT)

```sql
UPDATE employees2025.employees0125
SET Termination_type = 'IT'
WHERE Termination_reason IN (
  'Disciplinary dismissal (individual)',
  'Dismissal for objective business reasons',
  'End of temporary or fixed-term contract',
  'Failed probation period',
  'Probation termination at employer request',
  'Involuntary termination',
  'Death',
  'Retirement',
  'Long-term sick leave exhaustion',
  'Employee dismissal'
);
```

### Terminations count by type

```sql
SELECT
  Company,
  Termination_type,
  Department,
  COUNT(*)
FROM employees2025.employees0125
WHERE Termination_type IN ('IT', 'VT')
GROUP BY Company, Termination_type, Department;
```

### Cumulative losses by type

```sql
SELECT
    Company,
    Department,
    EmployeeId,
    TerminationDate,
    TerminationType,
    COUNT(*) AS TotalEmployees
FROM (
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2024.employees_0124
    WHERE TerminationType IN ('BI', 'BV')

    UNION ALL
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2024.employees_0224
    WHERE TerminationType IN ('BI', 'BV')

    UNION ALL
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2024.employees_0324
    WHERE TerminationType IN ('BI', 'BV')

    UNION ALL
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2024.employees_0424
    WHERE TerminationType IN ('BI', 'BV')

    UNION ALL
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2024.employees_0524
    WHERE TerminationType IN ('BI', 'BV')

    UNION ALL
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2024.employees_0624
    WHERE TerminationType IN ('BI', 'BV')

    UNION ALL
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2024.employees_0724
    WHERE TerminationType IN ('BI', 'BV')

    UNION ALL
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2024.employees_0824
    WHERE TerminationType IN ('BI', 'BV')

    UNION ALL
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2024.employees_0924
    WHERE TerminationType IN ('BI', 'BV')

    UNION ALL
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2024.employees_1024
    WHERE TerminationType IN ('BI', 'BV')

    UNION ALL
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2024.employees_1124
    WHERE TerminationType IN ('BI', 'BV')

    UNION ALL
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2024.employees_1224
    WHERE TerminationType IN ('BI', 'BV')

    UNION ALL
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2025.employees_0125
    WHERE TerminationType IN ('BI', 'BV')

    UNION ALL
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2025.employees_0225
    WHERE TerminationType IN ('BI', 'BV')

    UNION ALL
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2025.employees_0325
    WHERE TerminationType IN ('BI', 'BV')

    UNION ALL
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2025.employees_0425
    WHERE TerminationType IN ('BI', 'BV')

    UNION ALL
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2025.employees_0525
    WHERE TerminationType IN ('BI', 'BV')

    UNION ALL
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2025.employees_0625
    WHERE TerminationType IN ('BI', 'BV')

    UNION ALL
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2025.employees_0725
    WHERE TerminationType IN ('BI', 'BV')

    UNION ALL
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2025.employees_0825
    WHERE TerminationType IN ('BI', 'BV')

    UNION ALL
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2025.employees_0925
    WHERE TerminationType IN ('BI', 'BV')

    UNION ALL
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2025.employees_1025
    WHERE TerminationType IN ('BI', 'BV')

    UNION ALL
    SELECT Company, Department, EmployeeId, TerminationType, TerminationDate
    FROM employees_2025.employees_1125
    WHERE TerminationType IN ('BI', 'BV')
) termination_data
GROUP BY
    Company,
    Department,
    EmployeeId,
    TerminationType,
    TerminationDate
ORDER BY
    TerminationDate,
    EmployeeId,
    Company,
    Department,
    TerminationType;
```

---

## Number of hires

### Hires in a specific month

```sql
SELECT
  Company,
  Department,
  COUNT(*)
FROM employees2025.employees0725
WHERE MONTH(Hire_date) = 7
  AND YEAR(Hire_date) = 2025
GROUP BY Company, Department;
```

### Hires within a specific date

```sql
SELECT
  Company,
  Department,
  Employee_id,
  Hire_date,
  COUNT(*)
FROM employees2025.employees1125
WHERE Hire_date >= '2024-01-01'
  AND Hire_date < '2025-12-01'
GROUP BY Company, Department, Employee_id, Hire_date;
```

---

## Seniority in the company

```sql
SELECT
  Company,
  Department,
  Employee_name,
  ROUND(DATEDIFF(NOW(), Hire_date) / 365) AS Seniority
FROM employees2026.employees0226;
```

### Seniority by groups

```sql
-- Add a new column to store employee tenure groups
ALTER TABLE employees_2026.employees_0226
ADD COLUMN TenureGroup VARCHAR(50);

-- Assign tenure group: 0–1 years
UPDATE employees_2026.employees_0226
SET TenureGroup = '0_1'
WHERE DATEDIFF(NOW(), HireDate) / 365 >= 0
  AND DATEDIFF(NOW(), HireDate) / 365 <= 1;

-- Assign tenure group: 1–3 years
UPDATE employees_2026.employees_0226
SET TenureGroup = '1_3'
WHERE DATEDIFF(NOW(), HireDate) / 365 > 1
  AND DATEDIFF(NOW(), HireDate) / 365 <= 3;

-- Assign tenure group: 3–5 years
UPDATE employees_2026.employees_0226
SET TenureGroup = '3_5'
WHERE DATEDIFF(NOW(), HireDate) / 365 > 3
  AND DATEDIFF(NOW(), HireDate) / 365 <= 5;

-- Assign tenure group: 5–10 years
UPDATE employees_2026.employees_0226
SET TenureGroup = '5_10'
WHERE DATEDIFF(NOW(), HireDate) / 365 > 5
  AND DATEDIFF(NOW(), HireDate) / 365 <= 10;

-- Assign tenure group: 10–20 years
UPDATE employees_2026.employees_0226
SET TenureGroup = '10_20'
WHERE DATEDIFF(NOW(), HireDate) / 365 > 10
  AND DATEDIFF(NOW(), HireDate) / 365 <= 20;

-- Assign tenure group: 20+ years
UPDATE employees_2026.employees_0226
SET TenureGroup = '20_100'
WHERE DATEDIFF(NOW(), HireDate) / 365 > 20;
``
```

---

## Contracts

### Temporary contracts

```sql
SELECT
  Company,
  Department,
  COUNT(*)
FROM employees2026.employees0226
WHERE Contract_code IN ('402','410','300','999','502','420','452','500','520','540')
GROUP BY Company, Department;
```

### Number of contracts

```sql
SELECT
    Company,
    COUNT(*) AS TotalEmployees
FROM employees_2025.employees_0725
GROUP BY Company;
```

---

## Ages

```sql
SELECT
  Birth_date,
  ROUND(DATEDIFF(NOW(), Birth_date) / 365) AS Age
FROM employees2025.employees0125;
```

```sql
-- Calculate employee age in years based on birth date
SELECT
    DATEDIFF(NOW(), BirthDate) / 365 AS AgeInYears
FROM employees_2025.employees_0725
WHERE EmployeeId = '(NIF number)';
```

## Create an ages column

```sql
 Add a new column to store employee age
ALTER TABLE employees_2025.employees_0725
ADD COLUMN Age VARCHAR(50);

-- Calculate age in years based on birth date
UPDATE employees_2025.employees_0725
SET Age = DATEDIFF(NOW(), BirthDate) / 365;

-- Round age to the nearest whole number
UPDATE employees_2025.employees_0725
SET Age = ROUND(Age);

-- Count employees by company, department, age, and age group
SELECT
    Company,
    Department,
    Age,
    AgeGroup,
    COUNT(*) AS TotalEmployees
FROM employees_2025.employees_0725
GROUP BY
    Company,
    Department,
    Age,
    AgeGroup;
```

## Group ages

```sql
-- Add a new column to store employee age groups
ALTER TABLE employees_2025.employees_0725
ADD COLUMN AgeGroup VARCHAR(50);

-- Assign age group: 18–30
UPDATE employees_2025.employees_0725
SET AgeGroup = '18_30'
WHERE DATEDIFF(NOW(), BirthDate) / 365 <= 30;

-- Assign age group: 31–40
UPDATE employees_2025.employees_0725
SET AgeGroup = '31_40'
WHERE DATEDIFF(NOW(), BirthDate) / 365 > 30
  AND DATEDIFF(NOW(), BirthDate) / 365 <= 40;

-- Assign age group: 41–50
UPDATE employees_2025.employees_0725
SET AgeGroup = '41_50'
WHERE DATEDIFF(NOW(), BirthDate) / 365 > 40
  AND DATEDIFF(NOW(), BirthDate) / 365 <= 50;

-- Assign age group: 51–60
UPDATE employees_2025.employees_0725
SET AgeGroup = '51_60'
WHERE DATEDIFF(NOW(), BirthDate) / 365 > 50
  AND DATEDIFF(NOW(), BirthDate) / 365 <= 60;

-- Assign age group: 61+
UPDATE employees_2025.employees_0725
SET AgeGroup = '61_100'
WHERE DATEDIFF(NOW(), BirthDate) / 365 > 60;

-- Count employees by company, department, and age group
SELECT
    Company,
    Department,
    AgeGroup,
    COUNT(*) AS TotalEmployees
FROM employees_2025.employees_0725
GROUP BY
    Company,
    Department,
    AgeGroup;
```

## Convert dates into a readable format

```sql
UPDATE employees_2025.employees_0125
SET BirthDate = REPLACE(BirthDate, '/', '-');

-- Create or update a new column called BirthDate_parsed
UPDATE employees_2025.employees_0125
SET BirthDate_parsed = STR_TO_DATE(BirthDate, '%d-%m-%Y');

SELECT
    BirthDate_parsed,
    DATEDIFF(NOW(), BirthDate_parsed) / 365 AS AgeInYears
FROM employees_2025.employees_0125;
```

---

## Headcount (HC)

```sql
SELECT
  Company,
  Employee_id,
  Employee_name,
  Birth_date,
  Hire_date,
  Termination_date,
  Termination_reason,
  Gender,
  Contract_code,
  Department
FROM employees2025.employees0725
WHERE Company = '3'
  AND Department = '(department name)';
```

### Add one-month terminations and hires

```sql
SELECT
    Company,
    EmployeeId,
    EmployeeName,
    BirthDate,
    HireDate,
    TerminationDate,
    TerminationReason,
    Gender,
    ContractCode,
    Department
FROM employees_2025.employees_0225
WHERE Company = '3'
  AND Department = 'COM TIENDA'
  AND (
        TerminationReason <> ''
        OR (
            MONTH(HireDate) = 2
            AND YEAR(HireDate) = '2025'
        )
      );
```

### Accumulated head count over several months

```sql
SELECT
    Company,
    Department,
    SnapshotDate,
    COUNT(*) AS TotalEmployees
FROM (
    SELECT Company, Department, DATE('2024-01-01') AS SnapshotDate
    FROM employees_2024.employees_0124

    UNION ALL
    SELECT Company, Department, DATE('2024-02-01') AS SnapshotDate
    FROM employees_2024.employees_0224

    UNION ALL
    SELECT Company, Department, DATE('2024-03-01') AS SnapshotDate
    FROM employees_2024.employees_0324

    UNION ALL
    SELECT Company, Department, DATE('2024-04-01') AS SnapshotDate
    FROM employees_2024.employees_0424

    UNION ALL
    SELECT Company, Department, DATE('2024-05-01') AS SnapshotDate
    FROM employees_2024.employees_0524

    UNION ALL
    SELECT Company, Department, DATE('2024-06-01') AS SnapshotDate
    FROM employees_2024.employees_0624

    UNION ALL
    SELECT Company, Department, DATE('2024-07-01') AS SnapshotDate
    FROM employees_2024.employees_0724

    UNION ALL
    SELECT Company, Department, DATE('2024-08-01') AS SnapshotDate
    FROM employees_2024.employees_0824

    UNION ALL
    SELECT Company, Department, DATE('2024-09-01') AS SnapshotDate
    FROM employees_2024.employees_0924

    UNION ALL
    SELECT Company, Department, DATE('2024-10-01') AS SnapshotDate
    FROM employees_2024.employees_1024

    UNION ALL
    SELECT Company, Department, DATE('2024-11-01') AS SnapshotDate
    FROM employees_2024.employees_1124

    UNION ALL
    SELECT Company, Department, DATE('2024-12-01') AS SnapshotDate
    FROM employees_2024.employees_1224

    UNION ALL
    SELECT Company, Department, DATE('2025-01-01') AS SnapshotDate
    FROM employees_2025.employees_0125

    UNION ALL
    SELECT Company, Department, DATE('2025-02-01') AS SnapshotDate
    FROM employees_2025.employees_0225

    UNION ALL
    SELECT Company, Department, DATE('2025-03-01') AS SnapshotDate
    FROM employees_2025.employees_0325

    UNION ALL
    SELECT Company, Department, DATE('2025-04-01') AS SnapshotDate
    FROM employees_2025.employees_0425

    UNION ALL
    SELECT Company, Department, DATE('2025-05-01') AS SnapshotDate
    FROM employees_2025.employees_0525

    UNION ALL
    SELECT Company, Department, DATE('2025-06-01') AS SnapshotDate
    FROM employees_2025.employees_0625

    UNION ALL
    SELECT Company, Department, DATE('2025-07-01') AS SnapshotDate
    FROM employees_2025.employees_0725

    UNION ALL
    SELECT Company, Department, DATE('2025-08-01') AS SnapshotDate
    FROM employees_2025.employees_0825

    UNION ALL
    SELECT Company, Department, DATE('2025-09-01') AS SnapshotDate
    FROM employees_2025.employees_0925

    UNION ALL
    SELECT Company, Department, DATE('2025-10-01') AS SnapshotDate
    FROM employees_2025.employees_1025

    UNION ALL
    SELECT Company, Department, DATE('2025-11-01') AS SnapshotDate
    FROM employees_2025.employees_1125
) snapshot_data
GROUP BY
    Company,
    Department,
    SnapshotDate
ORDER BY
    SnapshotDate,
    Company,
    Department;
```

---

## Update table data

### Delete table contents

```sql
DELETE FROM employees2024.employees0724;
```

### Drop table

```sql
DROP TABLE employees2024.employees0724;
```

### Update table contents

```sql
UPDATE employees_2025.employees_0725
SET Company = '24'
WHERE Company IN ('1906', '1909', '1910', '1911', '1912', '1916');
```

---

## Working hours reduction

```sql
SELECT
  Company,
  Department,
  Employee_id,
  Gender,
  Working_hours_percentage,
  COUNT(*)
FROM employees2024.employees0124
WHERE Working_hours_percentage != '100'
GROUP BY Company, Department, Gender, Employee_id, Working_hours_percentage;
```



