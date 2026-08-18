# Accommodations Tourism – Base de Datos PostgreSQL

## Descripción

Este proyecto contiene un script de restauración y carga de datos para una base de datos PostgreSQL llamada **`accommodations_tourism`**.

La base de datos está orientada a la administración de alojamientos turísticos, huéspedes, propietarios, reservas, habitaciones, pagos, reseñas y usuarios del personal.

El archivo SQL fue generado a partir de un dump de PostgreSQL y está indicado como compatible con **PostgreSQL 14 o superior**. El script también incluye datos de ejemplo y consultas SQL utilizadas para realizar pruebas sobre la información almacenada.

## Tecnologías utilizadas

- **PostgreSQL**
- **SQL**
- **PL/pgSQL**
- **pgAdmin** (para administrar y ejecutar la base de datos)
- **psql** (para ejecutar el script desde la terminal)

## Base de datos

Nombre:

```text
accommodations_tourism
```

El script establece la codificación:

```text
UTF8
```

y utiliza el esquema:

```text
public
```

## Estructura de la base de datos

El proyecto contiene las siguientes tablas principales:

| Tabla | Descripción |
|---|---|
| `accommodation_types` | Tipos de alojamiento, como Hotel, Hostel, Apartment, House, Villa, Cabin, Resort y Guesthouse. |
| `accommodations` | Información de los alojamientos, incluyendo propietario, ubicación, precio, capacidad y horarios. |
| `amenities` | Servicios disponibles, como WiFi, piscina, estacionamiento, aire acondicionado, cocina y gimnasio. |
| `accommodation_amenities` | Relaciona alojamientos con sus servicios. |
| `owners` | Información de los propietarios de los alojamientos. |
| `locations` | País, estado, ciudad, dirección y coordenadas de los alojamientos. |
| `guests` | Información de los huéspedes. |
| `booking_statuses` | Estados de una reserva, como Pending, Confirmed, CheckedIn, CheckedOut, Cancelled y NoShow. |
| `bookings` | Información de las reservas, fechas, huéspedes, alojamiento, cantidades y montos. |
| `booking_guests` | Información adicional de los huéspedes asociados a una reserva. |
| `rooms` | Habitaciones disponibles dentro de los alojamientos. |
| `payments` | Pagos realizados para las reservas. |
| `reviews` | Reseñas y calificaciones realizadas por los huéspedes. |
| `staff_users` | Usuarios del personal y sus roles dentro del sistema. |

## Funciones

El script crea la función:

```sql
set_updated_at()
```

Esta función actualiza automáticamente el campo `updated_at` con la fecha y hora actuales cuando se modifica un registro.

La función está implementada en **PL/pgSQL**.

## Secuencias

Se utilizan secuencias para generar identificadores automáticamente en varias tablas. Entre ellas:

- `accommodation_types_accommodation_type_id_seq`
- `accommodations_accommodation_id_seq`
- `amenities_amenity_id_seq`
- `booking_guests_booking_guest_id_seq`
- `booking_statuses_booking_status_id_seq`
- `bookings_booking_id_seq`
- `guests_guest_id_seq`
- `locations_location_id_seq`
- `owners_owner_id_seq`
- `payments_payment_id_seq`
- `reviews_review_id_seq`
- `rooms_room_id_seq`
- `staff_users_staff_user_id_seq`

Estas secuencias están asociadas a las columnas de identificación correspondientes.

## Relaciones principales

La base de datos utiliza claves foráneas para mantener la integridad de los datos.

Algunas relaciones importantes son:

```text
owners
   │
   └── accommodations
          ├── locations
          ├── accommodation_types
          ├── accommodation_amenities ── amenities
          └── rooms

guests
   │
   └── bookings
          ├── accommodations
          ├── rooms
          ├── booking_statuses
          ├── booking_guests
          ├── payments
          └── reviews
```

La tabla `accommodation_amenities` funciona como una relación entre alojamientos y servicios.

## Integridad y restricciones

El script incluye:

- Claves primarias.
- Claves foráneas.
- Restricciones `UNIQUE`.
- Restricciones `CHECK`.
- Índices.
- Secuencias.
- Triggers.
- Valores predeterminados.

Entre las validaciones existentes se encuentran:

- El precio por noche no puede ser negativo.
- La cantidad máxima de huéspedes debe ser mayor que cero.
- La cantidad de habitaciones y baños no puede ser negativa.
- Una reserva debe tener una fecha de salida posterior a la fecha de entrada.
- El número de adultos de una reserva debe ser como mínimo 1.
- Las cantidades monetarias no pueden ser negativas.
- La calificación de una reseña debe estar entre 1 y 5.
- La capacidad de una habitación debe ser mayor que cero.

## Índices

El script crea índices para facilitar las consultas relacionadas con:

