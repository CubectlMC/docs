# MVP

## Цель MVP

MVP должен закрыть базовое управление инстансами без дополнительной инфраструктурной сложности

## Входит в MVP

- Регистрация по никнейму и паролю
- Login и logout
- JWT refresh
- Управление пользователями
- Глобальные роли и permissions
- Создание инстанса
- Start как upsert контейнера
- Stop
- Restart
- Выбор контента инстанса
- Поиск модов
- Поиск плагинов
- Поиск дата-паков
- Проверка обновлений контента
- Обновление контента при остановленном инстансе
- Instance-level members
- Живые Docker-логи через SSE
- Простая runtime-информация по инстансу
- Админский экран лимитов ресурсов
- Отображение IP и порта подключения

## Не входит в MVP

- DNS
- A-записи
- SRV-записи
- REG.RU integration
- Metric-service
- Графики метрик
- Исторические графики
- Resource packs
- Config files API
- Backups
- Generic file upload
- Install plan
- Команды в консоль сервера
- Server properties editor

## Подключение к инстансу

В MVP интерфейс показывает только IP сервера и публичный порт инстанса

Доменное имя и SRV-записи остаются future-функциональностью

## Runtime-информация

В MVP не строятся графики

Интерфейс показывает текущие значения:

- Статус контейнера
- Health
- CPU
- RAM
- Disk
- Network
- Uptime
- Public port

## Легковесная модель метрик

Runtime-monitor внутри `instance-service` собирает состояние контейнеров централизованно раз в 15 секунд

На каждый инстанс не создается отдельный Docker stats stream на каждого пользователя

Сервис кладет последний snapshot в Redis

Ключ Redis имеет формат `runtime:instance:{instanceId}`

TTL ключа составляет от 60 до 120 секунд

PostgreSQL для runtime-состояния не используется

UI получает runtime snapshot через `GET /instances/{id}/runtime`

UI делает polling раз в 15 секунд

Такой подход не должен заметно нагружать контейнеры, потому что Docker API опрашивается одним backend collector, а не каждым клиентом отдельно

## Future

- DNS provider abstraction
- REG.RU provider
- A/SRV records
- Исторические графики метрик
- Отдельный observability microservice
- Prometheus поверх Docker metrics
- Grafana dashboards
- SSE для runtime-информации
- Alerting
- Resource packs
- Backups
- Config files API
