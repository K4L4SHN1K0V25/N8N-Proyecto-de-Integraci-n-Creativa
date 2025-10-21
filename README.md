# Clasificador Inteligente de Emails con N8N y Gemini

Este proyecto es una solución de automatización que utiliza **N8N** y la inteligencia artificial de **Google Gemini** para clasificar automáticamente correos electrónicos de Gmail. El flujo de trabajo está diseñado específicamente para identificar alertas de empleo de LinkedIn que contengan una palabra clave y moverlas a una carpeta designada, limpiando así la bandeja de entrada.

![Flujo de Trabajo Completo N8N](URL_DE_TU_IMAGEN_DEL_FLUJO_COMPLETO)

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

### 2. Iniciar N8N con Docker

En tu terminal, navega a la carpeta del proyecto y ejecuta:

```bash
docker-compose up -d
