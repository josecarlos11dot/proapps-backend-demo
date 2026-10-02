# ProApps Carwash — Backend (demo)

API REST y servidor en tiempo real del sistema de operación de mi autolavado. El frontend está en el repositorio `proapps-dashboard-demo`, donde se explica el proyecto completo.

> **Versión demo:** con `DEMO_MODE=true`, el servidor carga datos ficticios al arrancar (si la base está vacía) y los reinicia todos los días a las 3:00 a.m. (hora de México). No contiene datos de clientes reales.

## Tecnologías

Node.js · Express 5 · MongoDB con Mongoose · Socket.io · node-cron

## Endpoints principales

| Método | Ruta | Descripción |
|---|---|---|
| POST | `/api/registros/placa` | Registra un auto por placa; avisa si ya está en servicio |
| POST | `/api/registros/foto` | Registra un auto solo con foto (placa pendiente) |
| PATCH | `/api/registros/:id/completar` | Asigna color, tamaño, paquete, lavador y precio |
| PATCH | `/api/registros/:id/editar` | Cambia paquete, lavador o tipo (normal, cortesía, taller) |
| GET | `/api/registros/activos` | Autos en espera y en lavado del día |
| GET | `/api/registros/buscar` | Historial por placa o modelo |
| DELETE | `/api/registros/:id` | Cancela un registro |
| GET / POST | `/api/cortes` | Historial de cortes / genera el corte del día |
| GET / POST / PATCH | `/api/lavadores` | Consulta, alta y edición de lavadores |
| GET / PATCH | `/api/precios` | Tabla de precios editable |

Eventos de Socket.io emitidos a los clientes: `nuevo-registro`, `registro-actualizado`, `registro-editado`, `registro-cancelado`.

## Datos de la demo

El archivo `src/demo/seed.js` genera de forma determinista:

- 5 lavadores activos y 1 inactivo, con nombres inventados.
- 60 vehículos con placas ficticias (`DMO-101-B`, `DMO-102-C`, …).
- Unos 45 días de historial con sus cortes diarios, para que los reportes de semana y mes tengan datos.
- Autos del día en espera y en lavado, incluido uno nuevo con foto genérica.

Para reiniciar la demo a mano: `npm run seed` (solo funciona con `DEMO_MODE=true`, porque borra toda la base).

## Variables de entorno

| Variable | Descripción |
|---|---|
| `MONGODB_URI` | Conexión a una base **exclusiva para la demo** |
| `DEMO_MODE` | `true` para cargar datos ficticios y activar el reinicio nocturno |
| `PORT` | Puerto (Railway lo asigna automáticamente) |

## Despliegue de la demo

1. **Base de datos:** en MongoDB Atlas crea un cluster gratuito nuevo (o una base nueva, por ejemplo `proapps_demo`) y copia su cadena de conexión. Nunca uses la de producción.
2. **Backend en Railway:** crea un proyecto nuevo desde este repositorio y agrega las variables `MONGODB_URI` y `DEMO_MODE=true`. Al arrancar, el servidor carga los datos ficticios. En Settings → Networking genera un dominio público y cópialo.
3. **Frontend en Vercel:** importa el repositorio `proapps-dashboard-demo` y agrega las variables `VITE_API_URL` (el dominio de Railway del paso anterior) y `VITE_DEMO_MODE=true`.

## Correr en local

```bash
npm install
cp .env.example .env   # llena MONGODB_URI con la base de demo
npm run dev
```
# proapps-backend-demo
