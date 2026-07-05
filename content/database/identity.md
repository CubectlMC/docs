# Identity database

База данных: `cubectl_identity`

## Назначение

`identity-service` хранит пользователей, роли, permissions, refresh tokens и глобальные назначения ролей

Instance-level access хранится в `instance-service`

## Таблицы

### users

Хранит пользователей

Поля:

- `id uuid primary key`
- `username varchar unique not null`
- `password_hash varchar not null`
- `status varchar not null`
- `last_login_at timestamptz`
- `created_at timestamptz not null`
- `updated_at timestamptz not null`
- `deleted_at timestamptz`

### roles

Хранит глобальные роли

Поля:

- `id uuid primary key`
- `code varchar unique not null`
- `display_name varchar not null`
- `description text`
- `system_role boolean not null`
- `created_at timestamptz not null`
- `updated_at timestamptz not null`

### permissions

Хранит permissions

Поля:

- `id uuid primary key`
- `code varchar unique not null`
- `description text`
- `created_at timestamptz not null`

### role_permissions

Связывает роли и permissions

Поля:

- `role_id uuid not null`
- `permission_id uuid not null`
- `created_at timestamptz not null`

Ограничения:

- `primary key (role_id, permission_id)`
- `foreign key (role_id) references roles(id)`
- `foreign key (permission_id) references permissions(id)`

### user_roles

Назначает глобальные роли пользователям

Поля:

- `user_id uuid not null`
- `role_id uuid not null`
- `assigned_by uuid`
- `created_at timestamptz not null`

Ограничения:

- `primary key (user_id, role_id)`
- `foreign key (user_id) references users(id)`
- `foreign key (role_id) references roles(id)`

### refresh_tokens

Хранит refresh tokens для JWT flow

Поля:

- `id uuid primary key`
- `user_id uuid not null`
- `token_hash varchar unique not null`
- `expires_at timestamptz not null`
- `revoked_at timestamptz`
- `created_at timestamptz not null`

Ограничения:

- `foreign key (user_id) references users(id)`

## Индексы

- `users_username_idx`
- `users_status_idx`
- `roles_code_idx`
- `permissions_code_idx`
- `refresh_tokens_user_id_idx`
- `refresh_tokens_expires_at_idx`
