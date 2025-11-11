# Proyecto 2: Generador de Artículos de Blog con IA (RAG)

Este repositorio contiene un flujo de trabajo de n8n que automatiza la creación de contenido. El sistema monitorea noticias de la industria, extrae el contenido, lo resume, y utiliza un modelo de IA (con RAG) para redactar un borrador de artículo de blog alineado con un tono de voz específico.

---

## 1. Descripción del Problema

Para una estrategia de marketing de contenidos, es vital publicar artículos de blog que reaccionen a las noticias y tendencias de la industria. Este proceso manual es lento:
1.  Requiere monitoreo constante de múltiples fuentes.
2.  Extraer la información clave es tedioso.
3.  Redactar un borrador original consume mucho tiempo.
4.  Asegurar que el borrador mantenga el "tono de voz" de la marca requiere revisiones exhaustivas.

## 2. El Flujo de Trabajo (La Solución)

Diseñé un flujo de trabajo de IA avanzada que automatiza todo este proceso, entregando un borrador de alta calidad, listo para revisión humana, directamente en Google Docs.

* El flujo completo de n8n se encuentra en el archivo `workflow.json` de este repositorio.


### Flujo de Trabajo Visual

![flujo](https://github.com/user-attachments/assets/9f006f72-1c9a-49db-841c-094ac121c876)


### Pasos del Flujo:

1.  **Disparador (RSS):** Múltiples nodos `RSS Feed Trigger` monitorean (ej. cada día) varios blogs de la industria (TechCrunch, MIT, etc.).
2.  **Filtro (IF):** Un nodo `IF` descarta artículos antiguos, procesando solo los publicados en las últimas 24 horas.  
3.  **Límite (Items Lists):** Un nodo `Items Lists` (en modo `Limit`) asegura que solo se procese un número manejable de artículos (ej. los 3 primeros) para evitar errores de límite de tokens.
4.  **Extracción (HTTP):** Un nodo `HTTP Request` descarga el HTML completo del artículo y convertido a Markdown para una mayor comprensión por parte de los LLM,s.
5.  **Resumen (IA 1 - El Resumidor):** Un segundo nodo `OpenAI` recibe el texto limpio (que puede ser muy largo) y tiene un prompt para **crear un resumen objetivo** y denso.
6.  **Redacción (IA 2 - El Escritor RAG):** El nodo `OpenAI` principal recibe el **Resumen** (de la IA 2) y la **Guía de Estilo** (del Read File). Su prompt (RAG) le instruye redactar un borrador de 100 palabras.
7.  **Guardado (Google Docs):** Se usan dos nodos `Google Docs`:
    * `Create document` crea un archivo en blanco en Google Drive con el título del artículo.
    * `Append content` añade el borrador de la IA al documento recién creado.
8. **Notificación (Telegram):** Un nodo `Telegram` envía un mensaje a un editor con un enlace directo al nuevo Google Doc, listo para revisar.

---

## 3. Herramientas Utilizadas

* **Orquestación:** n8n
* **Fuentes de Datos:** RSS Feed Reader, HTTP Request, MARKDOWN nodo (con IA)
* **IA (RAG y Redacción):** OpenAI API (usando una cadena de 2 LLMs)
* **Gestión de Contenido:** Google Docs API, Google Drive API
* **Notificaciones:** Telegram API

---

## 4. Resultados Obtenidos

El flujo se ejecutó con éxito. Detectó un nuevo artículo en un feed, lo procesó a través de la cadena de IA y generó un borrador de alta calidad en la carpeta de Google Drive designada.

**Resultado: Borrador de Artículo Generado en Google Docs**
![articulo-1](https://github.com/user-attachments/assets/fff91023-bab9-4d0c-a636-3c05cf92a69a)
![articulo-2](https://github.com/user-attachments/assets/9c814629-b556-4e71-ab1c-e15e9cf11a8b)
![articulo-3](https://github.com/user-attachments/assets/2c88aa50-7e26-4c9d-88dc-0f398e2cd736)


**Resultado: Notificación de Revisión en Telegram**

![Screenshot_2025_11_11-27](https://github.com/user-attachments/assets/a1656674-d433-4d2d-9611-17fcf4805389)

