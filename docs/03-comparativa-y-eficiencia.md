# 3. Comparativa entre Gestores y Eficiencia

## 3.1 Diferencias estructurales clave

| Aspecto | Relacional (SQL) | No Relacional (NoSQL) |
|---|---|---|
| **Estructura** | Tablas con esquema fijo | Documentos, clave-valor, columnas o grafos con esquema flexible |
| **Relaciones** | Explícitas vía PK/FK y JOIN | Normalmente embebidas o referenciadas manualmente en la app |
| **Escalabilidad** | Principalmente vertical (más CPU/RAM al mismo servidor) | Principalmente horizontal (más servidores/nodos) |
| **Consistencia** | Fuerte (ACID) | Frecuentemente eventual (BASE), aunque algunas ofrecen ACID parcial |
| **Validación de datos** | Estricta a nivel de motor (tipos, restricciones) | Flexible, la validación suele recaer en la aplicación |
| **Lenguaje de consulta** | SQL estándar | Varía por gestor (query de MongoDB, comandos de Redis, Cypher en Neo4j) |
| **Ideal para** | Datos estructurados y relacionales | Datos variables, alto volumen, alta velocidad |

## 3.2 Comparativo de gestores

### Relacionales

| Gestor | Rendimiento | Facilidad de uso | Escalabilidad | Costo | Uso típico |
|---|---|---|---|---|---|
| **PostgreSQL** | Muy bueno, excelente en consultas complejas | Medio (curva de aprendizaje moderada) | Vertical, con opciones de réplicas | Gratis (open source) | Backends modernos, apps con lógica de negocio compleja |
| **MySQL** | Muy rápido en lecturas simples | Fácil, muy documentado | Vertical, replicación sencilla | Gratis | Sitios web, WordPress, apps pequeñas/medianas |
| **SQL Server** | Muy bueno, optimizado para Windows/.NET | Medio-alto | Vertical y en la nube (Azure) | De pago (licencias) | Empresas con ecosistema Microsoft |
| **Oracle** | Excelente para cargas empresariales masivas | Complejo | Vertical + clustering avanzado | Muy costoso | Bancos, gobierno, grandes corporativos |
| **SQLite** | Bueno para cargas ligeras | Muy fácil | Nula (un solo archivo) | Gratis | Apps móviles, prototipos, almacenamiento local |

### No relacionales

| Gestor | Rendimiento | Facilidad de uso | Escalabilidad | Costo | Uso típico |
|---|---|---|---|---|---|
| **MongoDB** | Muy bueno para documentos grandes/variables | Fácil, API intuitiva | Horizontal (sharding nativo) | Gratis / Atlas de pago en la nube | Catálogos, contenido variable, APIs modernas |
| **Redis** | Excepcional (memoria RAM, microsegundos) | Muy fácil | Horizontal (clustering) | Gratis / versiones cloud de pago | Caché, sesiones, colas, rate limiting |
| **Cassandra** | Excelente para escritura masiva distribuida | Complejo | Horizontal, diseñada para eso | Gratis | Big data, IoT, series de tiempo |
| **DynamoDB** | Excelente, totalmente gestionado por AWS | Fácil (pero atado a AWS) | Horizontal automática | De pago (según uso) | Apps serverless en AWS |
| **Neo4j** | Muy bueno para consultas de relaciones profundas | Medio | Vertical principalmente | Gratis (community) / pago (enterprise) | Redes sociales, recomendaciones, fraude |

## 3.3 Eficiencia: ¿quién gana en qué?

- **Lecturas simples por clave (buscar por ID):** Redis > MongoDB > PostgreSQL. Redis al vivir en RAM es el más rápido posible para esto.
- **Consultas complejas con relaciones entre muchas entidades:** PostgreSQL (con JOINs bien indexados) suele superar a MongoDB, que requiere hacer varias consultas o usar `$lookup` (menos eficiente que un JOIN nativo de SQL).
- **Escritura masiva y distribuida (millones de eventos/seg):** Cassandra o MongoDB con sharding superan ampliamente a un solo servidor PostgreSQL.
- **Datos con estructura muy cambiante:** MongoDB gana en velocidad de desarrollo porque no requiere migraciones constantes de esquema.
- **Consistencia garantizada al 100% (ej. transferencias bancarias):** PostgreSQL (o cualquier RDBMS con ACID) es la opción correcta; NoSQL puede introducir riesgos de inconsistencia temporal.

## 3.4 En la práctica: arquitecturas híbridas (poliglot persistence)

En un ambiente laboral real es muy común **combinar ambos tipos**, no elegir uno solo:

```
[App Web] 
   │
   ├── PostgreSQL   → usuarios, pedidos, pagos (datos críticos y relacionales)
   ├── MongoDB      → catálogo de productos con atributos variables
   └── Redis        → caché de sesiones, rate limiting, contador de visitas
```

Este patrón se llama **"poliglot persistence"**: usar la base de datos correcta para cada tipo de dato, en lugar de forzar todo a un solo motor.
