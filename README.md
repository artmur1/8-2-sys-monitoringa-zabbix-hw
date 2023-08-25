# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Мурчин Артем`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, напри>
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решени>
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  >
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на>
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в в>
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в>

Желаем успехов в выполнении домашнего задания!

### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1

![alt text](https://github.com/artmur1/sys-monitoringa-zabbix-hw/blob/main/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202023-08-23%20230141.png)

   sudo -i
   apt update
   apt install postgresql
wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-4+debian11_all.deb
dpkg -i zabbix-release_6.0-4+debian11_all.deb
apt update
apt install zabbix-server-pgsql zabbix-frontend-php php7.4-pgsql zabbix-apache-conf zabbix-sql-scripts
su - postgres -c 'psql --command "CREATE USER zabbix WITH PASSWORD '\'123456789\'';"'
su - postgres -c 'psql --command "CREATE DATABASE zabbix OWNER zabbix;"'
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
find / -name zabbix_server.conf
cat /etc/zabbix/zabbix_server.conf | grep DBP
sed -i 's/# DBPassword=/DBPassword=123456789/g' /etc/zabbix/zabbix_server.conf
systemctl restart zabbix-server apache2
systemctl enable zabbix-server apache2
systemctl status zabbix-server.service

---

### Задание 2

![alt text](https://github.com/artmur1/sys-monitoringa-zabbix-hw/blob/main/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202023-08-25%20232939.png)
![alt text](https://github.com/artmur1/sys-monitoringa-zabbix-hw/blob/main/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202023-08-25%20233529.png)
![alt text](https://github.com/artmur1/sys-monitoringa-zabbix-hw/blob/main/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202023-08-25%20233113.png)

apt install zabbix-agent
systemctl status zabbix-agent.service
systemctl restart zabbix-agent
systemctl enable zabbix-agent
find / -name zabbix_agentd.conf
sed -i 's/Server=127.0.0.1/Server=10.129.0.10, 127.0.0.1, 10.129.0.8/g' /etc/zabbix/zabbix_agentd.conf
cat /etc/zabbix/zabbix_agentd.conf | grep Server=
systemctl restart zabbix-agent.service
systemctl status zabbix-agent.service
