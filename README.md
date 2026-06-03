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

## Variables importantes

- `POSTGRES_PASSWORD`: contraseña de PostgreSQL.
- `SPRING_DATASOURCE_URL`: URL del datasource del backend.
- `APP_BASIC_AUTH_USER` y `APP_BASIC_AUTH_PASSWORD`: usuario bootstrap de acceso.
- `API_BASE_URL`: URL base usada por el frontend en runtime.

## Personalización

Antes de publicar el repositorio, cambia los valores de ejemplo en `.env`, `docker/.env.example`, Postman y Cypress por los tuyos reales. El código ya queda preparado para leerlos desde variables de entorno.

## Repositorio propio

No puedo crear ni publicar el repositorio remoto desde aquí, pero el proyecto ya queda preparado para que lo subas a tu GitHub personal con tu propio remoto.

```bash
git remote remove origin
git remote add origin git@github.com:TU_USUARIO/TU_REPO.git
git push -u origin main
```