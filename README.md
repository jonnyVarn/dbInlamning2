### inlämningsuppgift 2

#### MySQL 
1:
insert into locations (country, address) values ("SE", "Vimmerbygatan 20"), ("US", "Asteroid road 5"), ("US", "Comet road 42"), ("SE", "Brunnsgatan 7");
varchar(256));
---
#### MongoDB

#### 
2: 
Skapa upp en till tabell i MySQL / MariaDB.
---
CREATE TABLE relations (
location_ID int PRIMARY KEY NOT NULL, 
bankkonto_ID int UNIQUE NOT NULL, 
FOREIGN KEY (location_ID) REFERENCES locations(id), 
FOREIGN KEY (bankkonto_ID) REFERENCES bank_accounts(id)
);
---

SELECT  * FROM bank_accounts WHERE first_name="Corbin" or first_name="Vanya" or first_name="Eldon" or first_name="Ingunna";
