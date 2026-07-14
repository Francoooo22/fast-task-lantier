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

## Próximos Pasos

1. **Refactor**: Separar HTML, CSS y JS en archivos independientes
2. **Testing**: Agregar tests unitarios y E2E
3. **Build**: Agregar minificación y bundling
4. **CI/CD**: Automatizar tests y deploys
5. **Análisis**: Agregar Google Analytics
6. **Seguridad**: Implementar reglas de Firestore más restrictivas

---

**Última actualización**: Julio 2026
