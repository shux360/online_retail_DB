# online_retail_DB

CREATE DATABASE online_retail_store_management_system;
USE online_retail_store_management_system;
-- drop database online_retail_store_management_system;

CREATE TABLE delivery_Man(
Delivery_Man_ID int not null,
First_Name varchar(15) not null,
Last_Name varchar(15) not null,
Phone varchar(10),
primary key(Delivery_Man_ID)
);

CREATE TABLE customer(
Customer_ID int not null,
First_Name varchar(15) not null,
Last_Name varchar(15) not null,
DOB date,
Deliver_Man_ID int not null,
primary key(Customer_ID),
CONSTRAINT fk_deliver foreign key(Deliver_Man_ID) REFERENCES delivery_Man(Delivery_Man_ID) on delete cascade
);

CREATE TABLE order_details(
Order_ID int not null,
Order_Date date not null,
Total_Amount float,
CustomerID int not null,
DeliverMan_ID int not null,
Stats varchar(15),
primary key(Order_ID),
CONSTRAINT fk_deliver1 foreign key(DeliverMan_ID) REFERENCES delivery_Man(Delivery_Man_ID) on delete cascade,
CONSTRAINT fk_customer foreign key(CustomerID) REFERENCES customer(Customer_ID) on delete cascade
);

CREATE TABLE customer_email(
Email varchar(30) not null,
City varchar(15),
District varchar(15),
Street_Name varchar(15),
Phone varchar(10),
primary key(Email)
);

CREATE TABLE shipping_address(
CustomerID int not null,
AddressCode varchar(10) not null,
City varchar(15),
State varchar(15),
ZipCode varchar(10),
Street varchar(15),
primary key(CustomerID),
CONSTRAINT fk_customer1 foreign key(CustomerID) REFERENCES customer(Customer_ID) on delete cascade
);

CREATE TABLE payment(
Payment_ID int not null,
CustomerID int not null,
OrderID int not null,
Amount int,
Payment_Date date,
Payment_Method varchar(20),
primary key(Payment_ID),
CONSTRAINT fk_order foreign key(OrderID) REFERENCES order_details(Order_ID) on delete cascade,
CONSTRAINT fk_customer2 foreign key(CustomerID) REFERENCES customer(Customer_ID) on delete cascade
);

CREATE TABLE warehouse(
Warehouse_ID int not null,
Payment_Date date,
Location varchar(10),
Capacity int,
primary key(Warehouse_ID)
);

CREATE TABLE inventory(
Inventory_ID int not null,
WarehouseID int not null,
Stock_Quantity int,
Last_Stock_Update int,
primary key(Inventory_ID),
CONSTRAINT fk_whouse foreign key(WarehouseID) REFERENCES warehouse(Warehouse_ID) on delete cascade
);


CREATE TABLE product(
Product_ID int not null,
InventoryID int not null,
Stock_Quantity int,
Price int,
Product_Name varchar(20),
Product_Description varchar(100),
primary key(Product_ID),
CONSTRAINT fk_invent foreign key(InventoryID) REFERENCES inventory(Inventory_ID) on delete cascade
);

CREATE TABLE order_item(
Order_Item_ID int not null,
ProductID int not null,
OrderID int not null,
Quantity int,
Price int,
primary key(Order_Item_ID),
CONSTRAINT fk_product foreign key(ProductID) REFERENCES product(Product_ID) on delete cascade,
CONSTRAINT fk_order1 foreign key(OrderID) REFERENCES order_details(Order_ID) on delete cascade
);

CREATE TABLE supplier(
Supplier_ID int not null,
Supplier_Name varchar(20),
Contact_Person varchar(20),
Phone_NO int,
primary key(Supplier_ID)
);

CREATE TABLE product_supplier(
ProductID int not null,
SupplierID int not null,
primary key(ProductID,SupplierID),
CONSTRAINT fk_product1 foreign key(ProductID) REFERENCES product(Product_ID) on delete cascade,
CONSTRAINT fk_supplier foreign key(SupplierID) REFERENCES supplier(Supplier_ID) on delete cascade
);

