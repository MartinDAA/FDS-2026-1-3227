# Tendencias de Videos de Youtube

## Proyecto de Fundamentos de Data Science - FDS-2026-1-3227

### Objetivo del Proyecto

El objetivo del proyecto es analizar el dataset de videos que son tendencia en el país de Alemania donde nos encargaremos de organizar y limpiar los datos que sean necesarios para resolver el caso de negocio que planteamos.
### Objetivos de Data Science
 - La meta de nuestro análisis es visualizar de manera gráfica la cantidad de visitas que tiene los videos de cada estado de Alemania.
 - Crear un modelo que pueda categorizar el número de visitas que tiene cada estado tomando en cuenta el número de vistas del video y del lugar donde se publicó, categorizar los videos que tiene más "me gustas" y las que tienen más "no me gustas", etc.


### Integrantes

- Martin Alonso del Águila Arévalo (Data Scientist) - u202014659
- Sergio Andres Saavedra Cervera (Data Engineer) - u202311021
- David Angelo Zavala Arteaga (Business Project Sponsor) - u202318335


### Descripción del Dataset

#### Estructura de variables del dataset original

* `video_id` (string): Conjunto de letras que cada video de Youtube tiene
* `trending_date`: Fecha en el que el video consiguió una mayor cantidad de visitas a comparación de otros días
* `title`: Nombre del video
* `channel_title`: Nombre del canal de la persona que publica los videos
* `category_id`: Categoría a la que pertenece el video(Ejemplo: 24,27,23...)
* `publish_time`: Fecha y hora en la que se publicó el video
* `tags`: Lista de tags del video, separado por una barra (‘|’)
* `views`: Número de visita que tiene el video
* `likes`:  Número de me gusta que tiene el video
* `dislikes`: Número de no me gusta que tiene el video
* `comment_count`: Número de comentarios que tiene el video
* `thumbnail_link`: Link de la imagen que contiene el video
* `comments_disabled`: Saber si se puede o no se puede comentar en el video(TRUE,FALSE)
* `ratings_disabled`: Saber si el contador de me gusta o no me gusta es visible(TRUE= No es visible, FALSE= Si es visible)
* `videos_error_or_removed`: Saber si el video puede ser visible para cualquier persona(TRUE= No se puede visualizar el video, FALSE= Si se puede visualizar el video)
* `description`: Descripción que contiene información sobre lo que trata el video
* `state`: Nombre del Estado donde se publicó el video
* `lat`: Latitud geográfica donde se publicó el video
* `lon`: Longitud geográfica donde se publicó el video
* `geometry`: Registro de coordenadas de las geometrías donde se ubica el Estado dentro del planeta.


### Conclusiones

> Las conclusiones del documento
