# Quickstart: Frontend Docker + Nginx integrado a Compose

## Prerrequisitos

- Docker Engine + Docker Compose v2
- Repositorio clonado en la rama `008-dockerize-frontend-nginx`

## 1) Levantar stack completo con frontend

1. Desde raíz del repositorio:
   - `docker compose -f docker/docker-compose.yml up --build -d`
2. Verificar estado:
   - `docker compose -f docker/docker-compose.yml ps`
3. Confirmar frontend accesible:
   - Abrir `http://localhost:4200`

## 2) Validar healthcheck frontend

1. Inspeccionar estado de salud:
   - `docker inspect --format='{{json .State.Health}}' empleados-frontend`
2. Confirmar que el estado final sea `healthy`.

## 3) Validar configuración de API base

1. Ejecutar con default local (sin sobreescribir variable) y comprobar que la UI funciona contra `http://localhost:8080/api/v1`.
2. (Opcional) Sobrescribir variable en Compose:
   - `API_BASE_URL=/api/v1 docker compose -f docker/docker-compose.yml up --build -d`
3. Confirmar que las llamadas de red usan la URL configurada.

## 4) Ejecutar pruebas automatizadas del contenedor frontend

1. Ejecutar smoke test:
   - `bash docker/tests/frontend-smoke.sh`
2. Ejecutar integración de stack:
   - `bash docker/tests/frontend-stack-integration.sh`
3. Ejecutar validación runtime config:
   - `bash docker/tests/frontend-runtime-config.sh`
4. Ejecutar runner unificado:
   - `bash docker/tests/run-frontend-container-tests.sh`

## 5) Validar rutas SPA

1. Abrir una ruta interna de frontend directamente (ejemplo: `/login` o `/departamentos`).
2. Refrescar navegador.
3. Confirmar que no aparece 404 de Nginx y la app Angular carga correctamente.

## 6) Verificar Swagger/OpenAPI

1. Validar disponibilidad de contrato OpenAPI:
   - `curl -fsS http://localhost:8080/v3/api-docs | head`
2. Confirmar respuesta HTTP exitosa.

## 7) Verificar paginación de 10

1. Ejecutar consulta de colección paginada:
   - `curl -fsS "http://localhost:8080/api/v1/departamentos?page=0" -u "admin@localhost:AdminLocal123!"`
2. Confirmar que el payload reporta tamaño de página fijo de 10 elementos.

## 8) Verificar preservación de HTTP Basic Auth

1. Sin credenciales debe fallar:
   - `curl -i http://localhost:8080/api/v1/departamentos?page=0`
2. Con credenciales válidas debe responder exitosamente:
   - `curl -i -u "admin@localhost:AdminLocal123!" http://localhost:8080/api/v1/departamentos?page=0`

## 9) Apagar stack

- `docker compose -f docker/docker-compose.yml down`

## Evidencias mínimas de aceptación

- `frontend` en estado `healthy`.
- UI accesible por `http://localhost:4200`.
- Backend y PostgreSQL siguen operativos en el mismo `docker-compose`.
- Rutas internas de SPA se resuelven al refrescar.
- OpenAPI/Swagger continúa accesible tras integrar frontend.
- La paginación de 10 elementos no presenta regresión.
- La seguridad HTTP Basic Auth se mantiene vigente.

## Evidencia de ejecución (2026-03-26)

- Contexto de validación:
  - El daemon Docker no fue accesible en este entorno (`permission denied` sobre `/var/run/docker.sock`), por lo que la validación E2E completa del stack con Compose queda pendiente (T030).
  - Se ejecutaron validaciones de no regresión sobre API local activa en `http://localhost:8080`.
- Verificación Swagger/OpenAPI (T031):
  - `GET /v3/api-docs` -> `200 OK`.
  - `openapi` reportado: `3.0.1`.
  - Presencia de ruta: `/api/v1/departamentos` = `true`.
- Verificación paginación fija de 10 (T032):
  - `GET /api/v1/departamentos?page=0` con Basic Auth válido -> `200 OK`.
  - Campos observados en payload: `size=10`, `content_len=2`.
- Verificación preservación HTTP Basic Auth (T033):
  - Sin credenciales: `GET /api/v1/departamentos?page=0` -> `401 Unauthorized`.
   - Con credenciales válidas (`admin@localhost:AdminLocal123!`): `GET /api/v1/departamentos?page=0` -> `200 OK`.
