Set-PSReadLineOption: C:\Users\arnay\SynologyDrive\Documents\PowerShell\Microsoft.PowerShell_profile.ps1:3
Line |
   3 |  Set-PSReadLineOption -PredictionViewStyle ListView
     |  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     | The handle is invalid.
# TDD-06 — Reparaciones autónomas y motor de políticas

**Proyecto:** Windows Autonomous IT Diagnostic  
**Versión:** 0.1  
**Estado:** Aprobado por el usuario como línea base inicial el 12 de noviembre de 2026  
**Implementación y pruebas:** Pendientes

## 1. Propósito

Definir límites iniciales para evaluar recomendaciones de reparación y controlar si una acción puede proponerse, requerir aprobación o ejecutarse. Este documento no autoriza por sí mismo ninguna reparación concreta.

## 2. Decisiones confirmadas

- El producto busca resolver automáticamente los problemas cuando sea posible y seguro.
- Debe informar al usuario cuando necesite intervención.
- No se permite ejecutar PowerShell arbitrario.
- No se permiten reparaciones críticas automáticas.
- Una recomendación de IA no constituye autorización para ejecutar una acción.

## 3. Estados de una reparación

Estados lógicos propuestos:

1. **Recomendada:** se informa al operador, sin ejecución.
2. **Pendiente de aprobación:** requiere una decisión explícita del operador.
3. **Autorizada:** una política y/o aprobación válida permite solicitar la acción.
4. **En ejecución:** el ejecutor controlado está realizando una acción catalogada.
5. **Completada:** la acción terminó; todavía no implica que el problema esté resuelto.
6. **Verificada:** comprobaciones posteriores respaldan el resultado esperado.
7. **Fallida o cancelada:** la acción no se completó o fue detenida.

La máquina de estados final requiere validación en el diseño de implementación.

## 4. Evaluación de política

Antes de cualquier ejecución, el motor debe comprobar:

- que la acción existe en un catálogo permitido y versionado;
- que la plataforma y el contexto coinciden con sus condiciones;
- que se cuenta con privilegios requeridos y autorización válida;
- que la clasificación de riesgo permite el modo solicitado;
- que los parámetros pasan validación estricta;
- que existe evidencia diagnóstica suficiente según reglas pendientes;
- que la acción no está bloqueada por política;
- que se puede registrar el resultado y aplicar verificaciones definidas.

Si una condición no se cumple, la acción debe rechazarse o quedar pendiente de intervención humana.

## 5. Catálogo de acciones

Las acciones deben ser operaciones específicas con identificador, descripción, parámetros tipados, permisos, clasificación de riesgo, condiciones previas, efectos, verificación posterior y reversión cuando sea posible. No se aceptarán cadenas de comandos o scripts provistos por el modelo, los eventos o la interfaz.

El catálogo inicial está pendiente de selección y revisión. No se declara aquí ninguna acción como segura para autoejecución.

## 6. Riesgo, aprobación y automatización

- Las acciones críticas no se ejecutan automáticamente.
- El nivel y la definición de “crítica” deben establecerse en una taxonomía de riesgo.
- El modo de aprobación para acciones no críticas aún está pendiente.
- La automatización debe poder deshabilitarse globalmente y por tipo de acción.
- Una aprobación debe referirse a una acción, objetivo y parámetros concretos; no ser una autorización genérica.
- Las acciones destructivas, irreversibles o que afecten seguridad/disponibilidad deben escalarse a revisión humana según política.

## 7. Ejecutor controlado

El ejecutor debe aceptar únicamente identificadores de acción conocidos y parámetros validados. No debe ofrecer una consola genérica, un intérprete de scripts ni una interfaz de PowerShell libre. La identidad, privilegios, aislamiento, límites de tiempo y mecanismos de parada requieren diseño posterior.

## 8. Auditoría y verificación

Para cada decisión de política y acción se propone registrar:

- análisis y hallazgo relacionados;
- acción y versión del catálogo;
- parámetros no sensibles o representación segura de ellos;
- política y reglas aplicadas;
- identidad del operador/aprobador cuando corresponda;
- fecha y hora de solicitud, aprobación, inicio y resultado;
- resultado, errores y verificación posterior;
- razón de rechazo, cancelación o escalamiento.

No guardar secretos o datos sensibles innecesarios en la auditoría. Retención y acceso se definen en TDD-07.

## 9. Fallas y recuperación

- No reintentar automáticamente acciones de resultado incierto sin evaluar idempotencia.
- Informar si una acción quedó en estado desconocido.
- No declarar el problema resuelto solo porque el ejecutor terminó sin error.
- Cuando exista reversión, comprobar sus condiciones y registrar su resultado.
- La estrategia de recuperación por acción debe formar parte de su definición en el catálogo.

## 10. Pendientes de decisión

- Taxonomía formal de riesgo y definición de acción crítica.
- Acciones iniciales permitidas y sus condiciones.
- Si una acción no crítica requiere aprobación siempre o admite automatización condicionada.
- Identidad, roles y privilegios del usuario aprobador.
- Verificaciones y mecanismos de reversión por acción.
- Modo de operación en equipo local y futuro equipo remoto.
- Requisitos de pausa global, cancelación y recuperación.
- Retención, integridad y protección de la auditoría.

## 11. Estado

Este documento establece la línea base aprobada v0.1. No activa reparaciones ni define acciones concretas autorizadas para ejecución. El catálogo requiere aprobación específica antes de implementarse. Implementación y pruebas pendientes.

