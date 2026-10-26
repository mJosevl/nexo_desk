# Nexo Desk

Asistente de soporte TI con IA: valida definiciones, ayuda a diagnosticar incidentes y responde consultas de administración (Control-M).

Proyecto final de la Especialización en Desarrollo con IA Generativa (Accenture / Skillnest).

Hoy incluye la **capacidad 1**: validar y normalizar definiciones XML de jobs y calendarios, en Python estándar y sin IA.

## Capacidades del asistente

| # | Capacidad | Estado |
|---|---|---|
| 1 | Validar y normalizar XML de jobs y calendarios | En construcción |
| 2 | Relacionar cada problema con el incidente que causa, citando a BMC | Futuro (`docs/fuentes.md` es la base) |
| 3 | Responder preguntas de soporte con Gemini + RAG sobre documentación pública | Futuro |

## Estructura

```
nexo_desk/
├── src/
│   ├── main.py          # Punto de entrada (línea de comandos)
│   ├── reglas.py        # Reglas configurables (provisionales hasta confirmar con BMC)
│   ├── lector_xml.py    # Carga el XML y extrae jobs y calendarios
│   ├── validador.py     # Una función por regla
│   ├── normalizador.py  # Correcciones seguras (espacios y fechas)
│   └── reporte.py       # Salida en pantalla y JSON
├── ejemplos/            # XML inventados, con y sin errores
├── salida/              # Reportes y XML normalizados (no se suben)
├── docs/fuentes.md      # Respaldo oficial de cada regla
├── requirements.txt
└── .gitignore
```

## Requisitos

- Python 3.10 o superior
- No necesita librerías externas (solo biblioteca estándar)

## Ejecución

```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
cd src
python main.py ..\ejemplos\jobs_correcto.xml
python main.py ..\ejemplos\jobs_con_errores.xml --normalizar
python main.py ..\ejemplos\calendarios_con_errores.xml
```

Los resultados quedan en `salida/`: un `_reporte.json` por archivo y, con `--normalizar`, un `_normalizado.xml`. El archivo original nunca se modifica.

## Qué corrige y qué no

El normalizador solo hace cambios seguros: quita espacios sobrantes en atributos y convierte fechas como `2026-01-15` o `15/01/2026` a `20260115`. Los duplicados, los atributos faltantes y las fechas imposibles solo se reportan, porque requieren una decisión de una persona.

## Datos

Solo se usan XML inventados o anonimizados. Nunca datos reales de clientes.
