# 🚀 Backend Architecture Template (Node.js 20 + TS)

Este repositorio contiene la arquitectura base para microservicios de alto rendimiento, diseñada con un enfoque en **Type-Safety**, **escalabilidad** y **documentación automática**.

## 🛠 Stack Tecnológico

| Componente | Tecnología |
| :--- | :--- |
| **Runtime** | Node.js 20 (LTS) |
| **Lenguaje** | TypeScript 5 (Strict Mode + Decorators) |
| **Framework HTTP** | Express 4 |
| **ORM** | TypeORM 0.3 (PostgreSQL) |
| **Cache / Colas** | Redis (ioredis) |
| **Autenticación** | Passport.js (Google & Microsoft SSO) + JWT |
| **Documentación** | TSOA (OpenAPI/Swagger) |
| **Infraestructura** | AWS EventBridge (Event-driven) |
| **Observabilidad** | New Relic + @alanszp/logger (Structured JSON) |
| **Testing** | Jest + Supertest + Rosie (Factories) |

---

## 📂 Estructura del Proyecto

La arquitectura sigue una separación estricta entre la **Capa de Transporte (API)** y la **Capa de Dominio (Lib)**.



### 🌐 Capa de API (`src/api/`)
* **`endpoints/`**: Controladores de TSOA decorados. Son la fuente de verdad para la documentación Swagger.
* **`middlewares/`**: Gestión de autenticación, roles, MFA y contexto de request.
* **`ExpressApp.ts`**: Configuración central de Express y registro de rutas.

### 🧠 Capa de Dominio (`src/lib/`)
* **`commands/`**: Patrón Command. Orquestadores de la lógica de negocio.
* **`models/`**: Entidades de TypeORM y DTOs de entrada validados con `class-validator`.
* **`repositories/`**: Lógica de persistencia y consultas complejas.
* **`views/`**: Presenters encargados de transformar entidades en respuestas JSON consistentes.

### ⚙️ Otros Módulos
* **`workers/`**: Procesamiento asíncrono con BullMQ/Redis.
* **`serverless/`**: Funciones desacopladas para ejecución en AWS Lambda.

---

## 🏗 Patrón de Implementación

Para asegurar la mantenibilidad, cada flujo debe seguir esta cadena de responsabilidades:

1. **Route/Endpoint**: Recibe el request.
2. **Input Validation**: `class-validator` asegura que los datos sean correctos antes de seguir.
3. **Command**: Ejecuta la lógica de negocio (ej. `CreateUserCommand`).
4. **Repository**: Interactúa con PostgreSQL.
5. **View**: Formatea la salida (Entity -> JSON).

---
