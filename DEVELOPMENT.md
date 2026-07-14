# 🛠️ Guía de Desarrollo

## Configuración del Entorno

### Requisitos
- Python 3+ (para servir archivos localmente)
- Navegador moderno (Chrome, Firefox, Edge, Safari)
- Cuenta de Gmail (para autenticarse)

### Setup Local

```bash
# 1. Clonar y entrar al proyecto
git clone https://github.com/tu-usuario/fast-task-lantier.git
cd fast-task-lantier

# 2. Iniciar servidor local
python3 -m http.server 8000

# 3. Abrir en navegador
open http://localhost:8000
# o simplemente ir a http://localhost:8000
```

## Estructura del Proyecto

```
fast-task-lantier/
├── index.html          # Aplicación SPA completa
├── README.md           # Documentación principal
├── DEVELOPMENT.md      # Esta guía
├── .gitignore          # Archivos a ignorar en Git
└── docs/              # (futuro) Documentación adicional
```

## Arquitectura

El proyecto es una **Single Page Application (SPA)** en un único archivo HTML que contiene:

1. **HTML** - Estructura y componentes
2. **CSS** - Estilos inlineados (140 líneas)
3. **JavaScript ES6** - Lógica de la aplicación (500+ líneas)

### Estructura del Código JavaScript

#### Configuración Firebase
```javascript
const firebaseConfig = { ... };
const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);
const storage = getStorage(app);
```

#### Variables Globales
```javascript
let user = null;
let currentTableroId = null;
let currentTarjetaId = null;
let tableros = [];
let listas = [];
let tarjetas = [];
let usuarios = [];
let filtros = {};
```

#### Funciones Principales

| Función | Propósito |
|---------|-----------|
| `handleLogin(e)` | Autenticación con Firebase |
| `loadData()` | Cargar datos de Firestore |
| `renderTableros()` | Renderizar vista de tableros |
| `renderKanban()` | Renderizar tablero Kanban |
| `openTarjeta(id)` | Abrir modal de tarjeta |
| `createTablero()` | Crear nuevo tablero |
| `createTarjeta(listaId)` | Crear tarjeta en lista |
| `dropTarjeta(e, listaId)` | Mover tarjeta entre listas |

## Flujo de Datos

```
Usuario Login → Firebase Auth → onAuthStateChanged
                                    ↓
                                loadData()
                        ↓ ↓ ↓ ↓
                   Cargar de Firestore
                   (tableros, listas, tarjetas, usuarios)
                        ↓
                   renderTableros() o renderKanban()
                        ↓
                      Mostrar UI
```

## Desarrollo

### Agregar una Nueva Funcionalidad

**Ejemplo: Agregar prioridad a las tarjetas**

1. **Modificar el esquema de Firestore** (en `createTarjeta`):
   ```javascript
   await addDoc(collection(db, 'tarjetas'), {
     // ... campos existentes
     prioridad: 'normal', // Agregar este campo
   });
   ```

2. **Actualizar `renderTarjeta`** para mostrar prioridad:
   ```javascript
   const prioridad = t.prioridad || 'normal';
   return `
     <div class="tarjeta">
       <div style="color: ${getPriorityColor(prioridad)};">▪ ${prioridad}</div>
       ...
   ```

3. **Agregar interfaz en el modal** (`tarjetaModal`):
   ```html
   <select id="tarjetaPrioridad" class="input-text">
     <option value="baja">Baja</option>
     <option value="normal" selected>Normal</option>
     <option value="alta">Alta</option>
   </select>
   ```

4. **Guardar cambios** en `openTarjeta`:
   ```javascript
   document.getElementById('tarjetaPrioridad').onchange = () => 
     updateTarjeta('prioridad', document.getElementById('tarjetaPrioridad').value);
   ```

### Depuración

1. **Abrir Console** en DevTools (F12 → Console)
2. **Ver errores** de Firebase
3. **Inspeccionar elementos** (F12 → Elements)
4. **Ver Network** (F12 → Network) para ver llamadas a Firestore

### Errores Comunes

**Error: "Firebase is not defined"**
- Verificar que los imports de Firebase están correctamente en el `<script type="module">`

**Error: "Unsafe attempt to load URL"**
- CORS/módulos ES6 necesitan un servidor HTTP (no funciona con file://)
- Solución: Usar `python3 -m http.server 8000`

**Error: "No se pudo conectar a Firestore"**
- Verificar conexión a internet
- Verificar que Firebase está online (console.firebase.google.com)
- Revisar credenciales de Firebase

## Firebase Firestore - Seguridad

> **⚠️ IMPORTANTE**: Actualmente las reglas de Firestore permiten acceso sin autenticación específica.

**Reglas de seguridad recomendadas** (en Firebase Console):

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Autenticado
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

## Testing Manual

### Checklist de Funcionalidades

- [ ] Login/Logout funciona
- [ ] Crear tablero nuevo
- [ ] Cambiar color de tablero
- [ ] Crear lista en tablero
- [ ] Crear tarjeta en lista
- [ ] Editar tarjeta (título, descripción)
- [ ] Agregar etiquetas
- [ ] Subir archivos
- [ ] Agregar comentarios
- [ ] Asignar usuarios
- [ ] Filtrar por etiqueta
- [ ] Filtrar por usuario
- [ ] Buscar tarjetas
- [ ] Drag & drop de tarjetas
- [ ] Eliminar tarjeta
- [ ] Responsividad en mobile

## Publicación (Deploy)

### Opción 1: Firebase Hosting

```bash
# 1. Instalar Firebase CLI
npm install -g firebase-tools

