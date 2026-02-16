# Prompts para el Ejercicio Creando un Pipeline en GitHub Actions

> **Nota:** Este ejercicio fue desarrollado utilizando **Antigravity** como asistente de codificación e IDE inteligente.

Lista de prompts utilizados en el desarrollo del ejercicio:

## 1. Configuración de Infraestructura y Seguridad

* **En AWS (EC2):**
    * Lanzar instancia EC2 (Amazon Linux 2 o Ubuntu).
    * Configurar **Security Groups**: Abrir puertos 22 (SSH) y el puerto que use tu backend (ej. 3000, 8080).
    * Descargar el par de claves `.pem`.

* **En GitHub (Secrets):**
    * `AWS_SSH_KEY`: El contenido de tu archivo `.pem`.
    * `EC2_HOST`: La IP pública o DNS de tu instancia.
    * `EC2_USER`: El usuario (ej. `ec2-user` o `ubuntu`).

## 2. Estrategia de Prompts (Documentación)

| Paso del Pipeline | Sugerencia de Prompt para la IA |
| --- | --- |
| **Trigger & Setup** | "Genera un workflow de GitHub Actions que se dispare solo cuando haya un push en una rama que tiene un Pull Request abierto hacia main." |
| **Tests** | "Añade un job de testing para un backend en [Node.js/Python/Java] que ejecute los comandos de instalación y tests." |
| **Build** | "Añade un paso para compilar el proyecto y generar los artefactos necesarios para producción." |
| **Deploy** | "Crea un script para GitHub Actions que use SSH para conectarse a un EC2, haga un git pull del código y reinicie el servicio usando PM2 o Docker." |

## 3. Construcción del Workflow (`pipeline.yml`)

El archivo se ubicará en `.github/workflows/pipeline.yml`.

## 4. Checklist de Entrega Final

* [ ] **`.github/workflows/pipeline.yml`**: Con los 3 pasos (test, build, deploy).
* [ ] **`prompts/prompts-WAOR.md`**: Con el listado de instrucciones que le diste a la IA.
* [ ] **Verificación**: El tab "Actions" en GitHub debe mostrar el check verde ✅ tras abrir un PR.

## 5. Corrección de Errores (Bonus)

Se registraron los siguientes errores de compilación y sus soluciones:

| Incidencia | Prompt Utilizado | Solución Aplicada |
| --- | --- | --- |
| **Errores de Compilación (TypeScript)** | Copiar y pegar el log de error: `src/application/services/positionService.ts:23:33 - error TS7006...` | Se añadieron tipos explícitos `any` en `map` y se corrigió el acceso a `Prisma.PrismaClientInitializationError` casteando a `any`. |

| **Error ejecución Seed (`ts-node`)** | `ts-node : El término 'ts-node' no se reconoce...` | Se añadió configuración `prisma` en `package.json` para definir `seed` usando `ts-node`. Se debe ejecutar con `npx prisma db seed` o usar `npx ts-node`. |

| **Error `Debug Failure` en `ts-node`** | `Error: Debug Failure. False expression...` | Incompatibilidad de versiones. Se actualizó `ts-node` (`npm i -D ts-node@latest`) y se ejecutó `npx prisma migrate reset` para limpiar la BD y correr el seed. |

| **Tests duplicados y fallidos en `dist/`** | `FAIL dist/presentation/controllers/positionController.test.js` | Jest estaba ejecutando los archivos compilados en `dist/` además de los fuentes en `src/`. Se configuró `testPathIgnorePatterns: ['/node_modules/', '/dist/']` en `jest.config.js`. |

| **Error en Deploy: `git: command not found`** | `bash: line 6: git: command not found` | El servidor EC2 no tenía Git instalado. Se solucionó conectándose manualmente y ejecutando `sudo yum install git -y` y clonando el repo por primera vez. |
