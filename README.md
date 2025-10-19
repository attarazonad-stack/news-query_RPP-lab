# Sistema de Recuperación de Noticias - RPP Perú

Este proyecto implementa un flujo completo de procesamiento de lenguaje natural (PLN) para recuperar y consultar noticias del portal RPP Perú utilizando su fuente RSS. Se emplean incrustaciones semánticas, almacenamiento vectorial y consultas por similitud.

---

## Descripción del Proyecto

El objetivo de este sistema es:

- Extraer automáticamente las últimas noticias desde el RSS de RPP.
- Tokenizar los textos para verificar si cumplen con los límites de contexto de los modelos de lenguaje.
- Generar incrustaciones utilizando **SentenceTransformers**.
- Almacenar incrustaciones y metadatos en una base vectorial (**ChromaDB**).
- Permitir consultas como: *"noticias recientes de economía"* y devolver los resultados más relevantes.

---

## Flujo de Trabajo

### 1. Carga de Datos (RSS Feed)

- Se utiliza `feedparser` para leer las noticias desde:  
  `https://rpp.pe/rss`
- De cada noticia se extraen los campos:
  - `title`
  - `description`
  - `link`
  - `published`

### 2. Tokenización

- Se usa `tiktoken` para contar la cantidad de tokens en el texto.
- Si un artículo supera **1024 tokens**, se considera dividirlo en fragmentos para evitar problemas con los modelos de lenguaje.

### 3. Generación de Incrustaciones (Embeddings)

- Modelo utilizado:  
  `sentence-transformers/all-MiniLM-L6-v2`
- Cada descripción se convierte en un vector numérico.
- Estos vectores representan el significado semántico del texto.

### 4. Almacenamiento en ChromaDB

- Se crea o conecta una colección llamada **rpp_news**.
- Se almacenan:
  - Descripciones de noticias (`documents`)
  - Incrustaciones (`embeddings`)
  - Metadatos (`title`, `link`, `date_published`)

### 5. Consulta de Noticias

- Se permite buscar noticias mediante texto libre, por ejemplo:  
  `"Últimas noticias de economía"`
- Se recuperan los documentos más similares usando búsqueda vectorial.
- Los resultados se muestran en un DataFrame con columnas:
  - `title | description | link | date_published`

---



Los resultados se muestran en un DataFrame (pandas) con columnas:
title | description | link | date_published
