# AIcloset-backend
Repo para aprender back (node | express | tsoa | jest | docker | y todo los que se te ocurra)


## Stack Tecnológico Principal
Componente	Tecnología
Runtime	Node.js 20
Lenguaje	TypeScript 5 (strict mode, decorators habilitados)
Framework HTTP	Express 4
ORM	TypeORM 0.3 (PostgreSQL)
Cache / Colas	Redis vía ioredis
Autenticación	Passport.js (local, Google SSO, Microsoft SSO) + JWT propio
Configuración	convict + dotenv
Validación	class-validator
Documentación API	TSOA (genera OpenAPI/Swagger automáticamente)
Eventos	AWS EventBridge
i18n	i18next con backend de filesystem
Logging	Librería propia @alanszp/logger (structured JSON logging)
Monitoreo	New Relic
Testing	Jest + Supertest + Rosie (factories) + Faker


 ##Estructura de carpetas  
 
src/
├── api/                        # Capa HTTP
│   ├── index.ts                # Entrypoint del servidor
│   ├── ExpressApp.ts           # Configuración de Express (middlewares + rutas)
│   ├── controllers/            # Funciones que manejan request/response
│   ├── routes/                 # Definición de rutas Express (Router)
│   ├── endpoints/              # Controllers TSOA (decorados, generan docs automáticas)
│   ├── middlewares/            # Middlewares (auth, context, roles, MFA)
│   └── tsoa/                   # Rutas auto-generadas por TSOA
├── lib/                        # Capa de dominio/negocio
│   ├── commands/               # Lógica de negocio (orquestadores)
│   ├── models/                 # Entidades TypeORM + Inputs (DTOs validables)
│   ├── repositories/           # Queries a base de datos
│   ├── views/                  # Transformadores entity → response JSON
│   ├── clients/                # Clientes HTTP a otros microservicios
│   ├── events/                 # Publishers de eventos (EventBridge)
│   ├── helpers/                # Utilidades (error handling, audit, etc.)
│   ├── reports/                # Lógica de reportes complejos
│   ├── validators/             # Validadores adicionales
│   ├── mappers/                # Mapeadores
│   └── utils/                  # Funciones utilitarias
├── workers/                    # Workers que procesan colas (BullMQ/Redis)
├── queues/                     # Configuración del queue manager
├── serverless/                 # Funciones lambda/serverless
├── scripts/                    # Scripts utilitarios
├── test/                       # Tests unitarios e integración
├── config.ts                   # Configuración centralizada (convict)
├── dbConnection.ts             # Conexiones a BD (principal, read-replica, DW)
├── ormconfig.ts                # Configuración de TypeORM
├── cache.ts                    # Wrapper de Redis para cache
├── logger.ts / getLogger.ts    # Logger con contexto compartido
├── getContext.ts               # Shared context (request-scoped)
├── passportInstance.ts          # Estrategias de autenticación
└── translations.ts             # i18next setup  


## Para Replicar esta Arquitectura en un Nuevo Proyecto
 0.  Inicializar proyecto TypeScript con Node 20 y tsconfig.json con decorators habilitados
1. Instalar dependencias core: express, typeorm, pg, convict, dotenv, class-validator, ioredis, passport, tsoa
2. Crear la estructura de carpetas: api/{controllers,routes,middlewares,endpoints}, lib/{commands,models,repositories,views,clients,helpers}
3. Configurar convict con esquema tipado para toda la configuración
4. Configurar TypeORM con DataSource y SnakeNamingStrategy
5. Implementar el patrón: Route → Controller → Input.validate() → Command → Repository → View
6. Implementar SharedContext (usando AsyncLocalStorage) para logger y audit por request
7. Configurar TSOA para documentación automática
8. Dockerizar con Dockerfile multi-stage y docker-compose para Swagger UI
