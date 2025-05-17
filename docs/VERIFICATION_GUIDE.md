# Guía de Verificación de la Pipeline CI/CD del Backend

Esta guía proporciona los pasos detallados para probar y verificar que la pipeline de CI/CD configurada en `.github/workflows/pipeline.yml` para el backend (pruebas, construcción y despliegue en EC2) está implementada correctamente y funciona como se espera.

## 1. Verificación del Trigger

El workflow está configurado para activarse en eventos de `pull_request` (cuando se abren, sincronizan o reabren) dirigidos a ramas específicas (si se configuró `branches` en el trigger) o a cualquier rama.

**Pasos para verificar:**

*   **Escenario 1: Trigger en PR nuevo/actualizado (Comportamiento esperado)**
    1.  Crea una nueva rama desde tu rama de desarrollo principal (ej. `develop` o `main`):
        ```bash
        git checkout develop
        git pull
        git checkout -b feature/test-pipeline-trigger
        ```
    2.  Realiza un cambio pequeño en cualquier archivo del backend (ej. añade un comentario).
    3.  Haz commit y push de la rama:
        ```bash
        git add .
        git commit -m "feat: test pipeline trigger"
        git push origin feature/test-pipeline-trigger
        ```
    4.  Ve a tu repositorio en GitHub y abre un Pull Request (PR) desde `feature/test-pipeline-trigger` hacia tu rama base (ej. `develop` o `main`).
    5.  **Verificación:**
        *   Navega a la pestaña "Actions" de tu repositorio.
        *   Deberías ver que el workflow "CI/CD Backend Pipeline" se ha iniciado automáticamente para este PR.
        *   Realiza otro commit en la rama `feature/test-pipeline-trigger` y haz push:
            ```bash
            # Realiza otro cambio pequeño
            git commit -am "feat: test pipeline trigger sync"
            git push origin feature/test-pipeline-trigger
            ```
        *   **Verificación:** El workflow debería activarse nuevamente (evento `synchronize`).

*   **Escenario 2: NO trigger en push directo a `main` (sin PR asociado)**
    1.  Asegúrate de que no haya PRs abiertos desde otras ramas hacia `main`.
    2.  Realiza un push directo a `main` (esto generalmente está protegido y no es una práctica recomendada sin PRs, pero es para verificar el trigger):
        *   Si tienes permisos, clona el repo, haz un cambio, comitea y haz `git push origin main`.
    3.  **Verificación:**
        *   Navega a la pestaña "Actions".
        *   El workflow "CI/CD Backend Pipeline" **NO** debería haberse activado por este push directo, ya que el trigger está en `pull_request`. (Si tienes otro workflow que sí se activa en `push` a `main`, ese sí se ejecutaría).

*   **Escenario 3: NO trigger en push a rama sin PR**
    1.  Crea una nueva rama:
        ```bash
        git checkout -b临时-test-no-pr
        ```
    2.  Realiza un cambio, comitea y haz push:
        ```bash
        # Realiza un cambio
        git commit -am "test: push to branch without PR"
        git push origin temp-test-no-pr
        ```
    3.  **Verificación:**
        *   Navega a la pestaña "Actions".
        *   El workflow "CI/CD Backend Pipeline" **NO** debería haberse activado, ya que no hay ningún PR asociado a la rama `temp-test-no-pr`.

## 2. Verificación del Job `backend-tests`

Este job es responsable de ejecutar las pruebas automatizadas del backend.

**Pasos para verificar:**

1.  **Inspección de Logs (Caso Exitoso):**
    *   Después de que el workflow se haya activado por un PR (Escenario 1 anterior), ve a la pestaña "Actions", selecciona el run del workflow.
    *   Haz clic en el job "Backend Tests".
    *   **Verificación:**
        *   El job debe mostrar un estado de "Success" (marca de verificación verde).
        *   Expande los logs de cada paso:
            *   `Checkout repository`: Debe completarse correctamente.
            *   `Set up Node.js`: Debe mostrar la versión de Node.js configurada.
            *   `Cache Node.js modules (backend)`: Debe indicar si la caché fue encontrada/restaurada o creada.
            *   `Install backend dependencies`: Debería mostrar la salida de `npm ci`, incluyendo la instalación de paquetes.
            *   `Wait for PostgreSQL to be healthy`: Debería mostrar los intentos de conexión hasta que PostgreSQL esté listo.
            *   `Run backend tests`:
                *   Busca la salida de tu framework de pruebas (ej. Jest, Mocha). Deberías ver un resumen de tests pasados, como `✓ Test de conexión a BD exitoso`, `Tests: XX passed, XX total`.
                *   Confirma que el paso finaliza con un código de salida 0 (implícito en el éxito del paso en GitHub Actions).

