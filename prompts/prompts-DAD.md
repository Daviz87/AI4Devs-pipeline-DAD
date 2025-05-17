## Prompt 1
**Fecha:** 17/05/2025
**Modelo:** Gemini 2.5 Pro (inferido)

**Usuario:** 
## Role: Expert Senior Developer

You are an Expert Senior Developer.

Your primary task is to perform a comprehensive analysis of the entire current project accessible within the IDE. This includes, but is not limited to, thoroughly examining the @README.md  file, all @backend  source code, and all @frontend source code.

Your objectives are to:
1.  **Identify and Clearly Document Core Functionality:** Understand and describe the main purpose and features of the application.
2.  **Describe Software Architecture:** Outline the overall software architecture (e.g., microservices, monolithic, client-server), including key components, their responsibilities, and their interactions.
3.  **Compile Dependencies and Libraries:** Create an exhaustive list of all dependencies and libraries used in both the backend and frontend. Specify their versions if readily available from manifest files (e.g., `package.json`, `pom.xml`, `requirements.txt`).
4.  **Extract Business Domain Concepts:** Identify and summarize key business domain concepts, rules, and logic embedded within the codebase or project documentation.
5.  **Generate a Comprehensive Project Guide:** Produce a detailed guide covering the following aspects:
    * **Installation:** Step-by-step instructions for setting up the development environment and running the project locally.
    * **Testing:** Instructions on how to execute existing tests (unit, integration, end-to-end), including any specific commands or configurations required.
    * **Build Process:** Description of how the application (backend and frontend, if applicable) is currently built, including any build scripts or commands.
    * **Deployment:** Overview of the current deployment process, if one exists.
    * **CI/CD:** Description of any existing Continuous Integration/Continuous Delivery practices or configurations.

**Contextual Instructions:**
* You have access to the entire codebase and project files within the IDE.
* Focus on providing practical, accurate, and actionable information that would be useful for a developer new to the project or for planning subsequent CI/CD pipeline implementation.
* If certain information (e.g., specific versions, detailed deployment steps) is not explicitly available, note this in your analysis.

**Output Format:**
Organize your findings into a clear, well-structured report. Use Markdown formatting, including headings, subheadings, bullet points, and code blocks where appropriate for clarity and readability.

---
**Mandatory Language Instruction for LLM Interaction:**
You MUST provide ALL your outputs and conduct ALL your interactions with the user exclusively in **SPANISH**.
---

**Asistente:**
¡Entendido! Procederé a analizar el proyecto como un Desarrollador Senior Experto para generar la guía que has solicitado.

Comenzaré examinando el archivo `README.md` y los archivos `package.json` del frontend y backend para obtener una visión general y la lista de dependencias. Luego exploraré la estructura de los directorios `src` de ambos para entender la arquitectura y finalmente intentaré extraer los conceptos de dominio.

Aquí tienes el análisis del proyecto:

## Guía Comprensiva del Proyecto: LTI - Sistema de Seguimiento de Talento

### 1. Funcionalidad Principal del Proyecto

El proyecto "LTI - Sistema de Seguimiento de Talento" es una aplicación full-stack diseñada para gestionar y realizar el seguimiento de candidatos. Sus funcionalidades principales incluyen:

*   **Gestión de Candidatos:** Permite crear, ver, y potencialmente actualizar y eliminar perfiles de candidatos.
*   **Almacenamiento de Información del Candidato:** Guarda detalles como nombre, apellido, email, teléfono, dirección, historial educativo, experiencia laboral y currículum vitae (CV).
*   **API Backend:** Proporciona endpoints para interactuar con los datos de los candidatos.
*   **Interfaz Frontend:** Ofrece una interfaz de usuario (presumiblemente basada en web) para que los usuarios interactúen con el sistema.

El sistema parece estar orientado a reclutadores o departamentos de recursos humanos para facilitar el proceso de selección de personal.

### 2. Arquitectura del Software

El proyecto sigue una **arquitectura cliente-servidor**:

*   **Frontend (Cliente):** Una aplicación React (creada con Create React App) responsable de la interfaz de usuario y la interacción con el usuario.
*   **Backend (Servidor):** Una aplicación Node.js con Express, escrita en TypeScript, que gestiona la lógica de negocio, el acceso a datos y la API.

**Componentes Clave del Backend y sus Responsabilidades:**

Basado en la estructura de directorios descrita en `README.md` y las convenciones comunes de este tipo de arquitecturas:

*   `backend/src/`: Código fuente del servidor.
    *   `index.ts`: Punto de entrada del servidor backend.
    *   `application/`: Contiene la lógica de aplicación (casos de uso, servicios de aplicación). Coordina la infraestructura y el dominio.
    *   `domain/`: Alberga la lógica de negocio pura (entidades, objetos de valor, reglas de negocio). Es el núcleo del backend.
    *   `infrastructure/`: Se encarga de las implementaciones concretas de interfaces definidas en `application` o `domain`, especialmente para la comunicación con sistemas externos como la base de datos (usando Prisma ORM).
    *   `presentation/`: Maneja las solicitudes HTTP (controladores) y la presentación de datos a través de la API.
    *   `routes/`: Define las rutas de la API y las asocia con los controladores correspondientes en la capa de presentación.
    *   `tests/`: Contiene los archivos de prueba para el backend.
*   `backend/prisma/`: Contiene el esquema de la base de datos (`schema.prisma`) gestionado por Prisma ORM y los archivos de migración.

**Componentes Clave del Frontend y sus Responsabilidades:**

*   `frontend/src/`: Código fuente de la aplicación React.
    *   Probablemente contendrá componentes de UI, lógica de estado (posiblemente con Context API o Redux si se ha añadido), servicios para comunicarse con la API del backend y las vistas de la aplicación.
*   `frontend/public/`: Archivos estáticos (HTML principal, imágenes, etc.).

**Interacciones:**

1.  El usuario interactúa con el **Frontend** a través de su navegador.
2.  El **Frontend** realiza solicitudes HTTP (GET, POST, PUT, DELETE) a la **API del Backend** para obtener o modificar datos.
3.  El **Backend**, a través de sus `routes` y `presentation layer` (controladores), recibe estas solicitudes.
4.  Los controladores invocan servicios en la `application layer`.
5.  La `application layer` utiliza el `domain layer` para la lógica de negocio y el `infrastructure layer` (específicamente Prisma) para acceder y persistir datos en la base de datos PostgreSQL.
6.  El **Backend** envía respuestas HTTP (generalmente en formato JSON) al **Frontend**.
7.  El **Frontend** actualiza la interfaz de usuario basándose en las respuestas recibidas.

### 3. Dependencias y Librerías

A continuación, se listan las dependencias principales. Las versiones específicas se extraen de los archivos `package.json`.

#### Backend (`backend/package.json`)

**Dependencias de Producción (`dependencies`):**

*   `@prisma/client`: `^5.13.0` (Cliente de Prisma para interactuar con la base de datos)
*   `cors`: `^2.8.5` (Middleware para habilitar Cross-Origin Resource Sharing)
*   `dotenv`: `^16.4.5` (Para cargar variables de entorno desde un archivo `.env`)
*   `express`: `^4.19.2` (Framework web para Node.js)
*   `multer`: `^1.4.5-lts.1` (Middleware para manejar `multipart/form-data`, usado para subida de archivos)
*   `swagger-jsdoc`: `^6.2.8` (Genera especificaciones OpenAPI (Swagger) desde comentarios JSDoc)
*   `swagger-ui-express`: `^5.0.0` (Sirve la UI de Swagger para visualizar la documentación de la API)
    *   *Nota:* `backend: file:` parece ser una referencia circular o un error de configuración en el `package.json`, ya que un paquete no puede depender de sí mismo de esta manera.

**Dependencias de Desarrollo (`devDependencies`):**

