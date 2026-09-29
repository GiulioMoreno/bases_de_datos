# 1. Bases de Datos Relacionales (SQL)

## 1.1 ¿Qué son?

Una base de datos relacional organiza la información en **tablas** (también llamadas relaciones), compuestas por **filas** (registros) y **columnas** (atributos/campos). Cada tabla tiene un **esquema fijo**: se define de antemano qué columnas existen, de qué tipo de dato son y qué restricciones tienen (obligatorio, único, valor por defecto, etc.).

La característica central es que las tablas se **relacionan entre sí** mediante:

- **Llave primaria (Primary Key, PK):** identifica de forma única cada fila de una tabla.
- **Llave foránea (Foreign Key, FK):** referencia la PK de otra tabla, creando una relación (uno a uno, uno a muchos, muchos a muchos).

Se consultan y manipulan usando **SQL (Structured Query Language)**, un lenguaje declarativo estándar para crear, leer, actualizar y borrar datos (CRUD), además de definir estructuras (DDL) y controlar permisos (DCL).

## 1.2 ¿Cómo funcionan internamente?

1. **Motor de almacenamiento:** guarda los datos en disco en estructuras optimizadas (páginas, índices B-Tree) para lecturas y escrituras eficientes.
2. **Planificador de consultas (query planner):** cuando ejecutas un `SELECT`, el motor decide la ruta más eficiente para obtener los datos (usar un índice, hacer un join de cierta forma, etc.).
3. **Transacciones ACID:**
   - **Atomicidad:** una transacción se ejecuta completa o no se ejecuta.
   - **Consistencia:** los datos siempre cumplen las reglas/restricciones definidas.
   - **Aislamiento:** transacciones concurrentes no se interfieren entre sí.
   - **Durabilidad:** una vez confirmada (`COMMIT`), la información persiste aunque el sistema falle.
4. **Índices:** estructuras adicionales que aceleran búsquedas a cambio de un poco más de espacio y tiempo de escritura.

Este enfoque prioriza la **integridad y consistencia** de los datos por encima de la flexibilidad de estructura.

## 1.3 Estructura típica

```
Tabla: usuarios
+----+----------+---------------------+
| id | nombre   | correo              |
+----+----------+---------------------+
| 1  | Ana      | ana@correo.com      |
| 2  | Luis     | luis@correo.com     |
+----+----------+---------------------+

Tabla: pedidos
+----+------------+------------+
| id | usuario_id | total      |
+----+------------+------------+
| 1  | 1          | 250.00     |
| 2  | 1          | 80.50      |
+----+------------+------------+
```

Aquí `usuario_id` en `pedidos` es una FK que apunta al `id` de `usuarios`: así se modela la relación "un usuario tiene muchos pedidos".

## 1.4 ¿Para qué se utilizan? (función en un ambiente laboral)

- **Sistemas financieros y contables:** donde la exactitud e integridad de cada transacción es crítica (bancos, facturación, nómina).
- **ERP y CRM empresariales:** inventarios, clientes, ventas, recursos humanos.
- **E-commerce:** catálogos de productos, órdenes, pagos, usuarios.
- **Backends de aplicaciones web tradicionales:** cualquier sistema donde los datos tengan relaciones claras y necesiten consistencia (por ejemplo, un sistema escolar: alumnos, materias, calificaciones).
- **Reportes y Business Intelligence (BI):** SQL es el estándar para hacer analítica y generar reportes con `JOIN`, `GROUP BY`, funciones de agregación, etc.

En el trabajo diario, un desarrollador backend usa una base relacional para modelar entidades del negocio con reglas estrictas (ej. "un pedido no puede existir sin un cliente").

## 1.5 Gestores (RDBMS) más comunes

| Gestor | Características clave |
|---|---|
| **PostgreSQL** | Open source, muy robusto, soporta JSON, extensiones (PostGIS para geodatos), altamente estándar en SQL. Favorito en backend moderno. |
| **MySQL / MariaDB** | Open source, muy usado en web (WordPress, aplicaciones LAMP), rápido en lecturas simples. |
| **SQL Server** | De Microsoft, integración fuerte con el ecosistema .NET y Azure, usado en entornos corporativos. |
| **Oracle Database** | Empresarial, muy usado en bancos y grandes corporativos, robusto pero costoso. |
| **SQLite** | Base de datos embebida en un solo archivo, ideal para apps móviles/de escritorio o prototipos. |

## 1.6 Ejemplo del stack: PostgreSQL + DBeaver

- **PostgreSQL** es el motor/gestor: el servidor donde realmente viven las tablas y se ejecutan las consultas.
- **DBeaver** es el **IDE/cliente gráfico universal** para bases de datos: te conectas a PostgreSQL (o a casi cualquier otro gestor) y desde ahí puedes:
  - Explorar tablas, vistas y esquemas visualmente.
  - Escribir y ejecutar consultas SQL con autocompletado.
  - Ver y editar datos como si fuera una hoja de cálculo.
  - Exportar resultados (CSV, JSON, Excel).
  - Generar diagramas ER (entidad-relación) automáticamente a partir del esquema.

Ejemplo de flujo típico en DBeaver:

```sql
-- Crear tabla
CREATE TABLE usuarios (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    correo VARCHAR(150) UNIQUE NOT NULL,
    creado_en TIMESTAMP DEFAULT NOW()
);

-- Insertar datos
INSERT INTO usuarios (nombre, correo) VALUES ('Ana', 'ana@correo.com');

-- Consultar con join
SELECT u.nombre, p.total
FROM usuarios u
JOIN pedidos p ON p.usuario_id = u.id;
```

## 1.7 Cuándo usar una base de datos relacional

Se usa cuando:
- Los datos tienen una **estructura clara y estable** (no cambia todo el tiempo).
- Necesitas **relaciones fuertes** entre entidades (usuarios-pedidos-productos).
- La **integridad de los datos** es crítica (dinero, inventario, contratos).
- Necesitas hacer consultas complejas con `JOIN`, agregaciones y reportes.

No se usa o se complementa con NoSQL cuando:
- El volumen de escritura/lectura es masivo y necesitas escalar horizontalmente sin fricción.
- La estructura de los datos cambia constantemente (esquemas muy variables).
- Necesitas almacenar datos no estructurados como logs masivos, sesiones temporales o caché de alta velocidad.
