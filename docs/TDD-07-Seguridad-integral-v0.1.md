# TDD-07 — Seguridad integral

**Proyecto:** Windows Autonomous IT Diagnostic  
**Versión:** 0.1  
**Estado:** Aprobado por el usuario como línea base inicial el 12 de noviembre de 2026  
**Implementación y pruebas:** Pendientes

## 1. Propósito

Establecer una base de seguridad para analizar archivos EVTX, utilizar proveedores de IA, presentar diagnósticos y preparar una futura conexión a PCs remotas. Los controles específicos deberán concretarse antes de implementar o desplegar el sistema.

## 2. Alcance y activos a proteger

- Archivos EVTX, que pueden contener información de usuarios, equipos y actividad.
- Configuración, prompts, respuestas y credenciales de proveedores de IA.
- Hallazgos, informes y registros de auditoría.
- Acciones de reparación y privilegios del proceso que pudiera ejecutarlas.
- Identidad y comunicaciones del servidor central Windows 11.
- En un hito futuro, identidad, estado y canal de cada agente Windows.

## 3. Límites de confianza

- Los archivos EVTX y sus mensajes son datos no confiables, no instrucciones ejecutables.
- Las respuestas del modelo son sugerencias no confiables y deben validarse.
- El proveedor externo es un límite de transferencia de datos que requiere autorización y configuración explícitas.
- La interfaz web es un cliente; no debe poder eludir las validaciones del servicio ni del motor de políticas.
- La futura red de agentes requiere identidad y autorización propias y no se presume confiable por estar en la red interna.

## 4. Controles de entrada y análisis

- Validar extensión, estructura, tamaño y legibilidad de archivos EVTX.
- Tratar los mensajes y metadatos como contenido hostil; neutralizar datos al mostrarlos en la interfaz e informes.
- Limitar consumo de CPU, memoria, disco y tiempo durante parseo y análisis.
- Evitar rutas arbitrarias y acceso fuera de las ubicaciones permitidas.
- Diseñar el parseo para fallar de forma controlada ante archivos truncados o malformados.
- Mantener evidencia y referencias para rastrear hallazgos a registros de origen.

## 5. IA y protección de datos

- Usar Ollama local como proveedor previsto en la PC de administración con GPU NVIDIA.
- Mantener deshabilitado el envío a API externa salvo que el operador la configure expresamente.
- Definir minimización, anonimización o redacción antes de cualquier transmisión externa.
- Proteger secretos mediante almacenamiento apropiado para Windows; nunca incluirlos en código, informes ni logs ordinarios.
- Validar la estructura de respuesta del modelo y separar hechos, inferencias y recomendaciones.
- Evitar que la inyección de instrucciones contenida en registros cambie políticas o solicite acciones.
- Definir retención y acceso a prompts/respuestas antes de registrar su contenido.

## 6. Autorización y reparaciones

- No admitir ejecución arbitraria de PowerShell.
- No permitir reparaciones críticas automáticas.
- Las acciones deben pertenecer a un catálogo explícito, tener parámetros validados y pasar por el motor de políticas.
- La recomendación del modelo no concede permisos ni constituye aprobación humana.
- Aplicar mínimo privilegio al servicio y a cualquier futuro ejecutor.
- Registrar decisiones, aprobaciones, ejecución, cancelación y verificación sin capturar secretos.

## 7. Seguridad de la interfaz y servicio local

- Definir explícitamente interfaces de escucha y limitar por defecto el acceso a la máquina local hasta decidir otro modo.
- Implementar autenticación/autorización si la interfaz puede ser accedida por otros usuarios o equipos.
- Proteger cargas de archivos y formularios contra entradas inválidas, cargas excesivas y solicitudes repetidas.
- Evitar exposición de errores internos, rutas sensibles y secretos.
- Definir CORS, CSRF, gestión de sesión, actualizaciones y configuración segura de producción al seleccionar el alojamiento web.

## 8. Auditoría, informes y retención

- Registrar eventos de seguridad y cambios de configuración relevantes.
- Limitar acceso a informes y auditoría según roles que se definan.
- Proteger integridad, confidencialidad y retención de los registros.
- Minimizar datos personales y sensibles en logs.
- Definir exportación, borrado, respaldo y recuperación.

## 9. Preparación para seguridad remota

Para el hito de agentes será necesario definir como mínimo:

- identidad única y ciclo de vida de certificados/credenciales;
- canal autenticado y cifrado con validación del servidor;
- enrolamiento, revocación y actualización segura;
- permisos mínimos y lista permitida de capacidades;
- defensa ante repetición, suplantación y agentes desactualizados;
- segmentación, conectividad saliente/entrante y respuesta a pérdida del canal.

Este documento no selecciona transporte ni mecanismos criptográficos concretos.

## 10. Amenazas y respuesta

El análisis de amenazas debe cubrir archivos malformados o maliciosos, credenciales expuestas, acceso local no autorizado, abuso de reparaciones, inyección de instrucciones, divulgación a terceros y compromiso de agentes. Deben definirse severidades, respuesta ante incidentes, actualización de dependencias y proceso de vulnerabilidades antes del despliegue.

## 11. Decisiones pendientes

- Modelo de amenazas y clasificación de datos.
- Autenticación, roles y gestión de sesiones.
- Límites de escucha y acceso a la interfaz.
- Protección de secretos en Windows y rotación.
- Proveedor externo permitido y datos transferibles.
- Retención, borrado, respaldo y acceso a auditoría.
- Firma y actualización del software.
- Controles concretos de comunicación y enrolamiento de agentes.
- Requisitos de hardening, monitoreo y respuesta a incidentes.

## 12. Estado

Este documento establece la línea base aprobada v0.1. No representa una evaluación de seguridad completada ni una implementación existente. Los controles concretos señalados como pendientes requieren definición antes del despliegue. Implementación y pruebas pendientes.

