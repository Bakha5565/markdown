## 1. Мадомилов Бахманёр Шахриёрович, ИС 22/9-1

## Гостиница
###### ![](screens/Hotel_bd.png)
Моя база данных про гостиницу
База данных имеет 4 таблицы

Таблица Booking (таблица хранит сведения о бронировании)

Таблица Guest (таблица хранит сведения о гоятх)

Таблица Hostel (таблица хранит сведения о гостинице)

Таблица Room (таблица хранит сведения о комнатах)

##### BOOKING
Таблица Booking состоит из следующих атрибутов:
id - создается по умолчанию, INT
guest_id - айди к таблице Guest, INT
room_id - айди к таблице Room, INT
check_in_date - айди к таблице Booking, Date
check_in_out - айди к таблице Booking, Date
Total_price - цена, float
###### ![](screens/booking1.png)
###### ![](screens/booking2.png)

##### GUEST
Таблица Guest состоит из следующих атрибутов:
id - создается по умолчанию, INT
name - имя клиента, VARCHAR(50)
email - электронная почта клиента, VARCHAR(50)
phone_number - номер телефона клиента, VARCHAR(20)
###### ![](screens/Guest1.png)
###### ![](screens/Guest2.png)

##### HOTEL
Таблица Hotel состоит из следующих атрибутов:
id - создается  по умолчанию, INT
name - название гостиницы, VARCHAR(50)
addrees - адрес гостиницы, VARCHAR(100)
city - город в котором находится гостиница, VARCHAR(500)

##### ROOM
Таблица Room состоит из слудующих атрибутов:
id - по умолчанию, INT
hotel_id - айди гостиницы, INT
room_id - айди номера, INT
room_type - тип номера, VARCHAR(50)
price - цена номера за сутки, VARCHAR(50)
![](screens/Room1.png)
![](screens/Room2.png)

* ###### Объединил две разные строки в таблице Guest с помощью команды UNION
```
 SELECT name From Guest
 
 UNION
 
 SELECT email FROM Guest
```
![](screens/Union.png)

*  ###### Отработал с командой ORDER BY в таблице Hotel, отсортировал цены по убыванию
```
SELECT price
FROM Hotel
Order by price DESC
```
![](screens/OrderBY.png)

* ###### Также проработал с командой Having проработал
```
    SELECT price
    FROM Hotel
    GROUP by name
    HAVING price > 500
```
![](screens/Having.png)

* ##### 6.1. Демонстрация работы вложенных запросов - Select
  Нашел имена гостей, которые сделали бронирования га сумму больше средней стоимости всех бронирований.
```
  SELECT Guest.name
FROM Guest
JOIN Booking ON Guest.id = Booking.guest_id
GROUP BY Guest.name
HAVING AVG(Booking.total_price) > (
    SELECT AVG(total_price)
    FROM Booking
);
```
![](screens/select.png)

* ##### 6.2.  Демонстрация работы вложенных запросов - Where

```
SELECT price
from Hotel
WHERE price = (
  SELECT min(price) 
  FROM Hotel);

```
![](screens/where.png)

* ### 7. Демонстрация работы оконных функций
 * ##### 7.1. Агрегатные функции
```
select max(price)
from Hotel

UNION

SELECT min(price)
FROM Hotel

UNION

SELECT avg(price)
from Hotel

UNION

SELECT count(price)
from Hotel

UNION

select sum(price)
from Hotel
```
![](screens/Агрег.функции.PNG)

* ##### 7.2 Ранжирующие функии

```
SELECT  city, name,
	RANK() OVER ( PARTITION BY name ORDER BY city) AS 'rank'
FROM Hotel
```
![](screens/ранжирующиефункции.png)

* ##### 7.3. Функции смещения

```
SELECT name, price,
	LAG(price) OVER(PARTITION BY name ORDER BY price) AS 'lag'
FROM Hotel
```
![](screens/функциясмещения.png)

* ### 8. Виды JOIN-ов
* ##### 8.1. INNER JOIN
Inner join возвращает только строки, имеющие совпадение в обеих таблицах по guest_id.
Получаем список всех бронирований с именем гостя, сделавшего бронирование.
```
SELECT
    Guest.name AS guest_name,
    Booking.room_id,
    Booking.check_in_date,
    Booking.check_out_date,
    Booking.total_price
FROM Booking AS Booking
INNER JOIN Guest AS Guest ON Booking.guest_id = Guest.id;
```
![](screens/INNERJOIN.png)

* ##### 8.2. LEFT JOIN

Left join возвращает все строки из таблицы Booking (левая таблица) и совпадающие строки из Guest (правая таблица).
```
SELECT
    Guest.name AS guest_name,
    Booking.room_id,
    Booking.check_in_date,
    Booking.check_out_date,
    Booking.total_price
FROM Booking AS Booking
LEFT JOIN Guest AS Guest ON Booking.guest_id = Guest.id;
```
![](screens/leftjoin.png)

* ### 9. Демонстрация работы оператора CASE
  Оператор Case расставляет гостиницы по популяризации.
```
  SELECT
    Hotel.name AS hotel_name,
    Hotel.rating,
    CASE
        WHEN Hotel.rating <= 4 THEN 'Высокий'
        WHEN Hotel.rating >= 4.5 THEN 'Средний'
        WHEN Hotel.rating >= 3 THEN 'Низкий'
    END AS popularity_level
FROM Hotel AS Hotel;
```

![](screens/Case.png)
