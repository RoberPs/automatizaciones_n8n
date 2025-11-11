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
**(Aquí pegas la captura del ticket creado en Jira)**
![Resultado en Jira](enlace_a_tu_captura_de_jira.png)

**Ejemplo 3: Ruta de "Marketing" (Registro en Google Sheets)**
**(Aquí pegas la captura de la fila creada en Google Sheets)**
![Resultado en Google Sheets](enlace_a_tu_captura_de_sheets.png)

---

## Cómo Replicar este Proyecto

El "código fuente" de este flujo de trabajo está incluido en este repositorio.

1.  Descarga el archivo `workflow.json`.
2.  En tu instancia de n8n, ve a `File` -> `Import` -> `From file`.
3.  Sube el archivo `workflow.json`.
4.  Configura tus propias credenciales para cada uno de los nodos (Gmail, OpenAI, Jira, etc.).
![clasificador_emails](https://github.com/user-attachments/assets/14c913cf-ffa7-430e-82f1-36467d0d001d)
