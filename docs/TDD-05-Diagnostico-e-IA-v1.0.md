Set-PSReadLineOption: C:\Users\arnay\SynologyDrive\Documents\PowerShell\Microsoft.PowerShell_profile.ps1:3
Line |
   3 |  Set-PSReadLineOption -PredictionViewStyle ListView
     |  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     | The handle is invalid.
# TDD-05 — Diagnóstico e IA

**Proyecto:** Windows Autonomous IT Diagnostic  
**Versión:** 1.0  
**Estado:** Aprobado por el usuario como línea base inicial el 12 de noviembre de 2026  
**Implementación y pruebas:** Pendientes

## 1. Propósito

Definir el flujo lógico para convertir eventos extraídos de archivos EVTX en hallazgos y recomendaciones comprensibles, con asistencia de reglas y de un proveedor de IA configurable.

## 2. Decisiones registradas

- El producto analiza registros EVTX y clasifica errores y alertas.
- La primera etapa prioriza análisis offline.
- Ollama se ejecuta en la PC de administración, equipada con GPU NVIDIA.
- Se contempla un proveedor externo por API como opción configurable.
- El diagnóstico debe estar estructurado y producir informes.
- La IA no puede ejecutar PowerShell arbitrario ni aplicar reparaciones críticas automáticamente.

## 3. Flujo de diagnóstico propuesto

1. Ingerir y validar los datos EVTX.
2. Extraer campos disponibles y conservar referencia al registro original.
3. Normalizar y clasificar eventos conforme a una taxonomía versionada.
4. Agrupar eventos relacionados cuando existan criterios definidos.
5. Aplicar reglas deterministas para patrones conocidos.
6. Preparar contexto limitado y relevante para el proveedor de IA.
7. Validar la salida estructurada y vincular cada hallazgo con evidencia.
8. Emitir recomendaciones separadas de cualquier acción ejecutable.
9. Pasar candidatos a reparación por el motor de políticas definido en TDD-06.
10. Presentar diagnóstico, nivel de certeza, evidencia y limitaciones en la interfaz e informe.

Las reglas específicas de agrupación y clasificación aún no están definidas.

## 4. Proveedores de IA

### 4.1 Ollama

Proveedor local previsto en la PC de administración con GPU NVIDIA. Se deben definir modelo, versión, endpoint, límites de recursos, disponibilidad y política de actualización.

### 4.2 API externa opcional

Debe poder habilitarse o deshabilitarse mediante configuración explícita. Antes de usarlo se debe definir qué datos pueden transmitirse, cómo se protegen las credenciales, qué proveedor está permitido y qué aviso requiere el operador. No se asume que archivos EVTX completos puedan enviarse fuera del equipo o de la red.

### 4.3 Independencia del proveedor

El flujo interno debe depender de un contrato común de entrada y salida para los proveedores. Si no hay proveedor disponible, el producto debe poder informar el estado y usar las reglas deterministas disponibles; los detalles del modo degradado quedan por especificar.

## 5. Salida de diagnóstico propuesta

Cada hallazgo debería incluir:

- título y descripción;
- categoría y severidad normalizadas;
- evidencia: referencias a eventos y campos que respaldan la interpretación;
- hipótesis de causa, separada de hechos observados;
- nivel de confianza y explicación de sus límites;
- recomendación para el operador;
- indicador de si se requiere intervención humana;
- procedencia: regla, proveedor/modelo y versión cuando corresponda.

Los campos finales y escalas deben alinearse con TDD-02 antes de implementar.

## 6. Reglas de calidad

- Distinguir hechos presentes en los eventos de inferencias del diagnóstico.
- No presentar correlación como causalidad demostrada.
- No afirmar que se resolvió un problema sin evidencia de verificación.
- Conservar referencias suficientes para que el operador pueda revisar el evento original.
- Validar esquema, longitud y contenido de la salida del modelo.
- Tratar mensajes EVTX como datos no confiables; no permitir que su contenido cambie instrucciones del sistema o políticas.
- Informar cuando falten datos, el proveedor falle o la conclusión sea incierta.

## 7. Seguridad y privacidad

- La salida de IA no se interpreta como autorización de ejecución.
- Las credenciales de proveedores externos no se guardan en informes ni logs ordinarios.
- Se debe aplicar minimización de datos antes de cualquier llamada externa.
- El uso externo debe requerir configuración habilitada y respetar la política de datos por definir.
- El diagnóstico debe tolerar entradas malformadas y contenido de eventos potencialmente hostil.

## 8. Evaluación y trazabilidad

Antes del despliegue se deberán preparar conjuntos de EVTX de prueba, casos esperados, mediciones de falsos positivos/negativos y revisión humana de recomendaciones. Los umbrales de calidad, criterios de aceptación y proceso de actualización de modelos están pendientes.

## 9. Pendientes de decisión

- Taxonomía de eventos, severidad y reglas de clasificación.
- Modelos y versiones soportados en Ollama.
- Proveedor externo permitido y condiciones de uso.
- Qué campos se envían al modelo y política de redacción de datos.
- Esquema final de respuesta y umbrales de confianza.
- Reglas de correlación, deduplicación y priorización.
- Comportamiento al fallar Ollama o la API externa.
- Métricas de calidad y validación de recomendaciones.
- Retención y auditoría de prompts y respuestas.

## 10. Estado

Este documento establece la línea base aprobada v1.0. Los detalles indicados como pendientes requieren definición antes de implementar la integración. No implica que exista una integración funcional. Implementación y pruebas pendientes.