CREATE TABLE product_warehouse(
ProductID int not null,
WarehouseID int not null,
primary key(ProductID,WarehouseID),
CONSTRAINT fk_product2 foreign key(ProductID) REFERENCES product(Product_ID) on delete cascade,
CONSTRAINT fk_whouse1 foreign key(WarehouseID) REFERENCES warehouse(Warehouse_ID) on delete cascade
);

CREATE TABLE employee(
Employee_ID int not null,
First_Name varchar(15) not null,
Last_Name varchar(15) not null,
DOB date,
Manager_ID int not null,
primary key(Employee_ID),
CONSTRAINT fk_manage foreign key(Manager_ID) REFERENCES employee(Employee_ID) on delete cascade
);

CREATE TABLE product_features(
ProductID int not null,
Product_Name varchar(20),
Product_Value varchar(15),
Product_Color varchar(15),
Product_Size varchar(10),
primary key(ProductID),
CONSTRAINT fk_customer3 foreign key(ProductID) REFERENCES product(Product_ID) on delete cascade
);

CREATE TABLE employee_email(
Email varchar(30) not null,
City varchar(15),
District varchar(15),
Street_Name varchar(15),
Phone varchar(10),
primary key(Email)
);

CREATE TABLE employee_inventory(
EmployeeID int not null,
InventoryID int not null,
primary key(EmployeeID,InventoryID),
CONSTRAINT fk_employee foreign key(EmployeeID) REFERENCES employee(Employee_ID) on delete cascade,
CONSTRAINT fk_inven foreign key(InventoryID) REFERENCES inventory(Inventory_ID) on delete cascade
);

-- Delivery_Man table
INSERT INTO delivery_Man (Delivery_Man_ID, First_Name, Last_Name, Phone) VALUES
(1, 'Saman', 'Perera', '0771234567'),
(2, 'Kamal', 'Fernando', '0712345678'),
(3, 'Nimal', 'Fernando', '0763456789'),
(4, 'Sunil', 'Ratnayake', '0784567890'),
(5, 'Ranil', 'Bandara', '0725678901'),
(6, 'Chaminda', 'Jayawardena', '0756789012');

-- Customer table
INSERT INTO customer (Customer_ID, First_Name, Last_Name, DOB, Deliver_Man_ID) VALUES
	(1, 'Nishantha', 'Perera', '1990-05-15', 1),
	(2, 'Saman', 'Silva', '1985-08-20', 1),
	(3, 'Kamal', 'Fernando', '1982-11-10', 2),
	(4, 'Nimal', 'Ratnayake', '1978-03-25', 3),
	(5, 'Sunil', 'Bandara', '1995-07-12', 4),
	(6, 'Ranil', 'Jayawardena', '1988-01-30', 5);

-- Order_details table
INSERT INTO order_details (Order_ID, Order_Date, Total_Amount, CustomerID, DeliverMan_ID, Stats) VALUES
(1, '2024-03-01', 2500.50, 1, 1, 'delivered'),
(2, '2024-03-05', 1500.25, 2, 1, 'pending'),
(3, '2024-03-10', 3200.75, 3, 2, 'delivered'),
(4, '2024-03-15', 1800.30, 4, 3, 'delivered'),
(5, '2024-03-20', 2000.00, 5, 4, 'pending'),
(6, '2024-03-25', 2800.80, 6, 5, 'delivered');

-- Customer_email table
INSERT INTO customer_email (Email, City, District, Street_Name, Phone) VALUES
('nishantha@gmail.com', 'Colombo', 'Colombo', 'Galle Road', '0711112222'),
('saman@gmail.com', 'Kandy', 'Kandy', 'Peradeniya Road', '0772223333'),
('kamal@gmail.com', 'Galle', 'Galle', 'Fort Road', '0763334444'),
('nimal@gmail.com', 'Jaffna', 'Jaffna', 'Main Street', '0754445555'),
('sunil@gmail.com', 'Matara', 'Matara', 'Beach Road', '0725556666'),
('ranil@gmail.com', 'Kurunegala', 'Kurunegala', 'Station Road', '0786667777');

