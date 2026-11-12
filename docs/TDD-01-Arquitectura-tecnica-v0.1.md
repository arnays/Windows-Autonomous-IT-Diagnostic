Set-PSReadLineOption: C:\Users\arnay\SynologyDrive\Documents\PowerShell\Microsoft.PowerShell_profile.ps1:3
Line |
   3 |  Set-PSReadLineOption -PredictionViewStyle ListView
     |  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     | The handle is invalid.
# TDD-01 — Arquitectura técnica

**Proyecto:** Windows Autonomous IT Diagnostic  
**Versión:** 0.1  
**Estado:** Aprobado por el usuario como línea base inicial el 12 de noviembre de 2026  
**Implementación y pruebas:** Pendientes

## 1. Propósito

Describir la arquitectura inicial del sistema para analizar archivos EVTX en una PC Windows 11 y establecer las decisiones confirmadas para la primera versión. Los elementos sin decisión explícita quedan identificados como pendientes.

## 2. Decisiones confirmadas

- Arquitectura mixta.
- Equipo central con Windows 11.
- Instalación directa en Windows; sin Docker.
- Ollama se ejecuta en la PC de administración equipada con GPU NVIDIA.
- El MVP prioriza análisis offline de archivos EVTX.
- La arquitectura debe poder evolucionar para conectar agentes Windows remotos; la operación de esos agentes queda para un hito posterior.
- No se permite ejecución arbitraria de PowerShell ni reparación automática de acciones críticas.

## 3. Vista lógica inicial

```text
Operador
  │
  ▼
Interfaz web y cuestionario inicial
  │
  ▼
Servicio central en Windows 11
  ├── Ingesta y análisis offline de EVTX
  ├── Clasificación y diagnóstico estructurado
  ├── Motor de políticas y control de acciones
  ├── Auditoría e informes
  └── Adaptador de proveedor de IA
        ├── Ollama en PC de administración (GPU NVIDIA)
        └── Proveedor externo por API (opcional, configurable)

Hito posterior: agente Windows remoto (protocolo y límites pendientes)
```

Este diagrama expresa únicamente el alcance registrado. No fija tecnologías concretas de interfaz, servidor web, almacenamiento o protocolo de comunicación.

## 4. Componentes y responsabilidades

### 4.1 Interfaz web

Presenta el cuestionario inicial, permite iniciar el flujo de análisis y muestra resultados. El diseño visual, autenticación, idiomas y perfiles de usuario están pendientes de especificación.

### 4.2 Servicio central

Se ejecuta en Windows 11 mediante instalación directa. Coordina la recepción de archivos EVTX, el análisis, el diagnóstico, la aplicación de políticas y la generación de informes. El framework y el modelo de despliegue como servicio o aplicación interactiva están pendientes.

### 4.3 Ingesta y análisis EVTX

Permite analizar archivos EVTX localmente, sin requerir conexión a un equipo remoto. Los límites de tamaño, fuentes de eventos, retención y estrategia de parseo se especificarán en TDD posteriores.

### 4.4 Diagnóstico e IA

La arquitectura admite Ollama en la PC de administración y contempla un proveedor API externo opcional y configurable. La IA no debe ejecutar acciones directamente; cualquier acción propuesta debe pasar por las políticas del sistema. Modelos, prompts, manejo de datos y comportamiento ante indisponibilidad del proveedor están pendientes.

### 4.5 Motor de políticas

Evalúa cualquier reparación propuesta antes de ejecutarla. La versión inicial excluye reparaciones críticas automáticas y comandos arbitrarios de PowerShell. El catálogo de acciones permitidas, niveles de riesgo, aprobación humana y reversión están pendientes.

### 4.6 Auditoría e informes

El sistema debe contemplar auditoría e informes según el alcance del MVP. Campos, formatos, almacenamiento, acceso y retención todavía no están definidos.

### 4.7 Agente remoto

La arquitectura debe poder incorporar agentes Windows, pero la conexión y operación remota se difieren a un hito posterior. No se selecciona aún protocolo, método de instalación, identidad del agente ni canal de actualización.

## 5. Flujos principales

### Análisis offline inicial

1. El operador completa el cuestionario inicial.
2. Selecciona o proporciona un archivo EVTX.
3. El servicio central analiza y clasifica eventos.
4. El sistema construye un diagnóstico y recomendaciones.
5. El motor de políticas determina si existe una acción permitida y qué intervención humana requiere.
6. El sistema presenta el resultado y produce un informe/auditoría conforme a las reglas que se definan.

Los detalles de cada paso y los formatos de intercambio quedan para TDD-02 a TDD-06.

## 6. Límites y principios de seguridad

- La IA produce análisis o propuestas; no recibe una vía de ejecución libre.
- Toda acción de reparación debe estar sujeta a políticas explícitas.
- Se excluyen la ejecución arbitraria de PowerShell y la reparación automática de acciones críticas.
- La conexión remota no forma parte de la operación del primer hito descrito.
- El envío de EVTX a un proveedor externo no se asume como permitido; requiere una decisión explícita de privacidad y configuración.

## 7. Decisiones pendientes

- Framework de la interfaz web y del servicio central.
- Límites entre procesos y mecanismos internos de comunicación.
- Almacenamiento de configuración, resultados, auditoría e informes.
- Autenticación y autorización local.
- Si el servicio se ejecuta como servicio Windows o bajo sesión del operador.
- Versiones de Windows compatibles además de Windows 11.
- Modelos de Ollama y proveedor externo permitido.
- Política de datos para IA local y externa.
- Protocolo, despliegue y seguridad de agentes remotos para el siguiente hito.

## 8. Relación con otros documentos

- RPD v0.1: requisitos y alcance de producto.
- TDD-02: modelo de datos.
- TDD-03: API e interfaces entre componentes.
- TDD-04: agente Windows remoto.
- TDD-05: diagnóstico e IA.
- TDD-06: reparaciones autónomas y políticas.
- TDD-07: seguridad integral.

## 9. Estado

Este TDD constituye la línea base aprobada para las decisiones de arquitectura indicadas. Los elementos señalados como pendientes requieren definición antes de su implementación. Implementación y pruebas pendientes.