*   `@types/cors`: `^2.8.17`
*   `@types/express`: `^4.17.9`
*   `@types/jest`: `^29.5.12`
*   `@types/multer`: `^1.4.11`
*   `@types/node`: `^20.12.12`
*   `eslint`: `^9.2.0` (Linter para JavaScript y TypeScript)
*   `eslint-config-prettier`: `^9.1.0` (Desactiva reglas de ESLint que conflictúan con Prettier)
*   `eslint-plugin-prettier`: `^5.1.3` (Ejecuta Prettier como una regla de ESLint)
*   `jest`: `^29.7.0` (Framework de testing)
*   `prettier`: `^3.2.5` (Formateador de código)
*   `prisma`: `^5.13.0` (CLI de Prisma para migraciones, generación de cliente, etc.)
*   `ts-jest`: `^29.1.2` (Preset de Jest para TypeScript)
*   `ts-node`: `^9.1.1` (Ejecución de TypeScript en Node.js directamente)
*   `ts-node-dev`: `^1.1.6` (Reinicia el servidor en desarrollo al detectar cambios en archivos TypeScript)
*   `typescript`: `^4.9.5` (Superset de JavaScript que añade tipado estático)

#### Frontend (`frontend/package.json`)

**Dependencias de Producción (`dependencies`):**

*   `@testing-library/jest-dom`: `^5.17.0` (Matchers personalizados de Jest para DOM)
*   `@testing-library/react`: `^13.4.0` (Utilidades para testear componentes React)
*   `@testing-library/user-event`: `^13.5.0` (Simula interacciones del usuario para tests)
*   `@types/jest`: `^27.5.2`
*   `@types/node`: `^16.18.97`
*   `@types/react`: `^18.3.1`
*   `@types/react-dom`: `^18.3.0`
*   `bootstrap`: `^5.3.3` (Framework CSS para diseño responsivo)
*   `dotenv`: `^16.4.5`
*   `react`: `^18.3.1` (Librería para construir interfaces de usuario)
*   `react-beautiful-dnd`: `^13.1.1` (Para funcionalidades de arrastrar y soltar - Drag and Drop)
*   `react-bootstrap`: `^2.10.2` (Componentes React para Bootstrap)
*   `react-bootstrap-icons`: `^1.11.4` (Iconos de Bootstrap como componentes React)
*   `react-datepicker`: `^6.9.0` (Componente de selector de fechas para React)
*   `react-dnd`: `^16.0.1` (Otro framework para Drag and Drop en React)
*   `react-dnd-html5-backend`: `^16.0.1` (Backend HTML5 para `react-dnd`)
*   `react-dom`: `^18.3.1` (Permite a React interactuar con el DOM)
*   `react-router-dom`: `^6.23.1` (Para la gestión de rutas en la aplicación React)
*   `react-scripts`: `5.0.1` (Scripts y configuraciones usadas por Create React App)
*   `typescript`: `^4.9.5`
*   `web-vitals`: `^2.1.4` (Para medir métricas de rendimiento web)
    *   *Nota:* `frontend: file:` también aparece aquí, lo que sugiere una posible copia de la estructura del `package.json` del backend sin ajustar esta línea, o un error similar.

**Dependencias de Desarrollo (`devDependencies`):**

*   `cypress`: `^13.11.0` (Framework para tests end-to-end)

### 4. Conceptos del Dominio de Negocio

Basándome en la descripción del proyecto, el `README.md`, y los nombres de los archivos y dependencias, los conceptos clave del dominio de negocio giran en torno al **Seguimiento de Talento y Reclutamiento**:

*   **Candidato (`Candidate`):** La entidad central. Posee atributos como:
    *   Nombre (`firstName`)
    *   Apellido (`lastName`)
    *   Email (`email`)
    *   Teléfono (`phone`)
    *   Dirección (`address`)
    *   Educación (`Education`)
    *   Experiencia Laboral (`WorkExperience`)
    *   Currículum Vitae (`CV`)
*   **Educación (`Education`):** Representa el historial educativo de un candidato.
    *   Institución (`institution`)
    *   Título (`title`)
    *   Fecha de Inicio (`startDate`)
    *   Fecha de Fin (`endDate`) (opcional)
*   **Experiencia Laboral (`WorkExperience`):** Representa el historial laboral de un candidato.
    *   Compañía (`company`)
    *   Cargo (`position`)
    *   Descripción (`description`)
    *   Fecha de Inicio (`startDate`)
    *   Fecha de Fin (`endDate`) (opcional)
*   **Currículum Vitae (CV):** Documento adjunto al perfil del candidato.
    *   Ruta del Archivo (`filePath`)
    *   Tipo de Archivo (`fileType`)

El archivo `ModeloDatos.md` (mencionado en el `README.md`) y `api-spec.yaml` seguramente contienen una descripción más detallada de estas entidades, sus relaciones y las reglas de negocio asociadas (por ejemplo, campos obligatorios, formatos de datos). Sería necesario revisarlos para una comprensión más profunda.

### 5. Guía Comprensiva del Proyecto

#### Instalación

Las instrucciones de instalación están detalladas en el `README.md`:

1.  **Clonar el repositorio.**
2.  **Instalar dependencias del Frontend:**
    ```sh
    cd frontend
    npm install
    ```
3.  **Instalar dependencias del Backend:**
    ```sh
    cd ../backend  # O la ruta correcta al directorio backend desde la raíz
    npm install
    ```
4.  **Configurar Variables de Entorno (Backend):**
    *   Crear un archivo `.env` en el directorio `backend/` (o en la raíz del proyecto si así está configurado para ser leído por el backend, aunque `README.md` sugiere en `backend/`).
    *   Añadir `DATABASE_URL` para la conexión a PostgreSQL:
        ```
        DATABASE_URL="postgresql://user:password@host:port/database"
        ```
        Ejemplo del `README.md` para Docker local: `postgresql://postgres:password@localhost:5432/mydatabase` (reemplazar `user`, `password`, `database` con los valores reales si son diferentes, especialmente si no se usa Docker o se tiene otra configuración).
        El `README.md` también menciona que si la URL no funciona, se puede probar a reemplazarla directamente en `prisma/schema.prisma`.

5.  **Configurar Docker y PostgreSQL (Opcional, pero recomendado por el `README.md`):**
    *   Asegurarse de tener Docker y Docker Compose instalados.
    *   Desde el directorio raíz del proyecto, ejecutar:
        ```sh
        docker-compose up -d
        ```
        Esto iniciará un contenedor PostgreSQL según la configuración en `docker-compose.yml`. Los detalles de conexión por defecto son: Host: `localhost`, Puerto: `5432`, Usuario: `postgres`, Contraseña: `password`, Base de datos: `mydatabase`. (Estos deben coincidir o ser actualizados en el archivo `.env` del backend).

6.  **Generar Prisma Client, Ejecutar Migraciones y Sembrar Datos (Backend):**
    *   Asegurarse de que el contenedor Docker de PostgreSQL esté corriendo y que `DATABASE_URL` esté correctamente configurada.
    *   Navegar al directorio `backend/`.
    *   Ejecutar los siguientes comandos:
        ```sh
        npx prisma generate  # Genera el cliente Prisma basado en el esquema
        npx prisma migrate dev # Aplica migraciones a la base de datos y la crea si no existe
        # El README menciona 'ts-node seed.ts'. Esto sugiere que hay un script de seed.
        # Asumiendo que ts-node está instalado globalmente o como devDependency:
        # npx ts-node seed.ts  (si seed.ts está en la raíz de /backend)
        # O si está en, por ejemplo, prisma/seed.ts:
        # npx ts-node prisma/seed.ts
        ```
        El `README.md` indica `ts-node seed.ts`. Habría que confirmar la ubicación exacta del archivo `seed.ts`. Si está en `backend/prisma/seed.ts` (una ubicación común), el comando podría ser `npx ts-node ./prisma/seed.ts` (o similar, dependiendo de la configuración de `tsconfig.json` y la ubicación del script). Asumo que `ts-node` está como `devDependency` como vi en el `package.json`, por lo que `npx` debería funcionar.

