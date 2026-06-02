# Plataforma ANE

Este repositorio agrupa la plataforma de sensado espectral de la ANE. En terminos simples, la plataforma permite operar sensores RF, recibir mediciones, analizarlas y convertirlas en informacion util para monitoreo, campañas y reportes de cumplimiento.

La vista general es corta a proposito. Para profundizar en cada parte, hay diagramas especificos en `frontend/`, `backend/` y `postprocesamiento/`.

## Diagrama general: de la medicion al reporte

```mermaid
%%{init: {'flowchart': {'curve': 'stepAfter'}}}%%
flowchart LR
    %% Clases de colores
    classDef orange fill:#fff7ed,stroke:#ea580c,stroke-width:2px,color:#9a3412
    classDef blue fill:#eff6ff,stroke:#2563eb,stroke-width:2px,color:#1e40af
    classDef green fill:#f0fdf4,stroke:#16a34a,stroke-width:2px,color:#166534
    classDef purple fill:#faf5ff,stroke:#9333ea,stroke-width:2px,color:#581c87
    classDef slate fill:#f8fafc,stroke:#475569,stroke-width:2px,color:#1e293b

    PERSONA["<b>Persona usuaria</b><br/>consulta, configura y revisa resultados"]:::slate
    SENSOR["<b>Sensores RF</b><br/>miden el espectro en campo"]:::slate

    subgraph FRONTEND[" frontend/ "]
        UI["<b>Interfaz web</b><br/>mapas, monitoreo, campañas,<br/>alertas y reportes"]:::blue
    end

    subgraph BACKEND[" backend/ "]
        COORD["<b>Centro de coordinacion</b><br/>usuarios, sensores, datos,<br/>campañas y tiempo real"]:::green
        DATOS["<b>Base de datos</b><br/>historico, configuracion y resultados"]:::purple
    end

    subgraph ANALISIS[" postprocesamiento/ "]
        PY["<b>Analisis normativo</b><br/>detecta emisiones y evalua cumplimiento"]:::orange
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

    style FRONTEND rx:6,ry:6
    style BACKEND rx:6,ry:6
    style ANALISIS rx:6,ry:6
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
