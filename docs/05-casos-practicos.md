# 5. Casos Prácticos y Ejemplos Reales

## 5.1 Ejemplo real: E-commerce (Amazon-like)

- **PostgreSQL:** usuarios, direcciones de envío, órdenes, pagos, facturación. Requiere consistencia total (no se puede "perder" un pago o duplicar un cobro).
- **MongoDB:** catálogo de productos. Un producto electrónico tiene specs técnicas distintas a una playera (talla, color) o un libro (autor, ISBN); el esquema flexible evita crear cientos de columnas vacías.
- **Redis:** carrito de compras temporal antes de confirmar la orden, caché de "productos más vistos", control de inventario en tiempo real para evitar sobreventa en flash sales.

## 5.2 Ejemplo real: Red social

- **PostgreSQL:** cuentas de usuario, configuración de privacidad, datos de facturación si hay planes premium.
- **MongoDB:** publicaciones, comentarios, likes — contenido que varía mucho en forma (una publicación puede tener imagen, video, encuesta, texto).
- **Redis:** feed en tiempo real, contadores de "vistas" y "likes" que se actualizan constantemente, sesiones activas, sistema de notificaciones push.
- **Neo4j (grafos, mencionado como referencia):** el grafo social "quién sigue a quién" y sugerencias de "personas que quizá conozcas" se modelan de forma natural con nodos y relaciones.

## 5.3 Ejemplo real: Banco / Fintech

- **PostgreSQL (u Oracle/SQL Server):** cuentas, saldos, transferencias, historial de transacciones. Aquí ACID no es opcional: una transferencia debe ser atómica (o se completa toda o no se completa nada) para evitar que el dinero "desaparezca".
- **Redis:** rate limiting para evitar ataques de fuerza bruta en el login, caché de tipo de cambio actualizado cada minuto.
- **MongoDB:** logs de auditoría o eventos de comportamiento del usuario (no crítico transaccionalmente, pero sí voluminoso).

## 5.4 Ejemplo real: Aplicación de streaming (tipo Netflix/Spotify)

- **PostgreSQL:** suscripciones, planes, facturación de usuarios.
- **MongoDB o Cassandra:** catálogo de contenido (películas, canciones) con metadatos muy variables (duración, elenco, género, temporadas).
- **Redis:** caché de "continuar viendo", contadores de reproducciones en tiempo real, colas de recomendaciones precalculadas.

## 5.5 Relaciones y variabilidad: cómo decidir

Pregúntate:

1. **¿Este dato tiene relaciones fuertes con otros datos que necesito consultar cruzadamente?** → Relacional.
   - Ej: "dame todos los pedidos de un cliente con el detalle de cada producto y su categoría" → varias tablas relacionadas, ideal para JOIN en SQL.
2. **¿La estructura de este dato cambia mucho entre un registro y otro?** → No relacional (documental).
   - Ej: catálogo de productos con atributos completamente distintos por categoría.
3. **¿Necesito acceso ultrarrápido a un dato simple, aunque sea temporal?** → No relacional (clave-valor).
   - Ej: sesión de usuario, contador de "me gusta", caché de resultados de una consulta pesada.
4. **¿Necesito modelar relaciones complejas tipo red (amigos de amigos, rutas, recomendaciones)?** → No relacional (grafos).

## 5.6 Caso práctico de "migración" o convivencia

Es común que un sistema empiece 100% en PostgreSQL y, al crecer, algunas partes se muevan a NoSQL cuando:
- El equipo nota que ciertas tablas crecen demasiado rápido y las consultas se vuelven lentas (ej. logs de eventos).
- Se necesita reducir carga en la base principal (agregar Redis como caché delante de PostgreSQL).
- Aparece un módulo con datos muy variables que no encajan bien en columnas fijas (ej. formularios dinámicos, catálogos con atributos por categoría).

Esto refuerza la idea de que **no es "SQL vs NoSQL"**, sino **"SQL y NoSQL, cada uno donde mejor rinde"**.
