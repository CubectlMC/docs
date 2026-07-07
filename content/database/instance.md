# Instance database

База данных: `cubectl_instance`

## Назначение

`instance-service` хранит профиль инстанса, lifecycle, runtime config, ports, resource limits, selected content и instance-level access

## Таблицы

### instances

Хранит инстансы

Поля:

- `id uuid primary key`
- `slug varchar unique not null`
- `display_name varchar not null`
- `status varchar not null`
- `health varchar not null`
- `owner_user_id uuid not null`
- `minecraft_version varchar not null`
- `loader_type varchar not null`
- `loader_version varchar`
- `java_version_mode varchar not null`
- `resolved_java_version integer`
- `server_settings_json jsonb not null`
- `start_after_create boolean not null`
- `restart_required boolean not null`
- `created_at timestamptz not null`
- `updated_at timestamptz not null`
- `deleted_at timestamptz`

### instance_resources

Хранит лимиты ресурсов конкретного инстанса

Поля:

- `instance_id uuid primary key`
- `memory_min_mb integer not null`
- `memory_max_mb integer not null`
- `cpu_limit numeric not null`
- `created_at timestamptz not null`
- `updated_at timestamptz not null`

Ограничения:

- `foreign key (instance_id) references instances(id)`

### instance_networks

Хранит сетевые настройки инстанса

Поля:

- `instance_id uuid primary key`
- `public_port integer unique not null`
- `container_port integer not null`
- `public_host varchar not null`
- `created_at timestamptz not null`
- `updated_at timestamptz not null`

Ограничения:

- `foreign key (instance_id) references instances(id)`

### instance_storage

Хранит storage paths

Поля:

- `instance_id uuid primary key`
- `host_data_path varchar not null`
- `container_data_path varchar not null`
- `created_at timestamptz not null`
- `updated_at timestamptz not null`

Ограничения:

- `foreign key (instance_id) references instances(id)`

### docker_containers

Хранит состояние Docker-контейнера

Поля:

- `id uuid primary key`
- `instance_id uuid unique not null`
- `container_id varchar unique`
- `container_name varchar unique not null`
- `image varchar not null`
- `status varchar not null`
- `last_seen_at timestamptz`
- `created_at timestamptz not null`
- `updated_at timestamptz not null`

Ограничения:

- `foreign key (instance_id) references instances(id)`

### instance_members

Хранит доступ пользователей к конкретному инстансу

Поля:

- `instance_id uuid not null`
- `user_id uuid not null`
- `role_code varchar not null`
- `created_by uuid`
- `created_at timestamptz not null`
- `updated_at timestamptz not null`

Ограничения:

- `primary key (instance_id, user_id)`
- `foreign key (instance_id) references instances(id)`

### instance_content

Хранит выбранный контент инстанса

Поля:

- `id uuid primary key`
- `instance_id uuid not null`
- `content_type varchar not null`
- `provider_id varchar not null`
- `project_id varchar not null`
- `version_id varchar not null`
- `display_name varchar not null`
- `file_id uuid`
- `enabled boolean not null`
- `created_at timestamptz not null`
- `updated_at timestamptz not null`

Ограничения:

- `foreign key (instance_id) references instances(id)`

### instance_events

Хранит lifecycle events и audit trail инстанса

Поля:

- `id uuid primary key`
- `instance_id uuid not null`
- `actor_user_id uuid`
- `event_type varchar not null`
- `message text`
- `metadata_json jsonb`
- `created_at timestamptz not null`

Ограничения:

- `foreign key (instance_id) references instances(id)`

### resource_limits

Хранит глобальные лимиты instance-service

Поля:

- `id uuid primary key`
- `memory_total_budget_mb integer not null`
- `memory_system_reserve_mb integer not null`
- `memory_min_per_instance_mb integer not null`
- `memory_max_per_instance_mb integer not null`
- `cpu_policy varchar not null`
- `port_range_start integer not null`
- `port_range_end integer not null`
- `max_running_instances integer not null`
- `updated_by uuid`
- `updated_at timestamptz not null`

## Индексы

- `instances_slug_idx`
- `instances_owner_user_id_idx`
- `instances_status_idx`
- `instance_networks_public_port_idx`
- `docker_containers_instance_id_idx`
- `instance_content_instance_id_idx`
- `instance_content_provider_project_version_idx`
- `instance_events_instance_id_created_at_idx`
