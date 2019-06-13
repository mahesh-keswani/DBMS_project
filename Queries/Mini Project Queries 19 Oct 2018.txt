SELECT * FROM dealer;
SELECT * FROM brand;
SELECT * FROM model;
SELECT * FROM car;
SELECT * FROM customer;
SELECT * FROM services;

CREATE TRIGGER trigger1 AFTER INSERT 
AS

//**********Procedures***********

//Car according to colour
CREATE PROCEDURE car1 @col varchar(20)
as
SELECT * FROM car WHERE colour=@col;
GO
EXEC car1 @col='Red';

//Car according to brand 
CREATE PROCEDURE car3 @brand varchar(20) 
AS
SELECT model_name FROM model WHERE br_id=(SELECT brand_id FROM brand WHERE brand_name=@brand);
GO
EXEC car3 @brand='Toyota';

//Car according to a particular price
CREATE PROCEDURE carprice @price int 
as
select * from carinfo where price <= @price;
go
exec carprice @price  = 500000;

//**********Views**********

//Example view
create view carinfo2 as select model_name,price from model,car where m_id=model_id;
select * from carinfo2;

//Information of a car
create view carinfo as
select brand_name,model_name,price,colour,car_type  from car as c inner join model as m on c.m_id=m.model_id
inner join brand as b on c.br_id=b.brand_id;
select * from carinfo;

//**********Queries**********

//Car info in ascending order of price
select price,colour,car_type,model_name 
from car as c inner join model as m on c.m_id=m.model_id order by price ;

//Car info in ascending order of price
select * from carinfo order by price;

//Car info in descending order of price
select * from carinfo order by price desc;

//Services Customers
select cust_name,phone,date_recieved,cust_car_id from customer as c inner join services as s 
on c.cust_car_id=s.ser_car_id; 

//View
CREATE VIEW customercar AS 
SELECT cust_name, model_name, brand_name, price, colour from customer as c inner join model as m on c.cust_car_id=(SELECT car_id from car where m_id=m.model_id)
inner join brand as b on b.brand_id=m.br_id inner join car as cc on c.cust_car_id=cc.car_id;
SELECT * FROM customercar; 

ALTER VIEW customercar add price FROM car int;
DROP VIEW customercar;

