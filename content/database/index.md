# Схемы баз данных

## Оглавление

- [Identity database](identity.md)
- [Instance database](instance.md)
- [File database](file.md)
- [Metric database](metric.md)
- [ER-диаграмма](schema.md)

## Общий подход

Для всех сервисов используется один контейнер PostgreSQL

Внутри контейнера создаются отдельные базы данных:

- `cubectl_identity`
- `cubectl_instance`
- `cubectl_file`
- `cubectl_metric`

Каждый сервис подключается только к своей базе данных

Прямые foreign key между базами разных сервисов не используются

Связи между сервисами хранятся через UUID внешних сущностей

ER-диаграмма показывает логические связи между сервисными базами

Физические foreign key создаются только внутри одной базы сервиса

## Общие поля

Для основных таблиц используются:

- `id`
- `created_at`
- `updated_at`

Для soft delete используется `deleted_at`, если сущность должна сохраняться для аудита

Для optimistic locking можно добавить `version`
