 ####

 
 inlämningsuppgift 2
-
 MySQL 
 1:
 ```sql
 insert into locations (country, address) values ("SE", "Vimmerbygatan 20"), ("US", "Asteroid road 5"), ("US", "Comet road 42"), ("SE", "Brunnsgatan 7");
 ```
-
MongoDB
 ```javascript


 db.locations.insertMany([{"Country":"SE"}, {"address":"vimmerbygatan 20"}, {"Country":"US"}, {"address":"Asteroid road 5"}, {"Country": "US"}, {"address":"Comet road 41"},  {"Country": "SE"}, {"address":"Brunnsgatan 7"} ]);
 ```
 2: 
Skapa upp en till tabell i MySQL / MariaDB.
-----
```sql
CREATE TABLE relations ( location_ID int PRIMARY KEY NOT NULL, bankkonto_ID int UNIQUE NOT NULL, FOREIGN KEY (location_ID) REFERENCES locations(id), FOREIGN KEY (bankkonto_ID) REFERENCES bank_accounts(id));
```
-----
```sql
SELECT  * FROM bank_accounts WHERE first_name="Corbin" or first_name="Vanya" or first_name="Eldon" or first_name="Ingunna"; 
 +-----+------------+-------------+---------+  
 | id  | first_name | last_name   | holding |  
 +-----+------------+-------------+---------+  
 |  55 | Corbin     | Hauck       |  449092 |  
 |  89 | Vanya      | Worsell     |  330641 |  
 | 170 | Ingunna    | Castellucci |  471372 |  
 | 174 | Eldon      | McCartan    |   75096 | 
```
-----
```sql
SELECT * FROM locations; 
+----+---------+------------------+ 
| id | country | address          | 
+----+---------+------------------+ 
|  1 | SE      | Vimmerbygatan 20 | 
|  2 | US      | Asteroid road 5  | 
|  3 | US      | Comet road 42    | 
|  4 | SE      | Brunnsgatan 7    |
```
-----
```sql
INSERT INTO relations (location_ID, bankkonto_ID) VALUES (55,4), (89, 2), (174, 1),  (170, 3);
```
-----
 2 skapa upp en tabell i MongoDB
-----
```javascript
db.bank_accounts.find({$or:[{"first_name":"Corbin"}, {"first_name":"Vanya"},{"first_name":"Eldon"}, {"first_name":"Ingunna"} ]});  
```

--
###### Resulted in nothing so i created the users:
--

```javascript
 db.bank_accounts.insertOne({"first_name" : "Corbin", "last_name" : "Hauck", "holding" : "9999"}) \ db.bank_accounts.insertOne({"first_name":"first_name" : "Vanya", "last_name" : "Worsell", "holding" : "9997"}) \ db.bank_accounts.insertOne({"first_name" : "Eldon", "last_name" : "McCartan", "holding" : "9998"}) \ db.bank_accounts.insertOne({"first_name" : "Ingunna", "last_name" : "Castellucci", "holding" : "8881"})
```
-- 
###### and then again:
--
```javascript
 db.bank_accounts.find({$or:[{"first_name":"Corbin"}, {"first_name":"Vanya"},{"first_name":"Eldon"}, {"first_name":"Ingunna"} ]});  \

{ "_id" : ObjectId("602a10ca9c99542f1ba383bf"), "first_name" : "Corbin", "last_name" : "Hauck", "holding" : "9999" } \
{ "_id" : ObjectId("602a11079c99542f1ba383c0"), "first_name" : "Vanya", "last_name" : "Worsell", "holding" : "9997" } \
{ "_id" : ObjectId("602a111b9c99542f1ba383c1"), "first_name" : "Eldon", "last_name" : "McCartan", "holding" : "9998" } \
{ "_id" : ObjectId("602a11339c99542f1ba383c2"), "first_name" : "Ingunna", "last_name" : "Castellucci", "holding" : "8881" } \
```
--
```javascript
 db.locations.insertMany([
...     {
...         "country": "SE",
...         "Address": "Vimmerbygatan 20",
...         bankID:
...         {
...             $ref:  "bank_accounts",
...             $id: ObjectId("602a111b9c99542f1ba383c1")
...         }
...     },
...     {
...         "country": "US",
...         "Address": "Asteroid road 5",
...         bankID:
...         {
...             $ref: "bank_accounts",
...             $id: ObjectId("602a11079c99542f1ba383c0")
...         },
...     },
...     {
...         "country": "US",
...         "Address": "Comet road 41",
...          bankID:
...         {
...             $ref: "bank_accounts",
...             $id: ObjectId("602a11339c99542f1ba383c2")
...         },
...     },
...     {
...         "country": "SE",
...         "Address": "Brunnsgatan 7",
...          bankID:
...         {
...             $ref: "bank_accounts",
...             $id: ObjectId("602a10ca9c99542f1ba383bf")
...         },
...     }
... ])
```
-----





 Del 3
 Nu skall vi utföra sökningar på vår data.
 Skriv en fråga i MySQL som hämtar alla bank-konton som är kopplade till “locations” där country är “SE”.
-----
```sql
 select first_name, last_name, address, country, location_ID, bankkonto_ID from locations, bank_accounts, relations where locations.id=relations.location_ID and country="SE" and bankkonto_ID=bank_accounts.id; \
```
alternativt:
```sql
SELECT * FROM bank_accounts AS ba LEFT JOIN relations AS br on ba.id=br.bankkonto_ID \
LEFT JOIN locations AS lt ON br.location_ID=lt.id WHERE country="SE"; \
```

 Del 4
Nu skall du visa förståelse på CRUD.
Skriv i din rapport, exempel på MongoDB och SQL frågor som är av karaktärerna:
1. Create
```sql
create database locations (countrycode varchar(2), address varchar(50));
```
```javascript
db.createCollection("relations")
```
2. Read
```sql
SELECT * FROM RELATIONS;
```
```javascript
db.relations.find().pretty();
```
3. Update
```sql
UPDATE bank_accounts SET holding=1 where id=1;
```
```javascript
db.bank_account.update({"_id" : ObjectId("601bce6e283d5120f20e1a68")}, {$set: {"holding": 888} });
```
4. Delete
```sql  
DELETE FROM bank_accounts WHERE ID=1;
```
```javascript
db.bank_account.remove({"first_name" : "Anders"});
```
 Frågor
1. Vad är motsvarigheten i MongoDB till en foreign key?
--
DBRefs or manual references.
--
2. Vad är motsvarigheten till en SELECT i MongoDB?
--
```javascript
db.collection.find()
```
3. Hur hade du löst del 2 och 3 i MongoDB? (du behöver inte göra en komplett lösning,
men beskriv på ett ungefär hur du hade gjort)
DBRef eller manuell reference. Jag försöker få till det men jag har lite feber ;)

4. Vad behöver du för information för att kunna logga in i någon annans databas?
Ip eller hostname username password och portnummer eventuellt databas namn som jag har tillgång till.

5. Varför skulle man vilja använda sig utav en databas?
För att enkelt kunna komma åt data sortera, ändra etc.


6. Nämn några andra ställen / situationer utöver databaser som CRUD används
HTTP (POST, GET, PUT, and DELETE), FILSYSTEM Create, Read, Update and Delete etc. 

