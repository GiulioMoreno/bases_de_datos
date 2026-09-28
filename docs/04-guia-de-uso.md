# 4. Guía Práctica de Uso

## 4.1 Guía rápida: PostgreSQL + DBeaver

1. **Instalar PostgreSQL** (localmente o usar un servicio como Supabase/Neon/RDS).
2. **Instalar DBeaver Community Edition** (gratuito).
3. **Crear una conexión nueva** en DBeaver: elegir "PostgreSQL", ingresar host, puerto (5432 por defecto), usuario, contraseña y base de datos.
4. **Crear tu esquema:**
   ```sql
   CREATE DATABASE tienda;

   CREATE TABLE clientes (
       id SERIAL PRIMARY KEY,
       nombre VARCHAR(100) NOT NULL,
       correo VARCHAR(150) UNIQUE
   );

   CREATE TABLE productos (
       id SERIAL PRIMARY KEY,
       nombre VARCHAR(100),
       precio NUMERIC(10,2)
   );

   CREATE TABLE pedidos (
       id SERIAL PRIMARY KEY,
       cliente_id INT REFERENCES clientes(id),
       producto_id INT REFERENCES productos(id),
       cantidad INT DEFAULT 1,
       fecha TIMESTAMP DEFAULT NOW()
   );
   ```
5. **Insertar y consultar datos** directamente desde el editor SQL de DBeaver.
6. **Ver el diagrama ER:** clic derecho sobre el esquema → "View Diagram" para visualizar todas las relaciones automáticamente.
7. **Buenas prácticas:**
   - Define siempre PK en cada tabla.
   - Usa índices en columnas que uses frecuentemente en `WHERE` o `JOIN`.
   - Usa `NOT NULL` y `UNIQUE` para forzar integridad desde el motor, no solo desde el código.
   - Haz respaldos (`pg_dump`) periódicos.

## 4.2 Guía rápida: MongoDB

1. **Instalar MongoDB Community Server** o usar **MongoDB Atlas** (nube, capa gratuita disponible).
2. **Conectarte** con MongoDB Compass o el driver de tu lenguaje (Node.js, Python, etc.).
3. **Crear una colección y documentos:**
   ```js
   use tienda;

   db.productos.insertMany([
     { nombre: "Laptop", precio: 15000, categoria: "electronica", specs: { ram: "16GB", ssd: "512GB" } },
     { nombre: "Playera", precio: 250, categoria: "ropa", talla: "M" }
   ]);
   ```
   Nota: cada documento puede tener campos distintos (specs vs. talla) sin problema.
4. **Consultar con filtros:**
   ```js
   db.productos.find({ categoria: "electronica" });
   db.productos.find({ precio: { $lt: 500 } });
   ```
5. **Buenas prácticas:**
   - Decide si "embeber" (guardar todo junto) o "referenciar" (guardar solo el ID) según qué tan seguido cambian y crecen los datos relacionados.
   - Crea índices (`createIndex`) sobre los campos que más consultas por filtrado.
   - No abuses de documentos gigantes: MongoDB tiene un límite de 16MB por documento.

## 4.3 Guía rápida: Redis

1. **Instalar Redis** (localmente, Docker, o usar Redis Cloud).
2. **Conectarte** vía `redis-cli` o un cliente gráfico como **RedisInsight**.
3. **Comandos básicos:**
   ```bash
   SET usuario:123:nombre "Ana"
   GET usuario:123:nombre

   SET cache:productos_destacados "[...]" EX 600   # expira en 10 min

   HSET usuario:123 nombre "Ana" edad 25            # hash (objeto)
   HGETALL usuario:123
   ```
4. **Buenas prácticas:**
   - Usa Redis como **caché o dato temporal**, no como tu única fuente de verdad para datos críticos.
   - Define siempre un tiempo de expiración (`EX`) quando sea posible, para no llenar la memoria.
   - Usa nombres de claves consistentes con ":" como separador (`usuario:123:carrito`).

## 4.4 Organización recomendada de un proyecto con ambos tipos

```
proyecto/
├── src/
│   ├── db/
│   │   ├── postgres.js       # conexión y modelos relacionales
│   │   ├── mongo.js          # conexión y modelos documentales
│   │   └── redis.js          # conexión y utilidades de caché
│   ├── models/
│   │   ├── usuario.model.js  # entidad relacional (PostgreSQL)
│   │   └── producto.model.js # entidad documental (MongoDB)
│   └── services/
└── docs/
    └── (esta documentación)
```

Regla práctica: los datos **críticos y con relaciones fuertes** van a PostgreSQL; los datos **variables o de alto volumen** van a MongoDB; los datos **temporales o de acceso ultrarrápido** van a Redis.