#### Testing

**Backend (`backend/`):**

*   El script `test` en `backend/package.json` es `"test": "jest"`.
*   Para ejecutar tests unitarios/integración del backend:
    ```sh
    cd backend
    npm test
    ```
    Esto ejecutará Jest. La configuración de Jest está en `backend/jest.config.js`.

**Frontend (`frontend/`):**

*   El script `test` en `frontend/package.json` es `"test": "jest --config jest.config.js"`.
    *   *Nota:* Aunque `react-scripts test` es lo común para Create React App, aquí se especifica `jest` directamente con una configuración. Es posible que se haya hecho `eject` o se haya personalizado la configuración de tests. El archivo `frontend/jest.config.js` (si existe, o el `jest.config.js` referenciado) contendría la configuración específica. Si no existe un `frontend/jest.config.js`, podría estar refiriéndose a uno global o hay un error en el script. Por el nombre, parece que debería haber un `jest.config.js` en la raíz del frontend.
*   Para ejecutar tests unitarios/componentes del frontend:
    ```sh
    cd frontend
    npm test
    ```
*   **Tests End-to-End (Frontend):**
    *   El frontend utiliza Cypress para tests e2e.
    *   Scripts disponibles en `frontend/package.json`:
        *   `"cypress:open": "cypress open"` (Abre la UI de Cypress para ejecución interactiva)
        *   `"cypress:run": "cypress run"` (Ejecuta tests Cypress en modo headless)
    *   Para abrir Cypress:
        ```sh
        cd frontend
        npm run cypress:open
        ```
    *   Para ejecutar tests Cypress en consola:
        ```sh
        cd frontend
        npm run cypress:run
        ```
    La configuración de Cypress está en `frontend/cypress.config.ts` y los tests en `frontend/cypress/`.

#### Proceso de Build

**Backend (`backend/`):**

*   El script `build` en `backend/package.json` es `"build": "tsc"`.
*   TypeScript se compila a JavaScript en el directorio `dist/` (según el `main` field: `"main": "dist/index.js"` y la configuración estándar de `tsc`).
*   Para construir el backend:
    ```sh
    cd backend
    npm run build
    ```

**Frontend (`frontend/`):**

*   El script `build` en `frontend/package.json` es `"build": "react-scripts build"`.
*   Esto crea una build de producción optimizada en el directorio `frontend/build/`.
*   Para construir el frontend:
    ```sh
    cd frontend
    npm run build
    ```

#### Ejecución Local (Desarrollo y Producción Simulada)

**Backend (`backend/`):**

*   **Desarrollo (con auto-recarga):**
    *   Script `dev`: `"dev": "ts-node-dev --respawn --transpile-only src/index.ts"`
    *   Ejecutar:
        ```sh
        cd backend
        npm run dev
        ```
        El servidor backend (por defecto, según el `README.md`) correrá en `http://localhost:3010`.
*   **Producción (después de un `npm run build`):**
    *   Script `start`: `"start": "node dist/index.js"`
    *   Ejecutar:
        ```sh
        cd backend
        # npm run build (si no se ha hecho ya)
        npm start
        ```

**Frontend (`frontend/`):**

*   **Desarrollo (servidor de desarrollo de Webpack):**
    *   Script `start`: `"start": "react-scripts start"`
    *   Ejecutar:
        ```sh
        cd frontend
        npm start
        ```
        El frontend (por defecto, según el `README.md`) estará disponible en `http://localhost:3000`.
*   Para servir la build de producción localmente (no hay un script explícito para esto, pero se podría usar `serve` o similar):
    ```sh
    cd frontend
    npm run build
    # Luego, por ejemplo, con el paquete 'serve':
    # npx serve -s build
    ```

#### Despliegue

El archivo `README.md` proporciona una sección detallada sobre "Configuración de EC2 y GitHub Actions", lo que sugiere que el despliegue se realiza en una instancia EC2 de AWS y está (o estará) automatizado con GitHub Actions.

**Pasos generales para el despliegue en EC2 (según `README.md`):**

1.  **Crear y Configurar Instancia EC2:**
    *   Lanzar instancia (Amazon Linux 2 o Ubuntu).
    *   Configurar Grupo de Seguridad para permitir tráfico en puertos 22 (SSH), 80 (HTTP), y 8080 (para el backend, aunque el `README.md` menciona 3010 como puerto por defecto localmente; esto podría variar en producción o ser un error tipográfico. El puerto de la aplicación debe ser consistente).
2.  **Instalar Dependencias en EC2:**
    *   Node.js, npm.
    *   PM2 (gestor de procesos para Node.js, para mantener la aplicación corriendo).
    *   Nginx (opcional, como reverse proxy, para SSL, servir contenido estático).
3.  **Configurar Variables de Entorno en EC2:**
    *   Crear archivo `.env` en el backend con `DATABASE_URL` apuntando a la base de datos de producción.
4.  **Obtener el Código en EC2:**
    *   Clonar el repositorio o usar GitHub Actions para desplegar el código.
5.  **Construir la Aplicación en EC2:**
    *   `cd backend && npm install --production && npm run build`
    *   `cd frontend && npm install --production && npm run build`
6.  **Ejecutar la Aplicación con PM2:**
    *   Para el backend: `pm2 start dist/index.js --name "lti-backend"` (o similar).
    *   Para servir el frontend (si no se usa Nginx): `pm2 serve build 3000 --spa --name "lti-frontend"` (o configurar Nginx).
7.  **Configurar Nginx (Recomendado):**
    *   Configurar Nginx como reverse proxy para el backend (e.g., `localhost:3010` o el puerto que use PM2).
    *   Configurar Nginx para servir los archivos estáticos del frontend desde `frontend/build`.

**Información no explícitamente disponible:**

*   Detalles específicos de la configuración de Nginx.
*   Estrategia de gestión de la base de datos en producción (¿RDS, Docker en EC2?). El `README.md` se enfoca en Docker local.
*   Scripts de despliegue específicos si no son los de GitHub Actions.

#### CI/CD (Integración Continua / Entrega Continua)

*   El `README.md` menciona **GitHub Actions** y la configuración de secretos necesarios (`AWS_ACCESS_ID`, `AWS_ACCESS_KEY`, `EC2_INSTANCE`). Esto indica que existe o se planea una pipeline de CI/CD utilizando GitHub Actions.
*   **Flujo de Trabajo Típico Esperado (inferido):**
    1.  Un desarrollador empuja cambios a una rama específica (e.g., `main` o `develop`) del repositorio en GitHub.
    2.  GitHub Actions se activa automáticamente.
    3.  **CI (Integración Continua):**
        *   Checkout del código.
        *   Instalación de dependencias (frontend y backend).
        *   Ejecución de linters y formateadores.
        *   Ejecución de tests (unitarios, integración).
        *   Build de los artefactos de producción (frontend y backend).
    4.  **CD (Entrega/Despliegue Continuo):**
        *   Si la CI es exitosa, la pipeline procede a desplegar.
        *   Se conecta a la instancia EC2 (usando SSH con las credenciales de AWS).
        *   Transfiere los nuevos artefactos de build a la instancia.
        *   Posiblemente ejecuta migraciones de base de datos (`npx prisma migrate deploy`).
        *   Reinicia los servicios de la aplicación usando PM2 (e.g., `pm2 reload lti-backend`).
*   **Archivos de Configuración de GitHub Actions:** Estos estarían ubicados en el directorio `.github/workflows/` del repositorio (no visible en la información proporcionada, pero es la ubicación estándar). Sería necesario revisar estos archivos YAML para entender los pasos exactos de la pipeline.

---

## Prompt 2
**Fecha:** 17/05/2025
**Modelo:** Gemini 2.5 Pro (cicd_expert)

