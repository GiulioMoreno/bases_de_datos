# Bases de Datos: Relacionales vs No Relacionales

Documentación completa sobre bases de datos relacionales (SQL) y no relacionales (NoSQL): qué son, cómo funcionan, cuándo usarlas, gestores comunes, comparativas de eficiencia y casos prácticos del mundo laboral.

> Stack de ejemplo usado en esta guía: **PostgreSQL** + **DBeaver** (relacional) y **MongoDB** + **Redis** (no relacional).

## Índice

| Documento | Contenido |
|---|---|
| [`docs/01-bases-relacionales.md`](docs/01-bases-relacionales.md) | Qué son, cómo funcionan, estructura, gestores, PostgreSQL + DBeaver |
| [`docs/02-bases-no-relacionales.md`](docs/02-bases-no-relacionales.md) | Qué son, tipos de NoSQL, MongoDB, Redis |
| [`docs/03-comparativa-y-eficiencia.md`](docs/03-comparativa-y-eficiencia.md) | Comparativo de gestores, rendimiento, cuándo usar cada una |
| [`docs/04-guia-de-uso.md`](docs/04-guia-de-uso.md) | Guía práctica paso a paso para empezar con cada una |
| [`docs/05-casos-practicos.md`](docs/05-casos-practicos.md) | Ejemplos reales, casos de uso en la industria, relaciones y variabilidad |
| [`docs/06-excepciones-y-limitaciones.md`](docs/06-excepciones-y-limitaciones.md) | Excepciones, límites y errores comunes de cada tipo |

## Objetivo

Este repositorio sirve como **documentación de referencia personal/profesional** para entender:

- La diferencia fundamental entre bases de datos relacionales y no relacionales.
- Su función dentro de un ambiente laboral real (backend, analítica, caché, microservicios).
- Qué gestor usar según el problema (PostgreSQL, MySQL, MongoDB, Redis, etc.).
- Buenas prácticas de organización, modelado y rendimiento.

## Resumen en mis propias palabras

- **Relacionales (SQL):** datos en tablas con esquema fijo y relaciones entre ellas mediante llaves primarias/foráneas. Prioridad: **consistencia e integridad**. Ejemplo: PostgreSQL.
- **No relacionales (NoSQL):** datos en documentos, pares clave-valor, columnas anchas o grafos, con esquema flexible. Prioridad: **velocidad y escalabilidad horizontal**. Ejemplos: MongoDB (documentos), Redis (clave-valor en memoria).

## Stack usado en los ejemplos

- **Relacional:** PostgreSQL + DBeaver como IDE/cliente.
- **No relacional (documentos):** MongoDB.
- **No relacional (clave-valor / caché):** Redis.

## Cómo leer esta documentación

Se recomienda leer en orden: relacionales, no relacionales, comparativa, y finalmente los casos prácticos para ver todo aplicado.

---
## TEMAS EN LOS QUE QUISIERA PROFUNDIZAR APRENDIZAJE:

1- **¿Cuándo aplicar Normalización de Datos frente a Desnormalización (Duplicación controlada)?**

2- **¿Cuál es la mejor manera de tener una lógica sin fallas y efectiva para un ambiente laboral?**

3- **¿Qué es el "Indexado" y cómo afecta al rendimiento real de las consultas lentas?**

---