# 2. Autenticar
firebase login

# 3. Inicializar proyecto
firebase init hosting

# 4. Deploy
firebase deploy
```

### Opción 2: GitHub Pages

1. Crear repo en GitHub
2. Activar GitHub Pages en Settings
3. Seleccionar branch y carpeta
4. El sitio se sirve en `https://usuario.github.io/repo`

### Opción 3: Cualquier hosting estático

- Vercel
- Netlify
- AWS S3
- DigitalOcean
- Azure Static Web Apps

## Cambios Recientes (14 Jul 2026)

### Fixes Completados

**1. Reemplazar prompt() por Modales (Crítico)**
- Agregados: `listaModal` y `nuevaTarjetaModal`
- Funciones nuevas: `confirmCreateLista()` y `confirmCreateTarjeta()`
- Validación integrada
- No-bloqueante, mobile-friendly

**2. Validación de Nombres**
- `createTablero()`: Valida nombre no vacío
- `confirmCreateLista()`: Valida nombre no vacío
- `confirmCreateTarjeta()`: Valida título no vacío

**3. Eliminar/Renombrar Tableros y Listas**
- Modal `editTableroModal`: Campo rename + botón eliminar
- Nuevo modal `editListaModal`: Rename + Delete
- Botón menú (⋮) en cada lista
- Funciones: `updateTablero()`, `deleteTablero()`, `updateLista()`, `deleteLista()`
- Confirmaciones antes de eliminar

**4. Búsqueda Global (Bug Fix)**
- Variables globales: `searchTerm`, `tarjetasFiltradas`
- Integración en `renderKanban()`
- Contador de resultados
- Funciona en tiempo real

**5. Filtros por Etiqueta/Usuario (Bug Fix)**
- Lógica de filtrado corregida en `renderKanban()`
- Multiple filters simultáneamente
- Visual clara de filtros activos
- Toast de confirmación

**6. Seguridad - Restricción de Usuarios (Bug Fix)**
- Campo `miembros` en cada tablero
- Filtros restringidos a miembros del tablero
- Emails legibles (no IDs internos)
- Panel "Miembros del tablero" en modal
- Funciones: `renderMiembros()`, `addMiembroTablero()`, `removeMiembroTablero()`
- Migración automática de datos antiguos

### Commits Realizados
- `8e54010` - Fix: Reemplazar prompt() por modales propios
- `3efed71` - Fix: Implementar búsqueda funcional
- `7cefaef` - Fix: Implementar filtros funcionales
- `d647644` - Fix: Seguridad y gestión de miembros

### Estado Actual
- ✅ MVP funcional con todas las mejoras
- ✅ Deployado en Firebase Hosting
- ✅ GitHub actualizado
- ✅ Documentación completa

## Próximos Pasos

1. **Refactor**: Separar HTML, CSS y JS en archivos independientes
2. **Testing**: Agregar tests unitarios y E2E
3. **Features**: Checklists/subtareas, historial, notificaciones
4. **Build**: Agregar minificación y bundling
5. **CI/CD**: Automatizar tests y deploys
6. **Análisis**: Agregar Google Analytics
7. **Reglas Firestore**: Implementar reglas de seguridad más restrictivas

---

**Última actualización**: 14 Julio 2026  
**Estado**: MVP v2.0 - Bugs Críticos Resueltos
