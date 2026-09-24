# ERP Multimadera

> Plataforma web para la gestión integral de clientes, empleados, materiales, pedidos y usuarios.

ERP Multimadera es una solución empresarial compuesta por un frontend web y una API backend independientes. La aplicación está diseñada para centralizar procesos operativos y mantener separadas las responsabilidades de presentación, negocio y persistencia.

## Funcionalidades principales

- Gestión de clientes.
- Gestión de empleados.
- Gestión de materiales.
- Gestión de pedidos.
- Administración de usuarios y permisos.
- Reportería para la consulta de información empresarial.
- Autenticación y autorización para proteger los recursos.
- Respuestas paginadas para facilitar el consumo de grandes volúmenes de información.

## Arquitectura y tecnologias

> Las tecnologías se muestran con badges SVG enlazados a su documentación oficial.

### Frontend

<p>
  <a href="https://www.typescriptlang.org/" target="_blank" rel="noopener noreferrer"><img src="https://img.shields.io/badge/TypeScript-3178C6.svg?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript" /></a>
  <a href="https://react.dev/" target="_blank" rel="noopener noreferrer"><img src="https://img.shields.io/badge/React-20232A.svg?style=for-the-badge&logo=react&logoColor=61DAFB" alt="React" /></a>
  <a href="https://vite.dev/" target="_blank" rel="noopener noreferrer"><img src="https://img.shields.io/badge/Vite-646CFF.svg?style=for-the-badge&logo=vite&logoColor=white" alt="Vite" /></a>
  <a href="https://tailwindcss.com/" target="_blank" rel="noopener noreferrer"><img src="https://img.shields.io/badge/Tailwind_CSS-06B6D4.svg?style=for-the-badge&logo=tailwindcss&logoColor=white" alt="Tailwind CSS" /></a>
</p>

### Backend

<p>
  <a href="https://www.typescriptlang.org/" target="_blank" rel="noopener noreferrer"><img src="https://img.shields.io/badge/TypeScript-3178C6.svg?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript" /></a>
  <a href="https://nodejs.org/" target="_blank" rel="noopener noreferrer"><img src="https://img.shields.io/badge/Node.js-339933.svg?style=for-the-badge&logo=node.js&logoColor=white" alt="Node.js" /></a>
  <a href="https://expressjs.com/" target="_blank" rel="noopener noreferrer"><img src="https://img.shields.io/badge/Express.js-000000.svg?style=for-the-badge&logo=express&logoColor=white" alt="Express.js" /></a>
  <a href="https://en.wikipedia.org/wiki/Multitier_architecture" target="_blank" rel="noopener noreferrer"><img src="https://img.shields.io/badge/Layered_Architecture-5C2D91.svg?style=for-the-badge&logo=stackblitz&logoColor=white" alt="Layered Architecture" /></a>
  <a href="https://en.wikipedia.org/wiki/Dependency_injection" target="_blank" rel="noopener noreferrer"><img src="https://img.shields.io/badge/Dependency_Injection-6DB33F.svg?style=for-the-badge&logo=dependabot&logoColor=white" alt="Dependency Injection" /></a>
  <a href="https://auth0.com/docs" target="_blank" rel="noopener noreferrer"><img src="https://img.shields.io/badge/Auth_Middleware-FF6F00.svg?style=for-the-badge&logo=auth0&logoColor=white" alt="Authentication Middleware" /></a>
  <a href="https://swagger.io/specification/" target="_blank" rel="noopener noreferrer"><img src="https://img.shields.io/badge/Swagger%2FOpenAPI-85EA2D.svg?style=for-the-badge&logo=swagger&logoColor=black" alt="Swagger / OpenAPI" /></a>
</p>

### Persistencia y despliegue

