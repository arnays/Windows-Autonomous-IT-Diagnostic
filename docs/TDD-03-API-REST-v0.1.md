# TDD-03 — API REST

**Proyecto:** Windows Autonomous IT Diagnostic  
**Versión:** 0.1  
**Estado:** Aprobado por el usuario como línea base inicial el 12 de noviembre de 2026  
**Implementación y pruebas:** Pendientes

## 1. Propósito

Proponer una interfaz HTTP local para que la interfaz web coordine el cuestionario y el análisis offline de archivos EVTX. Este contrato lógico no selecciona framework, puerto, autenticación ni formatos finales.

## 2. Principios de diseño

- La API coordina operaciones; el procesamiento de EVTX y el diagnóstico se delegan a componentes internos.
- Las rutas de reparación, si se incorporan, no permiten enviar comandos arbitrarios.
- Las recomendaciones no ejecutan acciones por el mero hecho de ser devueltas por la API.
- La API remota para agentes Windows no se define en este documento; pertenece a un hito posterior.
- Respuestas y errores no deben incluir secretos ni datos de EVTX innecesarios.

## 3. Convenciones propuestas

- Prefijo versionado propuesto: `/api/v1`.
- Formato de intercambio propuesto: JSON UTF-8.
- Identificadores opacos para análisis, fuentes y hallazgos.
- Fechas en formato ISO 8601 con zona u offset cuando sea posible.
- Códigos de error estables separados del texto destinado a mostrar al usuario.

Estas convenciones requieren aprobación y no fijan una implementación técnica.

## 4. Recursos y operaciones propuestos

### 4.1 Cuestionario

| Método y ruta propuesta | Propósito |
|---|---|
| `GET /api/v1/questionnaire` | Obtener la definición del cuestionario inicial. |
| `POST /api/v1/questionnaire/answers` | Validar y registrar respuestas para una sesión de análisis. |

El contenido, versionado y persistencia del cuestionario quedan pendientes.

### 4.2 Análisis EVTX

| Método y ruta propuesta | Propósito |
|---|---|
| `POST /api/v1/analyses` | Iniciar un análisis proporcionando uno o más archivos EVTX y el contexto permitido. |
| `GET /api/v1/analyses/{analysisId}` | Consultar estado y resumen del análisis. |
| `GET /api/v1/analyses/{analysisId}/events` | Consultar eventos clasificados con paginación. |
| `GET /api/v1/analyses/{analysisId}/findings` | Consultar hallazgos y referencias a eventos. |
| `GET /api/v1/analyses/{analysisId}/report` | Obtener o descargar el informe, cuando se defina su formato. |

No se fija todavía si la carga usa multipart, una ruta local restringida o un flujo de carga por partes. Tampoco se fijan límites de tamaño, cancelación ni concurrencia.

### 4.3 Salud y configuración

| Método y ruta propuesta | Propósito |
|---|---|
| `GET /api/v1/health` | Indicar disponibilidad básica del servicio. |
| `GET /api/v1/providers` | Informar proveedores de IA configurados y su disponibilidad sin exponer credenciales. |

La API no debe devolver secretos, tokens ni configuración sensible.

## 5. Esquemas ilustrativos

Los siguientes ejemplos son ilustrativos; los campos y enumeraciones deben alinearse con TDD-02 y TDD-05 antes de implementarse.

### Crear análisis

```json
{
  "questionnaireAnswers": {},
  "files": [
    {
      "fileName": "System.evtx"
    }
  ]
}
```

### Respuesta inicial

```json
{
  "analysisId": "<id>",
  "status": "accepted"
}
```

### Error

```json
{
  "error": {
    "code": "INVALID_INPUT",
    "message": "No se pudo aceptar la solicitud.",
    "correlationId": "<id>"
  }
}
```

## 6. Errores y validación

La API debe diferenciar, como mínimo, solicitudes inválidas, archivo no compatible o ilegible, análisis inexistente, proveedor no disponible y error interno. La lista final de códigos HTTP, reintentos, límites y mensajes localizados está pendiente. La validación de archivos debe ocurrir antes del análisis y los errores no deben provocar ejecución de contenido del archivo.

## 7. Seguridad de la interfaz

- Limitar inicialmente la API a la interfaz local salvo decisión explícita distinta.
- Validar tamaño, tipo y contenido de cada archivo recibido.
- Evitar rutas arbitrarias del sistema de archivos suministradas por el cliente.
- Aplicar controles para cargas excesivas y solicitudes repetidas.
- Mantener datos de autenticación fuera de respuestas y registros ordinarios.
- Diseñar CORS, CSRF y autenticación según el modo final de alojamiento web.

Estos son requisitos de diseño propuestos; sus controles concretos quedan para TDD-07.

## 8. Operaciones no expuestas en esta versión

- Ejecutar comandos PowerShell enviados por cliente o modelo.
- Lanzar reparaciones críticas automáticamente.
- Administrar o controlar agentes remotos.
- Transmitir archivos EVTX a un proveedor externo sin política y configuración explícitas.

## 9. Pendientes de decisión

- Framework y ubicación de alojamiento de la API.
- Bind local, puerto y política de acceso desde otros equipos.
- Autenticación y autorización.
- Transporte de carga de archivos y límites máximos.
- Procesamiento síncrono o asíncrono, cancelación y progreso.
- Esquemas finales alineados con TDD-02 y TDD-05.
- Formato de informe y descarga.
- Códigos HTTP, límites de tasa, paginación y versionado.

## 10. Estado

Este contrato lógico constituye la línea base aprobada para continuar el diseño. Los detalles marcados como pendientes requieren definición antes de implementar la API. No representa una API implementada. Implementación y pruebas pendientes.

