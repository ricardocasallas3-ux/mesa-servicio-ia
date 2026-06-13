# MVP — Mesa de servicio inteligente con alerta proactiva de riesgo

Prueba de concepto del proyecto integrador (Especialización en IA — CUN). Ejecuta en **Google Colab** un pipeline de IA sobre tickets de soporte y publica los resultados en un **dashboard en GitHub Pages**.

## Qué hace

Pipeline de extremo a extremo:

1. **Seguridad (phishing):** detecta tickets maliciosos y los aísla; el flujo se detiene (no pasan a la IA).
2. **Privacidad (PBD):** anonimiza correos, teléfonos, identificadores y credenciales antes de procesar.
3. **Sentimiento:** calcula una métrica numérica de frustración (0–100).
4. **Clasificación:** clasifica el ticket en *correctivo* / *evolutivo* (TF-IDF + Regresión Logística).
5. **Riesgo:** estima un índice de no renovación (Gradient Boosting) e **identifica el impulsor principal** (importancia de variables).
6. **Alerta + dashboard:** genera recomendaciones para el account manager y exporta `docs/data/resultados.json` para el tablero.

> El MVP usa **datos sintéticos** para que corra sin dataset real. Para usar datos reales, define `RUTA` en el notebook. El índice de riesgo, sin un histórico real de renovación, es demostrativo (ver nota en el notebook).

## Estructura (en la raíz del repo)

```
mesa-servicio-ia/
├── mesa_servicio_mvp.ipynb     # Notebook de Colab (pipeline)
├── requirements.txt
├── docs/                        # Carpeta que publica GitHub Pages
│   ├── index.html               # Dashboard
│   └── data/resultados.json     # Datos del dashboard (muestra incluida)
└── README.md
```

## Cómo ejecutar en Colab

1. Sube esta carpeta a un repositorio de GitHub.
2. Abre `mesa_servicio_mvp.ipynb` en Colab (botón *Open in Colab* o `colab.research.google.com` → GitHub → tu repo).
3. Ejecuta todas las celdas (*Runtime → Run all*). Se genera `docs/data/resultados.json`.
4. Descarga ese archivo (la última celda lo ofrece) y súbelo a `docs/data/` en el repo, **o** usa la celda opcional de `git push` desde Colab.

## Cómo publicar el dashboard en GitHub Pages

1. En el repo: **Settings → Pages**.
2. *Source:* rama `main`, carpeta **/docs**.
3. Guarda. La URL queda como `https://<usuario>.github.io/<repo>/`.

El dashboard carga `data/resultados.json`. Si se abre como archivo local sin servidor, usa datos de muestra embebidos (fallback) para no quedar en blanco.

## Badge para abrir en Colab

```
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ricardocasallas3-ux/mesa-servicio-ia/blob/main/mesa_servicio_mvp.ipynb)
```

Dashboard publicado: `https://ricardocasallas3-ux.github.io/mesa-servicio-ia/`

## Limitaciones (MVP)

- Sentimiento por léxico simple (para upgrade: `pysentimiento` en español).
- Detección de phishing por reglas (no un servicio antiphishing real).
- Índice de riesgo demostrativo: requiere variable objetivo real (histórico de renovación) para ser predictivo.
