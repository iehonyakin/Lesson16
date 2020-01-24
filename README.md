# Lesson16
Настроить политику бэкапа директории /etc с клиента:
1) Полный бэкап - раз в день
2) Инкрементальный - каждые 10 минут
3) Дифференциальный - каждые 30 минут

Запустить систему на два часа. Для сдачи ДЗ приложить list jobs, list files jobid=<id>
и сами конфиги bacula-*
  
# Решение  
С обоих клиентов забираем все с директории /var/

FileSet {
  Name = "Eventstore"
  Include {
    Options {
      signature = MD5
    }
    File = /var/
  }
  Exclude {
  }
}
# вывод list files jobid=27

JobId  Level    Files      Bytes   Status   Finished        Name

    19  Full          0         0   OK       24-Jan-20 08:52 BackupClient2
    20  Full          0         0   OK       24-Jan-20 09:00 BackupClient1
    21  Full          0         0   OK       24-Jan-20 09:00 BackupClient2
    22  Full      2,455    31.47 M  OK       24-Jan-20 09:01 BackupCatalog
    23  Full      5,304    772.8 M  OK       24-Jan-20 09:05 BackupClient1
    24  Full      5,304    772.8 M  OK       24-Jan-20 09:06 BackupClient2
    25  Incr          3    2.196 K  OK       24-Jan-20 09:10 BackupClient1
    26  Incr          3    2.196 K  OK       24-Jan-20 09:10 BackupClient2
    27  Incr          8    154.2 K  OK       24-Jan-20 09:20 BackupClient1
    28  Incr          3    2.196 K  OK       24-Jan-20 09:20 BackupClient2


You have messages.
list files jobid=27
Automatically selected Catalog: MyCatalog
Using Catalog "MyCatalog"
+----------------------------------------+
| filename                               |
+----------------------------------------+
| /var/spool/bacula/bacula-fd.9102.state |
| /var/spool/bacula/                     |
| /var/spool/anacron/cron.monthly        |
| /var/log/cron                          |
| /var/log/messages                      |
| /var/lib/nfs/rpc_pipefs/               |
| /var/lib/rsyslog/imjournal.state       |
| /var/lib/rsyslog/                      |
+----------------------------------------+
+-------+---------------+---------------------+------+-------+----------+----------+-----------+
| jobid | name          | starttime           | type | level | jobfiles | jobbytes | jobstatus |
+-------+---------------+---------------------+------+-------+----------+----------+-----------+
|    27 | BackupClient1 | 2020-01-24 09:20:02 | B    | I     |        8 |  154,247 | T         |
+-------+---------------+---------------------+------+-------+----------+----------+-----------+



# C сервера забираем  все в /etc

