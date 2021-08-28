# The computer store

## Relational Schema
![Relational Schema](https://upload.wikimedia.org/wikipedia/commons/b/b2/Computer-store-db.png)

## Table creation code
```sql
CREATE TABLE Manufacturers (
	Code INTEGER PRIMARY KEY NOT NULL,
	Name CHAR(50) NOT NULL 
);

CREATE TABLE Products
(
    Code         INTEGER PRIMARY KEY NOT NULL,
    Name         CHAR(50)            NOT NULL,
    Price        REAL                NOT NULL,
    Manufacturer INTEGER             NOT NULL
        CONSTRAINT fk_Manufacturers_Code REFERENCES Manufacturers (Code)
);
```

## Sample dataset
```sql
INSERT INTO Manufacturers(Code, Name)
VALUES (1, 'Sony');
INSERT INTO Manufacturers(Code, Name)
VALUES (2, 'Creative Labs');
INSERT INTO Manufacturers(Code, Name)
VALUES (3, 'Hewlett-Packard');
INSERT INTO Manufacturers(Code, Name)
VALUES (4, 'Iomega');
INSERT INTO Manufacturers(Code, Name)
VALUES (5, 'Fujitsu');
INSERT INTO Manufacturers(Code, Name)
VALUES (6, 'Winchester');
INSERT INTO Manufacturers(Code, Name)
VALUES (7, 'Bose');

INSERT INTO Products(Code, Name, Price, Manufacturer)
VALUES (1, 'Hard drive', 240, 5);
INSERT INTO Products(Code, Name, Price, Manufacturer)
VALUES (2, 'Memory', 120, 6);
INSERT INTO Products(Code, Name, Price, Manufacturer)
VALUES (3, 'ZIP drive', 150, 4);
INSERT INTO Products(Code, Name, Price, Manufacturer)
VALUES (4, 'Floppy disk', 5, 6);
INSERT INTO Products(Code, Name, Price, Manufacturer)
VALUES (5, 'Monitor', 240, 1);
INSERT INTO Products(Code, Name, Price, Manufacturer)
VALUES (6, 'DVD drive', 180, 2);
INSERT INTO Products(Code, Name, Price, Manufacturer)
VALUES (7, 'CD drive', 90, 2);
INSERT INTO Products(Code, Name, Price, Manufacturer)
VALUES (8, 'Printer', 270, 3);
INSERT INTO Products(Code, Name, Price, Manufacturer)
VALUES (9, 'Toner cartridge', 66, 3);
INSERT INTO Products(Code, Name, Price, Manufacturer)
VALUES (10, 'DVD burner', 180, 2);
```

## Exercises
1. Select the names of all the products in the store.
3. Select the names and the prices of all the products in the store.
4. Select the name of the products with a price less than or equal to $200.
5. Select all the products with a price between $60 and $120.
6. Select the name and price in cents (i.e., the price must be multiplied by 100).
7. Compute the average price of all the products.
8. Compute the average price of all products with manufacturer code equal to 2.
9. Compute the number of products with a price larger than or equal to $180.
10. Select the name and price of all products with a price larger than or equal to $180, and sort first by price (in descending order), and then by name (in ascending order).
11. Select all the data from the products, including all the data for each product's manufacturer.
12. Select the product name, price, and manufacturer name of all the products.
13. Select the average price of each manufacturer's products, showing only the manufacturer's code.
14. Select the average price of each manufacturer's products, showing the manufacturer's name.
15. Select the names of manufacturer whose products have an average price larger than or equal to $150.
16. Select the name and price of the cheapest product.
17. Select the name of each manufacturer along with the name and price of its most expensive product.
18. Select the name of each manufacturer which have an average price above $145 and contain at least 2 different products.
19. Add a new product: Loudspeakers, $70, manufacturer 2.
20. Update the name of product 8 to "Laser Printer".
21. Apply a 10% discount to all products.
22. Apply a 10% discount to all products with a price larger than or equal to $120.

## Answers
```sql
 1. SELECT Name 
    FROM Products;
```
```sql
 2. SELECT Name, Price 
    FROM Products;
```
```sql
 3. SELECT Name
    FROM Products
    WHERE Price <= 200;
```
```sql
 4. SELECT *
    FROM Products
    WHERE Price >= 60
      AND Price <= 120;
```
```sql
 5. SELECT Name, Price * 100
    FROM Products;
```
```sql
 6. SELECT AVG(Price)  
    FROM Products;
```
```sql
 7. SELECT AVG(Price) 
    FROM Products 
    WHERE Manufacturer=2;
```
```sql
 8. SELECT COUNT(*) 
    FROM Products 
    WHERE Price >= 180;
```
```sql
 9. SELECT Name, Price
    FROM Products
    WHERE Price >= 180
    ORDER BY Price DESC, Name;
```
```sql
10. SELECT *
    FROM Products,
    Manufacturers
    WHERE Products.Manufacturer = Manufacturers.Code;
```
```sql
11. SELECT Products.Name, Price, Manufacturers.Name
    FROM Products,
    Manufacturers
    WHERE Products.Manufacturer = Manufacturers.Code;
```
```sql
12. SELECT AVG(Price), Manufacturer
    FROM Products
    GROUP BY Manufacturer;
```
```sql
13. SELECT AVG(Price), Manufacturers.Name
    FROM Products,
    Manufacturers
    WHERE Products.Manufacturer = Manufacturers.Code
    GROUP BY Manufacturers.Name;
```
```sql
14. SELECT AVG(Price), Manufacturers.Name
    FROM Products,
    Manufacturers
    WHERE Products.Manufacturer = Manufacturers.Code
    GROUP BY Manufacturers.Name
    HAVING AVG(Price) >= 150;
```
```sql
15. SELECT name,price
    FROM Products
    ORDER BY price ASC
    LIMIT 1
```
```sql
16. SELECT A.Name, A.Price, F.Name
    FROM Products A,
    Manufacturers F
    WHERE A.Manufacturer = F.Code
    AND A.Price =
      (
          SELECT MAX(A.Price)
          FROM Products A
          WHERE A.Manufacturer = F.Code
      );
```
```sql
17. Select m.Name, Avg(p.price) as p_price, COUNT(p.Manufacturer) as m_count
    FROM Manufacturers m,
    Products p
    WHERE p.Manufacturer = m.code
    GROUP BY p.Manufacturer
    HAVING p_price >= 150
    and m_count >= 2;
```
```sql
18. INSERT INTO Products(Code, Name, Price, Manufacturer)
    VALUES (11, 'Loudspeakers', 70, 2);
```
```sql
19. UPDATE Products
    SET Name = 'Laser Printer'
    WHERE Code = 8;
```
```sql
20. UPDATE Products
    SET Price = Price - (Price * 0.1);
```
```sql
21. UPDATE Products
    SET Price = Price - (Price * 0.1)
    WHERE Price >= 120;
```
