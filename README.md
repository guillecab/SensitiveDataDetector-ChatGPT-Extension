# 🛡️ LLM Guard Extension

Extensión de navegador y entorno de evaluación diseñados para **detectar datos sensibles** en interacciones con modelos de lenguaje (LLMs).  
El proyecto se compone de dos módulos principales:

- **LLM_Guard_Extension/** → Código de la extensión de navegador + servidor backend para proteger la privacidad del usuario en ChatGPT.  
- **TEST/** → Experimentos de evaluación comparando 5 LLMs en la tarea de detección de información sensible.

---

## 🚀 Características principales

- **Extensión de navegador (Chromium-based)**  
  - Compatible con **Google Chrome**, **Microsoft Edge** y **Brave**.  
  - Intercepta los mensajes escritos por el usuario, los **PDF adjuntos** y las **respuestas generadas por ChatGPT**.  
  - Resalta y clasifica datos sensibles detectados en **tiempo real**.  
  - Panel flotante interactivo que muestra:  
    - Nivel de riesgo (Alto / Medio / Bajo).  
    - Campos sensibles identificados.  
    - Opciones: *Ocultar* o *Enviar de todos modos*.  

- **Servidor backend (FastAPI)**  
  - Endpoints:  
    - `/detect` → analiza texto plano.  
    - `/detect_file` → analiza PDFs (extracción de texto con `pdfplumber`, `PyPDF2`, `pdfminer`, OCR opcional).  
  - Comunicación entre extensión y modelo de detección.  
  - Respuesta en formato **JSON estructurado**.  

- **Detección de datos sensibles**  
  - Uso de un LLM (por defecto `OpenAI/gpt-oss-120B` vía API de **Groq**).  
  - Soporte para múltiples modos de análisis (*Zero-shot*, *Few-shot*…).  
  - Categorización de campos en **riesgo alto**, **medio** o **bajo**.  
  - Ejemplos: credenciales, números de tarjeta, correos electrónicos, direcciones, teléfonos, identificadores personales, etc.  

- **Experimentos con 5 LLMs**  
  - Modelos evaluados:  
    - GPT-4o-mini  
    - Gemini 1.5 Pro  
    - OpenAI/gpt-oss-120B  
    - LLaMA 4 Scout 17B 16E Instruct  
    - Claude-3 Haiku  
  - Comparación de precisión, exhaustividad y robustez en la tarea de detección de datos sensibles.  
  - Scripts y resultados disponibles en la carpeta `TEST/`.

---

## 📂 Estructura del repositorio

### 📂 `LLM_Guard_Extension/`

Contiene la implementación de la extensión y el backend.

```text
LLM_Guard_Extension/
├── FastAPI.py
├── server.py
├── extension/                  
│   ├── content.js              # Script inyectado en ChatGPT
│   ├── icon128.png             # Icono de la extensión
│   └── manifest.json           # Configuración de la extensión
├── server/                     
│   ├── app.py                  # Servidor FastAPI con endpoints /detect y /detect_file
│   ├── detector_gpt_4o_mini.py # Detector usando GPT-4o-mini
│   ├── detector_gpt_oss_120b.py# Detector usando gpt-oss-120B
│   ├── PROMPTS/                # Plantillas de prompts
│   │   ├── FS_prompt.txt
│   │   ├── ZS_enriquecido_prompt.txt
│   │   └── ZS_prompt.txt
│   └── __pycache__/            # Archivos compilados de Python

TEST/
├── chatgpt_wrapper.py          # Wrapper para interactuar con ChatGPT
├── english_pii_43k.jsonl       # Dataset con ejemplos sensibles en inglés
├── evaluation.py               # Script principal de evaluación
├── Middleware.py               # Funciones de soporte
├── UI.py                       # Interfaz de usuario básica
├── CLAUDE/                     # Experimentos con Claude-3 Haiku
│   ├── claude-3-haiku.py
│   ├── pii_detection_sample_claude-3-haiku_1000_FS.json
│   ├── pii_detection_sample_claude_ZS_1000.json
│   └── pii_detection_sample_claude_ZS_enriquecido_1000.json
├── GEMINI/                     # Experimentos con Gemini 1.5 Pro
│   ├── gemini1.5-pro.py
│   ├── pii_detection_sample_Gemini_1.5pro_FS_1000.json
│   ├── pii_detection_sample_Gemini_1.5pro_ZS_enriquecido_1000.json
│   └── pii_detection_sample_Gemini_1.5_pro_ZS_1000.json
├── GPT-4o-MINI/                # Experimentos con GPT-4o-mini
│   ├── gpt_4o_mini.py
│   ├── pii_detection_sample_gpt-4o-mini_FS.json
│   ├── pii_detection_sample_gpt_4o_mini_1000_ZS_enriquecido.json
│   └── pii_detection_sample_gpt_4o_mini_ZS_1000.json
├── GPT-OSS-120b/               # Experimentos con gpt-oss-120B
│   ├── gpt_oss_120_ZS.py
│   ├── pii_detection_sample_gpt_oss_120b_FS.json
│   ├── pii_detection_sample_gpt_oss_120b_ZS_1000.json
│   └── pii_detection_sample_gpt_oss_120b_ZS_enriquecido_1000.json
└── LLAMA/                      # Experimentos con LLaMA 4 Scout 17B 16E Instruct
    ├── llama4_scout_17b_16e_instruct.py
    ├── pii_detection_sample_llama4_scout_FS.json
    ├── pii_detection_sample_llama4_scout_ZS_1000.json
    └── pii_detection_sample_llama4_scout_ZS_enriquecido_1000.json

