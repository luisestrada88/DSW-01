# Empleados

Proyecto full-stack para autenticación y gestión de empleados con backend Spring Boot, frontend Angular, PostgreSQL y Docker Compose.

## Arranque local

1. Copia los ejemplos de entorno:

```bash
cp .env.example .env
cp docker/.env.example docker/.env
```

2. Ajusta los valores de `.env` y `docker/.env` a tu máquina.

3. Levanta el stack:

```bash
docker compose -f docker/docker-compose.yml up --build -d
```

4. Abre el frontend en:

```text
http://localhost:4200
```

## Nexus local

1. Levanta Nexus en un contenedor dedicado:

```bash
docker compose -f docker/nexus-compose.yml up -d
```

2. Abre la consola en:

```text
http://localhost:8081
```

3. Obtén la contraseña inicial de `admin`:

```bash
docker exec -it nexus cat /nexus-data/admin.password
```

4. Si quieres usar Maven contra Nexus, copia el archivo de ejemplo:

```bash
cp docker/nexus-settings.xml ~/.m2/settings.xml
```

## Variables importantes

- `POSTGRES_PASSWORD`: contraseña de PostgreSQL.
- `SPRING_DATASOURCE_URL`: URL del datasource del backend.
- `APP_BASIC_AUTH_USER` y `APP_BASIC_AUTH_PASSWORD`: usuario bootstrap de acceso.
- `API_BASE_URL`: URL base usada por el frontend en runtime.
- `docker/nexus-compose.yml`: stack mínimo para levantar Sonatype Nexus local.
- `docker/nexus-settings.xml`: configuración de Maven para usar Nexus local.

## Personalización

Antes de publicar el repositorio, cambia los valores de ejemplo en `.env`, `docker/.env.example`, Postman y Cypress por los tuyos reales. El código ya queda preparado para leerlos desde variables de entorno.

## Repositorio propio

El remoto ya quedó apuntando a tu GitHub personal y el proyecto está listo para seguir trabajando desde ahí.

## Workflows de GitHub Actions

- `01 CI Tests`: corre en push a `main` y `develop`, y también se puede lanzar manualmente desde GitHub Actions.
- `02-SonarCloud`: corre después de `01 CI Tests` o se puede lanzar manualmente.
- `03-Cypress Tests`: corre después de `02-SonarCloud` o manualmente.
- `04-DockerHub-Publish`: corre después de `03-Cypress Tests` o manualmente, con un tag opcional.

Para ejecutarlos desde GitHub:

1. Abre la pestaña `Actions` del repo.
2. Selecciona el workflow.
3. Pulsa `Run workflow` si aparece la opción manual.
4. En `04-DockerHub-Publish`, puedes escribir un `image_tag` personalizado; si lo dejas vacío, usa un tag automático.