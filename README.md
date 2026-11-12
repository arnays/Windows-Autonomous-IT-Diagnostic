# Windows Autonomous IT Diagnostic

Proyecto para analizar registros EVTX de Windows, diagnosticar errores y alertas, y orientar su resolución con controles de seguridad. El diseño contempla análisis offline en la primera etapa y conexión con PCs remotas mediante agentes en un hito posterior.

## Estado

Los documentos RPD v0.1 y TDD-01 a TDD-07 v0.1 (TDD-05 v1.0) están aprobados como línea base documental. Hay un MVP inicial para carga y análisis offline de EVTX. **La implementación todavía requiere validación y las pruebas siguen pendientes.**

## Decisiones de infraestructura registradas

- Arquitectura mixta.
- Equipo central Windows 11 con instalación directa, sin Docker.
- Ollama en la PC de administración con GPU NVIDIA.
- Análisis offline de archivos EVTX como prioridad del MVP.
- Proveedor externo de IA por API opcional y configurable.
- Agentes Windows remotos para un hito posterior.
- Sin ejecución arbitraria de PowerShell ni reparaciones críticas automáticas.

## Documentación

Consulta el [índice de documentos](docs/INDEX.md) para versiones, estados y enlaces a las especificaciones.

## Alcance actual

La documentación define los requisitos y propone la arquitectura, el modelo de datos, la API, el diagnóstico, las reparaciones y la seguridad. Los elementos todavía no fijados se señalan dentro de cada documento como decisiones pendientes.

## Inicio en Windows

Ejecuta `run-windows.bat` desde la raíz del proyecto. El script crea un entorno virtual, instala dependencias e inicia la interfaz en `http://127.0.0.1:8000`. Para solicitar análisis local con Ollama, configura `WAITD_OLLAMA_MODEL` antes de iniciar. El proveedor externo no está implementado en este MVP.

