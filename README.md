## inlämningsuppgift 2

### MySQL 
#### 1: \
#### insert into locations (country, address) values ("SE", "Vimmerbygatan 20"), ("US", "Asteroid road 5"), ("US", "Comet road 42"), ("SE", "Brunnsgatan 7");
---
### MongoDB

#### 2: \
Skapa upp en till tabell i MySQL / MariaDB.
---
CREATE TABLE relations (
location_ID int PRIMARY KEY NOT NULL, 
bankkonto_ID int UNIQUE NOT NULL, 
FOREIGN KEY (location_ID) REFERENCES locations(id), 
FOREIGN KEY (bankkonto_ID) REFERENCES bank_accounts(id)
);
---

SELECT  * FROM bank_accounts WHERE first_name="Corbin" or first_name="Vanya" or first_name="Eldon" or first_name="Ingunna"; \
+-----+------------+-------------+---------+ \
| id  | first_name | last_name   | holding | \
+-----+------------+-------------+---------+ \
|  55 | Corbin     | Hauck       |  449092 | \
|  89 | Vanya      | Worsell     |  330641 | \
| 170 | Ingunna    | Castellucci |  471372 | \
| 174 | Eldon      | McCartan    |   75096 | \

select * from locations; \
+----+---------+------------------+ \
| id | country | address          | \
+----+---------+------------------+ \
|  1 | SE      | Vimmerbygatan 20 | \
|  2 | US      | Asteroid road 5  | \
|  3 | US      | Comet road 42    | \
|  4 | SE      | Brunnsgatan 7    | \


insert into relations (location_ID, bankkonto_ID) values (55,4), (89, 2), (174, 1),  (170, 3); \

select first_name, last_name, address, country, location_ID, bankkonto_ID from locations, bank_accounts, relations where locations.id=relations.location_ID and country="SE" and bankkonto_ID=bank_accounts.id; \

alternativt: \
select * from bank_accounts as ba LEFT JOIN relations as br on ba.id=br.bankkonto_ID \
LEFT JOIN locations as lt on br.location_ID=lt.id where country="SE"; \


