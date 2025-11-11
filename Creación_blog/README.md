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

**[ENLACE AL CÓDIGO FUENTE]**
* El flujo completo de n8n se encuentra en el archivo `workflow.json` de este repositorio.
* La guía de estilo usada por la IA se encuentra en `guia_de_estilo.txt`.

### Flujo de Trabajo Visual

**[INSERTA AQUÍ LA CAPTURA DE PANTALLA COMPLETA DE TU FLUJO DE N8N DEL PROYECTO 2]**

### Pasos del Flujo:

1.  **Disparador (RSS):** Múltiples nodos `RSS Feed Trigger` monitorean (ej. cada día) varios blogs de la industria (TechCrunch, MIT, etc.).
2.  **Filtro (IF):** Un nodo `IF` descarta artículos antiguos, procesando solo los publicados en las últimas 24 horas.
3.  **Extracción (HTTP):** Un nodo `HTTP Request` descarga el HTML completo del artículo (configurado como `Response Format: Text`).
4.  **Límite (Items Lists):** Un nodo `Items Lists` (en modo `Limit`) asegura que solo se procese un número manejable de artículos (ej. los 3 primeros) para evitar errores de límite de tokens.
5.  **Extracción (IA 1 - El Limpiador):** Un nodo `OpenAI` recibe el HTML crudo y tiene un prompt específico para **extraer solo el texto limpio** del artículo, ignorando anuncios, menús y código.
6.  **Resumen (IA 2 - El Resumidor):** Un segundo nodo `OpenAI` recibe el texto limpio (que puede ser muy largo) y tiene un prompt para **crear un resumen objetivo** y denso.
7.  **Contexto (RAG - Read File):** Un nodo `Read File` lee el archivo `guia_de_estilo.txt` (que contiene el tono de voz de "RoAiAutomatizaciones").
8.  **Redacción (IA 3 - El Escritor RAG):** El nodo `OpenAI` principal recibe el **Resumen** (de la IA 2) y la **Guía de Estilo** (del Read File). Su prompt (RAG) le instruye redactar un borrador de 500 palabras.
9.  **Guardado (Google Docs):** Se usan dos nodos `Google Docs`:
    * `Create document` crea un archivo en blanco en Google Drive con el título del artículo.
    * `Append content` añade el borrador de la IA al documento recién creado.
10. **Notificación (Telegram):** Un nodo `Telegram` envía un mensaje a un editor con un enlace directo al nuevo Google Doc, listo para revisar.

---

## 3. Herramientas Utilizadas

* **Orquestación:** n8n
* **Fuentes de Datos:** RSS Feed Reader, HTTP Request, HTML Extract (con IA)
* **IA (RAG y Redacción):** OpenAI API (usando una cadena de 3 LLMs)
* **Gestión de Contenido:** Google Docs API, Google Drive API
* **Notificaciones:** Telegram API

---

## 4. Resultados Obtenidos

El flujo se ejecutó con éxito. Detectó un nuevo artículo en un feed, lo procesó a través de la cadena de IA y generó un borrador de alta calidad en la carpeta de Google Drive designada.

**Resultado: Borrador de Artículo Generado en Google Docs**

**[INSERTA AQUÍ LA CAPTURA DE PANTALLA DEL DOCUMENTO FINAL CREADO EN GOOGLE DOCS]**

**Resultado: Notificación de Revisión en Telegram**

**[INSERTA AQUÍ LA CAPTALLA DEL MENSAJE DE TELEGRAM CON EL ENLACE CONSTRUIDO: `https://docs.google.com/document/d/...`]**
