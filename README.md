# Домашнее задание к занятию «`Система мониторинга Zabbix`» - `Каргапольцев Константин Сергеевич`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1

`Установите Zabbix Server с веб-интерфейсом.

Процесс выполнения

    Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
    Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии Debian 11.
    Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.
    Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.

Требования к результаты

    Прикрепите в файл README.md скриншот авторизации в админке.
    Приложите в файл`
    
### Решение 1    
    
1. `Устанавливаем PostgreSQL:
    sudo apt install postgresql`
2. `Набор команд с сайта Zabbix:
     - скачиваем репазиторий с Zabbix:
       wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_7.0-2+ubuntu22.04_all.deb
     - устанавливаем пакет репазитория с Zabbix:  
       dpkg -i zabbix-release_7.0-2+ubuntu22.04_all.deb
     - обновляем репазиторий:  
       apt update'  
3. 'устанавливаем zabbix-server-pgsql zabbix-frontend-php php8.1-pgsql zabbix-apache-conf zabbix-sql-scripts:
    apt install zabbix-server-pgsql zabbix-frontend-php php8.1-pgsql zabbix-apache-conf zabbix-sql-scripts'       
4. 'Создал базу данных:
    sudo -u postgres createuser --pwprompt zabbix
    sudo -u postgres createdb -O zabbix zabbix'
5. 'На хосте Zabbix сервера импортировал начальную схему и данные:
    zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix'
6. 'Отредактировал файл /etc/zabbix/zabbix_server.conf:
    DBPassword=12345'
7. 'Запустил процессы Zabbix сервера: 
    systemctl restart zabbix-server apache2
    systemctl enable zabbix-server apache2
    systemctl status zabbix-server apache2'
8. 'Авторизовался через веб интерфейс:
    http://192.168.123.3/zabbix
    Логин и пароль для входа в админ-панель — Admin\zabbix/



`При необходимости прикрепитe сюда скриншоты
![zabbix1.png](/home/kargapoltcevks/-Zabbix-/img/)`

---

### Задание 2

'Установите Zabbix Agent на два хоста.
 Процесс выполнения

    Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
    Установите Zabbix Agent на 2 вирт.машины, одной из них может быть ваш Zabbix Server.
    Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.
    Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.
    Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.

 Требования к результаты

    Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу
    Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером
    Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.
    Приложите в файл README.md текст использованных команд в GitHub'

###  Решение 2

1. 'Установил Zabbix Agent на 2 хоста (kks@192.168.123.4 и kks2@192.168.123.10):'

   - `Зашел по SSH на 1й хост:
      sudo ssh kks@192.168.123.4'
   - 'Установил репозиторий Zabbix:
      wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_7.0-2+ubuntu22.04_all.deb'
      dpkg -i zabbix-release_7.0-2+ubuntu22.04_all.deb
      apt update'
   - 'Установил Zabbix агент:
      apt install zabbix-agent'
   - 'Запустил процесс Zabbix агента:
      systemctl restart zabbix-agent
      systemctl enable zabbix-agent
      systemctl status zabbix-agent.service - проверилстатус'
   - 'Всё тоже самое сделал со 2й машиной: kks2@192.168.123.10'
   
2. 'Добавил Zabbix Server в список разрешенных серверов Zabbix Agentов.' 
   - 'На обоих хостах внес изменения в файл zabbix_agentd.conf (Server=192.168.123.3)
      sudo nano /etc/zabbix/zabbix_agentd.conf'

3. 'Добавил Zabbix Agentов в раздел Configuration > Hosts Zabbix Servera.


    'СКРИНШОТЫ'

  - 'Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу:
    ![Agent.png](https://github.com/KargapoltcevKS/Zabbix/blob/main/img/Agent.png)`

  - 'Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером:
    Команда: tail -f /var/log/zabbix/zabbix_agentd.log
   ![log kks.png](/home/kargapoltcevks/-Zabbix-/img/)
   ![log kks2.png](/home/kargapoltcevks/-Zabbix-/img/)`
   
  - 'Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные:
   ![Latest data.png](/home/kargapoltcevks/-Zabbix-/img/)`
   
  - 'Приложите в файл README.md текст использованных команд в GitHub:
     git clone https://github.com/KargapoltcevKS/-Zabbix-.git
     git commit -m "comment"
     git push origin
   



---

