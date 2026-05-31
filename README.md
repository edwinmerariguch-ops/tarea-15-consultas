# 🏨 Gestión de Alojamientos Turísticos — SQL

Base de datos relacional para la gestión de alojamientos turísticos, propietarios, huéspedes, reservas, pagos y reseñas. Incluye script de creación, datos de prueba y 20 consultas SQL documentadas.

---

## 🛠️ Motor de base de datos

**PostgreSQL** (compatible con pgAdmin, DBeaver y psql)

---

## 📁 Estructura del repositorio

```
├── db_gestion_alojamientos_turisticos.sql       # Creación de tablas y datos de prueba
├── db_gestion_alojamientos_turisticos_consultas.sql  # 20 consultas SQL (CRUD + JOIN)
└── README.md
```

---

## 🗂️ Esquema de la base de datos

```
propietarios
├── id_propietario  PK  SERIAL
├── nombre          VARCHAR(100)
├── apellido        VARCHAR(100)
├── email           VARCHAR(150) UNIQUE
├── telefono        VARCHAR(20)
└── fecha_registro  DATE DEFAULT CURRENT_DATE

alojamientos
├── id_alojamiento  PK  SERIAL
├── id_propietario  FK → propietarios
├── nombre          VARCHAR(200)
├── descripcion     TEXT
├── tipo            VARCHAR(50)
├── direccion       VARCHAR(250)
├── ciudad          VARCHAR(100)
├── pais            VARCHAR(100)
├── precio_noche    DECIMAL(10,2)
├── capacidad_personas  INTEGER
├── num_habitaciones    INTEGER
├── num_banos           INTEGER
├── activo          BOOLEAN DEFAULT true
└── fecha_creacion  TIMESTAMP DEFAULT CURRENT_TIMESTAMP

huespedes
├── id_huesped      PK  SERIAL
├── nombre          VARCHAR(100)
├── apellido        VARCHAR(100)
├── email           VARCHAR(150) UNIQUE
├── telefono        VARCHAR(20)
├── nacionalidad    VARCHAR(100)
└── fecha_registro  DATE DEFAULT CURRENT_DATE

reservas
├── id_reserva      PK  SERIAL
├── id_alojamiento  FK → alojamientos
├── id_huesped      FK → huespedes
├── fecha_entrada   DATE
├── fecha_salida    DATE
├── num_personas    INTEGER
├── precio_total    DECIMAL(10,2)
├── estado          VARCHAR(50) DEFAULT 'confirmada'
├── fecha_reserva   TIMESTAMP DEFAULT CURRENT_TIMESTAMP
└── CHECK (fecha_salida > fecha_entrada)

pagos
├── id_pago         PK  SERIAL
├── id_reserva      FK → reservas
├── monto           DECIMAL(10,2)
├── metodo_pago     VARCHAR(50)
├── estado_pago     VARCHAR(50) DEFAULT 'completado'
└── fecha_pago      TIMESTAMP DEFAULT CURRENT_TIMESTAMP

resenas
├── id_resena       PK  SERIAL
├── id_alojamiento  FK → alojamientos
├── id_huesped      FK → huespedes
├── id_reserva      FK → reservas
├── calificacion    INTEGER CHECK (1–5)
├── comentario      TEXT
└── fecha_resena    TIMESTAMP DEFAULT CURRENT_TIMESTAMP
```

### Relaciones

```
propietarios  ──<  alojamientos  ──<  reservas  ──<  pagos
                        │               │
                        └──────<  resenas  >──────┘
                                    │
                   huespedes  ──<───┘
```

---

## 📋 Consultas incluidas

| N° | Categoría | Operación |
|----|-----------|-----------|
| 01 | INSERT | Insertar nuevo propietario |
| 02 | INSERT | Insertar alojamiento vinculado |
| 03 | INSERT | Registrar huésped y reserva |
| 04 | INSERT | Registrar pago |
| 05 | SELECT | Filtrar alojamientos activos |
| 06 | SELECT | Filtrar huéspedes por nacionalidad |
| 07 | SELECT | Reservas por rango de fechas (BETWEEN) |
| 08 | UPDATE | Actualizar precio de alojamiento |
| 09 | UPDATE | Actualizar estado de reserva |
| 10 | DELETE | Eliminar reseña |
| 11 | JOIN | Reservas + huésped (INNER JOIN) |
| 12 | JOIN | Alojamiento completo (INNER JOIN múltiple) |
| 13 | JOIN | Pagos + reservas + huésped (JOIN combinado) |
| 14 | LEFT JOIN | Alojamientos sin reseñas |
| 15 | LEFT JOIN | Alojamientos sin reservas |
| 16 | AGG | Total de ingresos por alojamiento (SUM) |
| 17 | AGG | Promedio de calificación por alojamiento (AVG) |
| 18 | AGG | Top 5 alojamientos con más reservas (COUNT + LIMIT) |
| 19 | HAVING | Alojamientos con más de 1 reserva (GROUP BY + HAVING) |
| 20 | Subconsulta | Alojamiento con precio más alto (Subquery) |

---

## 🚀 Cómo ejecutar

### 1. Crear la base de datos y tablas

```sql
-- En psql o pgAdmin, ejecutar primero:
\i db_gestion_alojamientos_turisticos.sql
```

### 2. Ejecutar las consultas

```sql
\i db_gestion_alojamientos_turisticos_consultas.sql
```

### Desde la terminal con psql

```bash
psql -U postgres -f db_gestion_alojamientos_turisticos.sql
psql -U postgres -d gestion_alojamientos -f db_gestion_alojamientos_turisticos_consultas.sql
```

---

## 📊 Datos de prueba incluidos

| Tabla | Registros |
|-------|-----------|
| propietarios | 5 |
| alojamientos | 10 |
| huespedes | 10 |
| reservas | 14 |
| pagos | 13 |
| resenas | 8 |

---

## 📌 Notas

- Todas las claves foráneas usan `ON DELETE CASCADE`.
- La tabla `reservas` incluye un `CHECK` que valida que `fecha_salida > fecha_entrada`.
- La tabla `resenas` restringe la `calificacion` al rango 1–5 con `CHECK`.
- El campo `activo` en `alojamientos` permite deshabilitar alojamientos sin eliminarlos.
