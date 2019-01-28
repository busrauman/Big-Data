### Impala Shell ile sorgu çalıştırma
```
impala-shell
```
### Create database

```
create database <DB_name>
use <DB_name>
```
### Create Table
```
create table calls(user string , other string , direction string, duration int, date_time string)  row format delimited fields terminated by ',';

```

### Load csv to table

```
docker cp calls.csv <container_name | ID> -> Docker konteynırına dosya kopylamak için

load data inpath '/tmp/calls.csv' overwrite into table calls;

alter table calls set tblproperties('skip.header.line.count'='1'); -> ilk satırı tabloya eklememek için

```
### Impala komut satırına girmeden de sorgu çalıştırmak için
```
impala-shell -q "select * from calls"

impala-shell -f file.sql

```
### External bir tablo yaratma
```
create external table messages (user string, other string , direction string , length int, `timestamp` String) row format delimited fields terminated by ',' stored as TEXTFILE location '/data/messages' tblproperties("skip.header.line.count"="1");

```
