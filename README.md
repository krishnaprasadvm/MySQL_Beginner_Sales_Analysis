CREATE DATABASE sales_analysis;
USE sales_analysis;

-- Customers table
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100),
    email VARCHAR(100),
    region VARCHAR(50)
);

-- Products table
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10, 2)
);

-- Orders table
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    product_id INT,
    order_date DATE,
    quantity INT,
    total_price DECIMAL(10, 2),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);

-- Insert customers
INSERT INTO customers VALUES
(1, 'John Doe', 'john@example.com', 'North'),
(2, 'Jane Smith', 'jane@example.com', 'South'),
(3, 'Mike Johnson', 'mike@example.com', 'East'),
(4, 'Sarah Williams', 'sarah@example.com', 'West'),
(5, 'David Brown', 'david@example.com', 'North');

-- Insert products
INSERT INTO products VALUES
(101, 'Laptop', 'Electronics', 999.99),
(102, 'Smartphone', 'Electronics', 699.99),
(103, 'Desk Chair', 'Furniture', 149.99),
(104, 'Coffee Maker', 'Appliances', 79.99),
(105, 'Headphones', 'Electronics', 99.99);

-- Insert orders
INSERT INTO orders VALUES
(1001, 1, 101, '2023-01-15', 1, 999.99),
(1002, 2, 102, '2023-01-16', 2, 1399.98),
(1003, 3, 103, '2023-02-05', 1, 149.99),
(1004, 4, 104, '2023-02-10', 3, 239.97),
(1005, 5, 105, '2023-03-01', 2, 199.98),
(1006, 1, 102, '2023-03-15', 1, 699.99),
(1007, 2, 101, '2023-04-20', 1, 999.99),
(1008, 3, 105, '2023-05-10', 1, 99.99);

-- 1. Calculate Total Sales Revenue

SELECT SUM(total_price) AS total_revenue 
FROM orders;

-- 2. Find the Top 5 Best-Selling Products

SELECT 
    product_name,
    sum(quantity),
    sum(total_price)
from orders
join products on orders.product_id = products.product_id
group by products.product_name
order by sum(quantity) desc
limit 5;

-- 3. Analyze Sales by Month	
    
select
	sum(total_price) as total_sale,
    monthname(order_date) as Month_Name
from orders
group by Month_Name
order by total_sale Desc;

-- 4. Find Customers with the Highest Lifetime Value (LTV)

SELECT 
    customer_name,
    SUM(total_price) AS total_spent
FROM orders
JOIN customers ON orders.customer_id = customers.customer_id
GROUP BY customer_name
ORDER BY total_spent DESC;


	-- 5. Sales by Region
select
	sum(total_price),
    region
from
	orders
join	customers on customers.customer_id = orders.customer_id
group by region
order by sum(total_price) Desc
