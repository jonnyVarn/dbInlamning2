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
INSERT INTO relations (bankkonto_ID, location_ID) VALUES (55,4), (89, 2), (174, 1),  (170, 3);
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
```javascript
db.locations.find({"country":"SE"});
{ "_id" : ObjectId("602b0b95a16a1ffdd4c86aa2"), "country" : "SE", "Address" : "Vimmerbygatan 20", "person" : DBRef("bank_accounts", ObjectId("602a111b9c99542f1ba383c1")) }
{ "_id" : ObjectId("602b0b95a16a1ffdd4c86aa5"), "country" : "SE", "Address" : "Brunnsgatan 7", "person" : DBRef("bank_accounts", ObjectId("602a10ca9c99542f1ba383bf")) }

 db.bank_accounts.find({ _id: { $in: [ ObjectId("602a111b9c99542f1ba383c1") ] } } )
 { "_id" : ObjectId("602a111b9c99542f1ba383c1"), "first_name" : "Eldon", "last_name" : "McCartan", "holding" : "9998" }
 db.bank_accounts.find({ _id: { $in: [ ObjectId("602a10ca9c99542f1ba383bf") ] } } )
 { "_id" : ObjectId("602a10ca9c99542f1ba383bf"), "first_name" : "Corbin", "last_name" : "Hauck", "holding" : "9999", "home_address" : DBRef("locations", ObjectId("601f3493bd1041f8f16667ff")) }
``` 
---



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
DBRef eller manuell reference. Enligt Mongo ovan.

4. Vad behöver du för information för att kunna logga in i någon annans databas?
Ip eller hostname username password och portnummer eventuellt databas namn som jag har tillgång till.

5. Varför skulle man vilja använda sig utav en databas?
För att enkelt kunna komma åt data sortera, ändra etc.


6. Nämn några andra ställen / situationer utöver databaser som CRUD används
HTTP (POST, GET, PUT, and DELETE), FILSYSTEM Create, Read, Update and Delete etc. 

--