**Usuario:** 
As an Expert CI/CD Engineer specializing in GitHub Actions and AWS @cicd_expert.mdc 

Your mission is to design and generate a GitHub Actions workflow file named `.github/workflows/pipeline.yml`. This workflow will automate the testing, building, and deployment of the project's backend. 

Use all the knowledge about the project you have analized before.

**Workflow Requirements:**

1.  **Trigger:**
    * The workflow MUST be triggered exclusively on a `push` event to any branch that has an **open Pull Request** associated with it.

2.  **Jobs:**
    The workflow should consist of the following sequential jobs:

    * **Job 1: Backend Tests**
        * **Purpose:** Run all backend tests.
        * **Environment:** Specify a suitable runner environment (e.g., `ubuntu-latest`).
        * **Services:** If the tests require services like a database, configure them (e.g., using `services` keyword).
        * **Steps:**
            * Checkout the code.
            * Set up the necessary runtime environment for the backend (e.g., Node.js, Python, Java JDK). Use information from the project analysis if available, otherwise use common versions and make a note.
            * Install backend dependencies.
            * Run the backend test command(s). Use specific commands if known from project analysis (e.g., `npm test`, `mvn test`, `pytest`). If not known, use a clear placeholder like `YOUR_BACKEND_TEST_COMMAND` and explicitly state that this needs to be filled in.
        * **Condition:** This job should always run when the workflow is triggered.

    * **Job 2: Backend Build**
        * **Purpose:** Build the backend application and prepare artifacts for deployment.
        * **Dependency:** This job MUST only run if the "Backend Tests" job completes successfully (`needs: backend-tests`).
        * **Steps:**
            * Checkout the code.
            * Set up the necessary runtime/build environment.
            * Execute the backend build command(s) (e.g., `npm run build`, `mvn package`, `docker build -t myapp:latest .`). If the command is not known, use a placeholder like `YOUR_BACKEND_BUILD_COMMAND`.
            * If generating build artifacts (e.g., JAR, ZIP), use `actions/upload-artifact` to store them. If building a Docker image, plan to push it to a registry (the actual push might be part of the deploy job or a separate job, depending on strategy; for this prompt, assume the build artifact is an EC2-deployable package or a Docker image that will be handled in the deploy step).
        * **Condition:** Runs only on success of the previous job.

    * **Job 3: Deploy Backend to EC2**
        * **Purpose:** Deploy the built backend artifact to an AWS EC2 instance.
        * **Dependency:** This job MUST only run if the "Backend Build" job completes successfully (`needs: backend-build`).
        * **Steps:**
            * Checkout code (if needed, or download artifact using `actions/download-artifact`).
            * Configure AWS credentials securely using GitHub Secrets (e.g., `secrets.AWS_ACCESS_KEY_ID`, `secrets.AWS_SECRET_ACCESS_KEY`, `secrets.AWS_REGION`).
            * If deploying files: Use `scp` or `rsync` (e.g., via `appleboy/scp-action` or `easingthemes/ssh-deploy`) to transfer the build artifact(s) to the EC2 instance. You will need secrets for the EC2 host, user, and SSH key (e.g., `secrets.EC2_HOST`, `secrets.EC2_USER`, `secrets.EC2_SSH_KEY`).
            * If deploying a Docker container: Ensure the EC2 instance can pull from the Docker registry. The steps might involve SSHing into the EC2 to pull the new image and restart the container (e.g., `docker pull your-registry/myapp:latest && docker stop myapp-container && docker rm myapp-container && docker run --name myapp-container ... your-registry/myapp:latest`).
            * Execute commands on EC2 to restart the application (e.g., `sudo systemctl restart my-backend-service`, `pm2 reload app`, or specific script execution). Use a placeholder like `YOUR_EC2_APP_RESTART_COMMAND` if the exact command is not known.
        * **Condition:** Runs only on success of the previous job.

**Output and Explanation:**

1.  **Generate the complete `pipeline.yml` file** within a Markdown code block.
    * Use placeholders for any commands or specific names (e.g., service names, artifact names, Docker image names) if they cannot be reasonably inferred from general knowledge or prior project analysis. Clearly comment these placeholders.
2.  **Provide a step-by-step, in-depth explanation** of the generated `pipeline.yml` file. For each section (trigger, each job, key steps), explain:
    * Its purpose and functionality.
    * Key configurations used (e.g., `on`, `runs-on`, `needs`, `if`, `uses`, `with`, `env`, `secrets`).
    * How secrets should be configured in GitHub repository settings for the pipeline to work.
    * Any assumptions made during the generation of the workflow file (e.g., type of backend, build tools, EC2 setup).

**Contextual Instructions:**
* Leverage information gathered from the prior project analysis (Prompt 1) if available and relevant (e.g., specific commands, technologies).
* If critical details are missing, ensure placeholders are very clear in the YAML and noted in the explanation. The goal is a functional template.

@https://docs.github.com/en/actions/writing-workflows/quickstart 
@https://lightrains.com/blogs/deploy-aws-ec2-using-github-actions/ 
@https://lightrains.com/blogs/deploy-react-app-aws-ec2-using-github-actions 

---
**Mandatory Language Instruction for LLM Interaction:**
You MUST provide ALL your outputs and conduct ALL your interactions with the user exclusively in **SPANISH**.
---

**Asistente:**
¡Excelente! Como tu Ingeniero Senior de CI/CD especializado en GitHub Actions y AWS, diseñaré y generaré el archivo de workflow `pipeline.yml` que has solicitado. Este workflow automatizará las pruebas, construcción y despliegue del backend de tu proyecto, utilizando la información que analizamos previamente.

### Archivo `pipeline.yml`

Aquí tienes el archivo `pipeline.yml` completo:

