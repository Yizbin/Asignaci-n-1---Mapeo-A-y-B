# Parte A: Taller Mecánico — Mapeo de Dominios a Prisma

Este repositorio contiene la solución a la **Parte A** de la Asignación 1 para la asignatura de *Tópicos de Aplicaciones Web*. Se realiza el mapeo de un dominio de taller mecánico utilizando **Prisma ORM** y **MySQL**.

## Modelos Mapeados
- **Cliente**: `id`, `nombre`, `telefono`, `correo` (único).
- **Vehiculo**: `id`, `placa` (única), `marca`, `modelo`, `anio`, y relación a `Cliente`.
- **OrdenServicio**: `id`, `descripcion`, `fechaIngreso`, `estado` (enum: `abierta`, `en_proceso`, `entregada`, `cancelada`), y relación a `Vehiculo`.
- **Refaccion**: `id`, `nombre`, `precio`, y relación a `OrdenServicio`.

---

## Pregunta de la Asignación

### **¿Qué pasaría si intentaras borrar un Cliente que todavía tiene un Vehiculo?**

Por defecto, Prisma configura las restricciones de clave foránea en MySQL con la acción `ON DELETE RESTRICT` (o `NO ACTION`). Si se intenta eliminar un registro de `Cliente` que tiene asociado uno o más registros en `Vehiculo`, MySQL bloqueará la operación arrojando un error de violación de restricción de clave foránea (`Foreign Key Constraint Failure`). 

Para poder eliminar el cliente, primero se tendrían que borrar manualmente sus vehículos asociados o configurar explícitamente un borrado en cascada (`ON DELETE CASCADE`) en la relación del esquema de Prisma.

---

## 📁 Estructura del Proyecto
- `prisma/schema.prisma`: Definición de los modelos y enums.
- `prisma/migrations/`: Archivos de migración SQL generados automáticamente.
- `evidencias/`: Captura de pantalla de la base de datos y sus tablas generadas.

- # Parte B: Reservaciones de Restaurante — Mapeo de Dominios a Prisma

Este repositorio contiene la solución a la **Parte B** de la Asignación 1 para la asignatura de *Tópicos de Aplicaciones Web*. Se realiza el mapeo de un dominio de sistema de reservaciones utilizando **Prisma ORM** y **MySQL**.

## Modelos Mapeados
- **Cliente**: `id`, `nombre`, `telefono`, `correo` (único).
- **Mesa**: `id`, `numero` (único), `capacidad`.
- **Turno**: `id`, `nombre`, `horaInicio`, `horaFin`.
- **Reservacion**: `id`, `fecha`, `estado` (enum: `confirmada`, `cancelada`, `completada`), y sus relaciones a `Cliente`, `Mesa` y `Turno`.

---

## Pregunta de la Asignación

### **¿Por qué esa combinación única evita reservar la misma mesa dos veces en el mismo turno?**

Al definir un índice compuesto único (`@@unique([mesaId, turnoId, fecha])`) a nivel de base de datos entre las columnas de mesa, turno y fecha, MySQL crea una restricción de unicidad (`Composite UNIQUE Index`). 

Esto garantiza que no puedan existir dos filas en la tabla `Reservacion` con los mismos valores coincidentes en esas tres columnas. Si la base de datos detecta un intento de inserción o actualización con un duplicado exacto de esa combinación, rechazará la transacción e impedirá el overbooking automáticamente a nivel del motor de base de datos.

---

## 📁 Estructura del Proyecto
- `prisma/schema.prisma`: Definición de los modelos, enums e índice compuesto único.
- `prisma/migrations/`: Archivos de migración SQL generados automáticamente.
- `evidencias/`: Captura de pantalla de la base de datos y sus tablas generadas.
