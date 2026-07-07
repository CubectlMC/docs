# Runtime-информация и логи

## Runtime-monitor

Runtime-monitor находится внутри `instance-service`

Он опрашивает Docker API раз в 15 секунд

Собираемые данные:

- Status
- CPU
- RAM
- Network
- Uptime

Последний snapshot сохраняется в Redis

Ключ Redis:

```text
runtime:instance:{instanceId}
```

TTL ключа составляет от 60 до 120 секунд

PostgreSQL для runtime-состояния не используется

## API

UI читает runtime-информацию через REST:

```text
GET /instances/{instance_id}/runtime
```

UI делает polling раз в 15 секунд

В MVP runtime-информация не передается через SSE

## Логи

Живые Docker-логи отдаются через SSE

Ответственность за живые логи находится в `instance-service`, потому что источник данных — поток Docker stdout/stderr

Файловые логи не входят в MVP как отдельный API

## Future observability

В MVP отдельного микросервиса метрик нет

Позже runtime-monitor можно вынести из `instance-service` в отдельный observability microservice

Для графиков можно добавить Prometheus или другой collector поверх Docker metrics

Для визуализации можно подключить Grafana dashboards

PostgreSQL не должен быть основным хранилищем высокочастотных метрик