-- Shipping_address table
INSERT INTO shipping_address (CustomerID, AddressCode, City, State, ZipCode, Street) VALUES
(1, '00100', 'Colombo', 'Western', '00100', 'Galle Road'),
(2, '20000', 'Kandy', 'Central', '20000', 'Peradeniya Road'),
(3, '80000', 'Galle', 'Southern', '80000', 'Fort Road'),
(4, '40000', 'Jaffna', 'Northern', '40000', 'Main Street'),
(5, '90000', 'Matara', 'Southern', '90000', 'Beach Road'),
(6, '60000', 'Kurunegala', 'North Western', '60000', 'Station Road');

-- Payment table
INSERT INTO payment (Payment_ID, CustomerID, OrderID, Amount, Payment_Date, Payment_Method) VALUES
(1, 1, 1, 2500, '2024-03-02', 'Cash'),
(2, 1, 6, 1500, '2024-03-06', 'Credit Card'),
(3, 3, 3, 3200, '2024-03-11', 'Cash'),
(4, 2, 6, 1800, '2024-03-16', 'Credit Card'),
(5, 5, 5, 2000, '2024-03-21', 'Cash'),
(6, 6, 6, 2800, '2024-03-26', 'Credit Card');

-- Warehouse table
INSERT INTO warehouse (Warehouse_ID, Payment_Date, Location, Capacity) VALUES
(1, '2024-03-01', 'Colombo', 5000),
(2, '2024-03-05', 'Kandy', 3000),
(3, '2024-03-10', 'Galle', 4000),
(4, '2024-03-15', 'Jaffna', 2000),
(5, '2024-03-20', 'Matara', 3500),
(6, '2024-03-25', 'Kurunegala', 2500);

-- Inventory table
-- edited
INSERT INTO inventory (Inventory_ID, WarehouseID, Stock_Quantity, Last_Stock_Update) VALUES
(1, 1, 1000, UNIX_TIMESTAMP('2024-03-01')),
(2, 1, 800, UNIX_TIMESTAMP('2024-03-05')),
(3, 3, 1200, UNIX_TIMESTAMP('2024-03-10')),
(4, 2, 600, UNIX_TIMESTAMP('2024-03-15')),
(5, 5, 900, UNIX_TIMESTAMP('2024-03-20')),
(6, 6, 1100, UNIX_TIMESTAMP('2024-03-25'));

-- Product table
INSERT INTO product (Product_ID, InventoryID, Stock_Quantity, Price, Product_Name, Product_Description) VALUES
(1, 1, 1000, 150, 'Rice', 'High-quality rice from local farms'),
(2, 2, 800, 250, 'Tea', 'Premium Ceylon tea'),
(3, 1, 1200, 300, 'Spices', 'Assorted spices for cooking'),
(4, 4, 600, 200, 'Coconut', 'Organic coconut oil'),
(5, 2, 900, 180, 'Apparel', 'Traditional Sri Lankan clothing'),
(6, 6, 1100, 350, 'Handicraft', 'Handmade crafts from local artisans');

-- Order_item table
INSERT INTO order_item (Order_Item_ID, ProductID, OrderID, Quantity, Price) VALUES
(1, 1, 1, 2, 300),
(2, 2, 2, 3, 750),
(3, 3, 3, 1, 300),
(4, 1, 2, 2, 300), 
(5, 2, 3, 4, 720), 
(6, 6, 6, 1, 350);

-- Supplier table
INSERT INTO supplier (Supplier_ID, Supplier_Name, Contact_Person, Phone_NO) VALUES
(1, 'ABC Suppliers', 'John Doe', '0771112222'),
(2, 'XYZ Suppliers', 'Jane Smith', '0762223333'),
(3, 'PQR Suppliers', 'David Brown', '0753334444'),
(4, 'LMN Suppliers', 'Emma Johnson', '0784445555'),
(5, 'DEF Suppliers', 'Michael Williams', '0715556666'),
(6, 'GHI Suppliers', 'Sophia Brown', '0726667777');

-- Product_supplier table
INSERT INTO product_supplier (ProductID, SupplierID) VALUES
(1, 1),
(2, 3),
(3, 3),
(4, 4),
(5, 1),
(6, 6);

-- Product_warehouse table
INSERT INTO product_warehouse (ProductID, WarehouseID) VALUES
(1, 1),
(2, 1),
(3, 1),
(4, 2),
(5, 2),
(6, 2);

