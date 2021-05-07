---
description: Setting up a shared PostgreSQL instance for the micro-services.
---

# Shared postgres

Currently the following services use Postgres as their primary database:

* Configuration service
* Madoc TS
* Tasks API
* Model API

Each service requires a database or schema in a database, a username and password. These are configured through environment variables when you are using the docker-compose. Check the development [docker-compose.yml](https://github.com/digirati-co-uk/madoc-platform/blob/v2/docker-compose.yml) for reference on where these are used.

| Environment variable | Description |
| :--- | :--- |
| `POSTGRES_DB` | The database name |
| `POSTGRES_PORT` | The port of the database |
| `POSTGRES_USER` | Default Postgres user |
| `POSTGRES_PASSWORD` | Default Postgres password |
| `POSTGRES_MADOC_TS_USER` | Madoc TS database user |
| `POSTGRES_MADOC_TS_SCHEMA` | Madoc TS database schema |
| `POSTGRES_MADOC_TS_PASSWORD` | Madoc TS database password |
| `POSTGRES_TASKS_API_USER` | Tasks API database user |
| `POSTGRES_TASKS_API_SCHEMA` | Tasks API database schema |
| `POSTGRES_TASKS_API_PASSWORD` | Tasks API database password |
| `POSTGRES_MODELS_USER` | Models API database user |
| `POSTGRES_MODELS_SCHEMA` | Models API database schema |
| `POSTGRES_MODELS_PASSWORD` | Models API database password |
| `POSTGRES_CONFIG_SERVICE_USER` | Config API database user |
| `POSTGRES_CONFIG_SERVICE_SCHEMA` | Config API database schema |
| `POSTGRES_CONFIG_SERVICE_PASSWORD` | Config API database password |

These are referenced in the docker compose. There are 2 ways to connect Madoc to an external Postgres. You can create a single database with multiple schemas, or you can split into multiple databases. The docker-compose is an example of the former, where a single database is created \(`postgres`\) and then a schema created for each service, and a role/user with access to that particular schema.

Each service from the list can be configured with different environment variables if you decide to configure the database differently.

An example provisioning script can be found [here](https://github.com/digirati-co-uk/madoc-platform/blob/v2/services/shared-postgres/entrypoint.sh) that takes you through the steps of using and creating the required roles, extensions and schemas.

### Database extensions

The following extensions are required by various services:

| Extension | Description | How |
| :--- | :--- | :--- |
| `uuid-ossp` | Allows us to index using UUIDs | `CREATE EXTENSION IF NOT EXISTS "uuid-ossp";` |
| `ltree` | Efficient storing of nested elements, used for creating indexes. | `CREATE EXTENSION IF NOT EXISTS "ltree";` |

### Role search path

When you create a user or role in Postgres you can also set a default search path.

```text
ALTER ROLE $ROLE_NAME SET search_path TO $SCHEMA_NAME, public;
```

Although this may not be required - this is how the services are tested and would be recommended.

### Database schemas

All of our services will bootstrap themselves if provided with database credentials on first start up, they will also migrate themselves if any schemas change. There is no requirement to add any tables or data when you create the database.

## Docker image reference

This is a verbose reference for the environment variables required for Postgres for each of the services. A fully up-to-date version of this can be derived from the [docker-compose ](https://github.com/digirati-co-uk/madoc-platform/blob/v2/docker-compose.yml)in the main Madoc repository. You can also see the default values of these match up to the environment variables listed above.

### Madoc TS

| Environment variable | Description |
| :--- | :--- |
| `DATABASE_HOST` | Resolvable hostname for connecting to the Postgres database. This has to to resolvable from inside of the container. |
| `DATABASE_NAME` | The name of the Postgres database |
| `DATABASE_PORT` | Port of the Postgres database. |
| `DATABASE_USER` | User or role that will be used to connect to Postgres. |
| `DATABASE_SCHEMA` | Schema that will be used when connecting to Postgres. |
| `DATABASE_PASSWORD` | Password matching the role that will be used to connect to Postgres. |

### Tasks API

| Environment variable | Description |
| :--- | :--- |
| `DATABASE_HOST` | Resolvable hostname for connecting to the Postgres database. This has to to resolvable from inside of the container. |
| `DATABASE_NAME` | The name of the Postgres database |
| `DATABASE_PORT` | Port of the Postgres database. |
| `DATABASE_USER` | User or role that will be used to connect to Postgres. |
| `DATABASE_SCHEMA` | Schema that will be used when connecting to Postgres. |
| `DATABASE_PASSWORD` | Password matching the role that will be used to connect to Postgres. |

### Model API

| Environment variable | Description |
| :--- | :--- |
| `DATABASE_HOST` | Resolvable hostname for connecting to the Postgres database. This has to to resolvable from inside of the container. |
| `DATABASE_NAME` | The name of the Postgres database |
| `DATABASE_PORT` | Port of the Postgres database. |
| `DATABASE_USER` | User or role that will be used to connect to Postgres. |
| `DATABASE_SCHEMA` | Schema that will be used when connecting to Postgres. |
| `DATABASE_PASSWORD` | Password matching the role that will be used to connect to Postgres. |

### Config service

| Environment variable | Description |
| :--- | :--- |
| `POSTGRES_HOST` | Resolvable hostname for connecting to the Postgres database. This has to to resolvable from inside of the container. |
| `POSTGRES_DB` | The name of the Postgres database |
| `POSTGRES_PORT` | Port of the Postgres database. |
| `POSTGRES_USER` | User or role that will be used to connect to Postgres. |
| `POSTGRES_SCHEMA` | Schema that will be used when connecting to Postgres. |
| `POSTGRES_PASSWORD` | Password matching the role that will be used to connect to Postgres. |
| `DATABASE_URL` | A full connection string for connecting to Postgres - required along with other. |

Note: the `DATABASE_URL` can be made using existing environment variables:

```text
postgres://${POSTGRES_USER}:${POSTGRES_PASSWORD}@shared-postgres:${POSTGRES_PORT}/${POSTGRES_DB}
```

