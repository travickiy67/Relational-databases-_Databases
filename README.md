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

Заказчик передал вам ![файл в формате Excel](https://github.com/travickiy67/Relational-databases-_Databases/blob/main/files/hw-12-1.xlsx), в котором сформирован отчёт. 

На основе этого отчёта нужно выполнить следующие задания.

### Задание 1

Опишите не менее семи таблиц, из которых состоит база данных:

- какие данные хранятся в этих таблицах;
- какой тип данных у столбцов в этих таблицах, если данные хранятся в PostgreSQL.

Приведите решение к следующему виду:

Сотрудники (

- идентификатор, первичный ключ, serial,
- фамилия varchar(50),
- ...
- идентификатор структурного подразделения, внешний ключ, integer).

**Пересмотреев еще раз лекцию и в результате долгих поисков в интернете своял такой код. Дату приема на работу в отдельную таблицу не выводил. Код рабочий. Таблица получилась. Первая табличка в этом коде создается последней. Она ссылается на все таблицы**  

```
create table Сотрудники(
id SERIAL primary key,
Фамилия varchar(50) not null,
Имя varchar(50) not null,
Отчество varchar(50) not null,
Оклад_id int not null,
constraint fk_оклад foreign key(Оклад_id) references Оклад(id),
Должность_id int not null,
constraint fk_должность foreign key(Должность_id) references Должность(id),
Тип_подразделения_id int not null,
constraint fk_тип_подразделения foreign key(Тип_подразделения_id) references Тип_подразделения(id),
Структурное_подразделение_id int not null,
constraint fk_подразделения foreign key(Структурное_подразделение_id) references Подразделение(id),
Дата_найма date not null,
Адрес_филиала_id int not null,
constraint fk_адрес_филиала foreign key(Адрес_филиала_id) references Адрес(id),
Проект_на_который_назначен_id int not null,
constraint fk_project foreign key(Проект_на_который_назначен_id) references Проект(id)
)

create table Оклад(
id SERIAL primary key,
Оклад money not null
)

create table Должность (
id SERIAL primary key,
Должность varchar(100) not null,
Оклад_id INTEGER not null,
constraint fk_оклад foreign key(Оклад_id) references Оклад(id)
)

create table Проект (
id SERIAL primary key, 
Проект_на_который_назначен VARCHAR(100) not null 
)

create table Подразделение (
id SERIAL primary key,
Структурное_подразделение VARCHAR(200) not null 
)

create table Тип_подразделения (
id SERIAL primary key,
Тип_подразделения VARCHAR(100) not null
)


create table Адрес(
id SERIAL primary key,
Адрес_филиала varchar(100) not null,
Структурное_подразделение_id int not null,
constraint fk_структурное_подразделение foreign key(Структурное_подразделение_id) references Подразделение(id)
)
```
**Скрин 1-2**  

![img](https://github.com/travickiy67/Relational-databases-_Databases/blob/main/img/img1.1png.png)  

![img](https://github.com/travickiy67/Relational-databases-_Databases/blob/main/img/img1.2png.png)  
 
## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.


### Задание 2*

Перечислите, какие, на ваш взгляд, в этой денормализованной таблице встречаются функциональные зависимости и какие правила вывода нужно применить, чтобы нормализовать данные.

**Ни когда не занимался базами данных. Но точно первая таблица должна иметь внешние ключи на остальные таблички, оклад может ссылаться на должность, филиал потдее на адрес. Но могу ошибаться.**  

*Простой запрос на получение данных из таюлицы*

```
select  Фамилия, Имя, Отчество, Оклад, Адрес_филиала, Дата_найма FROM Сотрудники, Оклад, Адрес ;
```

**Скрин 1-2**

![img](https://github.com/travickiy67/Relational-databases-_Databases/blob/main/img/img2.1png.png)   

![img](https://github.com/travickiy67/Relational-databases-_Databases/blob/main/img/img2.2png.png)  

**Правда табличку надо доработать. Возможно дублировние записей.**   
**