2.  **Verificación de Falla en Tests (Comportamiento Esperado en Falla):**
    1.  En la rama de tu PR (`feature/test-pipeline-trigger`), introduce deliberadamente un test que falle o modifica el código para que un test existente falle.
        *   Por ejemplo, si usas Jest, podrías cambiar un `expect(true).toBe(true);` a `expect(true).toBe(false);` en un archivo de test del backend.
    2.  Haz commit y push de este cambio a la rama del PR.
    3.  **Verificación:**
        *   El workflow "CI/CD Backend Pipeline" se activará.
        *   Navega al run del workflow y al job "Backend Tests".
        *   El job "Backend Tests" debería mostrar un estado de "Failure" (cruz roja).
        *   En los logs del paso `Run backend tests`, deberías ver el error específico del test que falló y un código de salida distinto de cero.
        *   Los jobs subsiguientes (`backend-build`, `deploy-backend`) **NO** deberían haberse ejecutado. Deberían mostrarse como "Skipped" o no aparecer en la ejecución.

3.  **Verificación del Servicio de Base de Datos:**
    *   En los logs del paso `Run backend tests`, asegúrate de que las pruebas que interactúan con la base de datos (si las hay) se conecten correctamente al servicio PostgreSQL (`localhost:5432`) definido en `services`.
    *   La variable de entorno `DATABASE_URL` debe estar configurada para usar los secretos `POSTGRES_USER`, `POSTGRES_PASSWORD`, y `POSTGRES_DB_TESTS`.

## 3. Verificación del Job `backend-build`

Este job construye la aplicación backend y prepara los artefactos para el despliegue.

**Pasos para verificar:**

1.  **Inicio Condicional (Caso Exitoso):**
    *   Asegúrate de que el job `backend-tests` se haya completado exitosamente.
    *   **Verificación:** El job `backend-build` debería iniciarse automáticamente después. Si `backend-tests` falló, `backend-build` debe ser omitido.

2.  **Generación y Carga de Artefactos (Caso Exitoso):**
    *   Si `backend-build` se completa con éxito:
        *   **Verificación:**
            *   El job debe mostrar un estado de "Success".
            *   En los logs del paso `Build backend application`, deberías ver la salida de `npm run build` (o el comando de construcción específico), indicando que la compilación fue exitosa (ej. creación de la carpeta `dist/`).
            *   En la página de resumen del workflow run, al final, debería haber una sección de "Artifacts".
            *   Deberías ver un artefacto llamado `backend-dist-artifact`.
            *   Descarga este artefacto y verifica su contenido. Debería ser un archivo ZIP que contenga:
                *   `backend/dist/` (o solo `dist/`)
                *   `backend/prisma/` (o solo `prisma/`)
                *   `backend/package.json`
                *   `backend/package-lock.json`

3.  **Verificación de Falla en Build (Comportamiento Esperado en Falla):**
    1.  En la rama de tu PR, introduce un error en el proceso de build.
        *   Por ejemplo, modifica el script `build` en `backend/package.json` para que ejecute un comando inexistente o que falle.
    2.  Asegúrate de que los tests pasen para que el job `backend-build` intente ejecutarse.
    3.  Haz commit y push.
    4.  **Verificación:**
        *   El job `backend-tests` debería pasar.
        *   El job `backend-build` debería iniciarse pero luego fallar.
        *   En los logs del paso `Build backend application`, deberías ver el error que causó la falla del build.
        *   El job `deploy-backend` **NO** debería ejecutarse.

## 4. Verificación del Job `deploy-backend` (Despliegue a EC2)

Este es el job crítico que despliega tu aplicación en la instancia EC2.

**Pasos para verificar:**

1.  **Inicio Condicional (Caso Exitoso):**
    *   Asegúrate de que los jobs `backend-tests` y `backend-build` se hayan completado exitosamente.
    *   **Verificación:** El job `deploy-backend` debería iniciarse automáticamente. Si alguno de los anteriores falló, este job debe ser omitido.

2.  **Transferencia de Artefactos a EC2 (Caso Exitoso):**
    1.  Una vez que el job `deploy-backend` se complete con éxito:
    2.  Conéctate a tu instancia EC2 vía SSH:
        ```bash
        ssh -i /ruta/a/tu/clave-privada.pem ${{ secrets.EC2_USER }}@${{ secrets.EC2_HOST }}
        ```
    3.  **Verificación:**
        *   Verifica que se haya creado el directorio de la nueva release:
            ```bash
            ls -l /srv/my-app-backend/
            # Deberías ver un directorio como "release-XYZ" donde XYZ es el github.run_id
            # y un enlace simbólico "current" apuntando a este nuevo directorio.
            ```
        *   Inspecciona el contenido del directorio de la release:
            ```bash
            ls -la /srv/my-app-backend/current/
            # Debería contener: dist/, prisma/, package.json, package-lock.json, node_modules (instalado en EC2)
            ```
        *   Verifica timestamps o checksums de los archivos si necesitas una comprobación más rigurosa.
        *   En los logs del workflow en GitHub Actions para el paso `Deploy artifact to EC2 new release directory`, deberías ver la confirmación de la copia de archivos.

