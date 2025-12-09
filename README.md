# BDHSuiteCore – Entorno PostgreSQL 18 en Docker

Este proyecto proporciona un entorno **aislado, reproducible y portátil** de PostgreSQL 18 utilizando Docker Compose.

Incluye:

- PostgreSQL 18.1
- Usuario, contraseña y base de datos configurados desde `.env`
- Puerto personalizado
- Persistencia mediante volúmenes
- Logs accesibles desde el host

---

## 📂 Estructura del proyecto

```
BDHSuiteCore/
│
├── .env
├── docker-compose.yml
└── logs/        # PostgreSQL logs visibles desde el host
```

---

## 🔧 Requisitos

Antes de iniciar, asegúrate de tener instalado:

- Docker
- Docker Compose (integrado en Docker Desktop o paquete `docker-compose-plugin`)

Verificar:

```bash
docker --version
docker compose version
```

---

## ⚙️ Configuración del entorno

Toda la configuración del servicio se encuentra en el archivo `.env`:

```
POSTGRES_USER=HSuiteCoreUser
POSTGRES_PASSWORD=HSuiteCorePassword
POSTGRES_DB=BDHSuiteCore
POSTGRES_PORT=5442
POSTGRES_VOLUME=BDHSuiteCore_pgdata
```

Puedes ajustar cualquier valor según tus necesidades.

---

## ▶️ Cómo iniciar el contenedor

Desde la raíz del proyecto:

```bash
docker compose up -d
```

Esto realizará:

- Descarga de la imagen `postgres:18.1` (solo la primera vez)
- Creación del contenedor `BDHSuiteCore`
- Creación del volumen persistente
- Exposición de PostgreSQL en el puerto configurado

Verificar que el contenedor está corriendo:

```bash
docker ps
```

---

## 🛑 Cómo detener el contenedor

```bash
docker compose down
```

Esto detiene el contenedor pero **no elimina los datos**, ya que permanecen en el volumen.

---

## 🗑️ Cómo eliminar completamente datos y contenedor

> ⚠️ Esto eliminará la base de datos completa.

```bash
docker compose down
docker volume rm BDHSuiteCore_pgdata
```

---

## 🔌 Cómo conectarse a PostgreSQL

Puedes usar cualquier cliente compatible (psql, DBeaver, PgAdmin, etc.).

### Conexión desde terminal:

```bash
psql -h localhost -p 5442 -U HSuiteCoreUser -d BDHSuiteCore
```

Contraseña:

```
HSuiteCorePassword
```

### Configuración para herramientas GUI:

| Campo       | Valor               |
|-------------|----------------------|
| Host        | localhost            |
| Port        | 5442                 |
| User        | HSuiteCoreUser       |
| Password    | HSuiteCorePassword   |
| Database    | BDHSuiteCore         |

---

## 📜 Logs de PostgreSQL

Los logs están disponibles directamente en el host:

```
BDHSuiteCore/logs/
```

Monitorear logs:

```bash
tail -f logs/postgresql.log
```

---

## 🔍 Troubleshooting

### ❌ No puedo conectarme al puerto

Verificar si el contenedor está levantado:

```bash
docker ps
```

Ver logs del contenedor:

```bash
docker compose logs BDHSuiteCore
```

### ❌ Cambié la configuración y PostgreSQL no inicia

```bash
docker compose down
docker compose up -d
```

### ❌ Reiniciar completamente la base de datos (factory reset)

```bash
docker compose down
docker volume rm BDHSuiteCore_pgdata
docker compose up -d
```

---

## ✨ Autor

Proyecto configurado por **Johan**.