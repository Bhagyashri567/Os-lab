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



#### Trigger 


CREATE TRIGGER before_stock_insert
BEFORE INSERT ON stock
FOR EACH ROW
BEGIN
    IF NEW.quantity_available < 0 THEN
        SET NEW.quantity_available = 0;
    END IF;
END;







 
