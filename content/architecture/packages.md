# Пакетная архитектура

## Общие правила

Каждый сервис использует корневой пакет `org.cubectl.<service>`

Архитектура внутри сервиса должна быть доменной

Пакеты группируются по бизнес-областям, а не только по техническим слоям

Клиенты внешних API живут внутри доменного пакета провайдера вместе с DTO и mapper-кодом

## gateway

```text
org.cubectl.gateway
  routing
  security
  errors
  config
```

## identity-service

```text
org.cubectl.identity
  auth
  user
  role
  permission
  token
  security
  errors
  config
```

## instance-service

```text
org.cubectl.instance
  instance
  content
  member
  lifecycle
  docker
  runtime
  java
  port
  resource
  logs
  security
  errors
  config
```

## file-service

```text
org.cubectl.file
  mod
  plugin
  datapack
  contentprovider
  provider
    modrinth
      client
      dto
      mapper
      service
    curseforge
      client
      dto
      mapper
      service
  resolver
  security
  errors
  config
```

## metric-service

```text
org.cubectl.metric
  snapshot
  collector
  docker
  host
  stream
  alert
  security
  errors
  config
```
