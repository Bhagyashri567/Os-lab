DELIMITER $$
CREATE PROCEDURE example_cursor_loop()
BEGIN
    DECLARE done INT DEFAULT 0;
    DECLARE v_name VARCHAR(50); 
    DECLARE cur CURSOR FOR
        SELECT item_name FROM stock;
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = 1;
    OPEN cur;
    read_loop: LOOP
        FETCH cur INTO v_name;
        IF done THEN
            LEAVE read_loop;
            END IF;
-- Process row value
SELECT CONCAT('Item: ', v_name) AS Output;
END LOOP;
CLOSE cur;
END$$
DELIMITER ;

--------------------------------------

##### FUNCTION 

SYNTAX ------

DELIMITER $$

CREATE FUNCTION function_name (
    param_name data_type,   -- Parameters
    ...
)
RETURNS return_data_type
DETERMINISTIC  -- Optional: Ensures the function gives the same result every time for the same inputs.
BEGIN
    -- SQL statements
    RETURN value;
END$$

DELIMITER ;
--------------------------------------

EXAMPLE ---- 

DELIMITER $$
CREATE FUNCTION check_reorder_status(p_stock_no INT)
RETURNS VARCHAR(3)
DETERMINISTIC
BEGIN
    DECLARE v_quantity INT;
    DECLARE v_reorder_level INT;
    DECLARE v_result VARCHAR(3);
    SELECT quantity_available, reorder_level
    INTO v_quantity, v_reorder_level
    FROM stock
    WHERE stock_no = p_stock_no;
    IF v_quantity < v_reorder_level THEN
        SET v_result = 'YES';
    ELSE
        SET v_result = 'NO';
    END IF;
    RETURN v_result;
END$$
DELIMITER ;

---------------------------------------

#### Trigger 


CREATE TRIGGER before_stock_insert
BEFORE INSERT ON stock
FOR EACH ROW
BEGIN
    IF NEW.quantity_available < 0 THEN
        SET NEW.quantity_available = 0;
    END IF;
END;

---------------------------------------

1 ))) Find employees who earn more than the average salary.

SELECT name, salary
FROM employees
WHERE salary > (
 SELECT AVG(salary)
 FROM employees );


2 )) Find employees who work in the same department as 'John'.

SELECT name, department_id
FROM employees
WHERE department_id IN (
 SELECT department_id
 FROM employees
 WHERE name = 'John' );


3 ))  Find employees whose salary is higher than the average salary of their department.

SELECT e1.name, e1.salary, e1.department_id
FROM employees e1
WHERE e1.salary > (
 SELECT AVG(e2.salary)
 FROM employees e2
 WHERE e1.department_id = e2.department_id );


4 )) Get the number of employees in each department.

SELECT department_id,
 (SELECT COUNT(*) FROM employees e2 
 WHERE e2.department_id = e1.department_id) AS 
employee_count FROM employees e1
GROUP BY department_id;

 
5 )) Find departments where the average salary is more than 50,000.

SELECT department_id, avg_salary
FROM (
 SELECT department_id, AVG(salary) AS avg_salary
 FROM employees
 GROUP BY department_id ) AS dept_avg
WHERE avg_salary > 50000;


6 )) Find employees who earn more than the average salary using JOIN.

SELECT e1.name, e1.salary FROM employees e1
JOIN (SELECT AVG(salary) AS avg_salary FROM employees) e2
ON e1.salary > e2.avg_salary;

---------------------------------------
