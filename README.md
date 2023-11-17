
WITH RankedEmployees AS (
    SELECT
        departments.department_id,
        departments.department_name,
        employees.employee_id,
        ROW_NUMBER() OVER (PARTITION BY departments.department_id ORDER BY employees.employee_id) AS employee_rank
    FROM departments
    LEFT JOIN employees ON departments.department_id = employees.department_id
)
SELECT
    department_id,
    department_name,
    employee_id
FROM RankedEmployees
WHERE employee_rank IN (1, 2);
