# Instagram Scraper — Análisis exploratorio de Benford

Proyecto académico para recopilar contadores visibles de perfiles de Instagram y analizar la distribución del primer dígito de sus seguidores. El scraper usa Python y Playwright; el análisis genera un resumen estadístico, una gráfica y un archivo Excel.

## Alcance

Se elige una lista del perfil objetivo: `seguidores` o `seguidos`. Para cada cuenta de esa lista, se analiza el **primer dígito del número de seguidores de esa cuenta**.

La muestra de Benford se limita a 200 observaciones válidas de una sola lista. Los contadores ausentes se guardan como datos faltantes, no como cero. Los ceros reales se excluyen del análisis del primer dígito.

El análisis es exploratorio. Una diferencia con la distribución de Benford no permite clasificar cuentas como bots o personas, y no se presupone que los contadores de Instagram deban seguir esa ley.

## Funciones

- Extracción de publicaciones visibles del perfil.
- Análisis exploratorio de publicaciones con Big Five mediante Gemini, opcional.
- Recopilación de contadores visibles de cuentas seguidas o seguidores.
- Cálculo de frecuencias de primer dígito y prueba chi-cuadrado.
- Exportación de reportes en JSON, Excel y SVG.
- Dashboard local con Streamlit.

## Requisitos

- Python 3.13 o compatible.
- Google Chrome instalado.
- Una sesión de Instagram iniciada manualmente, si la interfaz la solicita.
- Paquetes de Python:

```powershell
pip install playwright beautifulsoup4 pandas streamlit google-genai python-dotenv
```

`google-genai` y `GEMINI_API_KEY` solo son necesarios para la función Big Five. No guardes la clave en Git.

## Instalación en Windows

```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
pip install playwright beautifulsoup4 pandas streamlit google-genai python-dotenv
```

El inicio de sesión, si hace falta, se realiza manualmente:

```powershell
python scraping_instagram.py --accion sesion
```

El programa utiliza un perfil local persistente de Chrome. No subas ese perfil, sus cookies ni credenciales al repositorio.

## Ejecutar Benford

Para analizar 200 cuentas de la lista de seguidos:

```powershell
python scraping_instagram.py --accion benford --usuario USUARIO --muestra 200 --objetivo-digitos --lista seguidos --lectura-perfil --max-sin-tarjeta 10
```

Para analizar la lista de seguidores, cambia `--lista seguidos` por `--lista seguidores`.

El modo `--lectura-perfil` consulta los contadores visibles en cada perfil individual cuando están disponibles. Instagram puede no mostrar los contadores o no cargar más cuentas; el programa conserva los resultados obtenidos y se detiene ante ciertas condiciones de falta de datos o carga.

Para regenerar los reportes desde los datos guardados, sin abrir Instagram:

```powershell
python scraping_instagram.py --accion benford-resumen --usuario USUARIO
```

Para abrir el dashboard:

```powershell
python -m streamlit run scraping_instagram.py
```

Para ejecutar las pruebas locales incluidas:

```powershell
python scraping_instagram.py --accion pruebas
```

Estas pruebas no garantizan que Instagram mantenga la misma estructura o disponibilidad.

## Archivos generados

Los resultados se guardan localmente en:

```text
resultados/
└── USUARIO/
    ├── red_benford.json
    ├── benford_resumen.json
    ├── seguidores_seguidos_benford.xlsx
    ├── benford_lineas_seguidores.svg
    └── benford_lineas_seguidos.svg
```

`red_benford.json` conserva el avance de la recopilación y permite continuar en la misma computadora mientras el archivo permanezca disponible. El Excel se genera a partir de esos registros; no es el archivo de reanudación.

Los archivos de resultados pueden contener nombres de cuentas y contadores individuales. No se incluyen en este repositorio. Para una demostración pública, utiliza datos sintéticos o comparte solo resultados agregados sin nombres de usuario.

## Uso responsable y limitaciones

- Recopila únicamente información visible cuyo acceso y tratamiento estén autorizados.
- El proyecto no utiliza APIs privadas ni intenta eludir autenticación, CAPTCHA, controles o restricciones.
- Si Instagram limita o impide el acceso, detén la recopilación.
- Los contadores abreviados, como `2.5K`, son aproximaciones y pueden afectar el primer dígito.
- La muestra disponible puede tener sesgo de selección.
- La prueba chi-cuadrado y el valor p describen el ajuste estadístico de la muestra; no identifican bots ni determinan la autenticidad de una cuenta.

## No subir al repositorio

- `venv/`
- `.env` y claves de API
- El perfil persistente de Chrome
- `resultados/`, incluidos el Excel y `red_benford.json`
- Capturas que muestren nombres de cuentas o información personal