3.  **Ejecución de Comandos de Despliegue en EC2 (Caso Exitoso):**
    *   **Verificación (en la instancia EC2 y logs de GitHub Actions):**
        *   En los logs del paso `Execute deployment commands on EC2` en GitHub Actions:
            *   Deberías ver mensajes como "Installing production dependencies...", "Running Prisma migrations...", "Updating symlink...", "Restarting application with PM2...".
        *   En la instancia EC2:
            *   Verifica que `npm install --production` se ejecutó en `/srv/my-app-backend/current/` (debería existir `node_modules`).
            *   Verifica el estado de las migraciones de Prisma. Puedes necesitar conectar a tu BD de producción y revisar el estado de la tabla `_prisma_migrations` o los logs de la aplicación si registran las migraciones.
            *   Verifica el estado de PM2:
                ```bash
                pm2 list
                # Deberías ver tu aplicación (ej. ${{ secrets.PM2_BACKEND_APP_NAME }}) con estado "online".
                pm2 logs ${{ secrets.PM2_BACKEND_APP_NAME }}
                # Revisa los logs recientes para ver si la aplicación se reinició y no hay errores de inicio.
                ```

4.  **Verificación Funcional de la Aplicación en EC2 (Caso Exitoso):**
    *   **Verificación:**
        *   Si tu aplicación tiene un endpoint de health check, pruébalo:
            ```bash
            curl http://localhost:PUERTO_APP/health # Ejecutar en la instancia EC2
            # O desde tu máquina local si el puerto está abierto en el Security Group:
            # curl http://${{ secrets.EC2_HOST }}:PUERTO_APP/health
            ```
            (Reemplaza `PUERTO_APP` con el puerto real en el que corre tu aplicación backend, ej. 3010).
        *   Realiza una prueba funcional básica que demuestre que la nueva versión está operativa.

5.  **Verificación de Falla en Despliegue (Comportamiento Esperado en Falla):**
    1.  Simula una condición de falla. Ejemplos:
        *   Modifica temporalmente el `EC2_SSH_KEY` en los secretos de GitHub para que la autenticación falle.
        *   Introduce un error en el script de `Execute deployment commands on EC2` (ej. un comando que falle o intente acceder a un directorio incorrecto).
        *   Detén el servicio PM2 manualmente en EC2 y modifica el script para que un `pm2 reload` falle y el `pm2 start` también (ej. ruta incorrecta al script de inicio).
    2.  Asegúrate de que los jobs `backend-tests` y `backend-build` pasen.
    3.  Haz commit y push.
    4.  **Verificación:**
        *   El job `deploy-backend` debería iniciarse pero luego fallar.
        *   Inspecciona los logs del paso fallido en GitHub Actions. Te dará pistas sobre por qué falló (ej. error de conexión SSH, error en el script ejecutado en EC2).
        *   Si el fallo ocurrió durante la ejecución de comandos en EC2, puede que necesites revisar logs en la propia instancia EC2 (ej. `/var/log/auth.log` para problemas de SSH, logs de PM2, logs de la aplicación).

6.  **Troubleshooting Común del Despliegue:**
    *   **Conexión SSH:**
        *   Verifica que la IP de la instancia EC2 (`EC2_HOST`) sea correcta y esté accesible.
        *   Asegúrate de que el `EC2_USER` sea el correcto.
        *   La `EC2_SSH_KEY` debe ser la clave privada correcta, sin passphrase (o manejar la passphrase si es necesario, aunque es más complejo en CI).
        *   El Security Group de la instancia EC2 debe permitir conexiones SSH (puerto 22) desde las IPs de los runners de GitHub Actions. Si usas OIDC para `configure-aws-credentials`, la conexión SSH sigue siendo directa desde el runner. Puede ser útil restringir el origen de SSH a un rango de IPs conocido si es posible, o usar un bastion host.
    *   **Permisos en EC2:**
        *   El `EC2_USER` debe tener permisos para crear directorios en `/srv/my-app-backend/` (o el directorio que hayas elegido), escribir en ellos, y ejecutar comandos como `sudo ln` y `pm2`. Considera configurar `sudo` sin contraseña para comandos específicos si es necesario y seguro en tu contexto.
    *   **Logs de la Aplicación en EC2:**
        *   Si la aplicación no inicia, revisa sus logs (gestionados por PM2: `pm2 logs ${{ secrets.PM2_BACKEND_APP_NAME }}`).
    *   **Variables de Entorno en EC2:**
        *   Asegúrate de que la aplicación en EC2 tenga acceso a todas las variables de entorno necesarias (ej. `DATABASE_URL` para producción, claves de API, etc.). Estas deben configurarse de forma segura en la instancia (ej. mediante archivos `.env` gestionados por PM2, o variables de entorno del sistema).
    *   **Security Groups de AWS:**
        *   Además del puerto 22 para SSH, el Security Group de la EC2 debe permitir tráfico entrante en el puerto en el que corre tu aplicación backend (ej. 3010) desde los orígenes necesarios (ej. tu IP para pruebas, o IPs del frontend si es una API pública).

