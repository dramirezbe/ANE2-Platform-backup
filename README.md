# Plataforma ANE

Este repositorio agrupa la plataforma de sensado espectral de la ANE. En terminos simples, la plataforma permite operar sensores RF, recibir mediciones, analizarlas y convertirlas en informacion util para monitoreo, campañas y reportes de cumplimiento.

La vista general es corta a proposito. Para profundizar en cada parte, hay diagramas especificos en `frontend/`, `backend/` y `postprocesamiento/`.

## Diagrama general: de la medicion al reporte

```mermaid
flowchart LR
  PERSONA["Persona usuaria<br/>consulta, configura y revisa resultados"]
  SENSOR["Sensores RF<br/>miden el espectro en campo"]

  subgraph FRONTEND["frontend/"]
    UI["Interfaz web<br/>mapas, monitoreo, campañas,<br/>alertas y reportes"]
  end

  subgraph BACKEND["backend/"]
    COORD["Centro de coordinacion<br/>usuarios, sensores, datos,<br/>campañas y tiempo real"]
    DATOS["Base de datos<br/>historico, configuracion y resultados"]
  end

  subgraph ANALISIS["postprocesamiento/"]
    PY["Analisis normativo<br/>detecta emisiones y evalua cumplimiento"]
  end

  PERSONA -->|opera la plataforma| UI
  UI -->|pide informacion o acciones| COORD
  COORD -->|actualiza pantallas| UI

  COORD -->|configuracion o campañas| SENSOR
  SENSOR -->|estado, ubicacion, espectro y audio| COORD

  COORD <--> DATOS

  COORD -->|mediciones de campaña| PY
  PY -->|emisiones detectadas y cumplimiento| COORD
  COORD -->|reporte listo para interpretar| UI

  classDef actor fill:#fff,stroke:#111,stroke-width:2px,color:#000;
  classDef frontend fill:#e3f2fd,stroke:#1e88e5,stroke-width:2px,color:#000;
  classDef backend fill:#e8f5e9,stroke:#43a047,stroke-width:2px,color:#000;
  classDef python fill:#fff7ed,stroke:#f97316,stroke-width:2px,color:#000;
  classDef data fill:#f3e8ff,stroke:#8b5cf6,stroke-width:2px,color:#000;

  class PERSONA,SENSOR actor;
  class UI frontend;
  class COORD backend;
  class DATOS data;
  class PY python;
```

## Como leer este mapa

La persona usuaria trabaja desde el `frontend/`: ve mapas, estados, monitoreo en vivo, campañas, alertas y reportes.

El `backend/` coordina todo lo que ocurre detras: autentica usuarios, administra sensores y campañas, recibe datos de campo, guarda informacion, avisa cambios en tiempo real y solicita analisis cuando se necesita un reporte.

El `postprocesamiento/` interpreta mediciones de espectro. Detecta emisiones, mide sus parametros y, cuando hay informacion normativa disponible, indica si esas emisiones cumplen, estan fuera de parametros o no tienen licencia asociada.

## Lectura en una frase

La plataforma permite que una persona configure mediciones desde la web, que los sensores midan el espectro, que el backend organice y guarde los datos, y que el modulo Python convierta esas mediciones en resultados de cumplimiento.

## Donde profundizar

- Flujo del frontend: [frontend/DIAGRAM.md](frontend/DIAGRAM.md)
- Flujo del backend: [backend/DIAGRAM.md](backend/DIAGRAM.md)
- Flujo del postprocesamiento: [postprocesamiento/DIAGRAM.md](postprocesamiento/DIAGRAM.md)
- Documentacion del backend: [backend/README.md](backend/README.md)
