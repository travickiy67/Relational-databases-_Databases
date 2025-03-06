# Травицкий сергей
# Домашнее задание к занятию «Базы данных»

### Инструкция по выполнению домашнего задания

1. Сделайте fork [репозитория c шаблоном решения](https://github.com/netology-code/sys-pattern-homework) к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/gitlab-hw или https://github.com/имя-вашего-репозитория/8-03-hw).
2. Выполните клонирование этого репозитория к себе на ПК с помощью команды `git clone`.
3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
   - впишите вверху название занятия и ваши фамилию и имя;
   - в каждом задании добавьте решение в требуемом виде: текст/код/скриншоты/ссылка;
   - для корректного добавления скриншотов воспользуйтесь инструкцией [«Как вставить скриншот в шаблон с решением»](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md);
   - при оформлении используйте возможности языка разметки md. Коротко об этом можно посмотреть в [инструкции по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md).
4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`).
5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
6. Любые вопросы задавайте в чате учебной группы и/или в разделе «Вопросы по заданию» в личном кабинете.

Желаем успехов в выполнении домашнего задания.

---
### Легенда

Заказчик передал вам [файл в формате Excel](https://github.com/netology-code/sdb-homeworks/blob/main/resources/hw-12-1.xlsx), в котором сформирован отчёт. 

На основе этого отчёта нужно выполнить следующие задания.

### Задание 1

Опишите не менее семи таблиц, из которых состоит база данных:

- какие данные хранятся в этих таблицах;

**Фамиля, имя, отчество, должность, оклад, тип подразделения, структурное подразделение, адрес, датаа найма, проект которым занимается сотрудник**

- какой тип данных у столбцов в этих таблицах, если данные хранятся в PostgreSQL.

**serial, character varying(varchar), money, date**  

Приведите решение к следующему виду:

Сотрудники (

- идентификатор, первичный ключ, serial,
- фамилия varchar(50),
- ...
- идентификатор структурного подразделения, внешний ключ, integer).

**Код для создания таблиц** 

```
create table salary(
id SERIAL primary key,
salary money not null
);

create table job_title(
id SERIAL primary key,
job_title varchar(100) not null,
salary_id INTEGER not null,
constraint fk_salary foreign key (salary_id) references salary(id)
);

create table project (
id SERIAL primary key, 
assigned_to_project VARCHAR (100) not null 
);

create table address(
id SERIAL primary key,
branch_address varchar(100) not null
);

create table unit_type(
id SERIAL primary key,
unit_type VARCHAR(100) not null
);

create table subdivision (
id SERIAL primary key,
structural_division VARCHAR(200) not null, 
unit_type_id int not null,
constraint fk_unit_type foreign key (unit_type_id) references unit_type(id)
);

create table date(
id SERIAL primary key,
date_of_hiring date not null
);

create table employees(
id SERIAL primary key,
last_name varchar(50) not null,
first_name varchar(50) not null,
surname varchar(50) not null,
job_title_id int not null, 
constraint fk_job_title foreign key (job_title_id) references job_title(id),
structural_division_id int not null,
constraint fk_subdivision foreign key (structural_division_id) references subdivision(id),
date_of_hiring_id int not null,
constraint fk_date_of_hiring foreign key (date_of_hiring_id) references date(id),
branch_address_id int not null,
constraint fk_branch_address foreign key (branch_address_id) references address(id),
assigned_to_project_id int not null,
constraint fk_project foreign key (assigned_to_project_id) references project(id)
);

insert into date (date_of_hiring)  
 values ('02.01.2023') returning id;

insert into salary (salary)
 values (100000) returning id;

 insert into job_title (job_title, salary_id)
 values ('Специалист', 1) returning id;
 
 insert into project (assigned_to_project)
 values ('NO') returning id;
 
 insert into address(branch_address)
 values ('Москва') returning id;
 
 insert into unit_type(unit_type)
 values ('Отдел') returning id;
 
 insert into subdivision (structural_division, unit_type_id)
 values ('it_отдел', 1) returning id;

 insert into employees (last_name, first_name, surname, job_title_id, structural_division_id, date_of_hiring_id, branch_address_id, assigned_to_project_id)
 values ('Травицкий', 'Сергей', 'Владимирович', 1, 1, 1, 1, 1);
 
```
**Скрин 1-3**  

![img](https://github.com/travickiy67/Relational-databases-_Databases/blob/main/img/img1.1png.png) 
 
![img](https://github.com/travickiy67/Relational-databases-_Databases/blob/main/img/img1.2png.png)  

```
select  last_name, first_name, surname, salary, job_title, unit_type, structural_division, date_of_hiring, branch_address, assigned_to_project FROM employees, subdivision, unit_type, address, project, job_title, salary, date;
```

![img](https://github.com/travickiy67/Relational-databases-_Databases/blob/main/img/img1.3png.png)  
 

