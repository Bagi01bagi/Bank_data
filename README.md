# Bank_data
Kaggle Datasets — скачала данные  "bank data"   Bank Data — содержит информацию о клиентах и их взаимодействии с банком, и я буду его использовать для анализа


1.Для этого для начала создала схему на Postgres на DBeaver
Create  Schema bank_data ;
 
Потом создала таблицу  изучая данные в csv файле старалась дать данным их тип
 CREATE TABLE bank_customers (
   age SMALLINT,
   job VARCHAR(20),
   marital VARCHAR(20),
   education VARCHAR(20),
   "default" VARCHAR(20),  -- Обратите внимание на кавычки
   housing varchar(10),
   loan varchar(20),
   contact VARCHAR(20),
   month VARCHAR(10),
   day_of_week VARCHAR(20),
   duration SMALLINT,
   campaign VARCHAR(20),
   pdays REAL,
   previous REAL,
   poutcome VARCHAR(20),
   emp_var_rate DOUBLE PRECISION,
   cons_price_idx DOUBLE PRECISION,
   cons_conf_idx DOUBLE PRECISION,
   euribor3m DOUBLE PRECISION,
   nr_employed DOUBLE PRECISION,
   y varchar(10)
);


2. COPY bank_customers
FROM 'C:\Users\Adm\Desktop\data.csv'
DELIMITER ','
CSV HEADER;

Через этот код загрузила данные . HEADER  помогла postgres 1 столбец взять кака заголовку

3.  

 INSERT INTO bank_customers (age, job, marital, education, "default", housing, loan, contact, month, day_of_week, duration, campaign, pdays, previous, poutcome, emp_var_rate, cons_price_idx, cons_conf_idx, euribor3m, nr_employed, y)
SELECT
   age,
   job,
   marital,
   education,
   "default",
   housing,  
   loan,     
   contact,
   month,
   day_of_week,
   duration,
   campaign,
   pdays,
   previous,
   poutcome,
   emp_var_rate,
   cons_price_idx,
   cons_conf_idx,
   euribor3m,
   nr_employed,
   y         
FROM bank_customers;

4.select*from bank_customers;  
Проверяем провильно ли идем

5.

1. Общее количество клиентов по возрастным категориям


SELECT
   CASE
       WHEN age < 18 THEN 'Дети (до 18)'
       WHEN age BETWEEN 18 AND 25 THEN 'Молодежь (18-25)'
       WHEN age BETWEEN 26 AND 35 THEN 'Взрослые (26-35)'
       WHEN age BETWEEN 36 AND 45 THEN 'Старшие взрослые (36-45)'
       WHEN age BETWEEN 46 AND 55 THEN 'Пожилые (46-55)'
       WHEN age > 55 THEN 'Пенсионеры (56+)'
   END AS age_group,
   COUNT(*) AS count
FROM bank_customers
GROUP BY age_group
ORDER BY age_group;




И СОЗДАЛА ТАКУЮ ВИЗУАЛИЗАЦИЮ


3, Ср возраст, мин возраст. Распределение возраста с использованием агрегированных данных
SELECT AVG(age) AS average_age FROM bank_customers;
SELECT MIN(age) AS min_age, MAX(age) AS max_age FROM bank_customers;


SELECT age, COUNT(*) AS count
FROM bank customers
GROUP BY age
ORDER BY age;

4. Возраст и наличие жилищного кредита

SELECT
   housing,
   AVG(age) AS average_age
FROM bank customers
GROUP BY housing;


Для выполнения анализа среднего возраста по категориям профессий и семейного положения в таблице bank customers, вы можете использовать SQL-запрос, который объединяет обе категории. Вот пример запроса, который позволит вам получить эту информацию:


SELECT
   job,
   marital,
   AVG(age) AS average_age
FROM bank customers
GROUP BY job, marital
ORDER BY job, marital;


Для сравнения доли клиентов с жилищными (housing) и потребительскими (loan) кредитами в таблице bank customers, вы можете использовать SQL-запрос для вычисления процентного соотношения клиентов с каждым типом кредита. Вот как это можно сделать:


SELECT
   SUM(CASE WHEN housing = 'yes' THEN 1 ELSE 0 END) AS housing_count,
   SUM(CASE WHEN loan = 'yes' THEN 1 ELSE 0 END) AS loan_count,
   COUNT(*) AS total_count,
   ROUND(SUM(CASE WHEN housing = 'yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS housing_percentage,
   ROUND(SUM(CASE WHEN loan = 'yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS loan_percentage
FROM bank_customers;


Коло-во уникальных job и общее кол-во по нему


select distinct job , count(* )as count
from bank customers
group by job;


Тут я пыталась включить cons_price_idx ,cons_conf_idx ,euribor3m ,nr_employed
по месяцам


select  "month" , cons_price_idx ,cons_conf_idx ,euribor3m ,nr_employed
from bank_customers bc
group by "month",cons_price_idx ,cons_conf_idx  ,euribor3m ,nr_employed
order by "month" ;

Здесь я  получила кол-во у кого есть housing и loan по опросам  и у кого  нет их 
select distinct housing, loan , count(*)as count
from bank_customers bc
group by housing,loan;


Тут я получила сколько женатых ,неженатых,разведенных


select distinct  (marital), count(*) as count
from bank_customers bc
group by marital  ;



На основе полученных таблиц в sql запросе я решила через power bi создать визуализацию  и для этого я подключилась на postgres в power bi и внизу написала sql запрос  и получала данные и их визуализировала


![Image alt](https://github.com/Bagi01bagi/Bank_data/blob/main/data_bank.png)
