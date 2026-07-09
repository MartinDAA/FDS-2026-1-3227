# Tendencias de Videos de Youtube

# Proyecto de Fundamentos de Data Science - FDS-2026-1-3227

## Objetivo del Proyecto

El presente proyecto tiene como objetivo principal aplicar la metodología **CRISP-DM** para responder a las necesidades de una empresa de marketing digital que busca comprender las dinámicas de viralidad en YouTube para optimizar sus estrategias de contenido y publicidad en el mercado alemán. Los objetivos particulares se dividen en dos:

### Objetivos del negocio

- Identificar las categorías y canales con mayor capacidad de generar tendencia y engagement (likes, comentarios) para orientar la inversión publicitaria.
- Determinar qué variables (frecuencia del canal, etiquetado, longitud del título, categoría) influyen en el éxito de un video en términos de visualizaciones.
- Proporcionar insights accionables que permitan priorizar contenidos, optimizar metadatos y seleccionar socios estratégicos (canales) para campañas futuras.

### Objetivos de Data Science

Para dar respuesta a los objetivos de negocio, se plantean las siguientes metas de minería de datos:

- **Análisis descriptivo y exploratorio**: Caracterizar el comportamiento de los videos en tendencia mediante visualizaciones y estadísticos que respondan a las preguntas del cliente (categorías más frecuentes, evolución temporal, canales más recurrentes, distribución geográfica, relación entre tendencia y aprobación).

- **Modelado predictivo**: Construir un modelo de Machine Learning (**Regresión Lineal Múltiple**) que estime el número de visualizaciones (`views`) de un video en función de variables disponibles al momento de la publicación. La variable dependiente es `views` (transformada a `log_views` para estabilizar la varianza). Las variables independientes principales seleccionadas son:
  - `channel_frequency` (frecuencia histórica del canal en tendencia)
  - `category_id` (categoría del video)
  - `num_tags` (número de etiquetas)
  - `title_length` (longitud del título)

  Una explicación más a detalle de estas variables las verás más adelante en este `README` y la justificación de su elección las puedes ver en el [código del modelo](code/model.ipynb)

El modelo permitirá evaluar la factibilidad de la predicción y cuantificar el impacto de cada variable en el rendimiento esperado de un video.


## Integrantes

- Martin Alonso del Águila Arévalo (Data Scientist)
- Sergio Andres Saavedra Cervera (Data Engineer)
- David Angelo Zavala Arteaga (Business Project Sponsor)


## Descripción del Dataset

### Variables del Dataset original

* `video_id` (string): Conjunto de caractéres alfanuméricos que identifica de forma única a cada video de YouTube.
* `trending_date` (fecha): Fecha en la que el video apareció en la lista de tendencias de YouTube.
* `title` (string): Título del video.
* `channel_title` (string): Nombre del canal que publicó el video.
* `category_id` (categórica): Identificador numérico de la categoría a la que pertenece el video (ej. 24, 27, 23). Requiere mapeo con el archivo JSON para obtener el nombre legible.
* `publish_time` (fecha/hora): Fecha y hora exacta en la que se publicó el video en la plataforma.
* `tags` (string): Lista de etiquetas asociadas al video, separadas por el carácter ‘|’.
* `views` (numérica entera): Número total de visualizaciones que ha acumulado el video.
* `likes` (numérica entera): Número total de "Me gusta" que ha recibido el video.
* `dislikes` (numérica entera): Número total de "No me gusta" que ha recibido el video.
* `comment_count` (numérica entera): Número total de comentarios que tiene el video.
* `thumbnail_link` (string): URL de la imagen de miniatura (thumbnail) del video.
* `comments_disabled` (booleana): Indica si la sección de comentarios está deshabilitada para el video (TRUE = deshabilitado, FALSE = habilitado).
* `ratings_disabled` (booleana): Indica si el contador de "Me gusta/No me gusta" está oculto (TRUE = no visible, FALSE = visible).
* `videos_error_or_removed` (booleana): Indica si el video ha sido eliminado o presenta un error que impide su visualización (TRUE = no visible, FALSE = visible).
* `description` (string): Descripción del video, que suele contener información sobre su contenido, enlaces y menciones.

### Variables geográficas

Estas son variables que fueron añadidas al dataset original por la coordinación del curso por motivos didácticos 

* `state` (categórica): Nombre del estado federado alemán asignado aleatoriamente al registro según la modificación del dataset.
* `lat` (numérica continua): Latitud geográfica correspondiente a la ubicación del estado.
* `lon` (numérica continua): Longitud geográfica correspondiente a la ubicación del estado.
* `geometry` (string/geometría): Registro de coordenadas (formato POINT) de la geometría donde se ubica el estado dentro del planeta, utilizable con librerías como GeoPandas.

### Variables derivadas creadas durante el proyecto

#### Variables para análisis de negocio y EDA

* `likes/dislikes` (numérica continua): Ratio calculado como `likes / dislikes`. Utilizado para responder la Q3 (categorías con mejor proporción de "Me gusta" / "No me gusta"). Los valores infinitos (división por cero) fueron reemplazados por `NaN`.
* `tasa_aprobacion` (numérica continua, 0-1): Proporción de reacciones positivas sobre el total, calculada como `likes / (likes + dislikes)`. Utilizada como proxy de "comentarios positivos" para la Q8. Valores entre 0 y 1, donde 1 indica aprobación total.
* `views/comments` (numérica continua): Ratio calculado como `views / comment_count`. Utilizado para responder la Q4 (categorías con mejor proporción de vistas/comentarios). Los valores infinitos (división por cero) fueron reemplazados por `NaN`.

