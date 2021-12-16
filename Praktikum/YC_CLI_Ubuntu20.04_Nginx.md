## Deploy Ubuntu 20.04 with Nginx


1. [Установка Yandex Cloud Cli](https://cloud.yandex.ru/docs/cli/quickstart)
2. Напишите скрипт для запуска на ВМ. Он получает список актуальных пакетов с софтом, устанавливает и запускает nginx, а затем меняет информационную страницу работающего сервера:
```
cat << EOF > startup.sh
#!/bin/bash
apt-get update
apt-get install -y nginx
service nginx start
sed -i -- "s/nginx/Yandex Cloud - ${HOSTNAME}/" /var/www/html/index.nginx-debian.html
EOF
```
3. Создание VM

```
yc compute instance create \
--name demo-1 \
--metadata-from-file user-data=startup.sh \
--create-boot-disk image-folder-id=standard-images,image-family=ubuntu-2004-lts \
--zone ru-central1-a \
--network-interface subnet-name=default-ru-central1-a,nat-ip-version=ipv4
```
| Command| Description |
| --- | --- |
|--name | Имя новой VM |
|--metadata-from-file  | Путь к файлу с метадатой |
|image-family=ubuntu-2004-lts | Образ диска |
|--zone| В какой зоне будет создана  VM |
|subnet-name=default-ru-central1-a| Виртуальная сеть, в котоую будет подключена VM|

4. Команда для полчения информации о VM
```
yc compute instance list
```
