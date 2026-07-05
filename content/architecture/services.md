# Микросервисы

## gateway

Пакет: `org.cubectl.gateway`

Ответственность:

- Единая входная точка для web-интерфейса
- Маршрутизация запросов к `identity-service`, `instance-service`, `file-service`, `metric-service`
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
- Интеграция с DNS-провайдером для A/SRV записей

DNS остается внутри `instance-service`, так как запись создается как часть жизненного цикла инстанса

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

## metric-service

Пакет: `org.cubectl.metric`

Ответственность:

- Сбор CPU
- Сбор RAM
- Disk usage
- Network usage
- Uptime
- Текущие метрики инстанса
- Исторические метрики инстанса
- Live-метрики инстанса через SSE
- Текущие метрики хоста
- Исторические метрики хоста
- Live-метрики хоста через SSE
- Запись снимков метрик в PostgreSQL каждые 15 секунд
