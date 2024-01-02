# Pharmaceutical_Product_Sales

SELECT * FROM Customers

--Query 2: Get a list of all products and their prices.

SELECT product_name, unit_price FROM Products

--Query 3: List all employees and their positions.

SELECT * FROM Employees

--Query 4: Display all sales with customer names, product names, and sale dates.

SELECT S.sale_id, C.customer_name, P.product_name, S.sale_date
FROM Sales S
JOIN Customers C ON S.customer_id = C.customer_id
JOIN Products P ON S.product_id = P.product_id;

--Query 5: Show total sales amount for each customer.

SELECT C.customer_name, SUM(S.total_amount) AS total_sales_amount
FROM Customers C
JOIN Sales S ON C.customer_id = S.Customer_id
GROUP BY C.customer_name;

--Query 6: Find the best-selling product.

SELECT P.product_name, SUM(S.quantity_sold) AS total_quantity_sold
FROM Products P
JOIN Sales S ON P.product_id = S.product_id
GROUP BY P.product_name
ORDER BY total_quantity_sold DESC

--Query 8: Filter sales by a specific province.

SELECT * FROM Sales
WHERE Province = 'Ontario';

--Query 9: Retrieve the total number of products sold by each employee.

SELECT E.employee_name, SUM(S.quantity_sold) AS total_products_sold
FROM Employees E
JOIN Sales S ON E.employee_id = S.employee_id
GROUP BY E.employee_name;

--Query 10: Display the average unit price of products.

SELECT AVG(unit_price) AS average_unit_price
FROM Products;

--Query 11: Find customers who have not made any purchases.

SELECT C.customer_name
FROM Customers C
LEFT JOIN Sales S ON C.customer_id = S.customer_id
WHERE S.sale_id IS NULL;

--Query 12: Identify products with low inventory (quantity sold less than 10).

SELECT P.product_name, P.unit_price, S.quantity_sold
FROM Products P
LEFT JOIN Sales S ON P.product_id = S.product_id
WHERE S.quantity_sold < 10 OR S.quantity_sold IS NULL;

--Query 13: Retrieve the latest sale for each customer.

SELECT C.customer_name, MAX(S.sale_date) AS latest_sale_date
FROM Customers C
LEFT JOIN Sales S ON C.customer_id = S.customer_id
GROUP BY C.customer_name;

--Query 14: Calculate the total revenue for each product.

SELECT P.product_name, SUM(S.total_amount) AS total_revenue
FROM Products P
LEFT JOIN Sales S ON P.product_id = S.product_id
GROUP BY P.product_name; 

--Query 15: Find customers who made a purchase in a specific province.

SELECT DISTINCT C.customer_name, S.province
FROM Customers C
JOIN Sales S ON C.customer_id = S.customer_id
WHERE S.province = 'Ontario';

--Query 16: Retrieve employees and the total sales amount they generated.

SELECT E.employee_name, SUM(S.total_amount) AS total_sales_amount
FROM Employees E
JOIN Sales S ON E.employee_id = S.employee_id
GROUP BY E.employee_name;

--Query 17: List products and their manufacturers for a specific supplier.

SELECT P.product_name, P.manufacturer
FROM Products P
WHERE P.supplier = 'Mississauga';

--Query 18:Identify customers who made a purchase using a specific payment method.

SELECT C.customer_name, S.payment_method
FROM Customers C
JOIN Sales S ON C.customer_id = S.customer_id
WHERE S.payment_method = 'Credit Card';

--Query 19: Display the top 5 customers with the highest total sales amount.

SELECT C.customer_name, SUM(S.total_amount) AS total_sales_amount
FROM Customers C
JOIN Sales S ON C.customer_id = S.customer_id
GROUP BY C.customer_name
ORDER BY total_sales_amount DESC

--Query 20: Retrieve the average quantity sold per sale.

SELECT AVG(quantity_sold) AS average_quantity_sold
FROM Sales;

--Query 21: Find products that have never been sold.

SELECT P.product_name
FROM Products P
LEFT JOIN Sales S ON P.product_id = S.product_id
WHERE S.product_id IS NULL;

--Query 22: Display the number of sales made in each province.

SELECT province, COUNT(*) AS num_sales
FROM Sales
GROUP BY province;


--Query 24: Calculate the total revenue for each customer, considering only successful deliveries.

SELECT C.customer_name, SUM(S.total_amount) AS total_revenue
FROM Customers C
JOIN Sales S ON C.customer_id = S.customer_id
WHERE S.delivery_status = 'Delivered'
GROUP BY C.customer_name;

--Query 27: Retrieve customers who have made purchases for more than $1000 in total.

SELECT C.customer_name, SUM(S.total_amount) AS total_spent
FROM Customers C
JOIN Sales S ON C.customer_id = S.customer_id
GROUP BY C.customer_name
HAVING SUM(S.total_amount) > 50000;

--Query 28: Display the top 3 products with the highest average sales quantity.

SELECT P.product_name, AVG(S.quantity_sold) AS average_quantity_sold
FROM Products P
JOIN Sales S ON P.product_id = S.product_id
GROUP BY P.product_name
ORDER BY average_quantity_sold DESC

--Query 29: Identify customers who have made purchases of products from multiple suppliers.

SELECT C.customer_name, COUNT(DISTINCT P.supplier) AS num_suppliers
FROM Customers C
JOIN Sales S ON C.customer_id = S.customer_id
JOIN Products P ON S.product_id = P.product_id
GROUP BY C.customer_name
HAVING COUNT(DISTINCT P.supplier) > 1;

--Query 30: Retrieve the sales with the highest total amount for each product.

WITH RankedSales AS (
  SELECT
    S.*,
    RANK() OVER (PARTITION BY S.product_id ORDER BY S.total_amount DESC) AS SalesRank
  FROM Sales S
)
SELECT * FROM RankedSales WHERE SalesRank = 1;

--Query 32: Calculate the running total of sales amount for each product.

SELECT
  P.product_name,
  S.sale_date,
  S.total_amount,
  SUM(S.total_amount) OVER (PARTITION BY S.product_id ORDER BY S.sale_date) AS running_total
FROM Products P
JOIN Sales S ON P.product_id = S.product_id;

--Query 33: Retrieve the top 5 customers with the highest average purchase amount.

SELECT
  C.customer_name,
  AVG(S.total_amount) AS average_purchase_amount
FROM Customers C
JOIN Sales S ON C.customer_id = S.customer_id
GROUP BY C.customer_name
ORDER BY average_purchase_amount DESC

--Query 35: Calculate the percentage of total revenue contributed by each product.

SELECT
  P.product_name,
  SUM(S.total_amount) AS total_revenue,
  (SUM(S.total_amount) / (SELECT SUM(total_amount) FROM Sales)) * 100 AS revenue_percentage
FROM Products P
LEFT JOIN Sales S ON P.product_id = S.product_id
GROUP BY P.product_name;
