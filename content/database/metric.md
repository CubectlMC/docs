# Metric database

База данных: `cubectl_metric`

## Назначение

`metric-service` хранит снимки метрик инстансов и хоста

Снимки собираются каждые 15 секунд

REST API читает исторические данные из PostgreSQL

SSE API отдает live-данные после первичной загрузки истории

## Таблицы

### instance_metric_snapshots

Хранит снимки метрик инстанса

Поля:

- `id uuid primary key`
- `collection_run_id uuid`
- `instance_id uuid not null`
- `captured_at timestamptz not null`
- `container_status varchar not null`
- `cpu_percent numeric not null`
- `memory_usage_mb integer not null`
- `memory_limit_mb integer not null`
- `disk_usage_mb integer`
- `network_rx_bytes bigint`
- `network_tx_bytes bigint`
- `block_read_bytes bigint`
- `block_write_bytes bigint`
- `uptime_seconds bigint`
- `created_at timestamptz not null`

Ограничения:

- `foreign key (collection_run_id) references metric_collection_runs(id)`

### host_metric_snapshots

Хранит снимки метрик хоста

Поля:

- `id uuid primary key`
- `collection_run_id uuid`
- `captured_at timestamptz not null`
- `cpu_percent numeric not null`
- `memory_total_mb integer not null`
- `memory_used_mb integer not null`
- `memory_free_mb integer not null`
- `disk_total_mb integer`
- `disk_used_mb integer`
- `disk_free_mb integer`
- `network_rx_bytes bigint`
- `network_tx_bytes bigint`
- `running_instances integer not null`
- `created_at timestamptz not null`

Ограничения:

- `foreign key (collection_run_id) references metric_collection_runs(id)`

### metric_collection_runs

Хранит запуски сборщика метрик

Поля:

- `id uuid primary key`
- `started_at timestamptz not null`
- `finished_at timestamptz`
- `status varchar not null`
- `instances_collected integer not null`
- `error_message text`

## Индексы

- `instance_metric_snapshots_instance_captured_idx`
- `instance_metric_snapshots_captured_at_idx`
- `host_metric_snapshots_captured_at_idx`
- `metric_collection_runs_started_at_idx`
