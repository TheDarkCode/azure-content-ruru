
<!--
includes/sql-data-warehouse-backup-retention-policies.md

Latest Freshness check:  2016-05-05 , barbkess.

As of circa 2016-04-22, the following topics might include this include:
articles/sql-data-warehouse/sql-data-warehouse-overview-expectations.md
articles/sql-data-warehouse/sql-data-warehouse-overview-backup-and-restore.md
-->
Хранилище данных SQL создает моментальные снимки всех актуальных данных минимум каждые 8 часов, используя для этого моментальные снимки службы хранилища Azure. Эти снимки хранятся в течение семи дней. Это позволяет восстанавливать данные до одной из 21 точки во времени в пределах 7 дней до момента создания последнего моментального снимка.

Хранилище данных SQL делает моментальный снимок базы данных перед ее удалением и хранит его в течение 7 дней. В таком случае моментальные снимки из актуальной базы данных больше не хранятся в хранилище. Это позволяет восстановить удаленную базу данных до точки, когда она была удалена.

<!---HONumber=AcomDC_0608_2016-->