Set-PSReadLineOption: C:\Users\arnay\SynologyDrive\Documents\PowerShell\Microsoft.PowerShell_profile.ps1:3
Line |
   3 |  Set-PSReadLineOption -PredictionViewStyle ListView
     |  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     | The handle is invalid.
# Windows Autonomous IT Diagnostic

Proyecto para analizar registros EVTX de Windows, diagnosticar errores y alertas, y orientar su resolución con controles de seguridad. El diseño contempla análisis offline en la primera etapa y conexión con PCs remotas mediante agentes en un hito posterior.

## Estado

Los documentos RPD v0.1 y TDD-01 a TDD-07 v0.1 (TDD-05 v1.0) están aprobados como línea base documental. **La implementación y las pruebas siguen pendientes.** La aprobación de documentos no implica que exista software funcional.

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

