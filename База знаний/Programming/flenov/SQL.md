
---
### Почитать
- [Нормализация отношений. Шесть нормальных форм ](https://habr.com/ru/articles/254773/)

sql - join:
- [SQL углубимся ](https://www.flenov.info/books/read/free-sql-book/103)
- [Таблицы - Таблицы и еще таблицы](https://www.flenov.info/books/read/free-sql-book/104)
- [Агрегатные функции SQL](https://www.flenov.info/books/read/free-sql-book/105)
- [Группировка](https://www.flenov.info/books/read/free-sql-book/106)


----


- База данных - это хранилище для нескольких таблиц.
- Реляционная база данных - данные хранятся в виде таблицы. Реляционная (relationship) - потому что все данные связаны между собой.
Есть таблицы, они связаны между собой и они должны храниться вместе в одной базе данных. Все эти таблицы собираются в одно хранилище, называемые бд и бд отвечает за их хранение и можно указать связи между таблицами ![[Screenshot 2024-09-12 at 2.48.32 PM.png]]


#Оптимизация_БД
- В зависимости от кодировки, занимает разное кол-во памяти.
	Если в поле много символов - лучше завести справочник. Например колонку с городами заменить на цифры. 
	 ![[Screenshot 2024-09-12 at 2.31.17 PM.png]]

	- Если можно разделить информацию - разделяй!!!
	![[Screenshot 2024-09-12 at 2.33.20 PM.png]]

	- Пол можно не выделять, а писать как есть - смотреть по ситуации

	- Если есть какой-то `код`, например это классы, которые нужно в дальнейшем изменить с рус на англ буквы и чтобы не менять 1 млн записей, лучше присвоить классам цифры, а потом поменять нужные поля, как в примере ниже.
	![[Screenshot 2024-09-12 at 2.42.43 PM.png]]

---
### Команды 

- `in` можно перевести как "одно из", удобно использовать когда нужно перечислить несколько значений
![[Screenshot 2024-09-14 at 4.10.49 PM.png]]

- `like` используется для поиска по символу, а `_` означает любой другой символ ('d__' - ищем людей у который фамилия начинается на "D" и имеет еще 2 сивола)
![[Screenshot 2024-09-14 at 4.15.25 PM.png]]
	если не знаем кол-во символов - использовать `%` (`SELECT * FROM phone WHERE lastname like 'D%'`) + можно использовать так: `'%o%'`,  `'%e'` ...

- `SELECT * FROM phone WHERE phoneid between 4 and 6;` - выбрать включая от 4 до 6

- `is` - для поиска строк, где пустое значение 
`NULL` - пустота, отсутствующее значение, не действительное, не существующее, т.е. в этой ячейке нет никакого значения. 

"" - пустая строка - это значение
NULL - вообще ничего нет
		![[Screenshot 2024-09-14 at 4.59.25 PM.png]]

![[Screenshot 2024-09-14 at 4.30.40 PM.png]]


- **Сортировка**:
	`SELECT * FROM phone ORDER BY lastname ASC, firstname ASC;`
	ASC - in ascending order
	DESC - in descending order 

- **Добавить** значение в таблицу:
	`INSERT имя таблицы VALUES ()`
	- ключевое поле (первая колонка) - автоматически увеличивается при добавлении значения и его значение предоставлять не обязательно, но можно это сделать, однако это очень плохо, не рекомендуется!!!
	
	- добавить значения только для указанных колонок:  
	`INSERT phone (firstname, phone) VALUES ('Mary', '482935');` 

- **Изменить значение**:
	`UPDATE phone SET columname = value WHERE lastname is NULL and firstname = '';` (`UPDATE phone SET firstname = 'David' WHERE phoneid = 16;`)

- `limit` показывает ограниченное кол-во записей, если указать два числа, то первое - сколько пропустить, второе - сколько показать (пропустить 10 страниц на сайте и показать на 11 странице 6 записей):
	`select firstname, lastname from phone limit 10, 6;

- Показать уникальные имена (по конкретным колонкам):
	`select distinct lastname from phone;`

- Сложение колонок (любые мат. операции):
	`select *, phoneid + cityid from phone;`
	для строк:
	`select concat(firstname, ' ', lastname) from phone;`

- **Псевдонимы** (можно делать через пробелы, пропуская `as`):
	`select concat(firstname, ' ', lastname) as DisplayName from phone;`

- **JOIN - связи таблиц** (возвращает только те записи, которые есть и в одной, и в другой таблице): 
	`SELECT * FROM phone JOIN city ON phone.cityid = city.cityid;`
	чтобы не было повторов `cityid` - указать нужные колонки, с использованием псевдонимов
![[Screenshot 2024-09-16 at 6.47.59 PM.png]]
>Left join - означает, что все записи слева от `LEFT` будут выведены (все записи phone): 
>`SELECT * FROM phone p LEFT JOIN city c ON p.cityid = c.cityid;`

>Right join: используется редко, можно реализовать с помощью левого . 
>"Казахи никогда не отступают назад - потому что у них в языке нет слова назад"

- Быть или не быть (EXISTS):
	`SELECT p.* FROM phone p WHERE EXISTS(SELECT 1 FROM city c WHERE c.cityid = p.cityid);` - выбрать все из телефонов, где cityid = cityid 

	`WHERE NOT EXISTS(SELECT 1 FROM city c WHERE c.cityid = p.cityid);` - несуществующие

	замена `EXISTS` на `JOIN`:  `SELECT p.*, c.cityid, c.cityname FROM phone p JOIN city c ON p.cityid = c.cityid;` - если добавить `LEFT`, то увидим несуществующие (`SELECT p.*, c.cityid cid, c.cityname FROM phone p left JOIN city c ON p.cityid = c.cityid WHERE c.cityid is null;`)

- Объединения UNION: 
	`union` - показывает уникальные имена
	`union all` - все имена
	`SELECT firstname FROM phone UNION SELECT cityname FROM city;`

- **Агрегатные функции** (это какие-то подсчеты - count, sum, average, min/max):
	*Внутренний SELECT должен возвращать только одно значение: 
	`select *, (select count(*) from phone) from phone;`* - плохой пример!
	
	`count( )` считает только ненулевые значения
	
	`select count(cityid) count, sum(phoneid) sum, min(phoneid) min, max(phoneid) max, avg(phoneid) avg from phone;`
	
	![[Screenshot 2024-09-18 at 8.19.00 PM.png]]

- Группировка данных GROUP BY:
	Работает как `distinct`, смотрит какие есть уникальные значения в бд, а потом по этим данным начинает считать или собирать, что мы просим

	`SELECT DISTINCT cityid FROM phone;`
	`SELECT cityid FROM phone GROUP BY cityid;`

- Удалить строку (DELETE):
	`delete from city where cityid = 6;`

----
### Создаем таблицы и базы данных с помощью SQL

1. `create database _name_;` 
2. Удалить базу данный:
	`drop database _name_;`
3. `create database league charset = utf8 collate utf8_general_ci;` 
	 utf8 покрывает все необходимое 
4. Подкорректировать бд:
	`alter database league charset = utf8 collate utf8_general_ci;`
5. Создать таблицу:
	```mysql 
	create table name(
		column1 char(1), 
		name varchar(20), 
		age unsigned int 	 
	)
	```
	> Использование `char` - данные структурированы, неэффективное использование пространства
	> Использование `varchar` - данные не структурированы
```
18,Li.    , -- char, будут выровнены
12,Kolinov, -- varchar
```
> `unsigned int` - использование только положительных чисел, при этом диапазон сохраняется, но теперь начинается с 0 (от -2млрд до 2млрд =>  4.2млрд < 0)


----

```mysql 
create table player(
	playerid int AUTO_INCREMENT PRIMARY KEY,
	firstname varchar(20) NOT NULL,
	lastname varchar(20),
	-- password varchar(50) CHARACTER SET utf8 -- можно еще так написать, для паролей мастхев
	phone char(10) NULL UNIQUE, -- уникальные номера, нельзя повторяться
	country char(3) default 'ru'  
) CHARACTER SET utf8; -- кодировка таблицы 
```

`playerid int PRIMARY KEY` - первичный ключ таблицы, уникальность, гарантируемая бд.
	Мы не сможем создать:
		0, Вася
		0, Вася - оле, ты нарушаешь правила первичного ключа, он не может повторяться

`playerid int AUTO_INCREMENT PRIMARY KEY` - бд автоматически возьмет следующее значение
	-> 0
	-> 1
	-> 2
	-> 3


`mysql> describe player;` - прикол mysql, может не выполниться в другой 
	![[Screenshot 2024-09-19 at 1.58.29 PM.png]]

- Изменение колонки:
	1. добавление колонки - `alter table player add position varchar(20)` 
	2. удалить колонку - `alter table player drop position1`
	3. изменение типа колонки - `alter table player modify position int not null;`
	>при конвертации mysql не позволяет конвертнуть null в число
	

