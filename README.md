# LightWeight 🏋️

**Plataforma web que conecta entrenadores personales con sus clientes.** El entrenador crea y asigna rutinas, y el cliente entrena con ellas, solo o en directo con otra persona. Los dos se comunican por chat y videollamada en tiempo real.

Proyecto final del CFGS de Desarrollo de Aplicaciones Web (Institut Pedralbes, curso 2025-26), desarrollado por un equipo de 4 personas y desplegado en producción durante el curso.

![React](https://img.shields.io/badge/React_19-20232A?logo=react&logoColor=61DAFB)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white)
![NestJS](https://img.shields.io/badge/NestJS_11-E0234E?logo=nestjs&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL_17-4169E1?logo=postgresql&logoColor=white)
![Prisma](https://img.shields.io/badge/Prisma-2D3748?logo=prisma&logoColor=white)
![Socket.io](https://img.shields.io/badge/Socket.io-010101?logo=socketdotio&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![Playwright](https://img.shields.io/badge/Playwright-2EAD33?logo=playwright&logoColor=white)
![Vitest](https://img.shields.io/badge/Vitest-6E9F18?logo=vitest&logoColor=white)

---

## Mi papel en el proyecto

La idea de LightWeight fue mía. Propuse el stack tecnológico al equipo y coordiné el desarrollo. Trabajé en todas las capas de la aplicación, del frontend a la infraestructura. Algunas de mis aportaciones:

- **Arranque del proyecto:** estructura inicial del monorepo con Docker Compose y configuración del entorno.
- **Autenticación:** registro e inicio de sesión con JWT, y flujo de recuperación de contraseña.
- **Funcionalidad de negocio:** sistema de invitaciones entre entrenador y cliente, notificaciones, gestión de rutinas y ejercicios, y sesiones de entrenamiento.
- **Despliegue continuo:** workflow de GitHub Actions que despliega automáticamente en el servidor.
- **Estrategia de testing:** migración de Jest a Vitest, tests unitarios priorizados del backend y del frontend, suite E2E con Playwright y datos de prueba deterministas.

Trabajamos con Scrum, ramas por funcionalidad, pull requests con revisión de código y desarrollo *spec-driven* con [OpenSpec](https://github.com/Fission-AI/OpenSpec), usando agentes de IA (Claude) e integración con Jira.

---

## Funcionalidades

| Para el entrenador | Para el cliente |
|---|---|
| Crear rutinas a partir de un catálogo de ejercicios (series, repeticiones y descanso) | Ver y seguir las rutinas asignadas |
| Asignar rutinas a sus clientes | Entrenar en solitario o en una sesión en directo con otra persona |
| Invitar clientes con un código o directamente | Aceptar invitaciones de un entrenador |
| Notas privadas y objetivos de cada cliente | Chat y videollamada con su entrenador |

Además:
- **Chat en tiempo real** con WebSockets (Socket.io).
- **Videollamada P2P con WebRTC.** El servidor solo hace de canal de señalización: el vídeo va directamente entre navegadores.
- **Notificaciones en tiempo real** de mensajes e invitaciones.
- **Tres idiomas** (catalán, castellano e inglés) y **modo claro y oscuro**.

---

## Stack

| Capa | Tecnologías |
|---|---|
| **Frontend** | React 19, TypeScript, Vite, Tailwind CSS 4, React Router, i18next, Axios, Socket.io-client, WebRTC |
| **Backend** | NestJS 11, Prisma, PostgreSQL 17, JWT + Passport, Socket.io |
| **Infraestructura** | Docker Compose, Nginx (proxy inverso y HTTPS), Certbot, servidor TURN (coturn) |
| **Calidad** | Vitest (tests unitarios), Playwright (tests E2E), ESLint, GitHub Actions |

### Arquitectura

```
Navegador
    │
    ▼  :443
  Nginx ── /api/*        ──►  NestJS (REST)
        ── /socket.io/*  ──►  NestJS (WebSockets)
        ── /*            ──►  React
                                 │
                              Prisma
                                 │
                            PostgreSQL
```

En producción, solo Nginx está expuesto a internet. El backend y la base de datos quedan en la red interna de Docker.

---

## Cómo ejecutarlo en local

**Requisitos:** [Docker Desktop](https://www.docker.com/products/docker-desktop/) y [Node.js 20+](https://nodejs.org/).

```bash
git clone https://github.com/valeriazv99/lightweight-fitness.git
cd lightweight-fitness
cp .env.example .env          # revisa los valores
docker compose up
```

Con los contenedores en marcha, inicializa la base de datos:

```bash
docker exec -it lw-backend sh
npx prisma generate
npx prisma migrate dev --name init
exit
```

| Servicio | URL |
|---|---|
| Frontend | http://localhost:5173 |
| API | http://localhost:3000 |
| Adminer (base de datos) | http://localhost:8081 |

---

## Tests

```bash
# Tests unitarios
cd src/back && npm test
cd src/front && npm test

# Tests E2E (con la aplicación en marcha)
cd e2e && npm install && npx playwright test
```

La suite E2E cubre los flujos críticos: autenticación, recuperación de contraseña, rutinas, invitaciones y sesiones de entrenamiento individuales y en grupo.

---

## Documentación

- [Documentación técnica completa](doc/README-tecnico.md) (en catalán): modelo de datos, estructura de carpetas, señalización WebRTC, despliegue y CI/CD.
- [Guía de testing E2E](doc/guia-testing-e2e.md)
- [Historias de usuario](doc/user-stories.md) y [planificación de sprints](doc/planificacio-sprints.md)
- [Guía para contribuir y para agentes de IA](AGENTS.md)

---

## Equipo

- **Valeria Zavaleta** ([@valeriazv99](https://github.com/valeriazv99)): idea, coordinación y desarrollo full stack
- **Amin**: desarrollo full stack
- **David**: desarrollo full stack
- **Brian**: desarrollo full stack

Repositorio original: [inspedralbes/prj-final-lightweight](https://github.com/inspedralbes/prj-final-lightweight)
