# Docker-среда

## Принципы

`instance-service` управляет Docker-контейнерами через ограниченный слой доступа

Контейнер считается расходником

Данные инстанса хранятся на хосте и монтируются внутрь контейнера

## Ограничения безопасности

Сервис не должен позволять пользователю:

- Передавать произвольные Docker arguments
- Монтировать произвольные host paths
- Управлять контейнерами без Cubectl labels
- Менять Docker socket path
- Запускать privileged containers
- Добавлять произвольные Linux capabilities
- Подменять entrypoint или command без разрешенной политики
- Создавать контейнеры вне выделенного namespace проекта

Все контейнеры Cubectl должны иметь labels:

```text
org.cubectl.managed=true
org.cubectl.instance_id=<id>
org.cubectl.instance_slug=<slug>
org.cubectl.service=instance
```

Операции start, stop и restart разрешены только для контейнеров с корректными labels и совпадающим `instance_id`

## Storage

Данные инстанса хранятся в локальной директории сервера и монтируются в контейнер

Удаление контейнера не должно удалять данные

Удаление данных инстанса должно быть отдельной опасной операцией с отдельным permission

## Resource limits

У каждого инстанса задаются:

- Минимум RAM
- Максимум RAM
- CPU limit
- Порт
- Конфигурация среды выполнения

Администратор должен иметь экран управления лимитами:

- Общий RAM budget для всех инстансов
- Системный RAM reserve
- Минимальный RAM limit на инстанс
- Максимальный RAM limit на инстанс
- Проверка доступной RAM на хосте
- Режим CPU overcommit

Перед запуском `instance-service` выполняет resource preflight

`POST /instances/{instance_id}/start` работает как upsert контейнера

Если контейнер отсутствует, `instance-service` создает контейнер из сохраненного профиля инстанса и затем запускает его
