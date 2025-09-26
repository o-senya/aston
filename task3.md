# Домашнее задание 3

## Условие
Нужно написать скрипт, который:
- создаёт папку `/opt/app`
- создаёт файл `/opt/app/log.txt`
- каждые 17 секунд записывает туда случайную строку длиной до 20 символов (каждый раз с новой строки)

Дополнительно (по желанию):
- сделать так, чтобы скрипт запускался сам при старте системы
- настроить ротацию логов через logrotate

---

## Инструкция

### 1. Подготовка папки и файла
sudo mkdir -p /opt/app
sudo touch /opt/app/log.txt
sudo chown $USER:$USER /opt/app/log.txt


### 2. Создание скрипта
Открываем файл:
nano ~/logger.sh

Вставляем туда код:
#!/bin/bash

mkdir -p /opt/app

while true; do
head /dev/urandom | tr -dc A-Za-z0-9 | head -c 20 >> /opt/app/log.txt
echo >> /opt/app/log.txt
sleep 17
done

Сохраняем и делаем его исполняемым: 
chmod +x ~/logger.sh


### 3. Запуск вручную
./logger.sh

---

## [Опционально] Автозагрузка через systemd
Создаём unit-файл:
sudo nano /etc/systemd/system/logger.service

[Unit]
Description=Logger Service
After=network.target

[Service]
ExecStart=/home/USERNAME/logger.sh
Restart=always
User=kovalse

[Install]
WantedBy=multi-user.target

Запускаем:
sudo systemctl daemon-reload
sudo systemctl enable logger.service
sudo systemctl start logger.service

---

## [Опционально] Ротация логов
Создаём конфиг:
sudo nano /etc/logrotate.d/app-log

Вставляем:
/opt/app/log.txt {
daily
size 1M
rotate 7
compress
missingok
notifempty
copytruncate
}