-- Employee table
INSERT INTO employee (Employee_ID, First_Name, Last_Name, DOB, Manager_ID) VALUES
(1, 'Kasun', 'Perera', '1980-05-15',1),
(2, 'Nimal', 'Silva', '1975-08-20', 1),
(3, 'Sunil', 'Fernando', '1972-11-10', 2),
(4, 'Ranil', 'Ratnayake', '1968-03-25', 2),
(5, 'Chaminda', 'Bandara', '1985-07-12', 1),
(6, 'Kamal', 'Jayawardena', '1978-01-30', 1);

-- Product_features table
INSERT INTO product_features (ProductID, Product_Name, Product_Value, Product_Color, Product_Size) VALUES
(1, 'Rice', 'Premium', 'White', '5kg'),
(2, 'Tea', 'Premium', 'Black', '200g'),
(3, 'Spices', 'Assorted', 'Various', NULL),
(4, 'Coconut', 'Organic', 'Brown', NULL),
(5, 'Apparel', 'Traditional', 'Various', NULL),
(6, 'Handicraft', 'Artisanal', 'Various', NULL);

-- Employee_email table
INSERT INTO employee_email (Email, City, District, Street_Name, Phone) VALUES
('kasun@example.com', 'Colombo', 'Colombo', 'Galle Road', '0711112222'),
('nimal@example.com', 'Kandy', 'Kandy', 'Peradeniya Road', '0772223333'),
('sunil@example.com', 'Galle', 'Galle', 'Fort Road', '0763334444'),
('ranil@example.com', 'Jaffna', 'Jaffna', 'Main Street', '0754445555'),
('chaminda@example.com', 'Matara', 'Matara', 'Beach Road', '0725556666'),
('kamal@example.com', 'Kurunegala', 'Kurunegala', 'Station Road', '0786667777');

-- Employee_inventory table 
INSERT INTO employee_inventory (EmployeeID, InventoryID) VALUES
(1, 1),
(2, 1),
(3, 3),
(4, 4),
(5, 2),
(6, 6);

-- 2 updates for each table
-- delivery_Man table
-- 1.1
UPDATE delivery_Man
SET Phone = '0715555555'
WHERE Delivery_Man_ID = 1;

-- 1.2
UPDATE delivery_Man
SET Last_Name = 'Wijesinghe'
WHERE Delivery_Man_ID = 2;

-- customer table
-- 2.1
UPDATE customer
SET Deliver_Man_ID = 3
WHERE Customer_ID = 1;

-- 2.2
UPDATE customer
SET DOB = '1986-09-22'
WHERE Customer_ID = 2;

-- order_details table
-- 3.1
UPDATE order_details
SET Stats = 'shipped'
WHERE Order_ID = 2;

-- 3.2
UPDATE order_details
SET Order_Date = '2024-03-09'
WHERE Order_ID = 3;

-- customer_email table
-- 4.1
UPDATE customer_email
SET Phone = '0777777777'
WHERE Email = 'saman@gmail.com';

-- 4.2
UPDATE customer_email
SET District = 'Nuwara Eliya'
WHERE Email = 'nimal@gmail.com';

-- shipping_address table
-- 5.1
UPDATE shipping_address
SET City = 'Gampaha'
WHERE CustomerID = 2;

-- 5.2
UPDATE shipping_address
SET State = 'Western'
WHERE CustomerID = 4;

-- payment table
-- 6.1
UPDATE payment
SET Amount = 2200
WHERE Payment_ID = 3;

-- 6.2
UPDATE payment
SET Payment_Method = 'Debit Card'
WHERE Payment_ID = 5;

-- warehouse table
-- 7.1
UPDATE warehouse
SET Location = 'Colombo 03'
WHERE Warehouse_ID = 1;

-- 7.2
UPDATE warehouse
SET Capacity = 4500
WHERE Warehouse_ID = 3;

-- inventory  table
-- 8.1
UPDATE inventory
SET Stock_Quantity = 1500
WHERE Inventory_ID = 3;

-- 8.2
UPDATE inventory
SET Last_Stock_Update = UNIX_TIMESTAMP('2024-04-01')
WHERE Inventory_ID = 5;

-- product table
-- 9.1
UPDATE product
SET Price = 220
WHERE Product_ID = 4;