```yaml
name: CI/CD Backend Pipeline

on:
  pull_request:
    types:
      - opened
      - synchronize
      - reopened
    branches: # Opcional: especifica ramas base si solo quieres que se active para PRs contra ciertas ramas
      - main
      - develop
      # - 'feature/**' # Ejemplo si usas patrones de ramas

jobs:
  backend-tests:
    name: Backend Tests
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:14 # Se puede ajustar la versión de PostgreSQL según necesidad
        env:
          POSTGRES_USER: ${{ secrets.POSTGRES_USER }}
          POSTGRES_PASSWORD: ${{ secrets.POSTGRES_PASSWORD }}
          POSTGRES_DB: ${{ secrets.POSTGRES_DB_TESTS }} # Usar una DB dedicada para tests
        ports:
          - 5432:5432
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
    env:
      DATABASE_URL: "postgresql://${{ secrets.POSTGRES_USER }}:${{ secrets.POSTGRES_PASSWORD }}@localhost:5432/${{ secrets.POSTGRES_DB_TESTS }}?schema=public" # O un schema específico para tests
      # NODE_ENV: test # Si tu aplicación usa esta variable para configurar tests

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20.x' # Basado en @types/node: ^20.12.12 en backend/package.json

      - name: Cache Node.js modules (backend)
        uses: actions/cache@v4
        with:
          path: backend/node_modules
          key: ${{ runner.os }}-node-backend-${{ hashFiles('backend/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-node-backend-

      - name: Install backend dependencies
        working-directory: ./backend
        run: npm ci # 'ci' es preferible en pipelines para usar package-lock.json consistentemente

      - name: Wait for PostgreSQL to be healthy
        run: |
          echo "Waiting for PostgreSQL to start..."
          timeout 60s bash -c 'until pg_isready -h localhost -p 5432 -U ${{ secrets.POSTGRES_USER }}; do sleep 1; done'
          echo "PostgreSQL is ready."

      - name: Run backend tests
        working-directory: ./backend
        # Si los tests necesitan crear/migrar el esquema de la BD de tests:
        # run: |
        #   npx prisma migrate reset --force # O un script de setup de tests dedicado
        #   npm test
        run: npm test # Asume que los tests manejan su propio setup de BD o que la BD está limpia

  backend-build:
    name: Backend Build
    runs-on: ubuntu-latest
    needs: backend-tests # Depende del éxito del job de tests

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20.x'

      - name: Cache Node.js modules (backend)
        uses: actions/cache@v4
        with:
          path: backend/node_modules
          key: ${{ runner.os }}-node-backend-${{ hashFiles('backend/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-node-backend-

      - name: Install backend dependencies (si no se restauró del caché o para asegurar consistencia)
        working-directory: ./backend
        run: npm ci

      - name: Build backend application
        working-directory: ./backend
        run: npm run build # Esto crea la carpeta backend/dist/

      - name: Archive production artifacts
        uses: actions/upload-artifact@v4
        with:
          name: backend-dist-artifact # Nombre del artefacto
          path: | # Lista de rutas a incluir en el artefacto
            backend/dist/
            backend/prisma/
            backend/package.json
            backend/package-lock.json
            # Opcional: incluir ecosystem.config.js si lo usas con PM2
            # backend/ecosystem.config.js

  deploy-backend:
    name: Deploy Backend to EC2
    runs-on: ubuntu-latest
    needs: backend-build # Depende del éxito del job de build
    # Condición de ejemplo: Desplegar solo si el PR se dirige a 'develop' o 'main'
    # if: github.event.pull_request.merged == false && (github.base_ref == 'develop' || github.base_ref == 'main')
    # Para este caso, si quieres desplegar en cada push a un PR (para un entorno de revisión):
    if: success() # Simplemente se ejecuta si el build fue exitoso

    steps:
      - name: Download backend artifact
        uses: actions/download-artifact@v4
        with:
          name: backend-dist-artifact
          path: ./app-deploy # Descarga a ./app-deploy/dist, ./app-deploy/prisma, etc.

      - name: Configure AWS Credentials (OIDC)
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::${{ secrets.AWS_ACCOUNT_ID }}:role/${{ secrets.AWS_IAM_ROLE_NAME }}
          aws-region: ${{ secrets.AWS_REGION }}

      - name: Create target directory on EC2 for new release
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ${{ secrets.EC2_USER }}
          key: ${{ secrets.EC2_SSH_KEY }}
          script: |
            RELEASE_ID="release-${{ github.run_id }}"
            sudo mkdir -p /srv/my-app-backend/${RELEASE_ID}
            sudo chown ${{ secrets.EC2_USER }}:${{ secrets.EC2_USER }} /srv/my-app-backend/${RELEASE_ID}
            echo "Created release directory: /srv/my-app-backend/${RELEASE_ID}"

      - name: Deploy artifact to EC2 new release directory
        uses: appleboy/scp-action@master
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ${{ secrets.EC2_USER }}
          key: ${{ secrets.EC2_SSH_KEY }}
          source: "./app-deploy/" # Copia el contenido de app-deploy (dist/, prisma/, package.json, etc.)
          target: "/srv/my-app-backend/release-${{ github.run_id }}"
          strip_components: 1 # Remueve el directorio 'app-deploy' del path en el servidor, copiando su contenido directamente.

      - name: Execute deployment commands on EC2
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ${{ secrets.EC2_USER }}
          key: ${{ secrets.EC2_SSH_KEY }}
          script: |
            set -e # Salir inmediatamente si un comando falla

            NEW_RELEASE_DIR="/srv/my-app-backend/release-${{ github.run_id }}"
            APP_DIR_CURRENT="/srv/my-app-backend/current" # Symlink a la release actual
            PM2_APP_NAME="${{ secrets.PM2_BACKEND_APP_NAME }}"

            echo "Installing production dependencies in $NEW_RELEASE_DIR..."
            cd $NEW_RELEASE_DIR
            # El contenido del artefacto (dist, prisma, package.json) está ahora en $NEW_RELEASE_DIR
            npm install --production

            echo "Running Prisma migrations..."
            # DATABASE_URL debe estar disponible como variable de entorno en la instancia EC2,
            # o configurada en el ecosystem file de PM2, o en el .env del directorio de la app.
            # Asegúrate de que $NEW_RELEASE_DIR/.env exista si es necesario, o que PM2 lo gestione.
            npx prisma migrate deploy

            echo "Updating symlink for current release to $NEW_RELEASE_DIR..."
            sudo ln -sfn $NEW_RELEASE_DIR $APP_DIR_CURRENT

            echo "Restarting application with PM2 ($PM2_APP_NAME)..."
            # Asegúrate que PM2 está configurado para usar $APP_DIR_CURRENT/dist/index.js
            # o un ecosystem.config.js dentro de $APP_DIR_CURRENT (que apuntaría a $NEW_RELEASE_DIR via symlink)
            # Por ejemplo, si PM2 fue iniciado con `pm2 start $APP_DIR_CURRENT/dist/index.js --name $PM2_APP_NAME`
            # Un simple reload debería funcionar:
            pm2 reload $PM2_APP_NAME || pm2 start $APP_DIR_CURRENT/dist/index.js --name $PM2_APP_NAME --cwd $APP_DIR_CURRENT

            echo "Deployment to EC2 complete. Current release pointing to: $NEW_RELEASE_DIR"

            # Opcional: Limpieza de releases antiguas (ej. mantener las últimas 5)
            echo "Cleaning up old releases..."
            cd /srv/my-app-backend && ls -dt release-* | tail -n +6 | xargs sudo rm -rf
            echo "Cleanup complete."
```

### Explicación Detallada del `pipeline.yml`

1.  **`name: CI/CD Backend Pipeline`**
    *   **Propósito:** Define un nombre legible para el workflow que aparecerá en la interfaz de GitHub Actions.

2.  **`on:`**
    *   **Propósito:** Especifica los eventos que dispararán el workflow.
    *   **`pull_request:`**: El workflow se activa cuando ocurre un evento de Pull Request.
        *   **`types: [opened, synchronize, reopened]`**:
            *   `opened`: Cuando se abre un nuevo Pull Request.
            *   `synchronize`: Cuando se empujan nuevos commits a la rama de un Pull Request existente (esto cubre tu requisito de "push event to any branch that has an open Pull Request associated with it").
            *   `reopened`: Cuando se reabre un Pull Request.
        *   **`branches: [main, develop]` (Opcional):** Puedes descomentar y ajustar esta sección si solo quieres que el workflow se ejecute para Pull Requests que apuntan a ramas específicas (ej. `main` o `develop`). Si se omite, se ejecuta para PRs contra cualquier rama.

