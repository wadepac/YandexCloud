### Deploy Ubuntu 20.04 with Nginx


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

4.Создание второй VM

```
yc compute instance create \
--name demo-2 \
--metadata-from-file user-data=startup.sh \
--create-boot-disk image-folder-id=standard-images,image-family=ubuntu-2004-lts \
--zone ru-central1-a \
--network-interface subnet-name=default-ru-central1-a,nat-ip-version=ipv4
```

5. Команда для полчения информации о VM
```
yc compute instance list
```

### Creating YC Network Load Balancer

<img width="1115" alt="Снимок экрана 2021-12-16 в 15 36 03" src="https://user-images.githubusercontent.com/66735783/146355804-fcb32359-2e6e-4587-a01f-d563202cd897.png">
<img width="941" alt="Снимок экрана 2021-12-16 в 15 38 09" src="https://user-images.githubusercontent.com/66735783/146356205-e53ec6ad-ce29-462f-8a59-7e3a79a85714.png">
<img width="983" alt="Снимок экрана 2021-12-16 в 15 39 55" src="https://user-images.githubusercontent.com/66735783/146356439-cf8ccd77-caf9-4da6-87ea-5502a9af46f3.png">
