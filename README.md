# Instagram Scraper

## Descripción

Proyecto desarrollado en Python para extraer información de un perfil de Instagram mediante técnicas de Web Scraping, sin utilizar la API oficial de Instagram.

El programa permite recopilar publicaciones, obtener información disponible, descargar imágenes y visualizar los resultados en un dashboard.

Este proyecto fue realizado como parte de la asignatura **Arquitectura de Software**.

## Tecnologías utilizadas

- **Python:** lenguaje de programación principal.
- **Playwright:** automatización de Google Chrome.
- **BeautifulSoup:** extracción de información del HTML.
- **Streamlit:** visualización de datos mediante un dashboard.
- **Pandas:** organización y manejo de datos.

## Funcionalidades

- Inicio de sesión en Instagram mediante Chrome.
- Extracción de información del perfil.
- Recopilación de enlaces de publicaciones.
- Extracción de descripciones, fechas y métricas disponibles.
- Descarga de imágenes de las publicaciones.
- Almacenamiento de información en archivos JSON y CSV.
- Visualización de resultados mediante Streamlit.

## Instalación

**Requisitos:** Python 3.12 o 3.13 y Google Chrome.

**1. Crear un entorno virtual**

```powershell
python -m venv venv
```

**2. Activar el entorno virtual**

```powershell
.\venv\Scripts\Activate.ps1
```

**3. Instalar las bibliotecas**

```powershell
python -m pip install playwright beautifulsoup4 streamlit pandas openai
```

## Ejecución

**1. Iniciar sesión en Instagram (si es necesario)**

```powershell
python scraping_instagram.py --accion sesion
```

**2. Extraer publicaciones e imágenes**

```powershell
python scraping_instagram.py --accion extraer --usuario sausage_player --actualizar-enlaces --limite 10
```

El parámetro `--usuario` permite seleccionar el perfil que se desea analizar.

El parámetro `--limite` indica cuántas publicaciones pendientes se procesarán.

**3. Visualizar los resultados**

```powershell
python -m streamlit run scraping_instagram.py
```

Abrir en el navegador:

http://localhost:8501

**4. Ejecutar las pruebas**

```powershell
python scraping_instagram.py --accion pruebas
```

## Resultados obtenidos

Durante las pruebas realizadas con el perfil autorizado `@sausage_player`, se obtuvieron:

- 50 enlaces de publicaciones.
- 23 publicaciones procesadas en detalle.
- 23 imágenes descargadas.
- Archivos JSON y CSV con los datos recopilados.
- Dashboard funcional para consultar los resultados.

Estos resultados corresponden a las pruebas realizadas y pueden variar en futuras ejecuciones.

## Estructura del proyecto

```text
Scraping/
├── scraping_instagram.py
├── resultados/
│   └── sausage_player/
│       ├── perfil.json
│       ├── enlaces_publicaciones.json
│       ├── detalles_publicaciones.json
│       ├── publicaciones_enriquecidas.json
│       ├── publicaciones_enriquecidas.csv
│       └── imagenes/
└── venv/
```

Toda la lógica del programa se encuentra en un único archivo Python.

## Consideraciones

- El scraping se realiza sobre contenido al que se tiene acceso autorizado.
- Algunas publicaciones pueden no mostrar todas sus métricas.
- Las cookies y credenciales se conservan localmente y no deben publicarse en GitHub.
- El análisis Big Five mediante inteligencia artificial queda reservado para una siguiente etapa del proyecto.

## Autor

Proyecto académico — Arquitectura de Software.
