# N8N Automation Hub

Este repositorio contiene la infraestructura como código necesaria para desplegar una instancia de [n8n](https://n8n.io/) (automatización de flujos de trabajo) en la nube utilizando [Render](https://render.com/).

## 🚀 Despliegue en Render (Producción)

El archivo principal para despliegue es `render.yaml`. Este **Blueprint** aprovisiona dos servicios en la capa gratuita (Free Tier) de Render:
1. Una base de datos **PostgreSQL** para almacenar credenciales y configuraciones encriptadas.
2. Un servicio web **Docker** que ejecuta la última versión del contenedor oficial de n8n.

### Pasos para desplegar:
1. Asegúrate de que este repositorio esté subido a tu cuenta de GitHub.
2. Ingresa a tu panel de **[Render](https://dashboard.render.com/)**, haz clic en **"New +"** y selecciona **"Blueprint"**.
3. Conecta este repositorio (`n8n-automation-hub`).
4. Render leerá el archivo `render.yaml` y te pedirá que completes dos variables de entorno seguras en el menú web:
   - `N8N_BASIC_AUTH_USER`: Tu usuario de administrador.
   - `N8N_BASIC_AUTH_PASSWORD`: Tu contraseña segura.
5. Haz clic en "Apply" y espera aproximadamente 5 minutos a que finalice el despliegue.

⚠️ **Limitaciones de la Capa Gratuita (Render Free Tier):**
- La base de datos PostgreSQL gratuita se **eliminará automáticamente a los 90 días**. Se recomienda actualizar el plan de la BD a "Starter" ($7/mes) antes de ese plazo para no perder los datos.
- El servicio web **se dormirá tras 15 minutos de inactividad**. Esto significa que los flujos programados por tiempo (nodos Cron) pueden no ejecutarse. Los Webhooks funcionarán pero tendrán una demora de ~30 segundos en el primer llamado mientras el servidor se "despierta".

## 💻 Entorno Local (Desarrollo)

Para hacer pruebas en tu computadora antes de subir cambios:

```bash
# Iniciar n8n en el puerto 5678
docker-compose up -d
```

> **Nota de Seguridad:** Las bases de datos locales (`.n8n_data_local` y `.flowise_data_local`) y los archivos `.env` están ignorados en el `.gitignore` por defecto para evitar subir secretos o APIs a GitHub. Nunca modifiques esta regla.

## 📁 Estructura del Repositorio

- `render.yaml`: Configuración de infraestructura para la nube (Producción).
- `docker-compose.yml`: Archivo para levantar el entorno puramente en local (Desarrollo).
- `workflows/`: Carpeta recomendada para almacenar copias de seguridad estáticas de los flujos de n8n exportados en formato `.json`.
