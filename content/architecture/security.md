# Безопасность и доступы

## Авторизация

Используется регистрация по никнейму и паролю

Не используются:

- Email-верификация
- OAuth2-интеграции
- Внешние identity providers

После входа выдается JWT

JWT проверяется gateway и внутренними сервисами

Spring Security используется во всех сервисах, где есть HTTP API

## Глобальные permissions

Глобальные permissions хранятся в `identity-service`

Глобальные permissions назначаются через роли

Endpoints:

```text
GET    /permissions
GET    /users/{user_id}/roles
POST   /users/{user_id}/roles
DELETE /users/{user_id}/roles/{role_id}
```

## Instance-level access

Instance-level access хранится в `instance-service`

Пользователь может иметь роль внутри конкретного инстанса

Endpoints:

```text
GET    /instances/{instance_id}/members
POST   /instances/{instance_id}/members
PATCH  /instances/{instance_id}/members/{user_id}
DELETE /instances/{instance_id}/members/{user_id}
```

## Базовые permissions

- User read
- User manage
- Role read
- Role manage
- Permission read
- Instance create
- Instance read
- Instance update
- Instance delete
- Instance start
- Instance stop
- Instance restart
- Instance runtime update
- Instance java version update
- Instance member manage
- Instance content read
- Instance content update
- Instance logs read
- Instance live logs read
- Instance metrics read
- Resource limits read
- Resource limits manage

## Java version

Java version выбирается автоматически при создании инстанса

Ручное изменение Java version доступно только пользователям с permission `instance_java_version_update`
