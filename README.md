# Zabbix with local database

Эта конфигурация предназначена для развертывания системы мониторинга `Zabbix` с использованием Docker Compose. `Zabbix Server` использует PostgreSQL в качестве базы данных, а `Zabbix Web` предоставляет веб-интерфейс для управления мониторингом.

## 1. Подготовка окружения
Убедитесь, что у вас установлены `Docker` и `Docker Compose`, если нет воспользуйтесь следующей инструкцией: https://docs.docker.com/engine/install/

Убедитесь, что `Git` установлен. Если нет, установите его: https://git-scm.com/downloads

Убедитесь, что у вас установлена База данных MySQL, PostgreSQL или MariaDB. А так же созданна база данных с именем `zabbix` и пользователем `zabbix` с паролем.

## 2. Клонирование репозитория
Перейдите в директорию, в которой вы хотите развернуть Zabbix:
```bash
cd /opt/docker/
```
Клонируйте репозиторий с GitHub:
```bash
git clone https://github.com/A3th3rS3t/Zabbix-docker.git
cd Zabbix
```
В репозитории должны быть файлы `docker-compose.yaml` и `.env`.