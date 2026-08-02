# Contenerización con Docker — Shortly

Esta guía explica cómo construir, levantar y administrar la aplicación Shortly
usando Docker y Docker Compose.

## Prerrequisitos

- **Docker Desktop** (incluye Docker Engine + Docker Compose v2) — versión 4.x o superior.
  Descarga: https://www.docker.com/products/docker-desktop/
- **Docker Compose** v2.20+ (ya viene integrado en Docker Desktop, se invoca como `docker compose`, sin guion).
- En Windows: WSL2 habilitado (Docker Desktop lo solicita durante su instalación).

Verifica tu instalación:
```bash
docker --version
docker compose version
```

## Configuración inicial

1. Copia el archivo de variables de entorno de ejemplo y ajusta los valores si lo necesitas:
```bash
   cp .env.example .env
```
2. Revisa `.env` — por defecto usa credenciales de desarrollo (`postgres_dev_password`). **Cambia estos valores antes de cualquier despliegue real.**

## Construir y levantar la aplicación

```bash
docker compose up --build
```

Esto va a:
1. Construir la imagen `shortly-web` a partir del `Dockerfile` (multi-stage build).
2. Descargar la imagen de `postgres:16-alpine` si no la tienes localmente.
3. Crear la red interna `shortly-network` y el volumen nombrado `shortly-db-data`.
4. Esperar a que la base de datos reporte `healthy` antes de iniciar la aplicación web.
5. Sembrar datos iniciales (usuario admin + links de ejemplo) en el primer arranque.

Una vez levantado, la aplicación queda disponible en:

http://localhost:8080


## Detener y limpiar

Para detener los contenedores (conserva el volumen de datos, así no pierdes la base de datos):
```bash
docker compose down
```

## Ejecutar ya realizado el Builds
Para ejecutar la aplicación si ya se buildeo anteriormente:
```bash
docker compose up
```

## Verificar el estado de salud

```bash
docker compose ps
```

También puedes consultar el endpoint de salud directamente:

http://localhost:8080/health