```text
Rapport Inlämning2:
Jag lägger i MySQL in i tabellen locations genom att i terminalen skriva mysql -u root -p.
Jag skriver use bank_accounts; och sedan 
insert into locations (country, address) values ("SE", "Vimmerbygatan 20"), ("US", "Asteroid road 5"), ("US", "Comet road 42"), ("SE", "Brunnsgatan 7");
för att sätta in värden i tabellen locations.
Jag tänker sedan att jag vill ha en tabell som håller reda på relationer som jag döper till relations, en key som ska ha reference på bör ju vara unique om den inte har inodb som Engine men för enkelhet skull så får de vara primary och unique.
Sedan så står det i uppgiften att det inte ska inehålla mer än bankkontoID och locationsID så då är det det enda det innehåller.
CREATE TABLE relations ( location_ID int PRIMARY KEY NOT NULL, bankkonto_ID int UNIQUE NOT NULL, FOREIGN KEY (location_ID) REFERENCES locations(id), FOREIGN KEY (bankkonto_ID) REFERENCES bank_accounts(id));

Sedan ska jag tydligen leta reda på Corbin, Vanya Eldon och Ingunna och placera de på vissa adresser då gör jag:
SELECT  * FROM bank_accounts WHERE first_name="Corbin" or first_name="Vanya" or first_name="Eldon" or first_name="Ingunna"; 
 +-----+------------+-------------+---------+  
 | id  | first_name | last_name   | holding |  
 +-----+------------+-------------+---------+  
 |  55 | Corbin     | Hauck       |  449092 |  
 |  89 | Vanya      | Worsell     |  330641 |  
 | 170 | Ingunna    | Castellucci |  471372 |  
 | 174 | Eldon      | McCartan    |   75096 |

Sedan ska de ju bo på visa addresser så då kollar jag det med.
SELECT * FROM locations; 
+----+---------+------------------+ 
| id | country | address          | 
+----+---------+------------------+ 
|  1 | SE      | Vimmerbygatan 20 | 
|  2 | US      | Asteroid road 5  | 
|  3 | US      | Comet road 42    | 
|  4 | SE      | Brunnsgatan 7    |

Sedan hittar jag var Corbin etc ska bo så då lägger jag in frågan.

INSERT INTO relations (bankkonto_ID, location_ID) VALUES (55,4), (89, 2), (174, 1),  (170, 3);

Sedan försöker jag hitta på en fråga som passar:

select first_name, last_name, address, country, location_ID, bankkonto_ID from locations, bank_accounts, relations where locations.id=relations.location_ID and country="SE" and bankkonto_ID=bank_accounts.id;

och eftersom vi talade om as så var jag tvungen att fråga med as så att det blir lite mer lättläsligt:
SELECT * FROM bank_accounts AS ba LEFT JOIN relations AS br on ba.id=br.bankkonto_ID \
LEFT JOIN locations AS lt ON br.location_ID=lt.id WHERE country="SE"; \

Mongo delen är lite svårare för mig men jag börjar med mongo terminalen genom att skriva mongo
För att skapa och lägga in i locations så skriver jag:
db.locations.insertMany([{"Country":"SE"}, {"address":"vimmerbygatan 20"}, {"Country":"US"}, {"address":"Asteroid road 5"}, {"Country": "US"}, {"address":"Comet road 41"},  {"Country": "SE"}, {"address":"Brunnsgatan 7"} ]);

sedan försöker jag hitta i bank_accounts

db.bank_accounts.find({$or:[{"first_name":"Corbin"}, {"first_name":"Vanya"},{"first_name":"Eldon"}, {"first_name":"Ingunna"} ]});

men hittar inget så då lägger jag in en i taget.
db.bank_accounts.insertOne({"first_name" : "Corbin", "last_name" : "Hauck", "holding" : "9999"})  
db.bank_accounts.insertOne({"first_name":"first_name" : "Vanya", "last_name" : "Worsell", "holding" : "9997"})  
db.bank_accounts.insertOne({"first_name" : "Eldon", "last_name" : "McCartan", "holding" : "9998"})
db.bank_accounts.insertOne({"first_name" : "Ingunna", "last_name" : "Castellucci", "holding" : "8881"}

och sedan söker jag igen.

db.bank_accounts.find({$or:[{"first_name":"Corbin"}, {"first_name":"Vanya"},{"first_name":"Eldon"}, {"first_name":"Ingunna"} ]});  \

{ "_id" : ObjectId("602a10ca9c99542f1ba383bf"), "first_name" : "Corbin", "last_name" : "Hauck", "holding" : "9999" } \
{ "_id" : ObjectId("602a11079c99542f1ba383c0"), "first_name" : "Vanya", "last_name" : "Worsell", "holding" : "9997" } \
{ "_id" : ObjectId("602a111b9c99542f1ba383c1"), "first_name" : "Eldon", "last_name" : "McCartan", "holding" : "9998" } \
{ "_id" : ObjectId("602a11339c99542f1ba383c2"), "first_name" : "Ingunna", "last_name" : "Castellucci", "holding" : "8881" } \


Och jag får fram den här infon:

Sedan tänker jag lägga in relations men jag tänker fel så jag lägger in dbref i locations först så tömmer jag den med db.locations.remove(””).

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


Sedan har jag inte helt koll på hur jag skriver en bra fråga så det får bli en två stegs raket.
db.locations.find({"country":"SE"});
{ "_id" : ObjectId("602b0b95a16a1ffdd4c86aa2"), "country" : "SE", "Address" : "Vimmerbygatan 20", "person" : DBRef("bank_accounts", ObjectId("602a111b9c99542f1ba383c1")) }
{ "_id" : ObjectId("602b0b95a16a1ffdd4c86aa5"), "country" : "SE", "Address" : "Brunnsgatan 7", "person" : DBRef("bank_accounts", ObjectId("602a10ca9c99542f1ba383bf")) }

 db.bank_accounts.find({ _id: { $in: [ ObjectId("602a111b9c99542f1ba383c1") ] } } )
 { "_id" : ObjectId("602a111b9c99542f1ba383c1"), "first_name" : "Eldon", "last_name" : "McCartan", "holding" : "9998" }
 db.bank_accounts.find({ _id: { $in: [ ObjectId("602a10ca9c99542f1ba383bf") ] } } )
 { "_id" : ObjectId("602a10ca9c99542f1ba383bf"), "first_name" : "Corbin", "last_name" : "Hauck", "holding" : "9999", "home_address" : DBRef("locations", ObjectId("601f3493bd1041f8f16667ff")) }
´´´´