#### Variables para feature engineering y modelo predictivo

* `log_views` (numérica continua): Transformación logarítmica natural de la variable `views`, calculada como `log1p(views)`. Variable dependiente (target) utilizada en el modelo de regresión lineal para estabilizar la varianza y normalizar la distribución de vistas.
* `days_to_trend` (numérica entera): Días transcurridos entre la fecha de publicación (`publish_time`) y la fecha de entrada en tendencia (`trending_date`). Calculada como `(trending_date - publish_time).dt.days`. **Descartada del modelo final por target leakage**, aunque mostró alta correlación con `views`.
* `num_tags` (numérica entera): Número de etiquetas (tags) asociadas al video. Calculada como el conteo de elementos separados por el carácter `|` en la columna `tags`. Utilizada como variable independiente en el modelo.
* `title_length` (numérica entera): Longitud en caracteres del título del video (`title`). Utilizada como variable independiente en el modelo.
* `desc_length` (numérica entera): Longitud en caracteres de la descripción del video (`description`). Utilizada como variable independiente opcional en el modelo.
* `hour_published` (numérica entera, 0-23): Hora del día en que se publicó el video, extraída de `publish_time`. Utilizada como variable independiente exploratoria.
* `weekday_published` (numérica entera, 0-6): Día de la semana en que se publicó el video (0 = Lunes, 6 = Domingo), extraída de `publish_time`. Utilizada como variable independiente exploratoria.
* `is_top_10` (binaria, 0/1): Variable indicadora que toma el valor 1 si el canal pertenece al Top 10 de canales con mayor frecuencia en tendencia, y 0 en caso contrario. Utilizada como proxy de la importancia del canal.
* `channel_frequency` (numérica entera): Frecuencia absoluta de aparición del canal en la lista de tendencias a lo largo de todo el dataset. Calculada como el número total de veces que el `channel_title` aparece en el dataset. Utilizada como variable independiente en el modelo, sustituyendo a `is_top_10` por su mayor poder predictivo.
* `category_name` (categórica): Nombre legible de la categoría del video, obtenida mediante el mapeo de `category_id` con el archivo JSON `DE_category_id.json`. Utilizada para mejorar la interpretabilidad de los análisis.

## Conclusiones

### Conclusiones del negocio

* Las categorías Música y Entretenimiento concentran la mayor cantidad de videos en tendencia y el mayor número de "Me gusta", lo que las convierte en los segmentos más relevantes para estrategias de contenido.

* La tasa de aprobación (likes/(likes+dislikes)) es muy alta en todas las categorías (>95%), pero los canales del Top 10 destacan por su consistencia (menor variabilidad), no por picos de aprobación más altos.

* El 76.7% de los canales aparecen en tendencia 5 veces o menos, y el 40.8% solo una vez, lo que evidencia la alta rotación y la dificultad de mantener relevancia sostenida en la plataforma.

* Los canales con mayor frecuencia en tendencia (Top 10) generan más comentarios en sus videos típicos (mediana de 714 vs. 366 del resto), pero los canales de baja frecuencia pueden lograr picos virales de comentarios excepcionales.

* Es factible predecir el número de vistas con un modelo lineal simple (R² = 0.129) utilizando variables disponibles al momento de la publicación: channel_frequency, num_tags, title_length y category_id. El modelo es estadísticamente significativo y ofrece insights accionables.

### Conclusiones técnicas

* La limpieza de datos fue efectiva: se detectaron y unificaron falsos nulos (ej. "[none]" en tags) y se trataron outliers sin eliminarlos, preservando la cola larga de viralidad.

* La variable days_to_trend mostró alta correlación con views pero fue descartada por target leakage, lo que limitó el R² del modelo por una buena razón.

* La regresión lineal resultó interpretable y rápida, pero su poder predictivo es moderado (R² = 0.129), reflejando la alta componente aleatoria de la viralidad.

### Limitaciones

* La columna state fue incorporada de forma aleatoria, por lo que los análisis geográficos no representan el comportamiento real de los estados alemanes.

* El dataset solo incluye videos que ya alcanzaron la lista de tendencias, lo que introduce un sesgo de selección y no permite estudiar videos que no lograron popularidad.

* No se dispone del texto de los comentarios para realizar análisis de sentimiento, por lo que se utilizaron proxies (tasa_aprobación).

* La API usada para hacer scrapping de la data de YouTube limitó la recolección a 200 vídeos diarios, lo que restringe el universo de estudio.

* El quiebre estructural observado en mayo de 2018 (GDPR) afectó la consistencia temporal de los datos.

### Trabajo futuro

* Buscar datasets (o elaborar) que incorporen variables adicionales como el número de suscriptores del canal y el engagement en las primeras 2 horas para mejorar la precisión predictiva.

* Evaluar modelos no lineales (Random Forest, XGBoost) para comparar si superan al modelo lineal en capacidad predictiva.

* Realizar un análisis de sentimiento sobre los comentarios si se obtiene acceso a la API de YouTube.

* Extender el estudio a otros países para validar si los patrones observados en Alemania se replican en diferentes mercados.

## Licencia

Este proyecto se distribuye bajo la **Licencia MIT**, lo que permite su uso, modificación y distribución siempre que se incluya el aviso de copyright original.

Para más detalles, revisa el archivo [LICENSE](LICENSE) incluido en este repositorio.

