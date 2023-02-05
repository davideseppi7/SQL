
# SQL query

by Davide Seppi




## Deployment

To deploy this project run

Creare e cancellare databese
```sql
CREATE DATABASE_db_prova; --crea databese
DROP DATABASE db_prova; --elimina databese
```

Creare e eliminare tabelle
```sql
CREATE TABLE dipendenti (
id_dipendente int not null AUTO_INCREMENT,
telefono varchar(10) not null unique,
mansione varchar(255) not null default='impiegato'
stipendio decimal not null check (stipendio >= 1200 AND stipendio <= 5000),
PRIMARY KEY(id_dipendente),
FOREIGN KEY(id_dipendente) REFERENCES rapporto_clienti(id_dipendente)
);

DROP TABLE nometabella; --cancella tabell

CREATE TABLE IF NOT EXISTS dipendenti (
id_dipendente int not null AUTO_INCREMENT,
telefono varchar(10) not null unique,
mansione varchar(255) not null default='impiegato'
stipendio decimal not null check (stipendio >= 1200 AND stipendio <= 5000),
PRIMARY KEY(id_dipendente),
FOREIGN KEY(id_dipendente) REFERENCES rapporto_clienti(id_dipendente)
);
```
Insert
```sql
INSERT INTO clienti (denominazione, p_iva, indirizzo, telefono) 
VALUES ("Xyz Impianti", "9045234478", "Via Roma, Cesena", "1134600067"); --inserire (),(),.... per insert multipli
```
Select
```sql
SELECT *   --*=tutto
FROM clienti;

SELECT denominazione, p_iva, telefono FROM clienti;
```
Where, AND, OR, NOT
```sql
SELECT * FROM dipendenti WHERE stipendio › 1500 AND...; 
```
In, Between
```sql
SELECT *
FROM dipendenti
WHERE mansione IN ('impiegato', 'commerciale'); --solo quelli nella lista

SELECT *
FROM dipendenti
WHERE data_assunzione BETWEEN "2014" AND "2016;
```
Order
```sql
SELECT *
FROM dipendenti
ORDER BY data_assunzione,stipendio ASC/DESC;  --ascendente, discendente (solo uno)
```
Limit
```sql

SELECT *
FROM dipendenti
ORDER BY data_assunzione,stipendio ASC LIMIT 2; --solo 2 righe

SELECT *
FROM dipendenti
ORDER BY data_assunzione,stipendio ASC LIMIT 1,2; --salta la prima e dammene 2
```
Select senza duplicati, Count
```sql
SELECT DISTINCT citta FROM clienti; --solo città divese

SELECT  count(citta) FROM clienti; --conta città con ripetizione

SELECT  count(DISTINCT citta) FROM clienti; --conta città SENZA ripetizione
```
Update
```sql
UPDATE clienti
SET indirizzo = 'Via Augusto', citta="Roma"
WHERE id_cliente = 3;   --senze where vengono tutti cambiati
```
Delate
```sql
DELETE FROM dipendenti WHERE mansione = 'commerciale' AND...; --senza where vengono eliminati tutti; non resetta gli indici; scorre riga per riga

TRUNCATE TABLE clienti;--elimina tabella e resetta indici; non scorre riga per riga
```
Join
```sql
--A intersecato B
SELECT dipendenti.id_dipendente, dipendenti.nome, uffici.nome_ufficio --dati dientrabme le tabelle
FROM dipendenti   --tabella 1
INNER JOIN uffici ON dipendenti.id_ufficio = uffici.id_ufficio; --tabella 2 ON condizione

--A untito (A intersecato B)
SELECT dipendenti.id_dipendente, dipendenti.nome, uffici.nome_ufficio
FROM dipendenti
LEFT JOIN uffici ON dipendenti.id_ufficio = uffici.id_ufficio;

--B untito (A intersecato B)
SELECT dipendenti.id_dipendente, dipendenti.nome, uffici.nome_ufficio
FROM dipendenti
RIGHT JOIN uffici ON dipendenti.id_ufficio = uffici.id_ufficio;
```
Like
```sql
SELECT * FROM clienti
WHERE denominazione LIKE 'R%';

/*Comincia con 'Cer%'
finisce con '%C'
Contiene '%C%'
caratteri fissi e comincia con 'C_'
caratteri fissi e finisce con '_C'
caratteri fissi e contiene '_C_'
contiene stringa e finisce con C uno "%C_'
contiene stringa e inizia con uno '_C%'*/

--es: 'ca__etta%s'
```
Modify table
```sql
ALTER TABLE rapporto_cliente ADD prova varchar(50) not null; --aggiungere colonna

ALTER TABLE rapporto_clienti MODIFY prova varchar(50) AFTER id_rapporto; --cambiare posizione colonna

ALTER TABLE rapporto_clienti ADD UNIQUE(prova); --aggiungere constraint es:PRIMARY KEY

ALTER TABLE rapporto_clienti DRPO COLUM (prova); --elimina colonna

ALTER TABLE rapporto_clienti MODIFY int; --modificare tipo di dati

ALTER TABLE rapporto_clienti RENAME rapporto_persone --rinominare
```
Alias
```sql
SELECT x.id_dipendente, x.nome, y.nome_ufficio-- sostituisce i nomi delle tabelle con altri
FROM dipendenti AS x
INNER J0IN uffici AS y
ON dipendenti.id_ufficio = uffici. id_ufficio;

SELECT nome, DATE_FORMAT(data_assunzione, '%e %M, %Y') AS data_assunzione FROM dipendenti; --rinomina la colonna restituita(non quellle reali) con data_assunzione
```
Gruup by, Having
```sql
SELECT X. nome_ufficio, count(y.id_dipendente) AS totale_dipendenti --conta dipendenti per ufficio e chiama la colonna restituita totale_dipendenti
FROM uffici AS x
LEFT JOIN dipendenti AS y.
ON X.id ufficio = y. id_ufficio
GROUP BY X.nome_ufficio; --raggruppa gli uffici con lo stesso nome

SELECT X. nome_ufficio, count(y.id_dipendente) AS totale_dipendenti --conta dipendenti per ufficio e chiama la colonna restituita totale_dipendenti
FROM uffici AS x
LEFT JOIN dipendenti AS y.
ON X.id ufficio = y. id_ufficio
GROUP BY X.nome_ufficio; --raggruppa gli uffici con lo stesso nome
HAVING totale_dipendenti = 2; --solo gli uffici con 2 dipendenti
```
View
```sql
CREATE VIEW prova AS    --creo una view di nome prova
SELECT x. id_dipendente, x.nome, y.id_ufficio
FROM dipendenti AS X
LEFT J0IN uffici AS y
ON x.id_ufficio = y. id_ufficio;

SELECT * FROM prova; --utilizzo la view

CREATE OR REPLACE VIEW prova AS
SELECT * FROM dipendenti;  --cambio il contenuto della view
```
Time
```sql
CREATE TABLE IF NOT EXISTS prova(
id INT(4) NOT NULL PRIMARY KEY AUTO_ INCREMENT,
nome VARCHAR (50) NOT NULL UNIQUE,
data nascita DATE NOT NULL,
data inserimento DATETIME DEFAULT CURRENT TIMESTAMP, --orario attuale in automatico
data_modi fica TIMESTAMP DEFAULT CURRENT TIMESTAMP ON UPDATE CURRENT TIMESTAMP --orario attuale in automatico
);

INSERT INTO prova (nome, data nascita, data inserimento) VALUES ('Luca', '1995-06-06', NOW()); --inserire il momento attuale

SELECT nome, YEAR(data_nascita) FROM prova; --estrae dalla data solo l'anno es:YEAR(), MONTH(), DAYOFMONTHO(), MONTHNAME(), DAYNAME(), HOUR(), MINUTE(), SECOND()

SELECT nome, DATE_FORMAT(data_nascita, "%M,%e,%Y') AS datanascita FROM prova; --es: restituisce Februar 6,1967
```
Clonare tabelle
```sql
CREATE TABLE tabella_clonata_uno LIKE prova; --clona la tabella prova, costraint compresi
INSERT INTO tabellalonata_uno SELECT * FROM prova; --copia i valori nella nuova tabella

CREATE TABLE tabella_clonata semplice SELECT * FROM prova; --clona solo la struttura, senza costraint
```
Tabelle temporanee
```sql
CREATE TEMPORARY TABLE temp1( --la tabella si autoelimina alla chiusura della sessione
id INT PRIMARY KEY,
name VARCHAR (50) NOT NULL,
gender VARCHAR (50) NOT NULL,
age INT NOT NULL,
total_score INT NOT NULL,
);

DROP TEMPORARY TABLE temp1; --eliminare tabella temporanea
```

