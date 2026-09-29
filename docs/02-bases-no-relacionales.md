# 2. Bases de Datos No Relacionales (NoSQL)

## 2.1 ¿Qué son?

Las bases de datos NoSQL ("Not Only SQL") no organizan la información en tablas rígidas con esquema fijo. En su lugar, almacenan datos en estructuras más flexibles: documentos, pares clave-valor, columnas anchas o grafos. Nacieron para resolver problemas de **escalabilidad horizontal** y **flexibilidad de esquema** que las bases relacionales tradicionales no cubrían bien (grandes volúmenes de datos, alta velocidad, estructuras variables).

## 2.2 Tipos de bases NoSQL

| Tipo | Cómo almacena | Ejemplos |
|---|---|---|
| **Documentales** | Documentos tipo JSON/BSON, cada uno con su propia estructura | MongoDB, CouchDB |
| **Clave-valor** | Pares simples `clave -> valor`, extremadamente rápidas | Redis, DynamoDB |
| **Columnares (wide-column)** | Datos organizados por columnas dinámicas, ideal para big data | Cassandra, HBase |
| **De grafos** | Nodos y relaciones (aristas), ideal para redes y conexiones | Neo4j |

## 2.3 ¿Cómo funcionan internamente?

- **Esquema flexible (schemaless):** cada documento/registro puede tener campos distintos sin romper nada. Puedes agregar un campo nuevo a un solo documento sin migrar toda la base.
- **Escalabilidad horizontal (sharding):** en lugar de tener un solo servidor más potente, se reparte la carga entre varios servidores (nodos), lo cual es más natural en NoSQL que en SQL tradicional.
- **Consistencia eventual (en muchos casos):** en lugar de garantizar ACID estricto, muchas bases NoSQL priorizan disponibilidad y velocidad, aceptando que los datos se sincronicen "eventualmente" entre nodos (modelo **BASE**: Basically Available, Soft state, Eventually consistent).
- **Índices:** también existen, pero se usan de forma distinta según el tipo (índices de texto, geoespaciales, TTL para expiración automática, etc.).

## 2.4 ¿Para qué se utilizan? (función en un ambiente laboral)

- **Catálogos de productos con atributos variables:** un producto puede tener 5 campos y otro 20 (ropa vs. electrónica), algo incómodo en SQL rígido.
- **Redes sociales / feeds de contenido:** publicaciones, comentarios, likes, con estructuras muy variables y alto volumen de escritura.
- **Caché y sesiones de usuario:** para acelerar aplicaciones web (evitar golpear la base principal en cada request).
- **Analítica en tiempo real y big data:** logs, métricas, eventos de IoT, donde se escriben millones de registros por segundo.
- **Sistemas de recomendación y redes de relaciones:** amistades, "quién sigue a quién", rutas óptimas (bases de grafos).
- **Colas de tareas y rate limiting:** Redis se usa muchísimo para limitar peticiones por segundo, colas de trabajos (workers) y notificaciones en tiempo real (pub/sub).

## 2.5 Ejemplo del stack: MongoDB y Redis

### MongoDB (documental)

MongoDB guarda la información como documentos BSON (similares a JSON). Un ejemplo de documento en una colección `usuarios`:

```json
{
  "_id": "652f1a...",
  "nombre": "Ana",
  "correo": "ana@correo.com",
  "direcciones": [
    { "tipo": "casa", "calle": "Av. Reforma 123" },
    { "tipo": "trabajo", "calle": "Insurgentes 456" }
  ],
  "preferencias": {
    "notificaciones": true,
    "idioma": "es"
  }
}
```

Nota cómo un usuario puede tener un arreglo de direcciones o un objeto de preferencias **sin necesidad de tablas adicionales con JOIN**: todo vive dentro del mismo documento (esto se llama "desnormalización" o embedding).

Consulta típica en MongoDB (usando su shell/driver):

```js
db.usuarios.find({ "preferencias.idioma": "es" });

db.usuarios.updateOne(
  { correo: "ana@correo.com" },
  { $set: { "preferencias.notificaciones": false } }
);
```

IDEs/clientes comunes para MongoDB: **MongoDB Compass** (oficial, gráfico), **Studio 3T**, o el propio **DBeaver** (que también soporta MongoDB como conexión NoSQL).

### Redis (clave-valor en memoria)

Redis almacena todo en **memoria RAM**, lo que lo hace extremadamente rápido (microsegundos). No es para almacenamiento permanente masivo, sino para datos que necesitan velocidad extrema y/o son temporales.

```bash
# Guardar una sesión de usuario con expiración de 1 hora
SET session:usuario:123 "{ token: 'abc123' }" EX 3600

# Leer el valor
GET session:usuario:123

# Usar como caché de un conteo de visitas
INCR visitas:pagina_inicio

# Cola simple (lista)
LPUSH cola_tareas "enviar_email:usuario_123"
RPOP cola_tareas
```

Usos típicos de Redis en el trabajo: caché de consultas pesadas, sesiones de usuario, rate limiting (limitar cuántas peticiones hace un usuario por minuto), colas de mensajes ligeras, contadores en tiempo real (likes, vistas).

## 2.6 Cuándo usar una base de datos no relacional

Se usa cuando:
- Los datos tienen **estructura variable o semi-estructurada** (cada registro puede diferir).
- Necesitas **escalar horizontalmente** con facilidad (millones de usuarios/eventos).
- Necesitas **velocidad extrema** para lecturas/escrituras simples (caché, sesiones, contadores).
- El caso de uso es más de "documentos completos" que de relaciones complejas entre muchas tablas.

No se usa o se complementa con SQL cuando:
- Necesitas **transacciones estrictas y consistencia total** (dinero, contabilidad).
- Hay **muchas relaciones complejas** entre entidades que se benefician de JOINs (por ejemplo, reportes financieros con muchas tablas cruzadas).
- El equipo necesita fuerte validación de esquema para evitar datos inconsistentes.
