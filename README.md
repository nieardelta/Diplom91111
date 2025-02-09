# Этот проект создан для хранения информации по диплому. Разработчиком является NierDelta

#🌐 IP Geolocation Database 

Микросервис для хранения и обработки запросов и ответов с геоданными на основе IP-адресов (Яндекс АПИ).

## 🚀 Программы

- **Liquibase** — управление миграциями базы данных
- **PostgreSQL** — реляционная база данных
- **Java** — бэкенд (если применимо)
- **pgAdmin** - для удобства работы с бд
## 📦 Установка и настройка

1. **Предварительные требования и необходимые компоненты**:
   - Установите [Liquibase](https://www.liquibase.org/download)
   - Установите (В установщике сразу установится pgADmin) [PosgreSql] (https://www.enterprisedb.com/downloads/postgres-postgresql-downloads)
   - По возможности сразу отключть блокировку выполнения сценариев в системе через PowerShell[Set-ExecutionPolicy RemoteSigned]
   - Настройка подключения к БД в `liquibase.properties`:
     ```properties
     url=jdbc:postgresql://localhost:5432/geoip_db
     username=postgres
     password=123
     ```

2. **Запуск **:
 liquibase --changeLogFile=changelog.xml update
## 🗃️ Структура базы данных

### Таблица `requests`
| Столбец             | Тип         | Описание                  |
|---------------------|-------------|---------------------------|
| `id`                | BIGINT      | Первичный ключ (автоинкремент) |
| `ip_address`        | VARCHAR(15) | IP-адрес (обязательное поле) |
| `request_timestamp` | TIMESTAMP   | Временная метка запроса    |

### Таблица `responses`
| Столбец               | Тип          | Описание                  |
|-----------------------|--------------|---------------------------|
| `id`                  | BIGINT       | Первичный ключ (автоинкремент) |
| `request_id`          | BIGINT       | Внешний ключ к `requests.id` |
| `country_code`        | VARCHAR(2)   | Код страны (RU,EU) |
| `country_name`        | VARCHAR(100) | Название страны           |
| `city`                | VARCHAR(100) | Город                     |
| `latitude`            | DOUBLE       | Географическая широта     |
| `longitude`           | DOUBLE       | Географическая долгота    |
| `response_timestamp`  | TIMESTAMP    | Временная метка ответа    |

## 🛠️ Пример использования

**Добавление тестовых данных**:
```
-- Запрос
INSERT INTO requests (ip_address, request_timestamp)
VALUES ('192.168.1.1', NOW());

-- Ответ
INSERT INTO responses (request_id, country_code, city, response_timestamp)
VALUES (1, 'RU', 'Moscow', NOW());
```

**Получение данных**:
```
SELECT 
  requests.ip_address,
  responses.country_name,
  responses.city,
  responses.response_timestamp
FROM requests
JOIN responses ON requests.id = responses.request_id;
```

🔄 Команды Liquibase

1. Применить изменения:
   ```
   liquibase update
   ```

2. Откатить последний changeSet:
   ```
   liquibase rollbackCount 1
   ```

3. Сгенерировать документацию:
   ```
   liquibase generateDocs
   ```

4. Применитьь изменения к БД:
   ```
    liquibase --changeLogFile=changelog.xml update
   ```
