# [Zabbix] Clean up large history data
## Steps
### First, grant zabbix database login priviledge
```
$ psql -d zabbix -U zabbix
```
### List all table sizes
```
SELECT
   relname as "Table",
   pg_size_pretty(pg_total_relation_size(relid)) As "Size",
   pg_size_pretty(pg_total_relation_size(relid) - pg_relation_size(relid)) as "External Size"
   FROM pg_catalog.pg_statio_user_tables ORDER BY pg_total_relation_size(relid) DESC;
```
### get unix timestamp
```
Epoch timestamp: 1493596799
Timestamp in milliseconds: 1493596799000
Human time (GMT): 2017年Apr30日Sunday 23:59:59
Human time (your time zone): 2017年5月1日星期一 07:59:59 GMT+08:00
```
### delete data
```
$ psql
> delete from alerts where clock < 1493596799;
> delete from acknowledges where clock < 1493596799;
> delete from events where clock < 1493596799;
```
### create a empty table ,insert latest data, alter table.
```
$ psql
> create table history_uint_new ( like history_uint );
> insert into history_uint_new select * from history_uint where clock > 1506787199 limit 100000;
> alter table history_uint rename to history_uint_old;
> alter table history_uint_new rename to history_uint;
> alter index history_uint_1 rename to history_uint_old_1;
> create index history_uint_1 on history_uint using btree (itemid, clock);
> \d history_uint
## start zabbix server to check if everything okay...
$ psql
> drop table history_uint_old;
```
### dump to file if needed
```
 pg_dump -d zabbix -t trends_uint_old > trends_uint_old.sql
  pg_dump -d zabbix -t history_old > history_old.sql
  pg_dump -d zabbix -t history_uint_old > history_uint_old.sql
```

## References
- [Check size of tables and objects in PostgreSQL database - General IT Documentation - D-BSSE Wiki](https://wiki-bsse.ethz.ch/display/ITDOC/Check+size+of+tables+and+objects+in+PostgreSQL+database)
- [Epoch Converter - Unix Timestamp Converter](https://www.epochconverter.com/)
- https://gist.github.com/edtjones/33b666b1021e8b3b9836
- [Cleaning up the Zabbix database | Machinenoise.org](http://machinenoise.org/2014/cleaning-up-the-zabbix-database.html)
-[Zabbix housekeeper woes | Huyabbix](https://huyabbix.com/zabbix-housekeeper-woes/)