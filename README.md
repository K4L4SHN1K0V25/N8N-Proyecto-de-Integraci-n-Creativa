# Clasificador Inteligente de Emails con N8N y Gemini

Este proyecto es una solución de automatización que utiliza **N8N** y la inteligencia artificial de **Google Gemini** para clasificar automáticamente correos electrónicos de Gmail. El flujo de trabajo está diseñado específicamente para identificar alertas de empleo de LinkedIn que contengan una palabra clave y moverlas a una carpeta designada, limpiando así la bandeja de entrada.

<img width="1303" height="448" alt="Captura de pantalla 2025-10-20 191609" src="https://github.com/user-attachments/assets/f34a0c2e-598b-41cf-9c4e-1c01651f1810" />

---

## 🎯 Características Principales

* **Automatización Inteligente:** Utiliza un disparador en N8N que se activa con cada nuevo correo en Gmail.
* **Clasificación con IA:** Envía el remitente, asunto y cuerpo del correo a Google Gemini para un análisis preciso.
* **Lógica Condicional:** Usa un prompt específico para que Gemini devuelva una única palabra (`linkedin` u `otro`) basada en reglas estrictas.
* **Organización Automática:** Mueve los correos identificados a una carpeta/etiqueta específica de Gmail y los elimina de la bandeja de entrada.
* **Fácil Despliegue:** Se puede ejecutar localmente usando Docker, asegurando un entorno consistente.

---

## 🛠️ Tecnologías Utilizadas

* **[N8N](https://n8n.io/):** Plataforma de automatización de flujos de trabajo (Workflow Automation).
* **[Google Gemini](https://gemini.google.com/):** Modelo de lenguaje grande (LLM) para la clasificación de texto.
* **[Docker](https'://www.docker.com/):** Para ejecutar N8N en un contenedor local.
* **[API de Gmail](https://developers.google.com/gmail/api):** Para leer y gestionar correos electrónicos.

---

## 📋 Prerrequisitos

Antes de comenzar, necesitarás:

1.  **Docker** y **Docker Compose** instalados en tu máquina.
2.  Una **cuenta de Google** (Gmail).
3.  Una **clave de API de Google Gemini**. Puedes obtenerla en [Google AI Studio](https://aistudio.google.com/app/apikey).
4.  Credenciales **OAuth 2.0** de Google Cloud para la API de Gmail.

---

## 🚀 Guía de Instalación y Configuración

Sigue estos pasos para poner en marcha el flujo de trabajo.

### 1. Clonar o Descargar el Repositorio

Primero, obtén los archivos de este repositorio, que incluyen:

* `workflow.json` (el flujo de N8N)
* `docker-compose.yml` (para iniciar N8N)
* `README.md` (este archivo)

* Esto iniciará una instancia de N8N. Puedes acceder a ella en tu navegador visitando `http://localhost:5678`.

<img width="1361" height="681" alt="Captura de pantalla 2025-10-20 171557" src="https://github.com/user-attachments/assets/a684050d-eaff-4d8e-90de-6d4d8919d2a8" />

### 3. Configurar las Credenciales en N8N
Necesitarás configurar dos tipos de credenciales.

#### a) Credencial de Google Gemini
* En el menú de N8N, ve a **Credentials > Add credential**.
* Busca y selecciona **Google Gemini (PaLM) API**.
* Pega la clave de API que obtuviste de Google AI Studio.

![Configuración de Credencial de Gemini](URL_DE_TU_IMAGEN_DE_GEMINI_CREDENTIALS)

#### b) Credencial de Gmail (OAuth 2.0)
Este es el paso más complejo. Deberás crear credenciales OAuth en la [Consola de Google Cloud](https://console.cloud.google.com/).

* Crea un nuevo proyecto, habilita la **API de Gmail** y configura la **Pantalla de Consentimiento de OAuth**.
* Asegúrate de añadir tu correo como **Usuario de Prueba**.
* Crea un **ID de cliente de OAuth** de tipo "Aplicación web".
* Usa el **URI de redireccionamiento autorizado** que te proporciona N8N: `http://localhost:5678/rest/oauth2-credential/callback`.
* Copia el **Client ID** y el **Client Secret** en la configuración de la credencial de Gmail en N8N.

> Si recibes este error, significa que olvidaste añadir tu correo a la lista de "Usuarios de Prueba" en la Pantalla de Consentimiento de Google Cloud.

![Error de Acceso Denegado 403](URL_DE_TU_IMAGEN_DEL_ERROR_403)

### 4. Importar el Flujo de Trabajo
* En la pantalla principal de N8N, ve a **Workflows > Import from File**.
* Selecciona el archivo `workflow.json` que descargaste.

### 5. Activar el Flujo
* Una vez importado, revisa que cada nodo tenga las credenciales correctas seleccionadas.
* Haz clic en el interruptor en la esquina superior izquierda para poner el flujo en modo **"Active"**.

![Activación del Flujo de Trabajo](URL_DE_TU_IMAGEN_DE_ACTIVACION_DEL_FLUJO)

---

## 💡 Uso
Una vez activo, el flujo de trabajo se ejecutará en segundo plano. Cada vez que recibas un nuevo correo, se disparará el proceso:

1.  **Gemini** analizará si el correo es de `jobalerts-noreply@linkedin.com` y contiene "Ingeniero de software".
2.  Si ambas condiciones son verdaderas, el correo será movido a la carpeta/etiqueta **"linkedin"** y se eliminará de tu bandeja de entrada.
3.  Si no, el flujo terminará y el correo permanecerá en tu bandeja de entrada.

---

## 📚 Recursos y Referencias
* **Documentación de N8N:** [Instalación con Docker](https://docs.n8n.io/hosting/installation/docker/)
* **Video Tutorial:** [N8N Beginner Crash Course](https://www.youtube.com/watch?v=3VqVf0N0bcs)
* **Google Gemini:** [Página Oficial de Gemini](https://gemini.google.com/)

### 2. Iniciar N8N con Docker

En tu terminal, navega a la carpeta del proyecto y ejecuta:

```bash
docker-compose up -d
