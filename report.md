# Отчёт по лабораторной работе


## Шаг 1: Установка Docker

### Проверка установки Docker
Для проверки установки Docker выполните команду:
```bash
docker --version
```
Если Docker не установлен:
 - Для Linux: Установите Docker через пакетный менеджер (apt, yum, и т.д.).

## Запуск Docker
Для запуска Docker выполните команду:

```bash
sudo systemctl start docker
```
## Шаг 2: Подготовка Dockerfile для контейнера с AAFire
### Создание Dockerfile
Создайте файл Dockerfile в отдельной папке и добавьте в него следующий код:

```dockerfile

# Используем минимальный базовый образ
FROM ubuntu:latest

# Обновляем пакетный менеджер и устанавливаем aafire и ping
RUN apt-get update && apt-get install -y libaa-bin iputils-ping

# Указываем команду запуска по умолчанию
CMD ["aafire"]
```
Сохраните файл под именем Dockerfile.

## Шаг 3: Сборка Docker-образа
Перейдите в директорию с Dockerfile:
```bash
cd path/to/Dockerfile
```
Сборка образа:
```bash
docker build -t aafire .
```
Проверка наличия созданного образа:
```bash

docker images
```

## Шаг 4: Запуск контейнера с AAFire
Запуск контейнера:
```bash

docker run -it aafire
```
Завершение или сохранение состояния контейнера:
 - Для завершения: Ctrl+C
 - Для сохранения в работающем состоянии: Ctrl+P, затем Ctrl+Q

![image](https://github.com/user-attachments/assets/3cd63e88-374b-4672-a6ce-17e4e866d67c)



## Шаг 5: Создание сети между двумя контейнерами
Создание пользовательской сети:
```bash

docker network create myNetwork
```
Запуск двух экземпляров контейнера:
```bash
docker run -dit --name mycontainer1 aafire
docker run -dit --name mycontainer2 aafire
```

Подключение контейнеров к сети:

```bash
docker network connect myNetwork mycontainer1
docker network connect myNetwork mycontainer2
```

Проверка настроенной сети:

```bash
docker network inspect myNetwork
```

## Шаг 6: Проверка соединения между контейнерами
Зайти в первый контейнер:
```bash

docker exec -it mycontainer1 bash
```
Выполнить команду ping для второго контейнера:
```bash

ping mycontainer2
```

![image](https://github.com/user-attachments/assets/1893fa2e-c8f8-4e3c-ba9d-6e40b1e67f5a)



## Шаг 7: Очистка Docker после работы

Чтобы очистить Docker от созданных контейнеров, образов и сети, выполните следующие команды.

### Остановка и удаление контейнеров

```bash
docker stop mycontainer1 mycontainer2
docker rm mycontainer1 mycontainer2
```

### Удаление образа

```bash
docker rmi aafire_image:latest
```

### Удаление сети

```bash
docker network rm myNetwork
```

### Удаление неиспользуемых объектов Docker

```bash
docker system prune -a --volumes
```

**Пояснения:**

- `docker system prune -a --volumes` удаляет все неиспользуемые объекты: остановленные контейнеры, неиспользуемые образы, тома и сети.
- **Внимание:** Убедитесь, что вам не нужны эти данные перед выполнением команды.

---

## Шаг 8: Отключение и включение Docker

### Отключение Docker

Чтобы остановить сервис Docker, выполните:

```bash
sudo systemctl stop docker
```

### Включение Docker

Чтобы запустить сервис Docker, выполните:

```bash
sudo systemctl start docker
```

### Проверка статуса Docker

```bash
sudo systemctl status docker
```
### удаление логов
```bash
sudo find /var/lib/docker/containers/ -name "*.log" -exec truncate -s 0 {} \;
```

---

## Заключение

В ходе выполнения данной лабораторной работы мы:

- Создали Dockerfile для образа с `aafire` и `ping`.
- Собрали Docker-образ и запустили два контейнера с `aafire`.
- Создали пользовательскую сеть `myNetwork` и подключили к ней контейнеры.
- Проверили соединение между контейнерами с помощью `ping`.
- Очистили Docker после работы.
- Научились отключать и включать сервис Docker.

---
