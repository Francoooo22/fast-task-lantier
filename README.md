# 📋 Fast Task Lantier Pro

Gestor profesional de proyectos con sistema Kanban en tiempo real, integrado con Firebase.

## 🎯 Descripción

**Fast Task Lantier Pro** es una aplicación web moderna para gestionar tareas y proyectos mediante tableros Kanban interactivos. Permite crear múltiples tableros, organizarlos en listas customizables, y trabajar colaborativamente con comentarios, adjuntos y asignaciones.

### Características principales

- ✅ **Autenticación Firebase**: Login/Registro automático con email y contraseña
- 📊 **Tableros Kanban**: Múltiples tableros con fondos personalizables
- 🎨 **Diseño Responsivo**: Interfaz moderna y adaptable a cualquier dispositivo
- 🏷️ **Etiquetas Personalizadas**: Etiquetas de color para categorizar tarjetas
- 📎 **Adjuntos**: Carga de imágenes y archivos en Firebase Storage
- 💬 **Comentarios**: Sistema de comentarios integrado en tarjetas
- 👥 **Asignaciones**: Asignar tarjetas a usuarios de la plataforma
- 🔍 **Búsqueda Global**: Buscar tarjetas en tiempo real
- 🎯 **Filtros Avanzados**: Filtrar por etiquetas y usuarios asignados
- 🖱️ **Drag & Drop**: Arrastrar tarjetas entre listas y reordenar
- 📅 **Fechas de Vencimiento**: Asignar deadlines a tarjetas

## 🏗️ Arquitectura

### Stack Tecnológico
- **Frontend**: HTML5, CSS3, JavaScript (ES6+)
- **Backend**: Firebase (Auth, Firestore, Cloud Storage)
- **Base de Datos**: Firestore (NoSQL)
- **Almacenamiento**: Firebase Storage

### Estructura de Datos (Firestore)

```
/tableros
  - id (auto)
  - nombre: string
  - emoji: string
  - color: string (gradient CSS)
  - creadoPor: string (email)
  - creadoEn: timestamp

/listas
  - id (auto)
  - tableroId: string (referencia)
  - nombre: string
  - orden: number
  - creadoEn: timestamp

/tarjetas
  - id (auto)
  - listaId: string (referencia)
  - tableroId: string (referencia)
  - titulo: string
  - descripcion: string
  - etiquetas: array [{color, text}]
  - asignados: array [email, email...]
  - adjuntos: array [{nombre, url, tipo, tamaño, fecha}]
  - comentarios: array [{texto, autor, fecha}]
  - fechaVencimiento: date
  - creadoPor: string
  - creadoEn: timestamp

/usuarios
  - id (email)
  - email: string
```

## 🚀 Cómo Usar

### Opción 1: Online (Recomendado)

**Sin instalación requerida:**
```
https://fast-task-lantier.web.app
```

### Opción 2: Local (Desarrollo)

1. **Clonar repositorio**
   ```bash
   git clone https://github.com/Francoooo22/fast-task-lantier.git
   cd fast-task-lantier
   ```

2. **Servir localmente** (requiere Python 3)
   ```bash
   python3 -m http.server 8000
   ```

3. **Abrir en navegador**
   ```
   http://localhost:8000
   ```

### Primer Uso

1. Ingresa tu email y contraseña (se crea cuenta automáticamente)
2. Crea un nuevo tablero haciendo click en "+ Nuevo tablero"
3. Personaliza el emoji y color del tablero
4. Crea listas dentro del tablero (ej: "Por hacer", "En progreso", "Hecho")
5. Agrega tarjetas a cada lista con "+ Agregar tarjeta"
6. Haz click en una tarjeta para:
   - Editar título y descripción
   - Agregar etiquetas personalizadas
   - Adjuntar archivos e imágenes
   - Añadir comentarios
   - Asignar a otros usuarios
   - Establecer fecha de vencimiento

## 🔧 Configuración Firebase

### Estado Actual
- ✅ **Firebase Hosting**: Deployado en https://fast-task-lantier.web.app
- ✅ **Firestore**: Base de datos en tiempo real
- ✅ **Authentication**: Firebase Auth (email/password)
- ✅ **Storage**: Firebase Storage para adjuntos

### Credenciales
El proyecto usa las siguientes credenciales de Firebase:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyBYMDb8mLokqkLDdVjOsmuNmiUAzUIXcFg",
  authDomain: "fast-task-lantier.firebaseapp.com",
  projectId: "fast-task-lantier",
  storageBucket: "fast-task-lantier.firebasestorage.app",
  messagingSenderId: "317359004557",
  appId: "1:317359004557:web:1d019706d74ac85e313bc4"
};
```

> **⚠️ IMPORTANTE**: Estas credenciales son públicas (están en el frontend). No incluyen datos sensibles, pero en producción considera usar variables de entorno.

### Archivos de Configuración
- `firebase.json` - Configuración de Firebase Hosting
- `.firebaserc` - Asociación con proyecto (en .gitignore por seguridad)

## 📱 Responsividad

La aplicación está optimizada para:
- 📱 Dispositivos móviles (320px+)
- 💻 Tablets (768px+)
- 🖥️ Desktop (1024px+)

El layout se adapta automáticamente:
- En mobile: modal simplificado, grid de tableros más compacto
- En desktop: modal con 2 columnas (contenido + detalles)

## 🎨 Fondos Disponibles

Gradientes predefinidos para tableros:
1. Azul → Púrpura
2. Verde → Amarillo-verde
3. Rojo → Naranja
4. Púrpura → Rosa
5. Cian → Azul celeste
6. Ámbar → Naranja

## 🐛 Bugs y Limitaciones Conocidas

- [ ] Reordenar listas (está parcialmente implementado)
- [ ] Búsqueda global no filtra por tablero
- [ ] No hay notificaciones de cambios en tiempo real
- [ ] No hay rol de administrador
- [ ] La sincronización es por evento (no es en tiempo real multi-sesión)

## 🔮 Roadmap

- [ ] Sincronización en tiempo real (Firestore listeners)
- [ ] Roles y permisos
- [ ] Tableros compartidos/privados
- [ ] Notificaciones por email
- [ ] Vistas de Gantt/Timeline
- [ ] Exportar a Excel/PDF
- [ ] Temas oscuro/claro
- [ ] Integración con calendarios
- [ ] Automatizaciones (reglas, triggers)

## 📄 Licencia

Proyecto propio - Sin licencia específica

## 👤 Autor

Franco Bocchi (Arcángel Boutique)

## 📞 Contacto

Email: bfproductosyservicios@gmail.com

---

**Última actualización**: Julio 2026  
**Estado**: MVP Funcional
