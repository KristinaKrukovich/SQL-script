# SQL script
## Description
To create a database that supports the management of information related to coffee shop employees, locations, suppliers, and the types of coffee offered at each establishment.
## Relationships Between Tables

### `employees` → `shops`
This relationship connects employees to coffee shops. Employees are associated with one coffee shop (`FK coffeeshop_id` in `employees` references `coffeeshop_id` in `shops`). If a coffee shop is deleted, the `coffeeshop_id` in `employees` will be set to `NULL`.

### `shops` → `locations`
This relationship connects coffee shops to locations. Coffee shops are associated with locations (`FK city_id` in `shops` references `city_id` in `locations`). If a location is deleted, the `city_id` in `shops` will be set to `NULL`.

### `suppliers` → `shops`
This relationship connects suppliers to coffee shops. Suppliers are associated with coffee shops (`FK coffeeshop_id` in `suppliers` references `coffeeshop_id` in `shops`). If a coffee shop is deleted, the related records in `suppliers` will also be deleted (ON DELETE CASCADE).

## Create and Insert
## SQL Script

The following SQL script creates the necessary tables and populates them with initial data.

```sql
-- DROP TABLES

DROP TABLE IF EXISTS employees CASCADE;
DROP TABLE IF EXISTS shops CASCADE;
DROP TABLE IF EXISTS locations CASCADE;
DROP TABLE IF EXISTS suppliers CASCADE;

-- CREATE TABLES ========================================

-- Create locations table
CREATE TABLE locations (
    city_id INT PRIMARY KEY,
    city VARCHAR(50),
    country VARCHAR(50)
);

-- Create shops table
CREATE TABLE shops (
    coffeeshop_id INT PRIMARY KEY,
    coffeeshop_name VARCHAR(50),
    city_id INT
);

-- Add foreign key to shops table
ALTER TABLE shops
ADD FOREIGN KEY (city_id)
REFERENCES locations(city_id)
ON DELETE SET NULL;

-- Create employees table 
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(50),
    hire_date DATE,
    gender VARCHAR(1), -- "M"/"F" (male/female)
    salary INT,
    coffeeshop_id INT
);

-- Add foreign key to the employees table
ALTER TABLE employees
ADD FOREIGN KEY (coffeeshop_id)
REFERENCES shops(coffeeshop_id)
ON DELETE SET NULL;

-- Create suppliers table
CREATE TABLE suppliers (
    coffeeshop_id INT,
    supplier_name VARCHAR(40),
    coffee_type VARCHAR(20),
    PRIMARY KEY (coffeeshop_id, supplier_name),
    FOREIGN KEY (coffeeshop_id) REFERENCES shops(coffeeshop_id)
    ON DELETE CASCADE
);

-- INSERT INTO TABLES ====================================

-- Insert into the locations table
INSERT INTO locations VALUES(1, 'Los Angeles', 'United States');
INSERT INTO locations VALUES(2, 'New York', 'United States');
INSERT INTO locations VALUES(3, 'London', 'United Kingdom');

-- Insert into the shops table
INSERT INTO shops VALUES(1, 'Common Grounds', NULL);
INSERT INTO shops VALUES(2, 'Early Rise', 2);
INSERT INTO shops VALUES(3, 'Ancient Bean', 3);
INSERT INTO shops VALUES(4, 'Urban Grind', 1);
INSERT INTO shops VALUES(5, 'Trembling Cup', 2);

-- Now set the city_id in shops for coffeeshop_id = 1 to 1
UPDATE shops SET city_id = 1 WHERE coffeeshop_id = 1;

-- Insert first entries into employees table
INSERT INTO employees VALUES (501559, 'Carson', 'Mosconi', 'cmosconi0@census.gov', '2015-08-29', 'M', 32973, 1);
INSERT INTO employees VALUES (144108, 'Khalil', 'Corr', 'kcorr1@github.io', '2014-04-23', 'M', 52802, 1);

-- Insert into suppliers table
INSERT INTO suppliers VALUES(1, 'Beans and Barley', 'Arabica');
INSERT INTO suppliers VALUES(1, 'Cool Beans', 'Robusta');
INSERT INTO suppliers VALUES(2, 'Vanilla Bean', 'Liberica');
INSERT INTO suppliers VALUES(2, 'Beans and Barley', 'Arabica');
INSERT INTO suppliers VALUES(2, 'Cool Beans', 'Robusta');
INSERT INTO suppliers VALUES(3, 'Bean Me Up', 'Excelsa');
INSERT INTO suppliers VALUES(3, 'Vanilla Bean', 'Liberica');
INSERT INTO suppliers VALUES(3, 'Cool Beans', 'Robusta');
INSERT INTO suppliers VALUES(3, 'Beans and Barley', 'Arabica');
INSERT INTO suppliers VALUES(4, 'Vanilla Bean', 'Liberica');
INSERT INTO suppliers VALUES(4, 'Bean Me Up', 'Excelsa');
INSERT INTO suppliers VALUES(5, 'Beans and Barley', 'Arabica');
INSERT INTO suppliers VALUES(5, 'Vanilla Bean', 'Liberica');
INSERT INTO suppliers VALUES(5, 'Bean Me Up', 'Excelsa');
