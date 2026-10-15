# MySQL

## database
```
create database <name>;
show databases;
drop database <name>;
```
## table
```
create table <name>(
    id int auto increment primary key,
    name varchar(50) not null,
    age tinyint not null,
    date date,
    is_active boolean not null;
);

show tables;

drop table [IF EXISTS] <name>;
```

## columns&row
```
insert into <tablename>(name,age,date,is_active)
value
('user',18,1999-09-09,true)
('bob',20,2000-01-01,false)

或者

insert into <tablename>
value
(null,'user',18 ,1999-09-09,true)
-- null是id的占位符

```

## select
```
select * from <tablename>;
选择所有列

select name from <tablename>;
选择name列

select * from <tablename> where name = 'bob';
选择名为bob的人的所有列

select * from <balename> where name like 'b%b_' or age = 20 or email is not null
选择所有列，名为b+任意个字符+b+一个字符 或者 年龄为20的人 或者 邮箱不是空的人

select * from <tablename> order by <columnname> [asc|desc]
选择并按照column排序，asc升序（默认），desc降序

```

## update
```
update <tablename>
set column1 = value1, column2 = value2, ...
where name = 'bob';
修改名为bob的人的几列
```

## delete
```
delete from <tablename>
where name = 'bob';
删除bob的所有column


```