3.  **`jobs:`**
    *   **Propósito:** Define una o más tareas (jobs) que se ejecutarán como parte del workflow. Cada job se ejecuta en un entorno de runner.

    *   **Job 1: `backend-tests`**
        *   **`name: Backend Tests`**: Nombre legible para el job.
        *   **`runs-on: ubuntu-latest`**: Especifica que el job se ejecutará en la última versión disponible de un runner Ubuntu hospedado por GitHub.
        *   **`services:`**: Permite ejecutar contenedores de servicios que están disponibles para los pasos del job.
            *   **`postgres:`**: Define un servicio de base deatos PostgreSQL.
                *   `image: postgres:14`: Usa la imagen oficial de PostgreSQL versión 14.
                *   `env:`: Variables de entorno para el contenedor PostgreSQL, obtenidas de los secretos de GitHub. `POSTGRES_DB_TESTS` asegura que se use una base de datos separada para las pruebas.
                *   `ports: ['5432:5432']`: Mapea el puerto 5432 del contenedor al puerto 5432 del host del runner, permitiendo que la aplicación se conecte a `localhost:5432`.
                *   `options: >- --health-cmd pg_isready ...`: Configura una verificación de salud para asegurar que el servicio PostgreSQL esté listo antes de que los tests intenten conectarse.
        *   **`env:`**: Define variables de entorno para todos los pasos dentro de este job.
            *   `DATABASE_URL`: Cadena de conexión para que Prisma (u otro ORM/cliente) se conecte a la base de datos de prueba del servicio PostgreSQL.
        *   **`steps:`**: Secuencia de acciones a ejecutar.
            *   **`actions/checkout@v4`**: Descarga una copia de tu repositorio en el runner.
            *   **`actions/setup-node@v4`**: Configura el entorno de Node.js con la versión `20.x`.
            *   **`actions/cache@v4`**: Guarda en caché las dependencias de `node_modules` del backend para acelerar futuras ejecuciones. La caché se invalida si `backend/package-lock.json` cambia.
            *   **`Install backend dependencies`**: Ejecuta `npm ci` en el directorio `backend/` para instalar las dependencias de forma limpia y reproducible a partir del `package-lock.json`.
            *   **`Wait for PostgreSQL to be healthy`**: Un script que espera explícitamente a que el servicio PostgreSQL esté aceptando conexiones.
            *   **`Run backend tests`**: Ejecuta el comando `npm test` (definido en `backend/package.json`) para correr las pruebas del backend. Se comenta una opción para resetear la base de datos de tests con Prisma si es necesario antes de cada ejecución de tests.

    *   **Job 2: `backend-build`**
        *   **`name: Backend Build`**: Nombre del job.
        *   **`runs-on: ubuntu-latest`**: Se ejecuta en un runner Ubuntu.
        *   **`needs: backend-tests`**: Especifica que este job solo se ejecutará si el job `backend-tests` se completa exitosamente.
        *   **`steps:`**:
            *   Checkout, Setup Node.js, Cache, e Install Dependencies: Similares al job de tests, preparando el entorno para la construcción.
            *   **`Build backend application`**: Ejecuta `npm run build` en `backend/` para compilar el código TypeScript a JavaScript (generalmente en `backend/dist/`) y realizar otras tareas de construcción.
            *   **`actions/upload-artifact@v4`**: Recopila los artefactos de construcción (`dist/`, `prisma/`, `package.json`, `package-lock.json`) y los sube como un artefacto llamado `backend-dist-artifact`. Estos archivos son necesarios para el despliegue.

    *   **Job 3: `deploy-backend`**
        *   **`name: Deploy Backend to EC2`**: Nombre del job.
        *   **`runs-on: ubuntu-latest`**: Se ejecuta en un runner Ubuntu.
        *   **`needs: backend-build`**: Depende del éxito del job `backend-build`.
        *   **`if: success()`**: Asegura que el job solo se ejecute si los jobs anteriores fueron exitosos. Puedes añadir condiciones más específicas aquí si el despliegue solo debe ocurrir bajo ciertas circunstancias (e.g., `github.base_ref == 'develop'` para desplegar PRs solo si van a la rama `develop`).
        *   **`steps:`**:
            *   **`actions/download-artifact@v4`**: Descarga el `backend-dist-artifact` (creado en el job de build) al directorio `./app-deploy` en el runner.
            *   **`aws-actions/configure-aws-credentials@v4`**: Configura las credenciales de AWS de forma segura utilizando OpenID Connect (OIDC). Asume un rol de IAM (`AWS_IAM_ROLE_NAME`) que GitHub Actions asumirá para desplegar (e.g., `GitHubActionEC2DeployRole`). Este rol debe tener una política de confianza con el proveedor OIDC de GitHub y los permisos necesarios (e.g., para describir instancias EC2, y cualquier otro servicio que necesite la aplicación o el proceso de despliegue. Para SSH/SCP, los permisos de AWS no son directamente para la copia, sino para que el runner pueda autenticarse si fuera necesario para otras acciones AWS. La conexión SSH se basa en la clave SSH).
            *   **`Create target directory on EC2 for new release`**: Usa `appleboy/ssh-action` para conectarse a la instancia EC2 vía SSH y crear un directorio único para la nueva release (e.g., `/srv/my-app-backend/release-12345`). Esto ayuda a mantener las releases separadas y facilita los rollbacks.
            *   **`appleboy/scp-action@master`**: Copia de forma segura el contenido del directorio `./app-deploy` (que contiene los artefactos descargados) desde el runner al directorio de la nueva release en la instancia EC2. El parámetro `strip_components: 1` asegura que se copie el contenido de `app-deploy` directamente, sin el propio directorio `app-deploy`.
            *   **`Execute deployment commands on EC2`**: Usa `appleboy/ssh-action` nuevamente para ejecutar una serie de comandos en la instancia EC2:
                *   `set -e`: Asegura que el script falle si cualquier comando falla.
                *   Define variables para el directorio de la nueva release (`NEW_RELEASE_DIR`) y el directorio de la aplicación actual (`APP_DIR_CURRENT`, que será un symlink).
                *   Navega al directorio de la nueva release.
                *   `npm install --production`: Instala solo las dependencias de producción.
                *   `npx prisma migrate deploy`: Aplica las migraciones de base de datos pendientes. Requiere que `DATABASE_URL` esté configurada en el entorno del servidor EC2 (e.g., en un archivo `.env` que no se comitea, o mediante la configuración de PM2).
                *   `sudo ln -sfn $NEW_RELEASE_DIR $APP_DIR_CURRENT`: Actualiza un enlace simbólico (`current`) para que apunte al directorio de la nueva release. Esta es una práctica común para cambios de release atómicos y rollbacks fáciles.
                *   `pm2 reload $PM2_APP_NAME || pm2 start ...`: Reinicia la aplicación usando PM2. Intenta un `reload` (sin downtime si es posible) y si falla (o es el primer despliegue), hace un `start`. Asume que PM2 está configurado para ejecutar la aplicación desde `$APP_DIR_CURRENT/dist/index.js`. `PM2_BACKEND_APP_NAME` es el nombre o ID de tu aplicación en PM2.
                *   **Opcional: Limpieza de releases antiguas**: Un comando para eliminar releases antiguas y evitar que el disco se llene.

### Configuración de Secretos en GitHub

Para que este workflow funcione, necesitas configurar los siguientes secretos en tu repositorio de GitHub (Settings > Secrets and variables > Actions > New repository secret):

*   **`POSTGRES_USER`**: Usuario para la base de datos PostgreSQL (e.g., `postgres`).
*   **`POSTGRES_PASSWORD`**: Contraseña para el usuario de PostgreSQL.
*   **`POSTGRES_DB_TESTS`**: Nombre de la base de datos PostgreSQL a usar para las pruebas (e.g., `lti_test_db`).
*   **`AWS_ACCOUNT_ID`**: El ID de tu cuenta de AWS (necesario para OIDC).
*   **`AWS_IAM_ROLE_NAME`**: El nombre del rol de IAM en AWS que GitHub Actions asumirá para desplegar (e.g., `GitHubActionEC2DeployRole`). Este rol debe tener una política de confianza con el proveedor OIDC de GitHub y los permisos necesarios (e.g., para describir instancias EC2, y cualquier otro servicio que necesite la aplicación o el proceso de despliegue. Para SSH/SCP, los permisos de AWS no son directamente para la copia, sino para que el runner pueda autenticarse si fuera necesario para otras acciones AWS. La conexión SSH se basa en la clave SSH).
*   **`AWS_REGION`**: La región de AWS donde está tu instancia EC2 (e.g., `us-east-1`).
*   **`EC2_HOST`**: La dirección IP pública o el nombre DNS de tu instancia EC2.
*   **`EC2_USER`**: El nombre de usuario para conectarse a la instancia EC2 vía SSH (e.g., `ec2-user`, `ubuntu`).
*   **`EC2_SSH_KEY`**: La clave SSH privada (NO la pública) que permite el acceso al `EC2_USER` en la instancia. Debe estar completa, incluyendo `-----BEGIN RSA PRIVATE KEY-----` y `-----END RSA PRIVATE KEY-----`.
*   **`PM2_BACKEND_APP_NAME`**: El nombre o ID de tu aplicación backend como está registrada en PM2 en la instancia EC2.

