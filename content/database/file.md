# File database

База данных: `cubectl_file`

## Назначение

`file-service` хранит каталог контента, provider metadata, версии, файлы, зависимости, скачивания и результаты проверки обновлений

Эта база нужна для поиска, установки и обновления модов, плагинов и дата-паков

## Таблицы

### content_providers

Хранит провайдеры контента

Поля:

- `id uuid primary key`
- `code varchar unique not null`
- `display_name varchar not null`
- `enabled boolean not null`
- `status varchar not null`
- `last_checked_at timestamptz`
- `created_at timestamptz not null`
- `updated_at timestamptz not null`

### content_projects

Хранит проекты из внешних каталогов

Поля:

- `id uuid primary key`
- `provider_id uuid not null`
- `external_project_id varchar not null`
- `slug varchar`
- `content_type varchar not null`
- `display_name varchar not null`
- `summary text`
- `description text`
- `icon_url varchar`
- `source_url varchar`
- `client_side varchar`
- `server_side varchar`
- `last_synced_at timestamptz`
- `created_at timestamptz not null`
- `updated_at timestamptz not null`

Ограничения:

- `foreign key (provider_id) references content_providers(id)`
- `unique (provider_id, external_project_id)`

### content_versions

Хранит версии проектов

Поля:

- `id uuid primary key`
- `project_id uuid not null`
- `external_version_id varchar not null`
- `version_number varchar not null`
- `name varchar`
- `release_type varchar not null`
- `minecraft_versions text[] not null`
- `loader_types text[] not null`
- `published_at timestamptz`
- `changelog text`
- `created_at timestamptz not null`
- `updated_at timestamptz not null`

Ограничения:

- `foreign key (project_id) references content_projects(id)`
- `unique (project_id, external_version_id)`

### content_files

Хранит файлы версий

Поля:

- `id uuid primary key`
- `version_id uuid not null`
- `external_file_id varchar`
- `file_name varchar not null`
- `download_url text not null`
- `size_bytes bigint`
- `sha1 varchar`
- `sha512 varchar`
- `primary_file boolean not null`
- `created_at timestamptz not null`

Ограничения:

- `foreign key (version_id) references content_versions(id)`

### content_dependencies

Хранит зависимости версий

Поля:

- `id uuid primary key`
- `version_id uuid not null`
- `dependency_project_id uuid`
- `dependency_version_id uuid`
- `external_project_id varchar`
- `external_version_id varchar`
- `dependency_type varchar not null`
- `created_at timestamptz not null`

Ограничения:

- `foreign key (version_id) references content_versions(id)`

### content_downloads

Хранит факты скачивания файлов

Поля:

- `id uuid primary key`
- `instance_id uuid not null`
- `content_file_id uuid not null`
- `target_path varchar not null`
- `status varchar not null`
- `error_message text`
- `downloaded_at timestamptz`
- `created_at timestamptz not null`

Ограничения:

- `foreign key (content_file_id) references content_files(id)`

### content_update_checks

Хранит результаты проверки обновлений

Поля:

- `id uuid primary key`
- `instance_id uuid not null`
- `content_type varchar not null`
- `provider_id uuid not null`
- `project_id uuid not null`
- `current_version_id uuid not null`
- `latest_version_id uuid`
- `status varchar not null`
- `checked_at timestamptz not null`
- `created_at timestamptz not null`

Ограничения:

- `foreign key (provider_id) references content_providers(id)`
- `foreign key (project_id) references content_projects(id)`
- `foreign key (current_version_id) references content_versions(id)`
- `foreign key (latest_version_id) references content_versions(id)`

### content_update_actions

Хранит действия обновления контента

Поля:

- `id uuid primary key`
- `update_check_id uuid not null`
- `instance_id uuid not null`
- `from_version_id uuid not null`
- `to_version_id uuid not null`
- `status varchar not null`
- `requested_by uuid`
- `requested_at timestamptz not null`
- `completed_at timestamptz`
- `error_message text`

Ограничения:

- `foreign key (update_check_id) references content_update_checks(id)`
- `foreign key (from_version_id) references content_versions(id)`
- `foreign key (to_version_id) references content_versions(id)`

## Индексы

- `content_projects_provider_external_idx`
- `content_projects_type_name_idx`
- `content_versions_project_id_idx`
- `content_versions_minecraft_versions_idx`
- `content_versions_loader_types_idx`
- `content_files_version_id_idx`
- `content_dependencies_version_id_idx`
- `content_downloads_instance_id_idx`
- `content_update_checks_instance_id_idx`
- `content_update_checks_status_idx`
- `content_update_actions_instance_id_idx`
