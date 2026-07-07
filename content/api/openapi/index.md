# OpenAPI

Base path: `/api/v1`

## Сервисы

- `gateway`
- `identity-service`
- `instance-service`
- `file-service`

## Пакеты

- `org.cubectl.gateway`
- `org.cubectl.identity`
- `org.cubectl.instance`
- `org.cubectl.file`

## Контракты

- [Gateway](gateway.yaml)
- [Identity-service](identity.yaml)
- [Instance-service](instance.yaml)
- [File-service](file.yaml)

## Правила именования

- URI строятся от ресурсов
- Collections называются во множественном числе
- Path segments пишутся в lowercase
- В path segments используются дефисы
- Snake case используется в JSON и query parameters