-- 9.2
UPDATE product
SET Product_Description = 'Handmade crafts from local artisans, perfect for home decor'
WHERE Product_ID = 6;

-- order_item  table
-- 10.1
UPDATE order_item
SET Quantity = 3
WHERE Order_Item_ID = 4;

-- 10.2
UPDATE order_item
SET Price = 280
WHERE Order_Item_ID = 5;

-- supplier table
-- 11.1
UPDATE supplier
SET Contact_Person = 'Emma Watson'
WHERE Supplier_ID = 4;

-- 11.2
UPDATE supplier
SET Phone_NO = '0719998888'
WHERE Supplier_ID = 1;

-- product_supplier table
-- 12.1
UPDATE product_supplier
SET SupplierID = 5
WHERE ProductID = 6;

-- 12.2
UPDATE product_supplier
SET ProductID = 4, SupplierID = 3
WHERE ProductID = 1 AND SupplierID = 1;

-- product_warehouse  table
-- 13.1
UPDATE product_warehouse
SET WarehouseID = 4
WHERE ProductID = 5 AND WarehouseID = 2;

-- 13.2
UPDATE product_warehouse
SET ProductID = 2
WHERE ProductID = 4 AND WarehouseID = 2;

-- employee table
-- 14.1
UPDATE employee
SET Manager_ID = 3
WHERE Employee_ID = 2;

-- 14.2
UPDATE employee
SET DOB = '1978-02-15'
WHERE Employee_ID = 4;

-- product_features table
-- 15.1
UPDATE product_features
SET Product_Value = 'Organic', Product_Color = 'Brown', Product_Size = 'Medium'
WHERE ProductID = 4;

-- 15.2
UPDATE product_features
SET Product_Name = 'Brown Rice'
WHERE ProductID = 1;

-- employee_email table
-- 16.1
UPDATE employee_email
SET Phone = '0788889999'
WHERE Email = 'nimal@example.com';

-- 16.2
UPDATE employee_email
SET City = 'Gampaha'
WHERE Email = 'sunil@example.com';

-- employee_inventory table
-- 17.1
UPDATE employee_inventory
SET InventoryID = 2
WHERE EmployeeID = 3 AND InventoryID = 3;

-- 17.2
UPDATE employee_inventory
SET EmployeeID = 5
WHERE EmployeeID = 6 AND InventoryID = 6;

-- 1 deletion for each table
-- delivery_Man table
-- 1
DELETE FROM delivery_Man
WHERE Delivery_Man_ID = 6;

-- customer table
-- 2
DELETE FROM customer
WHERE Customer_ID = 6;

-- order_details table
-- 3
DELETE FROM order_details
WHERE Order_ID = 6;

-- customer_email table
-- 4
DELETE FROM customer_email
WHERE Email = 'sunil@gmail.com';

-- shipping_address table
-- 5
DELETE FROM shipping_address
WHERE CustomerID = 6;

-- payment table
-- 6
DELETE FROM payment
WHERE Payment_ID = 6;

-- warehouse table
-- 7
DELETE FROM warehouse
WHERE Warehouse_ID = 6;

-- inventory  table
-- 8
DELETE FROM inventory
WHERE Inventory_ID = 4;

-- product table
-- 9
DELETE FROM product
WHERE Product_ID = 5;

-- order_item  table
-- 10
DELETE FROM order_item
WHERE Order_Item_ID = 6;

-- supplier table
-- 11
DELETE FROM supplier
WHERE Supplier_ID = 6;

-- product_supplier table
-- 12
DELETE FROM product_supplier
WHERE ProductID = 2 AND SupplierID = 3;

-- product_warehouse  table
-- 13
DELETE FROM product_warehouse
WHERE ProductID = 2 AND WarehouseID = 2;

-- employee table
-- 14
DELETE FROM employee
WHERE Employee_ID = 6;

-- product_features table
-- 15
DELETE FROM product_features
WHERE ProductID = 2;

-- employee_email table
-- 16
DELETE FROM employee_email
WHERE Email = 'kasun@example.com';

-- employee_inventory table
-- 17
DELETE FROM employee_inventory
WHERE EmployeeID = 1 AND InventoryID = 1;



-- simple queries
-- 01
-- Select operation
SELECT * FROM order_details;