- Alojamientos y ubicaciones.
- Alojamientos y propietarios.
- Reservas y alojamientos.
- Reservas y fechas.
- Reservas y huéspedes.
- Reservas y habitaciones.
- Reservas y estados.
- Pagos y reservas.
- Reseñas y alojamientos.
- Reseñas y huéspedes.
- Habitaciones y alojamientos.

## Triggers

Se utilizan triggers para actualizar automáticamente `updated_at` en:

- `accommodations`
- `bookings`
- `guests`
- `owners`
- `rooms`
- `staff_users`

Estos triggers ejecutan la función `set_updated_at()` antes de realizar una actualización.

## Datos incluidos

El script contiene datos de prueba para diferentes entidades de la base de datos.

Entre los datos incluidos se encuentran:

- 8 tipos de alojamiento.
- 10 servicios.
- 20 propietarios.
- 20 ubicaciones.
- 20 alojamientos iniciales.
- Datos de huéspedes.
- Estados de reservas.
- Reservas.
- Pagos.
- Reseñas.
- Habitaciones.
- Usuarios del personal.

Además, el script contiene registros adicionales agregados posteriormente para realizar pruebas de inserción, actualización, eliminación y consultas.

## Instalación

### Requisitos

Se necesita:

1. PostgreSQL 14 o superior.
2. Un usuario con permisos para crear bases de datos.
3. El archivo SQL del proyecto.

### Crear la base de datos

Desde `psql`, se puede crear la base de datos con:

```bash
psql -U postgres -c "CREATE DATABASE accommodations_tourism WITH TEMPLATE=template0 ENCODING='UTF8' LOCALE_PROVIDER=libc LOCALE='en_US.UTF-8';"
```

Después, se puede ejecutar el script:

```bash
psql -U postgres -d accommodations_tourism -f accommodation_database_restore.sql
```

También se puede abrir el archivo SQL desde **pgAdmin** y ejecutarlo sobre la base de datos `accommodations_tourism`.

## Consultas incluidas

El archivo contiene ejemplos de diferentes operaciones SQL.

### SELECT

Se realizan consultas para:

- Mostrar alojamientos activos.
- Filtrar huéspedes por nacionalidad.
- Buscar reservas dentro de un rango de fechas.

Ejemplo:

```sql
SELECT *
FROM public.accommodations
WHERE is_active = true
ORDER BY accommodation_id ASC;
```

### INSERT

Se incluyen ejemplos para insertar:

- Propietarios.
- Alojamientos.
- Huéspedes de reservas.
- Reservas.
- Pagos.

### UPDATE

Se muestran ejemplos para:

- Modificar el precio de un alojamiento.
- Cambiar el estado de una reserva.

Ejemplo:

```sql
UPDATE accommodations
SET base_price_per_night = '600'
WHERE name = 'Panoramic Stay Ville';
```

### DELETE

También se incluye un ejemplo para eliminar una reseña:

```sql
DELETE FROM reviews
WHERE review_id = 7;
```

### INNER JOIN

Se utilizan `INNER JOIN` para combinar información de diferentes tablas.

Por ejemplo, se puede consultar el nombre del huésped junto con las noches y el monto total de su reserva.

### LEFT JOIN

Se utilizan `LEFT JOIN` para encontrar alojamientos que:

- No tienen reseñas.
- No tienen reservas.

### Funciones de agregación

El proyecto incluye ejemplos con:

- `SUM()` para calcular ingresos totales.
- `AVG()` para obtener el promedio de calificaciones.
- `COUNT()` para contar reservas.

## Restauración del proyecto

El orden general del script es:

1. Configuración de PostgreSQL.
2. Creación de funciones.
3. Creación de secuencias.
4. Creación de tablas.
5. Configuración de valores predeterminados.
6. Asociación de secuencias con columnas.
7. Inserción de datos.
8. Ajuste de los valores de las secuencias.
9. Creación de claves primarias y restricciones `UNIQUE`.
10. Creación de índices.
11. Creación de triggers.
12. Creación de claves foráneas.
13. Ejecución de consultas y operaciones adicionales de prueba.

## Consideraciones

El archivo contiene datos generados para pruebas y demostración. Algunos nombres, direcciones, correos electrónicos, descripciones y otros valores son datos de ejemplo.

El script también contiene una sección adicional digitalizada a partir de capturas de pantalla de pgAdmin, donde se muestran operaciones SQL de prueba como `INSERT`, `SELECT`, `UPDATE`, `DELETE`, `JOIN` y funciones de agregación.

## Objetivo del proyecto

El objetivo de esta base de datos es representar y administrar la información relacionada con un sistema de alojamientos turísticos.

Permite organizar:

- Alojamientos.
- Propietarios.
- Ubicaciones.
- Servicios.
- Habitaciones.
- Huéspedes.
- Reservas.
- Estados de reservas.
- Pagos.
- Reseñas.
- Usuarios del personal.

De esta manera, se puede utilizar la base de datos para practicar y demostrar conceptos de **SQL, relaciones entre tablas, claves primarias y foráneas, consultas, JOIN, funciones de agregación, índices, secuencias y triggers**.
