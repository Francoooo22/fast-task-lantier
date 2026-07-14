# Changelog - Fast Task Lantier Pro

Todos los cambios notables en este proyecto se documentan en este archivo.

## [v2.0] - 2026-07-14

### 🔴 CRÍTICO - Arreglado
- **Reemplazados prompt() nativos**: Los diálogos `prompt()` en "Nueva lista" y "Agregar tarjeta" ahora son modales propios
  - No bloquean la interfaz
  - Mobile-friendly
  - Manejo correcto de cancelación
  - Commit: `8e54010`

### 🟠 ALTO - Arreglado
- **Validación de nombres**: Tableros y listas no pueden tener nombres vacíos
  - Validación en `createTablero()`, `confirmCreateLista()`, `confirmCreateTarjeta()`
  - Mensajes de error claros

- **Eliminar/Renombrar**: Ahora es posible gestionar tableros y listas
  - Modal `editTableroModal` mejorado con opción de renombrar y eliminar
  - Nuevo modal `editListaModal` para renombrar/eliminar listas
  - Botón menú (⋮) en headers de listas
  - Confirmaciones antes de eliminar
  - Funciones: `updateTablero()`, `deleteTablero()`, `updateLista()`, `deleteLista()`

### 🟡 MEDIO - Arreglado
- **Búsqueda global funcional**: `performSearch()` ahora filtra y renderiza correctamente
  - Busca en título y descripción de tarjetas
  - Muestra contador de resultados
  - Funciona en tiempo real
  - Commit: `3efed71`

- **Filtros funcionales**: Filtrar por etiqueta/usuario ahora oculta tarjetas correctamente
  - Soporta múltiples filtros simultáneamente
  - Visual clara de filtros activos
  - Toast de confirmación
  - Commit: `7cefaef`

### 🔒 SEGURIDAD - Arreglado
- **Restricción de usuarios por tablero**:
  - Campo `miembros` agregado a cada tablero en Firestore
  - Panel "Asignados" muestra solo miembros del tablero actual
  - Emails legibles en lugar de IDs internos
  - UI para gestionar miembros (agregar/remover)
  - Creador del tablero es automáticamente miembro
  - Migración automática de datos antiguos
  - Commit: `d647644`

### 🎯 Mejoras de UX
- Modales no-bloqueantes y estilizados
- Validaciones de entrada integradas
- Mensajes de confirmación antes de operaciones destructivas
- Contador de resultados en búsqueda
- Estado visual de filtros activos
- Panel de miembros del tablero en settings

### 📊 Cambios de Archivo
- `index.html`: +250 líneas (modales, validaciones, funciones nuevas)
- `README.md`: Actualizado con cambios recientes
- `DEVELOPMENT.md`: Documentación de cambios y estado actual
- `CHANGELOG.md`: Este archivo (tracking de versiones)

### 🚀 Deploy
- Versión deployada en Firebase Hosting: https://fast-task-lantier.web.app
- 26 archivos nuevos subidos
- Versión finalizada y activa

---

## [v1.0] - 2026-07-08

### ✅ Inicial - MVP Funcional
- Autenticación con Firebase
- Sistema Kanban con tableros, listas y tarjetas
- Drag & drop de tarjetas
- Etiquetas personalizadas
- Adjuntos (imágenes/archivos)
- Comentarios
- Asignaciones de usuarios
- Fechas de vencimiento
- Búsqueda global (no funcional)
- Filtros (no funcionales)
- Cambio de color de tableros
- Deploy inicial en Firebase Hosting

---

## Roadmap Futuro

### v2.1 (Próximo)
- [ ] Checklists/subtareas dentro de tarjetas
- [ ] Historial de cambios
- [ ] Notificaciones por email
- [ ] Reglas de seguridad restrictivas en Firestore

### v3.0 (Futuro)
- [ ] Vistas alternativas (Timeline, Calendar)
- [ ] Roles y permisos (admin, editor, viewer)
- [ ] Tableros compartidos con privacidad
- [ ] Integración con calendarios
- [ ] Automatizaciones (reglas, triggers)
- [ ] Tema oscuro/claro

### Refactor
- [ ] Separar HTML, CSS, JS en archivos independientes
- [ ] Tests unitarios y E2E
- [ ] Build process (bundling, minificación)
- [ ] CI/CD automation
- [ ] Analytics (Google Analytics)

---

**Formato**: [SEMANTIC VERSIONING](https://semver.org/)  
**Última actualización**: 14 Julio 2026