<p>
  <a href="https://supabase.com/" target="_blank" rel="noopener noreferrer"><img src="https://img.shields.io/badge/Supabase-181818.svg?style=for-the-badge&logo=supabase&logoColor=3ECF8E" alt="Supabase" /></a>
  <a href="https://www.postgresql.org/" target="_blank" rel="noopener noreferrer"><img src="https://img.shields.io/badge/PostgreSQL-4169E1.svg?style=for-the-badge&logo=postgresql&logoColor=white" alt="PostgreSQL" /></a>
  <a href="https://www.docker.com/" target="_blank" rel="noopener noreferrer"><img src="https://img.shields.io/badge/Docker-2496ED.svg?style=for-the-badge&logo=docker&logoColor=white" alt="Docker" /></a>
  <a href="https://render.com/" target="_blank" rel="noopener noreferrer"><img src="https://img.shields.io/badge/Render-46E3B7.svg?style=for-the-badge&logo=render&logoColor=000000" alt="Render" /></a>
  <a href="https://vercel.com/" target="_blank" rel="noopener noreferrer"><img src="https://img.shields.io/badge/Vercel-000000.svg?style=for-the-badge&logo=vercel&logoColor=white" alt="Vercel" /></a>
</p>

## Arquitectura general

El frontend y el backend evolucionan como aplicaciones independientes. La SPA de React consume la API de Node.js mediante un cliente HTTP. El backend concentra las reglas de negocio y controla el acceso a la persistencia.

```mermaid

flowchart LR
    USER[Usuario]
    subgraph FRONTEND["React + Vite"]
        direction TB
        UI["Pages y componentes UI"]
        ROUTES["React Router"]
        FEATURES["Features por dominio"]
        AUTHSTORE["authStore"]
        APICLIENT["Cliente HTTP"]
        
    end

    subgraph BACKEND["API Node.js / Express.js"]
        direction TB
        SERVER["server.ts / app.ts"]
        CONTROLLERS["Controladores"]
        MIDDLEWARE["Middleware"]
        SERVICES["Servicios"]
        REPOSITORIES["Repositorios"]
    end

    SUPABASE[(Supabase / PostgreSQL)]

    USER --> UI
    UI --> ROUTES
    ROUTES --> FEATURES
    FEATURES --> AUTHSTORE
    FEATURES --> APICLIENT
    APICLIENT -->|HTTP| SERVER
    SERVER --> CONTROLLERS
    CONTROLLERS --> MIDDLEWARE
    MIDDLEWARE --> SERVICES
    SERVICES --> REPOSITORIES
    REPOSITORIES --> SUPABASE
```

## Arquitectura del backend

La API está organizada en capas con responsabilidades diferenciadas:

- **Controladores:** reciben las solicitudes HTTP y construyen las respuestas.
- **Middleware:** valida autenticación, permisos y otras preocupaciones transversales.
- **Servicios:** contienen los casos de uso y las reglas de negocio.
- **Contratos de repositorio:** desacoplan la lógica de negocio de la tecnología de persistencia.
- **Repositorios de Supabase:** implementan el acceso concreto a PostgreSQL.
- **DTOs:** definen y validan los contratos de entrada y salida de la API.

### Flujo de una petición

```mermaid
%%{init: {
  "flowchart": {
    "htmlLabels": false,
    "nodeSpacing": 8,
    "rankSpacing": 12,
    "padding": 6,
    "useMaxWidth": true
  },
  "themeVariables": {
    "fontSize": "10px"
  }
}}%%

flowchart LR
    CLIENT[React]
    SERVER[server.ts]
    APP[app.ts]
    CONTROLLER[Controladores]
    AUTH[Auth Middleware]
    PERM[Permisos Middleware]
    SERVICE[Servicios]
    CONTRACT[Contratos Repo]
    SUPABASE[Repos Supabase]
    DB[(PostgreSQL)]

    CLIENT -->|HTTP Req| SERVER
    SERVER --> APP --> CONTROLLER --> AUTH --> PERM --> SERVICE --> CONTRACT --> SUPABASE --> DB
    DB --> SUPABASE --> SERVICE --> CONTROLLER
    CONTROLLER -->|HTTP Res| CLIENT
```


