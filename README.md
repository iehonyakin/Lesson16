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



Логи и скрины прикладыаю
