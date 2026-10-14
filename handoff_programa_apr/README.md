# Handoff: Programa APR — Puerto Ventanas S.A.

Plataforma para programar las actividades diarias de los prevencionistas de riesgo (APR) por turno, registrar su ejecución con evidencia, levantar novedades del turno y cerrar con un reporte automático por correo. Digitaliza la planilla `insumos-ejemplo/programa-actividades-apr-1er-turno-14-10.xlsx`.

## Sobre estos archivos
`prototipo.dc.html` es una **referencia de diseño en HTML** (prototipo navegable), **no código de producción**. La tarea es **recrearlo en el stack del builder** (recomendado: React + TypeScript, BD relacional, auth por roles, almacenamiento de archivos y envío de correo). Se abre en un navegador con `support.js` al lado.

## Fidelidad
**Alta fidelidad**: colores, tipografía, espaciado e interacciones finales.

## Design tokens (compartidos)
- **Teal marca (logo)** `#00ABAC` · **Teal UI (botones/texto, AA)** `#007F80` · hover/texto teal `#005F60` · teal suave `#E2F4F4` / `#E8F5F5`
- **Sidebar / superficies oscuras** `#0E2A2B` · activo `#1B4A4A` · borde oscuro `#1F4344` · texto sidebar `#A9C1C0` / etiquetas `#6F9493` / avatar texto `#7FD6D6`
- **Fondo app** `#EDF2F1` con degradado superior `linear-gradient(180deg,#DCECEA 0,#EDF2F1 260px)`
- **Texto** `#1C2526` · secundario `#3C4849` · terciario `#6B7778` · bordes `#DDE5E4` / `#E3E7E7` / divisores `#EEF1F1` / inputs `#CDD4D4`
- **Ámbar (pendiente/aviso)** fondo `#FBF3DC` / `#FBFAF6`, texto `#7A5D0E`, barra `#C49A1C`, borde `#C9B98A`
- **Rojo (no ejecutado / no negativo)** `#9B3B2E`, botón `#B9574A`, fondo `#F6E4E0`
- **Tipografía**: IBM Plex Sans (UI 13–15px, 400/500/600), IBM Plex Mono (códigos, horas, cifras; eyebrows 12px +0.08em), Newsreader (títulos de pantalla 34px/1.1, títulos de tarjeta 20–24px)
- **Tarjetas**: fondo `#FFF`, borde 1px `#DDE5E4`, radio 10px, sombra `0 1px 2px rgba(14,42,43,.05), 0 6px 18px rgba(14,42,43,.04)`. KPIs con `border-top: 3px solid #00ABAC`.
- **Chips**: radio 14px, 5–7px × 11–12px; seleccionado = fondo y borde `#007F80`, texto blanco.
- **Layout**: sidebar fijo 232px (sticky, 100vh) + contenido fluido (padding 28px 32px), columnas con `flex-wrap` y panel lateral derecho 280–400px.


## Funcionalidad

### Modelo
- **Catálogo** (33 actividades de la hoja Dato): área, sitio/sector/función, proceso, descripción y **tipo**: `transversal` (todos los turnos), `según operación` (depende del correo de Operaciones), `según ocurrencia` (emergencias, eventos meteorológicos — opción "No ocurrió" que **no** cuenta para el cumplimiento).
- **Programación por fecha + turno**: asesor asignado + set de actividades + estado (borrador/publicada hh:mm).
- **Ejecución por actividad**: `ejecutado | no_ejecutado | no_ocurrio`, motivo (obligatorio si no ejecutado: Sin operación, Atención de emergencia, Prioricé otra actividad, Condición climática, Sin acceso o permiso, Otro), observaciones, evidencias (fotos/PDF), hora.
- **Cumplimiento** = ejecutadas / (programadas − no ocurrió).
- **Novedades**: lugar, tipo (Condición insegura, Acto inseguro, Incidente/cuasi accidente, Operacional, Medioambiente, Aviso al siguiente turno), criticidad (Baja/Media/Alta), descripción, **quién estuvo** (sugerido desde la nombrada del lugar/turno: jefe de nave + códigos; más texto libre), acción inmediata, evidencia.
- **Actividades mínimas mensuales** por asesor (Programa personalizado): charlas 5 min (4, registro físico), observación de conducta (4, SAFEX conductual), inspección planeada (4, SAFEX reportabilidad), inspección EPP (4, SAFEX listas de chequeo), reporte cuasi-accidentes (cada vez).

### Pantallas
1. **Programación** (Jefatura): selector de turno, asesor, catálogo por área con checkboxes y etiqueta de tipo; panel "Correo de Operaciones" con líneas detectadas y sugerencias; actualizaciones del día (p. ej. 13:10 atraca nave en Sitio 2) → "Aplicar sugerencias (+N)"; importar desde Excel; Publicar (notifica al asesor; republicar queda en bitácora).
2. **Mi turno** (asesor): KPIs (programadas, ejecutadas, no ejecutadas, % cumplimiento), tarjetas por grupo (Transversales / Según operación / Según ocurrencia) con borde izquierdo de color por estado; botones Ejecutado / No ejecutado / No ocurrió; evidencia + observaciones o motivo; panel lateral de cierre y mínimos del mes.
3. **Novedades y cierre**: listado + formulario de novedad; resumen; "Falta registrar"; entrega al siguiente turno; **Cerrar turno y enviar reporte** (bloqueado mientras falten registros). Al cerrar: correo automático a Jefatura de Prevención, Gerencia de Operaciones y asesor del turno siguiente, con **planilla Excel** (mismo formato que la original + motivo, evidencia, novedades, entrega) y **PDF**.
4. **Reportes**: KPIs del mes, motivos de no ejecución, cumplimiento por asesor, tabla de turnos cerrados con descarga Excel/PDF y consolidado mensual.

Prop: `exigirEvidencia` (boolean) — si true, "Ejecutado" sin evidencia bloquea el cierre.

---


## Integraciones pendientes (definir con el cliente)
- Lectura del correo de Operaciones (buzón / Microsoft Graph) para generar las sugerencias de programación.
- Lectura de la nombrada diaria (PDF) para sugerir "quién estuvo" en cada lugar.
- Envío de correo del reporte de cierre (SMTP corporativo / Microsoft 365).
- SAFEX (verificadores de actividades mínimas).
- Roles: Jefatura de Prevención (programa y ve todo), Asesor de turno (ejecuta y cierra), Gerencia (reportes).

## Datos de ejemplo
Avance del 1° turno precargado, historial 1–13 oct y correos de Operaciones simulados. Catálogo de actividades y nombres de asesores tomados de la planilla del cliente.

## Capturas (`capturas/`)
1. `01-mi-turno.png` — Mi turno (1° turno): KPIs y actividades transversales
2. `02-mi-turno-operacion.png` — Mi turno: actividades según operación, motivo de no ejecución
3. `03-novedades-cierre.png` — Novedades y cierre
4. `04-programacion-t1.png` — Programación 1° turno
5. `05-programacion-t2.png` — Programación 2° turno con actualización del correo
6. `06-reportes.png` — Reportes del mes

## Archivos
```
prototipo.dc.html      ← referencia principal
support.js             ← runtime para abrir el .dc.html
assets/pvsa-logo.png
capturas/
insumos-ejemplo/       ← planilla APR original y nombrada de ejemplo
```
