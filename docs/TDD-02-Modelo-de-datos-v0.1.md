Set-PSReadLineOption: C:\Users\arnay\SynologyDrive\Documents\PowerShell\Microsoft.PowerShell_profile.ps1:3
Line |
   3 |  Set-PSReadLineOption -PredictionViewStyle ListView
     |  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     | The handle is invalid.
# TDD-02 — Modelo de datos

**Proyecto:** Windows Autonomous IT Diagnostic  
**Versión:** 0.1  
**Estado:** Aprobado por el usuario como línea base inicial el 12 de noviembre de 2026  
**Implementación y pruebas:** Pendientes

## 1. Propósito

Proponer las entidades mínimas para representar un análisis offline de archivos EVTX, los eventos encontrados, el diagnóstico y sus recomendaciones. Este modelo es lógico; no prescribe una base de datos ni un formato físico.

## 2. Criterios

- Mantener referencia al archivo de origen y al equipo de origen cuando estén disponibles.
- Preservar los datos originales del evento necesarios para trazabilidad.
- Separar los datos del evento de la interpretación diagnóstica generada por reglas o IA.
- Registrar recomendaciones y cualquier acción controlada por políticas como elementos distintos.
- No asumir una persistencia permanente de datos: retención y almacenamiento están pendientes.

## 3. Entidades propuestas

### 3.1 AnalysisJob

Representa una ejecución de análisis.

| Campo | Tipo lógico | Descripción |
|---|---|---|
| analysis_id | identificador | Identificador único del análisis. |
| created_at | fecha/hora | Momento de inicio del análisis. |
| completed_at | fecha/hora opcional | Momento de finalización, si concluyó. |
| status | enumeración propuesta | Estado del proceso: pendiente, en curso, completado o fallido. |
| source_type | enumeración propuesta | Origen: archivo EVTX local; se reserva una opción remota futura. |
| operator_context | objeto opcional | Respuestas al cuestionario que sean pertinentes al análisis. |
| summary | texto opcional | Resumen resultante. |

### 3.2 EventSource

Describe el archivo analizado sin exigir que se conserve su contenido.

| Campo | Tipo lógico | Descripción |
|---|---|---|
| source_id | identificador | Identificador de la fuente dentro del análisis. |
| analysis_id | identificador | Análisis al que pertenece. |
| file_name | texto | Nombre presentado del archivo. |
| file_size_bytes | entero opcional | Tamaño observado. |
| file_hash | texto opcional | Huella para identificar el archivo; algoritmo por definir. |
| computer_name | texto opcional | Equipo indicado por los metadatos EVTX, si está disponible. |
| channel | texto opcional | Canal de eventos, si está disponible. |
| time_range_start | fecha/hora opcional | Inicio del rango temporal observado. |
| time_range_end | fecha/hora opcional | Fin del rango temporal observado. |

### 3.3 WindowsEvent

Representa un evento leído del EVTX.

| Campo | Tipo lógico | Descripción |
|---|---|---|
| event_id | identificador | Identificador interno del registro. |
| source_id | identificador | Archivo del que procede. |
| record_id | entero/texto opcional | Número de registro original, si está disponible. |
| provider | texto opcional | Proveedor del evento. |
| event_code | entero/texto opcional | ID del evento. |
| level | texto/código opcional | Nivel original informado por Windows. |
| timestamp | fecha/hora opcional | Fecha y hora del evento, normalizada con zona/offset cuando sea posible. |
| computer | texto opcional | Equipo indicado en el registro. |
| channel | texto opcional | Canal del registro. |
| message | texto opcional | Mensaje renderizado, si se pudo obtener. |
| raw_data_ref | referencia opcional | Referencia a datos originales preservados, si se define esa función. |
| classification | enumeración propuesta | Clasificación normalizada; las categorías finales están pendientes. |

### 3.4 Finding

Hallazgo de diagnóstico asociado a uno o más eventos.

| Campo | Tipo lógico | Descripción |
|---|---|---|
| finding_id | identificador | Identificador único del hallazgo. |
| analysis_id | identificador | Análisis que lo produjo. |
| title | texto | Título breve. |
| description | texto | Explicación comprensible del hallazgo. |
| severity | enumeración propuesta | Severidad normalizada; escala por definir. |
| confidence | número opcional | Confianza estimada, con escala y origen por definir. |
| event_refs | lista de identificadores | Eventos que respaldan el hallazgo. |
| cause_hypothesis | texto opcional | Hipótesis diagnóstica, no necesariamente confirmada. |
| generated_by | texto opcional | Regla, modelo o versión de componente que generó el hallazgo. |

### 3.5 Recommendation

Recomendación asociada a un hallazgo.

| Campo | Tipo lógico | Descripción |
|---|---|---|
| recommendation_id | identificador | Identificador único. |
| finding_id | identificador | Hallazgo asociado. |
| description | texto | Acción sugerida o pasos para el operador. |
| automation_candidate | booleano propuesto | Indica si puede evaluarse para automatización; no autoriza su ejecución. |
| risk_level | enumeración propuesta | Riesgo, pendiente de definir. |
| requires_human_action | booleano | Indica si el flujo requiere intervención humana. |
| status | enumeración propuesta | Estado de la recomendación; valores pendientes de definir. |

## 4. Relaciones

```text
AnalysisJob 1 ── N EventSource
EventSource 1 ── N WindowsEvent
AnalysisJob 1 ── N Finding
Finding N ── N WindowsEvent
Finding 1 ── N Recommendation
```

## 5. Datos sensibles y retención

Los archivos EVTX y sus mensajes pueden incluir nombres de equipos, usuarios, rutas u otros datos sensibles. El modelo no presupone que esos datos se envíen a un proveedor externo ni que se almacenen después del análisis. Deben definirse la política de retención, la sanitización, los permisos de acceso y el comportamiento de la integración externa antes de implementar persistencia o transmisión.

## 6. Pendientes de decisión

- Base de datos o almacenamiento de archivos, si se requiere persistencia.
- Algoritmo de hash y si la huella se calcula siempre.
- Campos EVTX obligatorios y estrategia para conservar XML/datos originales.
- Taxonomía de clasificación para error, advertencia y otras categorías.
- Escala de severidad y confianza.
- Esquema del cuestionario y qué respuestas se almacenan.
- Retención, borrado, exportación y anonimización.
- Versionado del esquema y compatibilidad de informes.

## 7. Estado

Este modelo lógico constituye la línea base aprobada. Los tipos y campos identificados como propuestos o pendientes requieren definición antes de fijar el esquema físico. Implementación y pruebas pendientes.

