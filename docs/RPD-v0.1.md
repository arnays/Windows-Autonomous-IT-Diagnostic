# Windows Autonomous IT Diagnostic

## Requisitos del producto (RPD) v0.1

**Estado:** Aprobado por el usuario como línea base inicial el 12 de noviembre de 2026.  
**Implementación y pruebas:** Pendientes.

## 1. Propósito

Definir un sistema que ayude a analizar registros de eventos de Windows, detectar errores y alertas, orientar su diagnóstico y facilitar su resolución. El diseño debe contemplar tanto análisis local de archivos como la posibilidad de trabajar con una PC remota.

## 2. Contexto y decisiones confirmadas

Los siguientes puntos aparecen en el historial recuperable del proyecto:

- Nombre del proyecto: **Windows Autonomous IT Diagnostic**.
- El sistema debe leer archivos de registro de Windows `.evtx` y analizar eventos clasificados como errores y alertas.
- Se busca que el sistema proponga y, cuando sea posible y seguro, aplique soluciones automáticamente. Debe informar al usuario cuando necesite intervención.
- Debe permitir conectarse a una PC remota.
- Infraestructura elegida: arquitectura mixta.
- Equipo central: Windows 11, instalación directa.
- Ollama se ejecutará en la PC de administración, que cuenta con GPU NVIDIA.
- La primera etapa prioriza el análisis offline de archivos EVTX; los agentes remotos quedan preparados para un hito posterior.
- Se indicó que no se debe permitir ejecución arbitraria de PowerShell ni reparaciones críticas automáticas.

## 3. Objetivos del producto

1. Permitir al operador proporcionar registros EVTX para su análisis.
2. Identificar y presentar eventos de error y advertencia con su contexto disponible.
3. Ayudar a diagnosticar problemas y presentar recomendaciones comprensibles.
4. Automatizar únicamente las acciones de reparación que se definan como seguras y autorizadas.
5. Señalar claramente cuándo se requiere una decisión o intervención humana.
6. Incorporar conexión a equipos remotos según un mecanismo y alcance todavía por definir.

## 4. Alcance inicial registrado

- Ejecución central en Windows 11 mediante instalación directa, sin Docker.
- Interfaz web con un cuestionario inicial interactivo.
- Importación y análisis offline de archivos EVTX.
- Clasificación de errores y advertencias.
- Integración configurable con Ollama; proveedor externo por API opcional.
- Diagnóstico estructurado, políticas de seguridad, auditoría e informes.
- Preparación documental para agentes Windows remotos; la operación remota queda para un hito posterior.

Estos puntos forman parte del alcance de la línea base aprobada. Las decisiones de implementación que aún no están detalladas se mantienen como pendientes de diseño.

## 5. Límites de seguridad registrados

- No ejecutar comandos arbitrarios de PowerShell.
- No aplicar automáticamente reparaciones críticas.
- Mantener informado al usuario cuando se requiera intervención.

Los niveles de riesgo, catálogo de acciones, aprobación previa, reversión y controles de acceso todavía no están especificados.

## 6. Fuera de alcance confirmado para la primera etapa

- Despliegue mediante Docker.
- Reparaciones críticas automáticas.
- Operación completa de agentes remotos en el MVP inicial; se dejó para un hito posterior.

## 7. Preguntas necesarias para cerrar el RPD

1. ¿Qué significa “alertas” en el producto: eventos de nivel Warning, eventos de auditoría u otras categorías además de errores?
2. ¿El sistema debe solo recomendar reparaciones inicialmente o puede ejecutar acciones no críticas tras aprobación?
3. ¿Qué mecanismo remoto se espera usar (por ejemplo, agente instalado, WinRM u otro) y en qué hito debe estar disponible?
4. ¿Qué formatos de informe y conservación de resultados se necesitan?
5. ¿Los archivos EVTX pueden contener datos sensibles y deben permanecer siempre dentro de la red local?
6. ¿Qué idiomas debe presentar la interfaz y los informes?

## 8. Estado del proyecto

Este documento establece la línea base aprobada del RPD v0.1 para el reinicio del trabajo. Las preguntas abiertas identifican detalles que requieren definición durante el diseño y no invalidan la aprobación del alcance documentado. La implementación y las pruebas siguen pendientes.

