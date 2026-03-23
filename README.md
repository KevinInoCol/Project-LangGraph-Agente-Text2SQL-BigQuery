# Project LangGraph — Agente Text2SQL con BigQuery

Agente conversacional que convierte **preguntas en lenguaje natural** en **consultas SQL**, las ejecuta en **Google BigQuery** y devuelve respuestas claras en **español**. Está construido con **LangGraph**, **LangChain** y **OpenAI (GPT-4o)**.

---

## Qué hace el proyecto

- El usuario formula una pregunta sobre datos de **Citi Bike (Nueva York)**.
- El modelo genera SQL válido para **BigQuery** sobre la tabla pública de viajes.
- Una **herramienta** (`run_sql_query`) ejecuta el SQL mediante SQLAlchemy + cliente de BigQuery.
- El grafo alterna entre el **nodo agente** (LLM + tools) y el **nodo de herramientas** hasta obtener la respuesta final.

**Datos:** tabla `bigquery-public-data.new_york_citibike.citibike_trips` (duración de viajes, estaciones, usuarios, etc.).

---

## Stack tecnológico

| Componente | Uso |
|------------|-----|
| **LangGraph** | Grafo con nodos `agent` ↔ `tools` y estado de mensajes |
| **LangChain** | `ChatOpenAI`, `ToolNode`, mensajes y binding de tools |
| **OpenAI** | Modelo `gpt-4o` (configurable en código) |
| **Google BigQuery** | Ejecución de SQL y dataset público CitiBike |
| **Streamlit** | Interfaz web de chat (`main.py`) |
| **Docker** | Imagen que sirve la app Streamlit en el puerto **8000** |

---

## Requisitos previos

- **Python 3.11+** (recomendado; el Dockerfile usa 3.11)
- Cuenta de **OpenAI** con API key
- Proyecto en **Google Cloud** con **BigQuery** habilitado y facturación (las consultas a datos públicos siguen las cuotas de BigQuery)
- **Credenciales de GCP**: cuenta de servicio (JSON) o `gcloud auth application-default login`

---

## Configuración

### 1. Clonar e instalar dependencias

```bash
git clone https://github.com/KevinInoCol/Project-LangGraph-Agente-Text2SQL-BigQuery.git
cd Project-LangGraph-Agente-Text2SQL-BigQuery
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

### 2. Variables de entorno (`.env`)

Crea un archivo `.env` en la raíz del proyecto (no lo subas a Git):

```env
OPENAI_API_KEY=sk-...
GOOGLE_APPLICATION_CREDENTIALS=nombre-de-tu-service-account.json
```

- **`OPENAI_API_KEY`**: obligatoria para el LLM.
- **`GOOGLE_APPLICATION_CREDENTIALS`**: ruta al JSON de la cuenta de servicio (o usa autenticación por defecto de aplicación con `gcloud`).

### 3. Proyecto de GCP en el código

En `tools/run_sql_query.py`, la constante `TU_PROYECTO_GCP_ID` debe coincidir con el **ID del proyecto de Google Cloud** donde ejecutas las consultas (facturación de BigQuery). Ajusta ese valor si usas otro proyecto.

---

## Cómo ejecutarlo

### Interfaz web (Streamlit)

```bash
streamlit run main.py
```

Abre el navegador en la URL que indique la terminal (por defecto `http://localhost:8501`).

### LangGraph Studio / CLI

El archivo `langgraph.json` expone el grafo compilado como `agent` desde `agent_langgraph:app`. Puedes usarlo con las herramientas de LangGraph según tu entorno.

### Docker

```bash
docker build -t citibike-agent .
docker run -p 8000:8000 --env-file .env citibike-agent
```

La app queda en `http://localhost:8000` (Streamlit en `0.0.0.0:8000`).

> **Nota:** En producción, monta credenciales y secretos de forma segura (secretos del orquestador, no solo copiar `.env` en la imagen).

---

## Estructura del repositorio

```
├── agent_langgraph.py    # Grafo LangGraph, prompt del sistema, run_agent()
├── main.py               # App Streamlit (chat)
├── tools/
│   └── run_sql_query.py  # Tool LangChain + conexión BigQuery/SQLAlchemy
├── langgraph.json        # Definición del grafo para LangGraph
├── requirements.txt
├── Dockerfile
├── Comandos.md
└── images/
```

---

## Seguridad

- El `.gitignore` excluye `.env` y archivos de credenciales JSON típicos.
- **No subas** claves de OpenAI ni JSON de service account al repositorio.

---

## Autor

Proyecto académico / de demostración — **Agente SQL CitiBike** (LangChain / LangGraph / BigQuery).

Repositorio: [KevinInoCol/Project-LangGraph-Agente-Text2SQL-BigQuery](https://github.com/KevinInoCol/Project-LangGraph-Agente-Text2SQL-BigQuery)
