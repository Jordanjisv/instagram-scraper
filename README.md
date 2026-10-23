# Instagram Scraper + Big Five con Gemini

**Arquitectura de Software | Segunda entrega**

Este proyecto utiliza Python para recopilar información de un perfil de Instagram mediante *web scraping* y analizar el contenido de **10 publicaciones** con la API de **Google Gemini**. Los resultados se consultan desde un dashboard desarrollado con Streamlit.

La extracción se realiza a través del navegador, **sin utilizar la API oficial de Instagram**. La lógica se encuentra en un único archivo: `scraping_instagram.py`.

> **Rama de esta entrega:** `feature/bigfive-metricas`. La primera versión del scraping permanece en [`main`](https://github.com/Jordanjisv/instagram-scraper/tree/main).

## Funcionalidades

- Extracción de datos del perfil: nombre, biografía, seguidores, seguidos y publicaciones.
- Recopilación de enlaces, fechas, descripciones y métricas disponibles de publicaciones.
- Descarga de imágenes y exportación de datos en JSON y CSV.
- Identificación de likes con cantidad visible o confirmados sin una cifra exacta.
- Análisis exploratorio de **10 publicaciones** con Gemini, utilizando texto y/o imágenes.
- Visualización de publicaciones, imágenes e indicadores Big Five en Streamlit.
- Reutilización de resultados de IA guardados para evitar consultas repetidas.

## Tecnologías utilizadas

| Tecnología | Función |
| --- | --- |
| Python 3.13 | Lenguaje principal |
| Playwright y Google Chrome | Navegación y sesión en Instagram |
| BeautifulSoup | Extracción de información del HTML |
| Pandas | Organización de datos |
| Streamlit | Dashboard interactivo |
| Google Gen AI SDK (`google-genai`) | Comunicación con Gemini |
| JSON y CSV | Almacenamiento y exportación |

## Instalación

Se requiere **Python 3.13** y **Google Chrome** instalado. Los comandos siguientes están pensados para PowerShell en Windows.

**1. Clonar la rama correspondiente a la segunda entrega:**

```powershell
git clone --branch feature/bigfive-metricas https://github.com/Jordanjisv/instagram-scraper.git
cd instagram-scraper
```

**2. Crear y activar un entorno virtual:**

```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

**3. Instalar las bibliotecas:**

```powershell
python -m pip install playwright beautifulsoup4 pandas streamlit google-genai python-dotenv
```

## Configuración de Gemini

Para realizar el análisis Big Five es necesaria una API Key de Google Gemini, que se puede obtener en [Google AI Studio](https://aistudio.google.com/api-keys).

Crear un archivo llamado **`.env`** en la raíz del proyecto con este contenido:

```dotenv
GEMINI_API_KEY=PEGA_AQUI_TU_API_KEY
```

El programa lee ese archivo con `python-dotenv`. La clave **no debe publicarse** y el archivo `.env` está excluido mediante `.gitignore`.

El proyecto utiliza como modelo predeterminado **`gemini-3.8-flash`**, que fue el utilizado durante las pruebas. La disponibilidad y los límites de uso dependen de la cuenta y de la API.

> La clave de Gemini no es necesaria para ejecutar el scraping, las pruebas locales o visualizar análisis ya guardados.

## Cómo ejecutar el proyecto

Con el entorno virtual activado y desde la carpeta del proyecto:

**1. Iniciar sesión en Instagram cuando sea necesario:**

```powershell
python scraping_instagram.py --accion sesion
```

Se abrirá Google Chrome para iniciar sesión manualmente. La sesión se conserva localmente.

**2. Recopilar publicaciones e imágenes:**

```powershell
python scraping_instagram.py --accion extraer --usuario sausage_player --actualizar-enlaces --limite 10
```

`--limite` indica la cantidad de publicaciones pendientes que se procesarán; con `--limite 0` se procesan todas las pendientes.

**3. Actualizar los likes y otras métricas de las publicaciones guardadas:**

```powershell
python scraping_instagram.py --accion extraer --usuario sausage_player --reprocesar --solo-guardadas --sin-imagenes --limite 0
```

Este comando vuelve a consultar las publicaciones procesadas sin descargar de nuevo las imágenes.

**4. Previsualizar las diez publicaciones que analizará Gemini, sin consumir cuota:**

```powershell
python scraping_instagram.py --accion bigfive --usuario sausage_player --cantidad 10 --previsualizar
```

**5. Ejecutar el análisis Big Five:**

```powershell
python scraping_instagram.py --accion bigfive --usuario sausage_player --cantidad 10
```

Los resultados se guardan progresivamente. El programa reutiliza resultados previos cuando el contenido y el modelo no han cambiado.

**6. Abrir el dashboard:**

```powershell
python -m streamlit run scraping_instagram.py
```

Acceder desde el navegador a **http://localhost:8501**. El dashboard tiene dos pestañas: **Publicaciones e imágenes** y **Big Five exploratorio**.

**7. Ejecutar las pruebas locales:**

```powershell
python scraping_instagram.py --accion pruebas
```

## Análisis Big Five (OCEAN)

Gemini busca evidencias relacionadas con cinco dimensiones en el **contenido** de cada publicación:

- **Apertura:** creatividad, curiosidad o interés por experiencias nuevas.
- **Responsabilidad:** organización, planificación o constancia.
- **Extraversión:** actividades e interacciones sociales mencionadas.
- **Amabilidad:** apoyo, colaboración o empatía expresados.
- **Neuroticismo:** referencias expresas a preocupación, inseguridad o tensión.

Para cada dimensión, el sistema registra si encontró evidencia (`presente` o `sin_evidencia`), de dónde proviene (`texto`, `imagen` o `ninguna`) y una justificación. Una publicación puede contener evidencias relacionadas con varias dimensiones o no presentar evidencia suficiente.

**Los resultados no constituyen una prueba psicológica ni una medición de la personalidad real de la dueña del perfil.** Son indicadores exploratorios de expresiones y temas presentes en las publicaciones.

En la ejecución de esta segunda entrega se analizaron **10 publicaciones**, con **50 evaluaciones OCEAN** (cinco dimensiones por publicación). El gráfico indica cuántas publicaciones contienen evidencia por dimensión, no una puntuación psicológica.

### Likes sin cantidad numérica

En ocasiones, Instagram muestra una frase como «A una persona y otras más les gusta esta publicación», pero no proporciona un total.

| Información visible | Resultado almacenado |
| --- | --- |
| `24 Me gusta` | Se guarda el número `24` |
| «A alguien y otras personas les gusta» | Se confirma que hay likes, sin asignar un número |
| Sin información visible | Se conserva como dato no disponible |

De esta manera, el sistema **no inventa cantidades de likes** cuando Instagram no las proporciona.

## Archivos generados

Los resultados se guardan localmente, por ejemplo:

```text
instagram-scraper/
├── scraping_instagram.py
├── README.md
├── .gitignore
├── .env                         # Solo local, no se sube
└── resultados/                  # Solo local, no se sube
    └── sausage_player/
        ├── perfil.json
        ├── enlaces_publicaciones.json
        ├── detalles_publicaciones.json
        ├── publicaciones_enriquecidas.json
        ├── publicaciones_enriquecidas.csv
        ├── big_five_10.json
        ├── big_five_10.csv
        └── imagenes/
```

**Importante:** `resultados/` y las fotografías se excluyen de GitHub. Al clonar el repositorio se obtiene el código, pero para visualizar datos reales se requiere realizar una extracción autorizada o disponer de los archivos generados localmente.

## Consideraciones

- El scraping se limita a contenido cuyo acceso y tratamiento estén autorizados y no evita restricciones de Instagram.
- Los datos dependen de la información disponible durante la consulta; algunas publicaciones carecen de descripciones o recuentos públicos.
- No se deben publicar claves API, cookies, sesiones ni imágenes personales sin autorización.
- El uso de Gemini está sujeto a disponibilidad, cuotas y condiciones del servicio; al analizar imágenes o textos, esos datos se envían a la API.

**Proyecto académico — Arquitectura de Software.**