### Asunciones Realizadas

*   Tu backend es una aplicación Node.js que usa `npm` como gestor de paquetes.
*   Los comandos `npm test` y `npm run build` están correctamente configurados en tu `backend/package.json`.
*   Utilizas Prisma para las migraciones de base de datos, y el comando `npx prisma migrate deploy` es adecuado para producción.
*   La instancia EC2 está configurada con Node.js, npm, y PM2.
*   La `DATABASE_URL` para la aplicación en producción está configurada en el entorno de la instancia EC2 (e.g., a través de un archivo `.env` que no se comitea, o mediante la configuración de PM2).
*   Tienes un rol IAM configurado en AWS para que GitHub Actions lo asuma mediante OIDC, con los permisos necesarios.
*   El usuario EC2 (`EC2_USER`) tiene permisos `sudo` para crear directorios en `/srv/` y para gestionar el enlace simbólico y, potencialmente, para ciertos comandos de PM2 si no se ejecutan como el propio usuario.
*   La estrategia de despliegue utiliza un directorio por release y un enlace simbólico `current` para apuntar a la release activa.

Este workflow proporciona una base sólida para tu pipeline de CI/CD. Puedes ajustarlo y expandirlo según las necesidades específicas de tu proyecto. Recuerda configurar el rol de IAM y la política de confianza correctamente en AWS para la autenticación OIDC.

---

## Prompt 3
**Fecha:** 17/05/2025
**Modelo:** Gemini 2.5 Pro (cicd_expert)

**Usuario:** 
Your task is to provide a detailed and comprehensive guide on how to thoroughly test and verify that the entire CI/CD pipeline (created via GitHub Actions for backend testing, building, and EC2 deployment, as per the previously defined `pipeline.yml`) has been implemented correctly and is functioning as expected.

**The verification guide should cover the following areas:**

1.  **Trigger Verification:**
    * Detailed steps to simulate a "push to a branch with an open Pull Request" to confirm the workflow triggers.
    * How to confirm it does NOT trigger on direct pushes to the main/master branch (unless it also has an open PR against it from another branch).
    * How to confirm it does NOT trigger on pushes to branches without an open Pull Request.

2.  **Backend Tests Job Verification:**
    * How to inspect GitHub Actions logs to confirm the test job started and completed.
    * How to verify that the backend tests were actually executed (e.g., looking for specific output related to test execution in logs).
    * How to confirm successful completion of tests (e.g., exit codes, summary reports in logs).
    * How to test and verify the pipeline's behavior when backend tests *fail* (e.g., ensuring the pipeline stops, the job is marked as failed, and subsequent jobs like "Build" and "Deploy" do not run).

3.  **Backend Build Job Verification:**
    * How to confirm the build job starts only after successful completion of the test job.
    * How to check for successful build artifact generation. If artifacts are uploaded to GitHub Actions, explain how to locate and inspect them. If a Docker image is built, explain how one might verify its creation (e.g., if pushed to a staging registry).
    * How to test and verify pipeline behavior if the build process *fails* (e.g., ensuring the pipeline stops, the job is marked as failed, and the "Deploy" job does not run).

4.  **Backend Deployment to EC2 Job Verification:**
    * How to confirm the deployment job starts only after a successful build.
    * Specific steps to verify that the application artifacts are correctly transferred to the specified EC2 instance (e.g., SSHing into the EC2 and checking file paths, checksums, or timestamps).
    * If deploying a Docker container, how to verify the correct image is pulled and the container is running on EC2.
    * How to check if the application is restarted or started correctly and is accessible/functional on the EC2 instance after deployment.
    * Common troubleshooting steps for this stage (e.g., checking SSH connection logs, file permissions on EC2, application logs on EC2, AWS security group settings).
    * How to test and verify pipeline behavior if the deployment process *fails*.

5.  **Secrets Management Verification:**
    * Guidance on how to ensure that secrets (e.g., AWS credentials, SSH keys) are being utilized correctly by the actions and not exposed in logs. (Emphasize *not* printing secrets, but checking for successful operations that depend on them).

6.  **Workflow Logs and Artifacts:**
    * Where to find detailed logs for each job in the GitHub Actions interface.
    * How to download and inspect any artifacts produced by the workflow (e.g., build artifacts, test reports if configured).

7.  **Overall Pipeline Flow:**
    * How to observe the dependencies between jobs (`needs` keyword) and ensure they are respected.

**Output Format:**
Present the guide as a series of actionable steps, checklists, or verification procedures. Use clear headings, subheadings, and bullet points for organization and ease of use. Where appropriate, suggest example commands or checks.

Include this guide in @docs 

---
**Mandatory Language Instruction for LLM Interaction:**
You MUST provide ALL your outputs and conduct ALL your interactions with the user exclusively in **SPANISH**.
---

**Asistente:**
¡Absolutamente! Como tu experto en CI/CD, prepararé una guía detallada y exhaustiva para que puedas probar y verificar minuciosamente la pipeline que hemos definido en el archivo `.github/workflows/pipeline.yml`.

Esta guía te permitirá asegurar que cada componente de tu automatización (triggers, pruebas, construcción, despliegue a EC2 y gestión de secretos) funcione exactamente como se espera.

Procederé a crear el archivo `docs/VERIFICATION_GUIDE.md` con la guía completa.

Se ha creado el archivo `docs/VERIFICATION_GUIDE.md` con la guía completa para verificar tu pipeline de CI/CD.

Este documento contiene todos los pasos y consideraciones que discutimos, cubriendo desde la activación del workflow hasta la verificación del despliegue en EC2 y la gestión de secretos. Debería serte de gran utilidad para asegurar la correcta implementación y funcionamiento de tu pipeline.

Ahora, procederé a registrar esta interacción en el archivo `prompts/prompts-DAD.md`.

## Prompt 4
**Fecha:** 17/05/2025
**Modelo:** Gemini 2.5 Pro (cicd_expert)

**Usuario:** 
Añadenos en @VERIFICATION_GUIDE.md la sección sobre cómo configurar tu entorno de AWS y obtener los secretos necesarios para que el workflow funcione correctamente.

**Asistente:**
Entendido. Voy a integrar la guía sobre cómo configurar tu entorno de AWS y obtener los secretos necesarios directamente en el archivo `docs/VERIFICATION_GUIDE.md`.

Primero, leeré el contenido actual del archivo para luego añadir la nueva sección.

Perfecto. Ahora añadiré la sección sobre la configuración del entorno AWS y la obtención de secretos al final del archivo `docs/VERIFICATION_GUIDE.md`.

Aquí está el contenido que se añadirá:

