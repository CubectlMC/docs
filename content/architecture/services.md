# Микросервисы

## gateway

Пакет: `org.cubectl.gateway`

Ответственность:

- Единая входная точка для web-интерфейса
- Маршрутизация запросов к `identity-service`, `instance-service`, `file-service`
- CORS
- Ограничение частоты запросов
- Проверка JWT на входе
- Прокидывание контекста пользователя во внутренние сервисы
- Единый формат ошибок

Gateway не содержит доменную бизнес-логику

## identity-service

Пакет: `org.cubectl.identity`

Ответственность:

- Пользователи
- Регистрация по никнейму и паролю
- Login
- Logout
- Refresh JWT
- Роли
- Permissions
- Глобальные назначения ролей пользователю
- Проверка глобального доступа
- Выдача контекста пользователя

Email, email-верификация, OAuth2 и внешние провайдеры личности не входят в текущий план

## instance-service

Пакет: `org.cubectl.instance`

Ответственность:

- Инстансы
- Docker containers
- Start
- Stop
- Restart
- Порты
- Конфигурация среды выполнения
- Статус
- Server lifecycle
- Модель выбранного контента инстанса
- Instance-level access
- Автоматический выбор Java version
- Ручное изменение Java version при отдельном permission
- Resource preflight
- Upsert контейнера при start
- Лимиты CPU/RAM
- Живые Docker-логи через SSE
- Отображение IP и публичного порта
- Runtime-monitor
- `GET /instances/{instance_id}/runtime`

DNS не входит в MVP

## file-service

Пакет: `org.cubectl.file`

Ответственность:

- Поиск модов
- Поиск плагинов
- Поиск дата-паков
- Карточки контента
- Версии контента
- Провайдеры контента
- Modrinth
- CurseForge
- Dependency resolver
- Скачивание файлов по provider/project/version ids
- Подготовка файлов для `instance-service`

Generic file upload не входит в MVP

## runtime-monitor

Runtime-monitor является частью `instance-service`

Ответственность:

- Опрос Docker API раз в 15 секунд
- Сбор status
- Сбор CPU
- Сбор RAM
- Сбор network
- Запись последнего snapshot в Redis
- TTL ключей от 60 до 120 секунд
- Чтение snapshot для `GET /instances/{instance_id}/runtime`

PostgreSQL для runtime-состояния не используется

Отдельный микросервис метрик не входит в MVP

Позже runtime-monitor можно вынести в отдельный observability microservice с Grafana dashboards поверх Docker metrics
