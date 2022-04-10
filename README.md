# test

**esercizio nazioni**

create table nazioni (
Code char(3) primary key,
Name char(52),
Continent char (50),
Region char(26),
SurfaceArea int,
population int
);
drop table nazioni;

insert into nazioni (code, name, Continent, region, SurfaceArea, Population)
select code, name, Continent, region, SurfaceArea, Population from world.country;

delete from nazioni where Continent= 'Asia';

select * from nazioni;

create table continenti (
NomeContinente char (52)  primary key ,
Popolazione int
);
drop table continenti;

select * from world.country;

insert into continenti (  nomecontinente, popolazione)
values ('Africa', '784475000'), ('Europe', '730074600'), ('Oceania', 30401150);

delete from continenti;
select continent, sum(population)
from  nazioni inner join continenti on nazioni.continent = continenti.NomeContinente
group by continent;

select * from continenti;
select * from nazioni;

**esercizio filmati** 

create table filmati(
id int unsigned auto_increment primary key,
titolo varchar (255),
duratanoleggio decimal (10,4),
durata int,
classificazione char (5));

insert into filmati ( id, titolo,duratanoleggio,durata,classificazione)
select film_id, title, rental_duration,length, rating
from sakila.film
WHERE sakila.film.replacement_cost >= 10.0 and sakila.film.replacement_cost <= 20.0;

delete from filmati;

create table dati_valutazioni(
classificazione char (5),
numero int
);

insert into dati_valutazioni (classificazione, numero )
select  classificazione, count(*)
from filmati
group by classificazione
having count(*)<100;

delete from dati_valutazioni ;
select * from dati_valutazioni ;

create view v_dati_valutazioni as
select  classificazione, count(*)
from filmati
group by classificazione
having count(*)<100;

**esercizio analisi_categorie** 

select * from analisi_categorie;
select * from categorie ;
drop table analisi_categorie;

create table analisi_categorie (
categoria int auto_increment primary key,
numero int
);

insert into analisi_categorie(categoria , numero )
select  category_id, count(*)
from sakila.film_category
group by  category_id
having count(*)>60;

create view v_analisi_categorie as
select categoria, numero, name_it
from analisi_categorie join categorie on categoria = cat_id;

**esercizio film** 

create  table  film(
id_film int unsigned auto_increment primary key,
titolo  varchar (255),
anno year (4),
lingua_id int,
durata int
);

Insert into film ( id_film, titolo, anno, lingua_id, durata)
select film_id, title, release_year, language_id,rental_duration from sakila.film ;

delete from film where durata = 3;

create table tipi_durate (
id_durata int primary key,
tipo_durata char (10));

insert into tipi_durate (id_durata,tipo_durata) VALUES (0,'bassa'),(1,'bassa'),(2,'bassa'), (3,'media'),(4,'media'),(5,'media'),(6,'media'),(7,'alta'),(8,'alta'),(9,'alta');

insert into tipi_durate ( tipo_durata, id_durata)
select tipo_durata, count(*)
from film inner join tipi_durate on durata = id_durata group by tipo_durata;

create  table frequenza (
durata int primary key,
n_film int

);
insert into frequenza (durata, n_film)
select durata, count(*)
from film
group by durata ;

select * from film;
select* from frequenza;

**esercizio * 

create table payment_status as
select max(last_name) as last_name, count(*) noofpayaments from sakila.payment
join sakila.staff on payment.staff_id = staff.staff_id
group by payment.staff_id;

- - creare una vista denominata v_actors_status con i seguenti campi i cui dati sono ricavabili dalla tabella film _actor:
-- actor_id identificativo attore
- - no_of_films il conteggio dei films a cui ha partecipato ciasun attore (cioè il numero di righe in cui è presente ciascun actor_id )

create view v_actors_status  as
select actor_id, count(*) from sakila.film_actor
group by actor_id
having count(*) > 30 ;