```markdown
## 8. Configuración del Entorno AWS y Obtención de Secretos Necesarios

Antes de que la pipeline pueda desplegar en AWS EC2 y para que los tests que usan servicios de AWS (si aplica) funcionen, necesitas configurar ciertos parámetros y obtener credenciales/identificadores. Estos valores se almacenarán como secretos en tu repositorio de GitHub.

### A. Obtención de Identificadores y Parámetros de AWS y EC2

1.  **`AWS_ACCOUNT_ID` (ID de tu Cuenta de AWS)**
    *   **Cómo obtenerlo:**
        1.  Inicia sesión en la [Consola de Administración de AWS](https://aws.amazon.com/console/).
        2.  En la esquina superior derecha, haz clic en el nombre de tu cuenta.
        3.  Tu ID de cuenta de AWS (un número de 12 dígitos) se muestra en el menú desplegable.

2.  **`AWS_REGION` (Región de AWS)**
    *   **Cómo obtenerlo:**
        1.  Esta es la región donde está alojada tu instancia EC2 (ej. `us-east-1`, `eu-west-2`).
        2.  Puedes ver la región seleccionada actualmente en la esquina superior derecha de la Consola de AWS.

3.  **`EC2_HOST` (IP Pública o DNS de tu Instancia EC2)**
    *   **Cómo obtenerlo (para una instancia EC2 existente):**
        1.  En la Consola de AWS, ve al servicio **EC2**.
        2.  Selecciona **"Instances"** (Instancias).
        3.  Elige tu instancia.
        4.  En la pestaña **"Details"** (Detalles), busca:
            *   **"Public IPv4 address"** o
            *   **"Public IPv4 DNS"**.

4.  **`EC2_USER` (Usuario para Conexión SSH a EC2)**
    *   **Cómo determinarlo:** Depende de la AMI (Amazon Machine Image) de tu instancia:
        *   Amazon Linux 2 / Amazon Linux AMI: `ec2-user`
        *   Ubuntu AMI: `ubuntu`
        *   CentOS AMI: `centos`
        *   Debian AMI: `admin`
        *   Consulta la documentación de tu AMI si es diferente.

5.  **`EC2_SSH_KEY` (Clave SSH Privada para EC2)**
    *   **Generación/Obtención:**
        1.  Al lanzar una instancia EC2, puedes crear un nuevo par de claves (archivo `.pem`) o usar uno existente.
        2.  **Importante:** Si creas uno nuevo, descarga y guarda el archivo `.pem` en un lugar seguro. AWS no te permitirá descargarlo de nuevo.
    *   **Contenido para el Secreto de GitHub:**
        1.  Abre tu archivo de clave privada `.pem` con un editor de texto.
        2.  Copia **todo el contenido**, incluyendo `-----BEGIN RSA PRIVATE KEY-----` y `-----END RSA PRIVATE KEY-----` (o las cabeceras equivalentes para otros formatos de clave como OpenSSH).

### B. Configuración de Rol IAM para GitHub Actions (OIDC)

Para una comunicación segura entre GitHub Actions y AWS sin claves de acceso de larga duración, se usa OpenID Connect (OIDC).

1.  **`AWS_IAM_ROLE_NAME` (Nombre del Rol de IAM)**
    *   **Pasos para crearlo y configurarlo:**
        1.  **Añadir Proveedor de Identidad OIDC de GitHub en IAM:**
            *   En la Consola de AWS > IAM > **"Identity providers"**.
            *   Haz clic en **"Add provider"**.
            *   Selecciona **"OpenID Connect"**.
            *   **Provider URL:** `https://token.actions.githubusercontent.com`
            *   **Audience:** `sts.amazonaws.com`
            *   Haz clic en **"Get thumbprint"** y luego en **"Add provider"**.
        2.  **Crear el Rol de IAM:**
            *   En IAM > **"Roles"** > **"Create role"**.
            *   **Tipo de entidad de confianza:** Selecciona **"Web identity"**.
            *   **Identity provider:** Elige el proveedor OIDC `token.actions.githubusercontent.com` que creaste.
            *   **Audience:** Selecciona `sts.amazonaws.com`.
            *   **Restringir por Repositorio/Rama (Recomendado):**
                *   Puedes añadir una condición en la política de confianza del rol para limitar qué repositorios o ramas de GitHub pueden asumir este rol.
                *   Ejemplo de condición para el campo `StringEquals` en `token.actions.githubusercontent.com:sub`:
                    ```
                    repo:TU_ORGANIZACION/TU_REPOSITORIO:pull_request
                    ```
                    o para una rama específica:
                    ```
                    repo:TU_ORGANIZACION/TU_REPOSITORIO:ref:refs/heads/main
                    ```
            *   Haz clic en **"Next"**.
        3.  **Adjuntar Políticas de Permisos al Rol:**
            *   Define los permisos que GitHub Actions necesitará. Sigue el principio de privilegio mínimo.
            *   Para la pipeline definida (`pipeline.yml`), el rol necesita principalmente:
                *   Poder ser asumido (`sts:AssumeRoleWithWebIdentity` - esto se configura en la política de confianza, no en los permisos directos).
                *   Si tu script en EC2 necesita interactuar con otros servicios AWS (ej. S3, SSM), añade esos permisos aquí. Para el `scp-action` y `ssh-action`, los permisos AWS no son para la transferencia SSH en sí (que usa `EC2_SSH_KEY`), sino para que `configure-aws-credentials` funcione.
            *   Ejemplo de política mínima si solo necesitas que `configure-aws-credentials` funcione y no hay otras llamadas a la API de AWS desde el workflow: No se necesitan permisos adicionales más allá de la capacidad de asumir el rol.
            *   Si necesitas más permisos (ej. para describir instancias EC2 desde el workflow):
                ```json
                {
                    "Version": "2012-10-17",
                    "Statement": [
                        {
                            "Effect": "Allow",
                            "Action": [
                                "ec2:DescribeInstances"
                                // Añade otros permisos necesarios aquí
                            ],
                            "Resource": "*" // Restringe los recursos si es posible
                        }
                    ]
                }
                ```
                Crea esta política y adjúntala al rol.
            *   Haz clic en **"Next"**, luego **"Next"**.
        4.  **Nombrar y Crear el Rol:**
            *   **Role name:** Dale un nombre descriptivo (ej. `GitHubActionEC2DeployRole`). Este es el valor para tu secreto `AWS_IAM_ROLE_NAME`.
            *   Revisa la configuración y haz clic en **"Create role"**.
        5.  El valor para el secreto `AWS_IAM_ROLE_NAME` es solo el nombre del rol (ej. `GitHubActionEC2DeployRole`). El ARN completo se construye en el workflow.

### C. Otros Parámetros Específicos de la Aplicación

1.  **`PM2_BACKEND_APP_NAME` (Nombre de la Aplicación Backend en PM2)**
    *   **Cómo obtenerlo/configurarlo:**
        1.  Este es el nombre o ID que tu aplicación tiene en PM2 en la instancia EC2.
        2.  Conéctate a tu instancia EC2 vía SSH.
        3.  Ejecuta: `pm2 list`.
        4.  Busca tu aplicación en la columna `name` o `id`.
        5.  Si aún no está gestionada por PM2, cuando la inicies (ej. `pm2 start dist/index.js --name tu-app-backend`), `tu-app-backend` sería el valor.

### D. Configuración de Secretos en GitHub Actions

Una vez que hayas obtenido todos estos valores, añádelos como secretos en tu repositorio de GitHub:
1.  Ve a tu repositorio en GitHub.
2.  Settings > Secrets and variables > Actions.
3.  Haz clic en "New repository secret" para cada uno de los valores listados:
    *   `AWS_ACCOUNT_ID`
    *   `AWS_REGION`
    *   `AWS_IAM_ROLE_NAME`
    *   `EC2_HOST`
    *   `EC2_USER`
    *   `EC2_SSH_KEY` (pega el contenido completo de tu archivo `.pem`)
    *   `PM2_BACKEND_APP_NAME`
    *   (Y los secretos de PostgreSQL si los usas para los tests: `POSTGRES_USER`, `POSTGRES_PASSWORD`, `POSTGRES_DB_TESTS`).

Esta sección te ayudará a asegurar que tienes todos los prerrequisitos de AWS configurados correctamente para que tu pipeline funcione.
```

Ahora, actualizaré el archivo `docs/VERIFICATION_GUIDE.md`.

¡Perfecto! El archivo `docs/VERIFICATION_GUIDE.md` ha sido actualizado para incluir la sección "8. Configuración del Entorno AWS y Obtención de Secretos Necesarios" al final.

Ahora tienes una guía de verificación aún más completa que no solo cubre cómo probar la pipeline, sino también cómo configurar los prerrequisitos esenciales en AWS y obtener los secretos para GitHub Actions.

Como siempre, si tienes más preguntas o necesitas ajustes, estoy aquí para ayudarte. También actualizaré el archivo `prompts/prompts-DAD.md` para reflejar nuestra última interacción.