FileSet {
  Name = "Full Set"
  Include {
    Options {
      signature = MD5
    }
    File = /etc  # бекапим ее только
  }
  Exclude {
        File = /tmp
    File = /proc
    File = /tmp
    File = /.journal
    File = /.fsck
  }

| /etc/grub.d/                                                       |
| /etc/grub.d/00_tuned                                               |
| /etc/grub.d/README                                                 |
| /etc/grub.d/41_custom                                              |
| /etc/grub.d/40_custom                                              |
| /etc/grub.d/30_os-prober                                           |
| /etc/grub.d/20_ppc_terminfo                                        |
| /etc/grub.d/20_linux_xen                                           |
| /etc/grub.d/10_linux                                               |
| /etc/grub.d/01_users                                               |
| /etc/grub.d/00_header                                              |
| /etc/resolv.conf                                                   |
| /etc/mtab                                                          |
| /etc/crypttab                                                      |
| /etc/fstab                                                         |
+--------------------------------------------------------------------+
+-------+---------------+---------------------+------+-------+----------+------------+-----------+
| jobid | name          | starttime           | type | level | jobfiles | jobbytes   | jobstatus |
+-------+---------------+---------------------+------+-------+----------+------------+-----------+
|    22 | BackupCatalog | 2020-01-24 09:01:03 | B    | F     |    2,455 | 31,476,052 | T         |
+-------+---------------+---------------------+------+-------+----------+------------+-----------+


# Статистика

status director
bacula-dir Version: 5.2.13 (19 February 2013) x86_64-redhat-linux-gnu redhat (Core)
Daemon started 24-Jan-20 09:04. Jobs: run=6, running=0 mode=0,0
 Heap: heap=135,168 smbytes=104,657 max_bytes=115,598 bufs=306 max_bufs=354

Scheduled Jobs:
Level          Type     Pri  Scheduled          Name               Volume

Incremental    Backup    10  24-Jan-20 09:30    BackupClient2      v1
Differential   Backup    10  24-Jan-20 09:35    BackupClient1      v1
Differential   Backup    10  24-Jan-20 09:35    BackupClient2      v1
Incremental    Backup    10  24-Jan-20 09:40    BackupClient1      v1
Incremental    Backup    10  24-Jan-20 09:40    BackupClient2      v1
Incremental    Backup    10  24-Jan-20 09:50    BackupClient1      v1
Incremental    Backup    10  24-Jan-20 09:50    BackupClient2      v1
Differential   Backup    10  24-Jan-20 10:05    BackupClient1      v1
Differential   Backup    10  24-Jan-20 10:05    BackupClient2      v1
Incremental    Backup    10  24-Jan-20 10:10    BackupClient1      v1
Incremental    Backup    10  24-Jan-20 10:10    BackupClient2      v1
Incremental    Backup    10  24-Jan-20 10:20    BackupClient1      v1
Incremental    Backup    10  24-Jan-20 10:20    BackupClient2      v1
Full           Backup    10  25-Jan-20 09:00    BackupClient1      v1
Full           Backup    10  25-Jan-20 09:00    BackupClient2      v1
Full           Backup    11  25-Jan-20 09:01    BackupCatalog      v1




 JobId  Level    Files      Bytes   Status   Finished        Name

    19  Full          0         0   OK       24-Jan-20 08:52 BackupClient2
    20  Full          0         0   OK       24-Jan-20 09:00 BackupClient1
    21  Full          0         0   OK       24-Jan-20 09:00 BackupClient2
    22  Full      2,455    31.47 M  OK       24-Jan-20 09:01 BackupCatalog
    23  Full      5,304    772.8 M  OK       24-Jan-20 09:05 BackupClient1
    24  Full      5,304    772.8 M  OK       24-Jan-20 09:06 BackupClient2
    25  Incr          3    2.196 K  OK       24-Jan-20 09:10 BackupClient1
    26  Incr          3    2.196 K  OK       24-Jan-20 09:10 BackupClient2
    27  Incr          8    154.2 K  OK       24-Jan-20 09:20 BackupClient1
    28  Incr          3    2.196 K  OK       24-Jan-20 09:20 BackupClient2






*list jobs
+-------+---------------+---------------------+------+-------+----------+-------------+-----------+
| jobid | name          | starttime           | type | level | jobfiles | jobbytes    | jobstatus |
+-------+---------------+---------------------+------+-------+----------+-------------+-----------+
|     1 | BackupClient1 | 2020-01-24 08:06:40 | B    | F     |        0 |           0 | T         |
|     2 | BackupClient1 | 2020-01-24 08:20:02 | B    | I     |        0 |           0 | T         |
|     3 | BackupClient2 | 2020-01-24 08:20:05 | B    | F     |        0 |           0 | T         |
|     4 | BackupClient1 | 2020-01-24 08:30:02 | B    | I     |        0 |           0 | T         |
|     5 | BackupClient2 | 2020-01-24 08:30:04 | B    | I     |        0 |           0 | T         |
|     6 | BackupClient1 | 2020-01-24 08:35:03 | B    | F     |        0 |           0 | T         |
|     7 | BackupClient1 | 2020-01-24 08:35:05 | B    | D     |        0 |           0 | T         |
|     8 | BackupClient2 | 2020-01-24 08:35:08 | B    | F     |        0 |           0 | T         |
|     9 | BackupClient2 | 2020-01-24 08:35:10 | B    | D     |        0 |           0 | T         |
|    10 | BackupClient1 | 2020-01-24 08:40:03 | B    | I     |        0 |           0 | T         |
|    11 | BackupClient2 | 2020-01-24 08:40:05 | B    | I     |        0 |           0 | T         |
|    12 | BackupCatalog | 2020-01-24 08:40:08 | B    | F     |        1 |      85,218 | T         |
|    13 | BackupCatalog | 2020-01-24 08:45:03 | B    | F     |        1 |      88,202 | T         |
|    14 | BackupClient1 | 2020-01-24 08:50:03 | B    | I     |        0 |           0 | T         |
|    15 | BackupClient2 | 2020-01-24 08:50:05 | B    | I     |        0 |           0 | T         |
|    16 | BackupClient1 | 2020-01-24 08:50:34 | B    | I     |        0 |           0 | T         |
|    17 | BackupClient2 | 2020-01-24 08:50:37 | B    | I     |        0 |           0 | T         |
|    18 | BackupClient1 | 2020-01-24 08:52:02 | B    | F     |        0 |           0 | T         |
|    19 | BackupClient2 | 2020-01-24 08:52:05 | B    | F     |        0 |           0 | T         |
|    20 | BackupClient1 | 2020-01-24 09:00:02 | B    | F     |        0 |           0 | T         |
|    21 | BackupClient2 | 2020-01-24 09:00:05 | B    | F     |        0 |           0 | T         |
|    22 | BackupCatalog | 2020-01-24 09:01:03 | B    | F     |    2,455 |  31,476,052 | T         |
|    23 | BackupClient1 | 2020-01-24 09:05:02 | B    | F     |    5,304 | 772,833,756 | T         |
|    24 | BackupClient2 | 2020-01-24 09:05:35 | B    | F     |    5,304 | 772,830,371 | T         |
|    25 | BackupClient1 | 2020-01-24 09:10:02 | B    | I     |        3 |       2,196 | T         |
|    26 | BackupClient2 | 2020-01-24 09:10:05 | B    | I     |        3 |       2,196 | T         |
|    27 | BackupClient1 | 2020-01-24 09:20:02 | B    | I     |        8 |     154,247 | T         |
|    28 | BackupClient2 | 2020-01-24 09:20:05 | B    | I     |        3 |       2,196 | T         |
|    29 | BackupClient1 | 2020-01-24 09:30:02 | B    | I     |        4 |     151,174 | T         |
|    30 | BackupClient2 | 2020-01-24 09:30:05 | B    | I     |        6 |     146,340 | T         |
+-------+---------------+---------------------+------+-------+----------+-------------+-----------+
*