-- 02
-- Project operation
SELECT Product_Name, Price FROM product;

-- 03
-- Cartesian product operation
SELECT * FROM customer, order_details;

-- 04
-- Creating a user view
CREATE VIEW customer_info AS
SELECT Customer_ID, First_Name, Last_Name, DOB FROM customer;

-- 05
-- Renaming operation
SELECT Customer_ID AS ID, First_Name AS FirstName, Last_Name AS LastName FROM customer;

-- 06
-- Aggregation function (Average)
SELECT AVG(Stock_Quantity) AS AverageStock FROM inventory;

-- 07
-- Query demonstrating the use of LIKE keyword
SELECT * FROM product WHERE Product_Description LIKE '%Sri Lankan%';

-- complex queries
-- 01
-- Basic Set Operation - Union
SELECT Delivery_Man_ID, First_Name, Last_Name FROM delivery_Man
UNION
SELECT Deliver_Man_ID, First_Name, Last_Name FROM customer;

-- 02
-- Basic Set Operation - Intersection(By using inner join)
SELECT w.Warehouse_ID, w.Location 
FROM warehouse w
INNER JOIN inventory i ON w.Warehouse_ID = i.WarehouseID;

-- 03
-- Basic Set Operation - Set Difference
(SELECT Delivery_Man_ID, First_Name, Last_Name FROM delivery_Man)
EXCEPT 
(SELECT Deliver_Man_ID, First_Name, Last_Name FROM customer);

-- 04
-- Basic Set Operation - Division
SELECT Delivery_Man_ID, First_Name, Last_Name FROM delivery_Man
WHERE Delivery_Man_ID NOT IN (
    SELECT Deliver_Man_ID FROM customer
);

-- 05
-- Inner Join with User View
CREATE VIEW OrderCustomer AS
SELECT od.Order_ID, c.First_Name, c.Last_Name
FROM order_details od
INNER JOIN customer c ON od.CustomerID = c.Customer_ID;

SELECT * FROM OrderCustomer;

-- 06
-- Natural Join with User View
CREATE VIEW CustomerOrder AS
SELECT c.First_Name, c.Last_Name, od.Order_ID
FROM customer c
NATURAL JOIN order_details od;

SELECT * FROM CustomerOrder;

-- 07
-- Left Outer Join with User View
CREATE VIEW CustomerOrder1 AS
SELECT c.First_Name, c.Last_Name, od.Order_ID
FROM customer c
LEFT JOIN order_details od ON c.Customer_ID = od.CustomerID;

SELECT * FROM CustomerOrder1;


-- 08
-- Right Outer Join with User View
CREATE VIEW OrderCustomer1 AS
SELECT od.Order_ID, c.First_Name, c.Last_Name
FROM order_details od
RIGHT JOIN customer c ON od.CustomerID = c.Customer_ID;

SELECT * FROM OrderCustomer1;

-- 09
-- Full Outer Join with User View
CREATE VIEW FullCustomerOrder AS
SELECT c.First_Name, c.Last_Name, od.Order_ID
FROM customer c
LEFT JOIN order_details od ON c.Customer_ID = od.CustomerID
UNION
SELECT c.First_Name, c.Last_Name, od.Order_ID
FROM order_details od
RIGHT JOIN customer c ON c.Customer_ID = od.CustomerID;

SELECT * FROM FullCustomerOrder;

-- 10
-- Outer Union with User Views
CREATE VIEW product_supplier_info AS
SELECT p.Product_ID, p.Product_Name, p.Price, s.Supplier_ID, s.Supplier_Name
FROM product p
INNER JOIN product_supplier ps ON p.Product_ID = ps.ProductID
INNER JOIN supplier s ON ps.SupplierID = s.Supplier_ID;

CREATE VIEW employee_inventory_info AS
SELECT e.Employee_ID, e.First_Name AS Employee_First_Name, e.Last_Name AS Employee_Last_Name, i.Inventory_ID, i.Stock_Quantity
FROM employee e
INNER JOIN employee_inventory ei ON e.Employee_ID = ei.EmployeeID
INNER JOIN inventory i ON ei.InventoryID = i.Inventory_ID;

SELECT * FROM product_supplier_info
UNION
SELECT * FROM employee_inventory_info;