## 5. Verificación de la Gestión de Secretos

Es crucial que los secretos no se expongan en los logs.

**Pasos para verificar:**

1.  **Revisión de Logs:**
    *   Inspecciona detenidamente los logs de todos los jobs y pasos en GitHub Actions.
    *   **Verificación:**
        *   En ningún lugar de los logs deberían aparecer los valores literales de tus secretos (ej. `AWS_ACCOUNT_ID`, `AWS_IAM_ROLE_NAME`, `AWS_REGION`, `EC2_HOST`, `EC2_USER`, `EC2_SSH_KEY`, `POSTGRES_PASSWORD`, `PM2_BACKEND_APP_NAME`). GitHub Actions intenta enmascarar automáticamente los secretos en los logs, pero es buena práctica confirmarlo.
        *   En el paso `Configure AWS Credentials (OIDC)`, busca mensajes que indiquen que el rol fue asumido exitosamente. No mostrará la clave, pero sí el éxito de la operación.
        *   En los pasos que usan `appleboy/ssh-action` o `appleboy/scp-action`, si la conexión es exitosa, es una indicación de que la clave SSH y el host/usuario se usaron correctamente. Si fallan por autenticación, te daría una pista.

2.  **Operaciones Exitosas:**
    *   **Verificación:** La mejor forma de verificar que los secretos se usan correctamente es confirmar que las operaciones que dependen de ellos tienen éxito:
        *   El job `backend-tests` se conecta a la base de datos PostgreSQL usando `secrets.POSTGRES_USER`, `secrets.POSTGRES_PASSWORD`, `secrets.POSTGRES_DB_TESTS`.
        *   El job `deploy-backend` puede conectarse a EC2 usando `secrets.EC2_HOST`, `secrets.EC2_USER`, `secrets.EC2_SSH_KEY`.
        *   El paso `Configure AWS Credentials (OIDC)` asume el rol `secrets.AWS_IAM_ROLE_NAME` en la cuenta `secrets.AWS_ACCOUNT_ID` y región `secrets.AWS_REGION`.

## 6. Logs y Artefactos del Workflow

**Pasos para verificar:**

1.  **Acceso a Logs:**
    *   Ve a la pestaña "Actions" en tu repositorio de GitHub.
    *   Haz clic en el workflow run específico que quieres inspeccionar.
    *   En el panel izquierdo, puedes ver la lista de jobs. Haz clic en un job para ver sus pasos.
    *   Cada paso puede expandirse para ver su log detallado.
    *   También hay una opción para "View raw logs" y "Download logs" para un análisis más profundo si es necesario.

2.  **Descarga de Artefactos:**
    *   En la página de resumen del workflow run (no dentro de un job específico), desplázate hacia abajo hasta la sección "Artifacts".
    *   **Verificación:** Deberías ver el artefacto `backend-dist-artifact` (si el job `backend-build` fue exitoso).
    *   Haz clic en el nombre del artefacto para descargarlo. Es un archivo ZIP.

## 7. Flujo General de la Pipeline y Dependencias

**Pasos para verificar:**

1.  **Visualización del Grafo:**
    *   En la página del workflow run, observa el grafo visual de los jobs.
    *   **Verificación:**
        *   Deberías ver que `backend-build` tiene una flecha que lo conecta desde `backend-tests`, indicando la dependencia (`needs: backend-tests`).
        *   Deberías ver que `deploy-backend` tiene una flecha que lo conecta desde `backend-build`, indicando la dependencia (`needs: backend-build`).

2.  **Comportamiento de `needs`:**
    *   Si `backend-tests` falla, verifica que `backend-build` y `deploy-backend` no se ejecuten y se marquen como "Skipped".
    *   Si `backend-tests` pasa pero `backend-build` falla, verifica que `deploy-backend` no se ejecute y se marque como "Skipped".

Siguiendo esta guía, deberías poder confirmar de manera exhaustiva que tu pipeline CI/CD está funcionando correctamente, identificar problemas y asegurar que tus despliegues sean confiables. 

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