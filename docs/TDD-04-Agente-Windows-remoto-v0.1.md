# TDD-04 — Agente Windows remoto

**Proyecto:** Windows Autonomous IT Diagnostic  
**Versión:** 0.1  
**Estado:** Aprobado por el usuario como diseño de línea base para un hito posterior el 12 de noviembre de 2026  
**Implementación y pruebas:** Pendientes

## 1. Propósito

Registrar cómo se debe preparar el proyecto para una futura conexión a PCs Windows remotas mediante agentes, sin dar por definida ni implementada esa conexión. La primera etapa se concentra en el análisis offline de EVTX.

## 2. Decisiones confirmadas

- El producto debe permitir conectarse a una PC remota.
- La arquitectura es mixta y debe admitir agentes Windows.
- La operación de agentes remotos queda para un hito posterior al MVP de análisis offline.
- El equipo central usa Windows 11 y se instala directamente.
- No se permite ejecución arbitraria de PowerShell ni reparaciones críticas automáticas.

## 3. Alcance de este documento

Este documento establece principios y preguntas de diseño. No especifica un protocolo, instalador, servicio, lista de comandos, mecanismo de actualización ni interfaz de agente definitiva. Ningún agente se considera implementado.

## 4. Responsabilidades futuras del agente

Las responsabilidades concretas deben decidirse. Como mínimo, el diseño deberá aclarar si el agente:

- recopila eventos o exporta registros para análisis;
- reporta identidad, versión y estado de salud;
- recibe solicitudes de diagnóstico;
- ejecuta acciones de remediación permitidas bajo políticas.

La capacidad de remediación no se asume habilitada. La recopilación de datos puede ser el único alcance inicial del agente remoto.

## 5. Principios de seguridad propuestos

- Autenticar de forma única cada equipo/agente y permitir revocar su acceso.
- Cifrar las comunicaciones y validar la identidad del servidor.
- Aplicar mínimo privilegio al proceso del agente.
- No aceptar scripts libres ni comandos arbitrarios.
- Aceptar solo operaciones definidas, versionadas y autorizadas por política.
- Registrar solicitudes, resultados y errores de manera auditable.
- Evitar almacenar credenciales reutilizables en texto claro.
- Proteger la actualización del agente y comprobar la integridad de los paquetes.
- Definir límites de datos recopilados y retención antes de habilitar transmisión.

Estos principios son propuestas para el diseño detallado de TDD-07; no equivalen a una selección de tecnología aprobada.

## 6. Ciclo de comunicación conceptual

```text
PC administradora / servicio central
        │ solicita operación identificada y autorizada
        ▼
Canal autenticado y cifrado (mecanismo pendiente)
        ▼
Agente Windows remoto
        │ valida identidad, política y parámetros
        ├── rechaza solicitud no permitida
        └── ejecuta solo capacidades explícitas cuando se habiliten
        ▼
Resultado estructurado y registro de auditoría
```

El diagrama es conceptual y no establece una conexión persistente o iniciada desde un lado específico.

## 7. Capacidades y límites

Antes de implementar, deberá existir una lista permitida de capacidades con sus permisos, entradas, salidas y nivel de riesgo. La operación remota no debe convertirse en una consola remota genérica. Las reparaciones críticas no se ejecutarán automáticamente.

## 8. Despliegue y administración pendientes

- Método de instalación y actualización del agente.
- Requisitos de privilegios y cuenta de servicio.
- Compatibilidad de ediciones y versiones de Windows.
- Descubrimiento y registro de nuevos equipos.
- Revocación, desconexión y desinstalación.
- Operación en redes segmentadas, VPN o sin conectividad entrante.
- Gestión de certificados o credenciales y su ciclo de vida.
- Monitoreo de versiones y estado del parque de agentes.

## 9. Decisiones necesarias para el hito remoto

1. ¿El agente será solo de lectura en su primera versión remota?
2. ¿Qué transporte se utilizará (por ejemplo, WinRM, HTTPS iniciado por el agente u otro)?
3. ¿Quién instala el agente y con qué privilegios?
4. ¿Cómo se enrolan, autentican y revocan los equipos?
5. ¿Qué datos y canales de eventos se recopilan?
6. ¿Qué acciones, si alguna, podrán ejecutarse remotamente tras aprobación humana?
7. ¿Cómo se firma y distribuye el agente y sus actualizaciones?

## 10. Estado

Documento de preparación aprobado para un hito posterior. La conexión remota no está implementada ni probada. Implementación y pruebas pendientes.