-- nested queries
-- 01
SELECT * FROM product
WHERE Product_ID IN (
    SELECT InventoryID FROM inventory
    WHERE Stock_Quantity > 1000
);

-- 02
SELECT * FROM employee
WHERE Employee_ID IN (
    SELECT e.Employee_ID FROM employee e
    INNER JOIN warehouse w ON e.Employee_ID = w.Warehouse_ID
    WHERE w.Capacity < 3000
);

-- 03
SELECT * FROM customer
WHERE Customer_ID IN (
    SELECT CustomerID FROM order_details
    WHERE Total_Amount > 2000
);

-- database tuning
-- 01 Basic Set Operation - Union
-- Original Query 01
EXPLAIN SELECT Delivery_Man_ID, First_Name, Last_Name FROM delivery_Man
UNION ALL
SELECT Deliver_Man_ID, First_Name, Last_Name FROM customer;

-- Tuned Query 01
CREATE INDEX idx_delivery_man_id ON delivery_Man(Delivery_Man_ID);
CREATE INDEX idx_customer_id ON customer(Customer_ID);
EXPLAIN SELECT Delivery_Man_ID, First_Name, Last_Name FROM delivery_Man USE INDEX (idx_delivery_man_id)
UNION ALL
SELECT Deliver_Man_ID, First_Name, Last_Name FROM customer USE INDEX (idx_customer_id);

-- DROP INDEX idx_delivery_man_id ON delivery_Man;
-- DROP INDEX idx_customer_id ON customer;

-- 02 Basic Set Operation - Intersection (By using inner join)
-- Original Query 02
EXPLAIN SELECT w.Warehouse_ID, w.Location 
FROM warehouse w
INNER JOIN inventory i ON w.Warehouse_ID = i.WarehouseID;

-- Tuned Query 02
CREATE INDEX idx_warehouse_id ON warehouse(Warehouse_ID);
EXPLAIN SELECT w.Warehouse_ID, w.Location 
FROM warehouse w USE INDEX (idx_warehouse_id)
INNER JOIN inventory i ON w.Warehouse_ID = i.WarehouseID;
 
-- 03 Basic Set Operation - Set Difference
-- Original Query 03
EXPLAIN (
    SELECT Delivery_Man_ID, First_Name, Last_Name FROM delivery_Man
)
EXCEPT 
(
    SELECT Deliver_Man_ID, First_Name, Last_Name FROM customer
);

-- Tuned Query 03
CREATE INDEX idx_delivery_man_id1 ON delivery_Man(Delivery_Man_ID);
CREATE INDEX idx_customer_id1 ON customer(Customer_ID);
EXPLAIN (
    SELECT Delivery_Man_ID, First_Name, Last_Name FROM delivery_Man USE INDEX (idx_delivery_man_id1)
)
EXCEPT 
(
    SELECT Deliver_Man_ID, First_Name, Last_Name FROM customer USE INDEX (idx_customer_id1)
);

-- 04 Basic Set Operation - Division
-- Original Query 04
EXPLAIN SELECT Delivery_Man_ID, First_Name, Last_Name FROM delivery_Man
WHERE Delivery_Man_ID NOT IN (
    SELECT Deliver_Man_ID FROM customer
);

-- Tuned Query 04
CREATE INDEX idx_delivery_man_id2 ON delivery_Man(Delivery_Man_ID);
CREATE INDEX idx_customer_id2 ON customer(Customer_ID);
EXPLAIN SELECT Delivery_Man_ID, First_Name, Last_Name FROM delivery_Man USE INDEX (idx_delivery_man_id2)
WHERE Delivery_Man_ID NOT IN (
    SELECT Deliver_Man_ID FROM customer USE INDEX (idx_customer_id2)
);

-- 05 Outer Union with User Views
-- Original Query 05
EXPLAIN SELECT * FROM product_supplier_info
UNION
SELECT * FROM employee_inventory_info;

-- Tuned Query 05
CREATE INDEX idx_product_supplier ON product_supplier(ProductID);
CREATE INDEX idx_employee_inventory ON employee_inventory(EmployeeID);
EXPLAIN SELECT * FROM product_supplier USE INDEX (idx_product_supplier)
UNION
SELECT * FROM employee_inventory USE INDEX (idx_employee_inventory);

