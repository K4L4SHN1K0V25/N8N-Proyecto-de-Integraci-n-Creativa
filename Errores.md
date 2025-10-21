# Historial de Errores y Soluciones del Proyecto

Este documento detalla los problemas encontrados durante la configuración del flujo de trabajo de N8N y Google Gemini, sus causas y las soluciones aplicadas.

---

## 1. Error de Conexión a N8N en `localhost`

* **Problema:** Después de iniciar el contenedor de N8N con Docker, no se podía acceder a la interfaz en `http://localhost:5678`.
* **Causa:** El puerto `5678` del contenedor no estaba mapeado al puerto de la máquina local. El log mostraba que N8N funcionaba, pero era inaccesible desde el exterior.
* **Solución:** Se detuvo el contenedor y se volvió a lanzar utilizando el parámetro `-p 5678:5678` en el comando `docker run` para mapear correctamente los puertos.

---

## 2. Error de Permisos de Gmail: `Error 403: access_denied`

* **Problema:** Al intentar conectar la cuenta de Gmail a N8N, Google bloqueaba el acceso con un error "access_denied".
* **Causa:** La aplicación creada en la Consola de Google Cloud estaba en "modo de prueba", y el correo electrónico del usuario no había sido añadido a la lista explícita de "Usuarios de Prueba" autorizados.
* **Solución:** Se navegó a la **Pantalla de Consentimiento de OAuth** en la Consola de Google Cloud y se añadió la dirección de correo del usuario a la sección **"Usuarios de Prueba"**.

---

## 3. Error en el Nodo de Gmail: `Forbidden - Gmail API has not been enabled`

* **Problema:** El nodo `Gmail Trigger` fallaba con un error "Forbidden", aunque las credenciales OAuth 2.0 ya estaban configuradas y conectadas.
* **Causa:** La **API de Gmail** no estaba activada para el proyecto en la Consola de Google Cloud. Las credenciales existían, pero no tenían permiso para usar esa API en específico.
* **Solución:** Se utilizó el enlace proporcionado en el mismo mensaje de error para ir directamente a la página de la API de Gmail y hacer clic en el botón **"Habilitar"**.

---

## 4. Error del Modelo Gemini: `Service unavailable`

* **Problema:** El nodo de Google Gemini devolvía un error indicando que el servicio no estaba disponible.
* **Causa:** Este es un error temporal del lado del servidor de Google, usualmente por sobrecarga o por exceder los límites de la cuota gratuita (`rate limiting`). No era un error de configuración.
* **Solución:**
    1.  Se esperó unos minutos y se reintentó la ejecución, lo cual solucionó el problema.
    2.  Como buena práctica, se configuró el nodo para que reintente automáticamente en caso de fallo, activando la opción **"Retry on Fail"** en la pestaña "Settings" del nodo.

---

## 5. La IA no Recibía el Contenido del Email

* **Problema:** El modelo Gemini respondía "Por favor, proporciona el contenido del email para que pueda clasificarlo", en lugar de dar una clasificación.
* **Causa:** La expresión usada en el prompt, `{{ $json.body }}`, era incorrecta porque los datos del nodo `Gmail Trigger` no contenían un campo llamado `body` en el nivel principal.
* **Solución:** Se corrigió la expresión para usar un campo que sí existía en los datos de entrada: `{{ $json.snippet }}`, que contiene un resumen del correo.
