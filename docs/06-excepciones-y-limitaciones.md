# 6. Excepciones y Limitaciones

## 6.1 Excepciones y matices de las bases relacionales

- **PostgreSQL puede comportarse como NoSQL:** soporta un tipo de dato `JSONB` que permite guardar documentos JSON flexibles dentro de una columna, combinando lo mejor de ambos mundos. Es una excepción importante: no siempre necesitas MongoDB solo por flexibilidad de esquema.
- **No siempre escalan mal horizontalmente:** existen soluciones como Citus (extensión de PostgreSQL) o CockroachDB (compatible con PostgreSQL) que sí permiten escalado horizontal distribuido manteniendo SQL y ACID.
- **Las migraciones de esquema no siempre son un problema grave:** con buenas prácticas (migraciones versionadas, columnas nullable por defecto) se puede evolucionar el esquema sin tanta fricción.
- **Límite real:** cuando el volumen de escritura supera lo que un solo servidor (o clúster pequeño) puede manejar de forma eficiente, un RDBMS tradicional empieza a sufrir, incluso con buen hardware.

## 6.2 Excepciones y matices de las bases no relacionales

- **MongoDB también soporta transacciones ACID multidocumento** desde la versión 4.0 en adelante, algo que antes solo se asociaba a SQL. No es tan eficiente como un RDBMS para esto, pero ya no es "imposible".
- **Redis no es solo caché:** con **RDB/AOF** (persistencia en disco) y su módulo **RedisJSON/RedisGraph**, puede usarse para casos que requieren cierta durabilidad, aunque sigue sin ser la mejor opción como fuente única de verdad para datos críticos.
- **NoSQL no siempre es más rápido:** para consultas relacionales complejas (varios JOINs), un RDBMS bien indexado puede superar a MongoDB, que tendría que hacer múltiples consultas (`$lookup`) o duplicar datos.
- **Consistencia eventual puede ser un problema real:** en sistemas donde el orden y la exactitud inmediata importan (ej. inventario en tiempo real durante una venta flash), la consistencia eventual de algunas bases NoSQL puede causar sobreventa o datos desincronizados si no se diseña con cuidado.

## 6.3 Errores comunes a evitar

**En relacionales:**
- No usar índices y luego preguntarse por qué las consultas son lentas.
- Normalizar en exceso (demasiadas tablas pequeñas) haciendo que cada consulta necesite 10 JOINs.
- No usar transacciones cuando varias operaciones deben ser atómicas.

**En no relacionales:**
- Modelar MongoDB "como si fuera SQL" (crear 20 colecciones relacionadas entre sí con referencias, perdiendo la ventaja de embeber datos).
- Usar Redis como base de datos principal sin persistencia configurada y perder datos importantes al reiniciar el servidor.
- No definir ningún tipo de validación de esquema en MongoDB (usando `$jsonSchema`) y terminar con datos completamente inconsistentes entre documentos.

## 6.4 Conclusión general

No existe "la mejor base de datos" en abstracto: existe la mejor base de datos **para el problema específico que estás resolviendo**. La habilidad clave en un ambiente laboral real no es elegir un bando (SQL o NoSQL), sino saber **diagnosticar el tipo de dato y el patrón de acceso** para elegir (o combinar) la herramienta correcta.