## Arquitectura del frontend

La interfaz utiliza React con Vite, Tailwind CSS y organiza el código por funcionalidades de negocio. Cada funcionalidad agrupa sus componentes, modelos y servicios, mientras que las piezas compartidas se mantienen en módulos reutilizables.

```mermaid
flowchart LR
    MAIN[main.tsx]
    APP[App.tsx]
    ROUTES[Rutas de la aplicación]
    LAYOUT[Layout protegido]
    FEATURE[Feature de negocio]
    SERVICE[Servicios de la feature]
    CLIENT[Cliente HTTP]
    API[API Multimadera]

    MAIN --> APP
    APP --> ROUTES
    ROUTES --> LAYOUT
    LAYOUT --> FEATURE
    FEATURE --> SERVICE
    SERVICE --> CLIENT
    CLIENT -->|HTTP| API
```

Las funcionalidades principales se organizan alrededor de los siguientes dominios:

- `clientes`
- `empleados`
- `materiales`
- `pedidos`
- `usuarios`
- `reporteria`

## Seguridad y autorización

La seguridad se implementa en dos niveles:

- En el frontend, `ProtectedRoutes`, `ProtectedLayout`, `authStore` y `usePermisos` controlan la navegación y la visibilidad de elementos de la interfaz.
- En el backend, los middleware de autenticación y permisos realizan la validación definitiva antes de ejecutar operaciones protegidas.

La protección del frontend mejora la experiencia de usuario, pero no sustituye la autorización del backend. Las reglas de acceso deben validarse siempre en la API antes de consultar o modificar información.

```mermaid
sequenceDiagram
    actor Usuario
    participant Web as React
    participant Store as authStore
    participant API as Node / Express
    participant Auth as Middleware de autenticación
    participant Permisos as Middleware de permisos
    participant DB as Supabase

    Usuario->>Web: Inicia sesión
    Web->>Store: Guarda el estado de autenticación
    Web->>API: Solicita un recurso protegido
    API->>Auth: Valida autenticación
    Auth->>Permisos: Comprueba permisos
    Permisos->>DB: Ejecuta la operación autorizada
    DB-->>API: Devuelve los datos
    API-->>Web: Respuesta paginada o resultado
    Web-->>Usuario: Renderiza la información
```

## Decisiones arquitectónicas

- **Separación de aplicaciones:** frontend y backend pueden evolucionar y desplegarse de forma independiente.
- **Arquitectura por funcionalidades:** cada módulo de negocio mantiene sus componentes, modelos y servicios relacionados.
- **Separación de responsabilidades:** controladores, servicios y repositorios tienen responsabilidades claras.
- **Inversión del acceso a datos:** los servicios dependen de contratos, mientras que Supabase queda aislado en la infraestructura.
- **Middleware transversal:** autenticación y permisos se gestionan fuera de los controladores.
- **DTOs y paginación:** favorecen contratos consistentes y respuestas adecuadas para el frontend.
- **Documentación de API:** Swagger / OpenAPI facilita consultar y mantener los endpoints documentados.
- **Contenerización:** Docker y Docker Compose simplifican los flujos de desarrollo y ejecución del backend.
- **Despliegue flexible:** Vercel y Render permiten desplegar de forma independiente el frontend y la API según las necesidades del proyecto.

## Estado del proyecto

El proyecto se encuentra estructurado como una solución modular orientada a la gestión empresarial. Las funcionalidades, enlaces de despliegue y comandos de ejecución deben mantenerse sincronizados con la implementación actual.

## Sobre este repositorio

Este README presenta la arquitectura y las decisiones técnicas de ERP Multimadera. Para una revisión técnica completa, se requiere autorización por cuestiones de seguridad basadas en la tríada CIA: confidencialidad, integridad y disponibilidad.
