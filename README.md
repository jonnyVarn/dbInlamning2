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
-----
 Resulted in nothing so i created the users:
-----
```javascript
 db.bank_accounts.insertOne({"first_name" : "Corbin", "last_name" : "Hauck", "holding" : "9999"}) \ db.bank_accounts.insertOne({"first_name":"first_name" : "Vanya", "last_name" : "Worsell", "holding" : "9997"}) \ db.bank_accounts.insertOne({"first_name" : "Eldon", "last_name" : "McCartan", "holding" : "9998"}) \ db.bank_accounts.insertOne({"first_name" : "Ingunna", "last_name" : "Castellucci", "holding" : "8881"})
 ´´´´
----- 
and then again:
-----
```javascript
 db.bank_accounts.find({$or:[{"first_name":"Corbin"}, {"first_name":"Vanya"},{"first_name":"Eldon"}, {"first_name":"Ingunna"} ]});  \

{ "_id" : ObjectId("602a10ca9c99542f1ba383bf"), "first_name" : "Corbin", "last_name" : "Hauck", "holding" : "9999" } \
{ "_id" : ObjectId("602a11079c99542f1ba383c0"), "first_name" : "Vanya", "last_name" : "Worsell", "holding" : "9997" } \
{ "_id" : ObjectId("602a111b9c99542f1ba383c1"), "first_name" : "Eldon", "last_name" : "McCartan", "holding" : "9998" } \
{ "_id" : ObjectId("602a11339c99542f1ba383c2"), "first_name" : "Ingunna", "last_name" : "Castellucci", "holding" : "8881" } \
---
 db.locations.InsertMany([
{"Country" : "SE", "address" : "vimmerbygatan 20" $ref: "bank_account", $id: ObjectId("602a111b9c99542f1ba383c1") },
{"Country" : "US", "address" : "Asteroid road 5"  $ref: "bank_account", $id: ObjectId("602a11079c99542f1ba383c0") }, 
{"Country" : "US", "address" : "Comet road 41" $ref: "bank_account", $id:  ObjectId("602a11339c99542f1ba383c2")}, 
{"Country" : "SE", "address" : "Brunnsgatan 7" $ref: "bank_account", $id:  ObjectId("602a10ca9c99542f1ba383bf")},
])
```
-----

not necessary for mongo db.createCollection("relations")



 Del 3
 Nu skall vi utföra sökningar på vår data.
 Skriv en fråga i MySQL som hämtar alla bank-konton som är kopplade till “locations” där country är “SE”.
-----
 select first_name, last_name, address, country, location_ID, bankkonto_ID from locations, bank_accounts, relations where locations.id=relations.location_ID and country="SE" and bankkonto_ID=bank_accounts.id; \

alternativt: \
SELECT * FROM bank_accounts AS ba LEFT JOIN relations AS br on ba.id=br.bankkonto_ID \
LEFT JOIN locations AS lt ON br.location_ID=lt.id WHERE country="SE"; \


 Del 4
Nu skall du visa förståelse på CRUD.
Skriv i din rapport, exempel på MongoDB och SQL frågor som är av karaktärerna:
1. Create
2. Read
3. Update
4. Delete

 Frågor
1. Vad är motsvarigheten i MongoDB till en foreign key?
2. Vad är motsvarigheten till en SELECT i MongoDB?
3. Hur hade du löst del 2 och 3 i MongoDB? (du behöver inte göra en komplett lösning,
men beskriv på ett ungefär hur du hade gjort)
4. Vad behöver du för information för att kunna logga in i någon annans databas?
5. Varför skulle man vilja använda sig utav en databas?
6. Nämn några andra ställen / situationer utöver databaser som CRUD används
