# Sql-practice-1
### relational scheme
![scheme picture](https://github.com/Erlibek/IMG/blob/7e10f3fc6678a5add2c3936ec00bcd552f53155a/WhatsApp%20Image%202022-09-18%20at%2009.05.22.jpeg) 
### Table creation code
``` sql
CREATE TABLE Manufacturers (
	Code INTEGER PRIMARY KEY NOT NULL,
	Name CHAR(50) NOT NULL 
);

CREATE TABLE Products (
	Code INTEGER PRIMARY KEY NOT NULL,
	Name CHAR(50) NOT NULL ,
	Price REAL NOT NULL ,
	Manufacturer INTEGER NOT NULL 
		CONSTRAINT fk_Manufacturers_Code REFERENCES Manufacturers(Code)
);
```
### Sample dataset
```sql
INSERT INTO Manufacturers(Code,Name) VALUES (1,'Sony'),(2,'Creative Labs'),(3,'Hewlett-Packard'),(4,'Iomega'),(5,'Fujitsu'),(6,'Winchester');
INSERT INTO Products(Code,Name,Price,Manufacturer) VALUES(1,'Hard drive',240,5),(2,'Memory',120,6),(3,'ZIP drive',150,4),(4,'Floppy disk',5,6),(5,'Monitor',240,1),
(6,'DVD drive',180,2),(7,'CD drive',90,2),(8,'Printer',270,3),(9,'Toner cartridge',66,3),(10,'DVD burner',180,2);
```
### Exercises
1. Select the names of all the products in the store.
2. Select the names and the prices of all the products in the store.
3. Select the name of the products with a price less than or equal to $200.
4. Select all the products with a price between $60 and $120.
5. Select the name and price in cents (i.e., the price must be multiplied by 100).
6. Compute the average price of all the products.
7. Compute the average price of all products with manufacturer code equal to 2.
8. Compute the number of products with a price larger than or equal to $180.
9. Select the name and price of all products with a price larger than or equal to $180, and sort first by price (in descending order), and then by name (in ascending order).
10. Select all the data from the products, including all the data for each product's manufacturer.
11. Select the product name, price, and manufacturer name of all the products.
12. Select the average price of each manufacturer's products, showing only the manufacturer's code.
13. Select the average price of each manufacturer's products, showing the manufacturer's name.
14. Select the names of manufacturer whose products have an average price larger than or equal to $150.
15. Select the name and price of the cheapest product.
16. Select the name of each manufacturer along with the name and price of its most expensive product.
17. Select the name of each manufacturer which have an average price above $145 and contain at least 2 different products.
18. Add a new product: Loudspeakers, $70, manufacturer 2.
19. Update the name of product 8 to "Laser Printer".
20. Apply a 10% discount to all products.
21. Apply a 10% discount to all products with a price larger than or equal to $120.
### Answers
```sql
1. SELECT Name FROM Products;

2. SELECT Name, Price FROM Products;

3. SELECT Name FROM Products WHERE Price <= 200;

4. SELECT * FROM Products
   WHERE Price BETWEEN 60 AND 120;

5. SELECT Name, Price * 100 AS PriceCents FROM Products;

6. SELECT AVG(Price) FROM Products;

7. SELECT AVG(Price) FROM Products WHERE Manufacturer=2;

8.SELECT COUNT(*) FROM Products WHERE Price >= 180;

9. SELECT Name, Price 
     FROM Products
    WHERE Price >= 180
 ORDER BY Price DESC, Name;

10. SELECT * FROM Products, Manufacturers
   WHERE Products.Manufacturer = Manufacturers.Code;

11. SELECT Products.Name, Price, Manufacturers.Name
   FROM Products INNER JOIN Manufacturers
   ON Products.Manufacturer = Manufacturers.Code;

12. SELECT AVG(Price), Manufacturer
    FROM Products
GROUP BY Manufacturer;

13. SELECT AVG(Price), Manufacturers.Name
   FROM Products INNER JOIN Manufacturers
   ON Products.Manufacturer = Manufacturers.Code
   GROUP BY Manufacturers.Name;

14. SELECT AVG(Price), Manufacturers.Name
   FROM Products INNER JOIN Manufacturers
   ON Products.Manufacturer = Manufacturers.Code
   GROUP BY Manufacturers.Name
   HAVING AVG(Price) >= 150;

15. SELECT name,price
  FROM Products
  ORDER BY price ASC
  LIMIT 1

16.  SELECT A.Name, A.Price, F.Name
   FROM Products A INNER JOIN Manufacturers F
   ON A.Manufacturer = F.Code
     AND A.Price =
     (
       SELECT MAX(A.Price)
         FROM Products A
         WHERE A.Manufacturer = F.Code
     );

17. Select m.Name, Avg(p.price) as p_price, COUNT(p.Manufacturer) as m_count
FROM Manufacturers m, Products p
WHERE p.Manufacturer = m.code
GROUP BY p.Manufacturer
HAVING p_price >= 150 and m_count >= 2;

18. INSERT INTO Products( Code, Name , Price , Manufacturer)
  VALUES ( 11, 'Loudspeakers' , 70 , 2 );

19. UPDATE Products
   SET Name = 'Laser Printer'
   WHERE Code = 8;

20. UPDATE Products
   SET Price = Price - (Price * 0.1);

21.  UPDATE Products
   SET Price = Price - (Price * 0.1)
   WHERE Price >= 120;
   ```