-- 06 
-- Original Query 06
CREATE VIEW CustomerOrder3 AS
SELECT c.First_Name, c.Last_Name, od.Order_ID
FROM customer c
LEFT JOIN order_details od ON c.Customer_ID = od.CustomerID;

EXPLAIN SELECT * FROM CustomerOrder3;

-- Tuned Query 06
EXPLAIN SELECT c.First_Name, c.Last_Name, od.Order_ID
FROM customer c
LEFT JOIN order_details od ON c.Customer_ID = od.CustomerID;

-- 07 
-- Original Query 07
CREATE VIEW FullCustomerOrder1 AS
SELECT c.First_Name, c.Last_Name, od.Order_ID
FROM customer c
LEFT JOIN order_details od ON c.Customer_ID = od.CustomerID
UNION
SELECT c.First_Name, c.Last_Name, od.Order_ID
FROM order_details od
RIGHT JOIN customer c ON c.Customer_ID = od.CustomerID;

EXPLAIN SELECT * FROM FullCustomerOrder1;

-- Tuned Query 07
EXPLAIN (SELECT c.First_Name, c.Last_Name, od.Order_ID
FROM customer c
LEFT JOIN order_details od ON c.Customer_ID = od.CustomerID
UNION
SELECT c.First_Name, c.Last_Name, od.Order_ID
FROM order_details od
RIGHT JOIN customer c ON c.Customer_ID = od.CustomerID);

-- 08
-- Original Query 08
EXPLAIN SELECT w.Warehouse_ID, w.Location
FROM warehouse w
INNER JOIN inventory i ON w.Warehouse_ID = i.WarehouseID;

-- Tuned Query 08
-- CREATE INDEX idx_inventory_ID ON inventory(WarehouseID);
-- CREATE INDEX idx_warehouse_ID ON warehouse(Warehouse_ID);
EXPLAIN SELECT w.Warehouse_ID, w.Location
FROM warehouse w
INTERSECT
SELECT i.WarehouseID, w.Location
FROM inventory i
INNER JOIN warehouse w ON i.WarehouseID = w.Warehouse_ID;

-- 09
-- Original Query 09
EXPLAIN SELECT e.Employee_ID, e.First_Name, e.Last_Name
FROM employee e
WHERE EXISTS (
    SELECT 1
    FROM employee_inventory ei
    WHERE ei.EmployeeID = e.Employee_ID
);

-- Tuned Query 09
CREATE INDEX idx_employee_inventory_EmployeeID ON employee_inventory(EmployeeID);
EXPLAIN SELECT DISTINCT e.Employee_ID, e.First_Name, e.Last_Name
FROM employee e
INNER JOIN employee_inventory ei ON e.Employee_ID = ei.EmployeeID;

-- 10 Nested Query 01
-- Original Query 10

EXPLAIN SELECT * FROM product
WHERE Product_ID IN (
    SELECT InventoryID FROM inventory
    WHERE Stock_Quantity > 1000
);

-- Tuned Query 10
CREATE INDEX idx_product_id ON product(Product_ID);
CREATE INDEX idx_inven_id ON inventory(Inventory_ID);
EXPLAIN SELECT * FROM product USE INDEX (idx_product_id)
WHERE Product_ID IN (
    SELECT InventoryID FROM inventory USE INDEX (idx_inven_id) 
    WHERE Stock_Quantity > 1000
);

-- 11 Nested Query 02
-- Original Query 11
EXPLAIN SELECT * FROM employee
WHERE Employee_ID IN (
    SELECT e.Employee_ID FROM employee e
    INNER JOIN warehouse w ON e.Employee_ID = w.Warehouse_ID
    WHERE w.Capacity < 3000
);

-- Tuned Query 11
CREATE INDEX idx_employee_id ON employee(Employee_ID);
EXPLAIN SELECT * FROM employee USE INDEX (idx_employee_id)
WHERE Employee_ID IN (
    SELECT e.Employee_ID FROM employee e USE INDEX (idx_employee_id)
    INNER JOIN warehouse w ON e.Employee_ID = w.Warehouse_ID
    WHERE w.Capacity < 3000
);



