# automatizaciones_n8n
Proyectos - clasificador de emails y creación de artículos de blog con IA

# Proyecto 1: Clasificador Inteligente de Emails con IA (n8n)

## Descripción del Problema

En una empresa en crecimiento, el buzón de soporte (ej. `soporte@...`) recibe un alto volumen de correos de naturaleza muy diversa: solicitudes de ventas, problemas técnicos, consultas de facturación y propuestas de marketing.

Actualmente, una persona debe leer, clasificar y reenviar manualmente cada correo, lo que genera cuellos de botella, tiempos de respuesta lentos y un alto coste de trabajo repetitivo.

## La Solución: Mi Flujo de Trabajo en n8n

Diseñé e implementé un flujo de trabajo de automatización "end-to-end" que resuelve este problema. El sistema se ejecuta 24/7 y gestiona el 100% del triaje de correos sin intervención humana.

### Flujo de Trabajo Visual

**(Aquí pegas la captura de pantalla de tu flujo completo de n8n)**
![clasificador_emails](https://github.com/user-attachments/assets/4b7c1959-941e-47b6-9311-6540a5ccebe6)


### Pasos del Flujo:

1.  **Disparador (Gmail):** El flujo se activa con cada nuevo correo que llega al buzón de soporte.
2.  **IA (OpenAI):** El contenido del correo se envía a un LLM (gpt-3.5-turbo) con un prompt que le pide clasificar el correo en 5 categorías: `Ventas`, `Soporte Técnico`, `Facturación`, `Marketing` o `Error`.
3.  **Lógica (Switch):** Un nodo `Switch` dirige el flujo basándose en la respuesta de la IA.
4.  **Acciones (Herramientas):**
    * **Ventas** ➡️ **Trello**: Crea una tarjeta en el tablero del equipo comercial.
    * **Soporte Técnico** ➡️ **Jira**: Crea una incidencia (Issue) en el proyecto de soporte.
    * **Marketing** ➡️ **Google Sheets**: Añade el remitente a una hoja de "Leads".
    * **Facturación** ➡️ **Telegram**: Envía una alerta urgente al equipo de finanzas.
    * **Error (Default)** ➡️ **Google Calendar**: Registra el fallo en un calendario para revisión manual.
5.  **Confirmación (Gmail):** Al final, el flujo envía un correo de confirmación al cliente ("Hemos recibido tu consulta...").

---

## Herramientas Utilizadas

* **Orquestación:** n8n
* **IA (Clasificación):** OpenAI API (GPT-3.5-Turbo)
* **Correo:** Gmail API
* **Gestión de Tareas:** Trello, Jira
* **Registro de Datos:** Google Sheets
* **Notificaciones:** Telegram API, Google Calendar

---

## Resultados Obtenidos (Pruebas)

El flujo clasifica y enruta correctamente las tareas a las plataformas designadas.

**Ejemplo 1: Ruta de "Ventas" (Creación en Trello)**
![flujo_ventas](https://github.com/user-attachments/assets/00e07b5d-e5c7-4789-aef1-48b9ba3f9cf1)
![trello](https://github.com/user-attachments/assets/755907f9-82f2-4375-a519-792928db7d67)
![email_respuesta_ventas](https://github.com/user-attachments/assets/7aef199a-a752-4bf6-9259-bb61b4325c64)


**Ejemplo 2: Ruta de "Soporte" (Creación en Jira)**
![flujo_soporte](https://github.com/user-attachments/assets/91318875-3328-41db-97e2-2078ad2a525b)
![model_soporte](https://github.com/user-attachments/assets/565885e6-f606-4e4e-b113-bca7e09450b2)
![jira](https://github.com/user-attachments/assets/828251a8-0e4e-4d76-bd92-26b906a7f2dc)


**Ejemplo 3: Ruta de "Marketing" (Registro en Google Sheets)**
![Marketing](https://github.com/user-attachments/assets/4fcf8c7c-e5cb-44cd-b772-d9c04306f69d)
![model-marketing](https://github.com/user-attachments/assets/8f11b642-4323-4d8f-908e-c74f7b8b0dab)
![sheets-marketing](https://github.com/user-attachments/assets/31cb69c1-b57a-4a76-9635-ded15f9ad552)


**Ejemplo 4: Ruta de "Facturación" (Notificación Telegram)**
![flujo_facturación](https://github.com/user-attachments/assets/6b78076d-00ec-4080-a9a4-46c30a3975a9)
![modelo_facturación](https://github.com/user-attachments/assets/1d9f4213-dc3d-4654-9ba8-fc2c2e7dd158)
![telegram](https://github.com/user-attachments/assets/7646c152-498a-47f9-8958-9cda3d0927c1)


**Ejemplo 5: Ruta de "Errores" (Evento en Google Calendar)**
![flujo_errores](https://github.com/user-attachments/assets/2ab0ab27-8c32-431e-825b-b56d7a5fbecf)
![modelo_response](https://github.com/user-attachments/assets/b6108eb8-5428-40e3-9385-92b4a4a0d63c)
![calendar](https://github.com/user-attachments/assets/ef1887d1-195c-43c4-9546-5910af04b65e)



---

## Cómo Replicar este Proyecto

El "código fuente" de este flujo de trabajo está incluido en este repositorio.

1.  Descarga el archivo `workflow.json`.
2.  En tu instancia de n8n, ve a `File` -> `Import` -> `From file`.
3.  Sube el archivo `workflow.json`.
4.  Configura tus propias credenciales para cada uno de los nodos (Gmail, OpenAI, Jira, etc.).
![clasificador_emails](https://github.com/user-attachments/assets/14c913cf-ffa7-430e-82f1-36467d0d001d)
