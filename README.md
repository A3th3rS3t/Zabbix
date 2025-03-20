# Zabbix with local database

Эта конфигурация предназначена для развертывания системы мониторинга `Zabbix` с использованием Docker Compose. `Zabbix Server` использует PostgreSQL в качестве базы данных, а `Zabbix Web` предоставляет веб-интерфейс для управления мониторингом.

## 1. Подготовка окружения
Убедитесь, что у вас установлены `Docker` и `Docker Compose`, если нет воспользуйтесь следующей инструкцией: https://docs.docker.com/engine/install/

Убедитесь, что `Git` установлен. Если нет, установите его: https://git-scm.com/downloads

Убедитесь, что у вас установлена База данных MySQL, PostgreSQL или MariaDB.

## 2. Клонирование репозитория
Перейдите в директорию, в которой вы хотите развернуть Zabbix:
```bash
cd /opt/docker/
```
Клонируйте репозиторий с GitHub:
```bash
git clone https://github.com/A3th3rS3t/Zabbix-docker.git
```
```bash
cd Zabbix-docker
```
В репозитории должны быть файлы `docker-compose.yaml` и `.env`.

## 3. Подготовка локальной PostgreSQL
Создайте пользователя и БД для Zabbix:
```bash
sudo -u postgres psql -c "CREATE USER zabbix WITH PASSWORD 'StrongPassword123';"
sudo -u postgres psql -c "CREATE DATABASE zabbix OWNER zabbix;"
```
Отредактируйте pg_hba.conf:
```bash
sudo nano /etc/postgresql/*/main/pg_hba.conf
```

Добавьте строку:
```bash
host    zabbix          zabbix          172.0.0.0/8           md5
```
Отредактируйте postgresql.conf:
```bash
sudo nano /etc/postgresql/*/main/postgresql.conf
```
Измените:
```bash
listen_addresses = '*'
```
Перезапустите PostgreSQL:
```bash
sudo systemctl restart postgresql
```

## 4. Инициализация базы данных

```bash
# Скачайте схему и данные
wget https://cdn.zabbix.com/zabbix/sources/stable/6.0/zabbix-6.0.0.tar.gz
tar -xzf zabbix-6.0.0.tar.gz

# Импортируйте данные
cd zabbix-6.0.0/database/postgresql
psql -U zabbix -h <localhost_ip> -d zabbix -f schema.sql
psql -U zabbix -h <localhost_ip> -d zabbix -f images.sql
psql -U zabbix -h <localhost_ip> -d zabbix -f data.sql
```

## 5. Запуск сервисов через Docker Compose
В директории с проектом выполните команду для запуска сервисов:
```bash
docker compose up -d
```
Это запустит два контейнера:
```bash
zabbix_server — сервис zabbix.
zabbix_web — веб интерфейс zabbix.
```

## 6. Проверка работоспособности
Откройте браузер и перейдите по адресу: localhost:8080 (или другой порт, если вы изменили его в docker-compose.yaml)
Стандартные данные для входа:
log: Admin
pass: zabbix

Если что-то не работает, проверьте логи контейнеров:
```bash
docker logs zabbix_server
```

## 7. Остановка и удаление сервисов
Если вам нужно изменить порты или другие параметры, отредактируйте файл 
docker-compose.yaml и перезапустите сервисы:
```bash
docker compose down
docker compose up -d
```
