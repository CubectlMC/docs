# API endpoints

Base path: `/api/v1`

## gateway

```text
GET /health
GET /status
GET /version
```

## identity-service

```text
POST /auth/register
POST /auth/login
POST /auth/logout
POST /auth/refresh
GET  /auth/me

GET    /users
GET    /users/{user_id}
PATCH  /users/{user_id}
DELETE /users/{user_id}

GET    /roles
POST   /roles
GET    /roles/{role_id}
PATCH  /roles/{role_id}
DELETE /roles/{role_id}

GET    /permissions

GET    /users/{user_id}/roles
POST   /users/{user_id}/roles
DELETE /users/{user_id}/roles/{role_id}
```

## instance-service

```text
GET    /instances
POST   /instances
GET    /instances/{instance_id}
PATCH  /instances/{instance_id}
DELETE /instances/{instance_id}

POST   /instances/{instance_id}/start
POST   /instances/{instance_id}/stop
POST   /instances/{instance_id}/restart

GET    /instances/{instance_id}/content
PATCH  /instances/{instance_id}/content

GET    /instances/{instance_id}/members
POST   /instances/{instance_id}/members
PATCH  /instances/{instance_id}/members/{user_id}
DELETE /instances/{instance_id}/members/{user_id}

GET    /instances/{instance_id}/logs/live

GET    /admin/resources
PATCH  /admin/resources
```

## file-service

```text
GET /mods/search
GET /mods/{mod_id}
GET /mods/{mod_id}/versions
GET /mods/{mod_id}/versions/{version_id}

GET /plugins/search
GET /plugins/{plugin_id}
GET /plugins/{plugin_id}/versions
GET /plugins/{plugin_id}/versions/{version_id}

GET /datapacks/search
GET /datapacks/{datapack_id}
GET /datapacks/{datapack_id}/versions
GET /datapacks/{datapack_id}/versions/{version_id}

GET /content-providers
GET /content-providers/{provider_id}/status

GET  /instances/{instance_id}/content/updates
POST /instances/{instance_id}/content/updates/check
POST /instances/{instance_id}/content/updates/{update_id}/apply
```

## metric-service

```text
GET /instances/{instance_id}/metrics/current
GET /instances/{instance_id}/metrics/live

GET /metrics/host/current
GET /metrics/host/live
```

## Исключено из MVP

- `/auth/sessions`
- `/auth/events`
- `/admin/auth/events`
- `/admin/users/activity`
- `/instances/{instance_id}/commands`
- `/instances/{instance_id}/server-properties`
- `/instances/{instance_id}/dns-records`
- `/instances/{instance_id}/metrics/history`
- `/metrics/host/history`
- `/instances/{instance_id}/files`
- `/instances/{instance_id}/mods`
- `/instances/{instance_id}/plugins`
- `/instances/{instance_id}/datapacks`
- `/config-files`
- `/resource-packs`
- `/backups`
- `/install-plan`
- `/generic-file-upload`
