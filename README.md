# 📱 Instagram Stories Clone

> **Un clon funcional de Instagram Stories construido con React, TypeScript y LocalStorage**

![React](https://img.shields.io/badge/React-18.2-blue)
![TypeScript](https://img.shields.io/badge/TypeScript-5.3-blue)
![Vite](https://img.shields.io/badge/Vite-5.0-purple)
![Tailwind](https://img.shields.io/badge/Tailwind-3.4-cyan)
![Docker](https://img.shields.io/badge/Docker-Ready-blue)

## ✨ Características

- 📸 **Subir imágenes** que se convierten automáticamente a historias
- ⏰ **Expiración de 24 horas** - Las historias desaparecen automáticamente
- ⏱️ **Timer de 3 segundos** por historia con barra de progreso
- 👆 **Gestos táctiles** - Swipe, tap y hold para navegar
- 💾 **Sin backend** - Todo se guarda en LocalStorage
- 📱 **100% Responsive** - Funciona en móvil y desktop
- 🎨 **UI estilo Instagram** - Círculos con gradiente, animaciones fluidas
- 🐳 **Docker Ready** - Fácil despliegue con Docker Compose

## 🛠️ Tecnologías

- **Frontend**: React 18 + TypeScript
- **Build Tool**: Vite
- **Estilos**: Tailwind CSS
- **Almacenamiento**: LocalStorage (Browser API)
- **Procesamiento de Imágenes**: Canvas API + FileReader
- **Containerización**: Docker + Docker Compose
- **Servidor Web**: Nginx (producción)

## 🚀 Instalación y Despliegue

### Opción 1: Con Docker Compose (Recomendado)

Esta es la forma más sencilla de levantar la aplicación:

```bash
# 1. Construir y levantar el contenedor
docker-compose up -d --build

# 2. La aplicación estará disponible en:
# http://localhost:3000
```

**Comandos útiles de Docker:**

```bash
# Ver logs
docker-compose logs -f

# Detener contenedores
docker-compose down

# Reconstruir sin cache
docker-compose build --no-cache
docker-compose up -d

# Ver estado de contenedores
docker-compose ps
```

### Opción 2: Desarrollo Local

```bash
# 1. Instalar dependencias
npm install

# 2. Iniciar servidor de desarrollo
npm run dev

# 3. La aplicación estará disponible en:
# http://localhost:5173
```

### Opción 3: Desarrollo con Docker

Si prefieres desarrollo con hot-reload en Docker:

```bash
docker-compose -f docker-compose.dev.yml up
```

## 🎮 Cómo Funciona

### Arquitectura

La aplicación está dividida en varios componentes y hooks:

1. **`useStories` Hook**: 
   - Gestiona el estado de las historias (CRUD)
   - Maneja la persistencia en LocalStorage
   - Implementa la expiración automática de 24 horas
   - Procesa y comprime imágenes automáticamente

2. **`useStoryViewer` Hook**:
   - Controla el visor fullscreen
   - Gestiona la navegación entre historias
   - Maneja timers y animaciones de progreso
   - Detecta gestos táctiles y eventos de teclado

3. **Componentes**:
   - `StoryList`: Lista horizontal de historias con scroll
   - `StoryViewer`: Visor fullscreen con navegación
   - `StoryCircle`: Círculo individual con borde gradiente
   - `ProgressBar`: Barras de progreso animadas
   - `AddStoryButton`: Botón para subir nuevas historias

### Flujo de Datos

```
Usuario sube imagen
    ↓
imageUtils.ts procesa (redimensiona + comprime)
    ↓
useStories crea objeto Story con Base64
    ↓
Se guarda en LocalStorage con timestamp
    ↓
StoryList renderiza círculos
    ↓
Usuario hace clic → StoryViewer se abre
    ↓
useStoryViewer inicia timer de 3 segundos
    ↓
Progreso se actualiza cada 100ms
    ↓
Al completar → siguiente historia automáticamente
```

### Persistencia

- **LocalStorage**: Todas las historias se guardan en `localStorage` del navegador
- **Formato**: Cada historia es un objeto JSON con:
  - `id`: Identificador único
  - `imageBase64`: Imagen en formato Base64
  - `createdAt`: Timestamp de creación
- **Expiración**: Las historias se eliminan automáticamente después de 24 horas
- **Límite**: ~5MB de almacenamiento (límite de LocalStorage)

## 📖 Cómo Usar

### Subir una Historia

1. Haz clic en el botón **"+"** en la lista de historias
2. Selecciona una imagen de tu dispositivo
3. La imagen se procesa automáticamente y aparece en la lista

### Ver Historias

- **Click/Tap** en cualquier historia para verla
- **Tap izquierdo** (25% pantalla): Historia anterior
- **Tap derecho** (75% pantalla): Historia siguiente
- **Swipe horizontal**: Navegar entre historias
- **Mantener presionado**: Pausar historia
- **Teclas ←/→**: Navegar (desktop)
- **ESC**: Cerrar visor

### Navegación

- Cada historia dura **3 segundos** automáticamente
- Las barras de progreso muestran el tiempo restante
- Al finalizar, pasa automáticamente a la siguiente
- Puedes pausar manteniendo presionado en cualquier momento

## 🏗️ Estructura del Proyecto

```
src/
├── components/          # Componentes UI
│   ├── StoryList.tsx   # Lista horizontal de historias
│   ├── StoryViewer.tsx # Visor fullscreen
│   ├── StoryCircle.tsx # Círculo individual
│   ├── ProgressBar.tsx # Barras de progreso animadas
│   └── AddStoryButton.tsx # Botón para subir historias
├── hooks/              # Custom Hooks
│   ├── useStories.ts   # Gestión de historias
│   └── useStoryViewer.ts # Control del visor
├── utils/              # Utilidades
│   ├── storage.ts      # LocalStorage + expiración
│   └── imageUtils.ts   # Procesamiento de imágenes
├── types/              # Tipos TypeScript
│   └── index.ts
└── App.tsx             # Componente principal
```

## 🔧 Configuración Técnica

### Límites

- **Tamaño máximo imagen**: 1080x1920px (se redimensiona automáticamente)
- **Almacenamiento**: ~5MB (límite de LocalStorage)
- **Duración historia**: 24 horas
- **Timer por historia**: 3 segundos

### Optimizaciones

- Compresión JPEG al 85%
- Redimensionado automático a 1080x1920px
- Limpieza automática de historias expiradas
- Animaciones a 60fps
- Lazy loading de imágenes

## 🐳 Configuración Docker

### Archivos Docker

- **`Dockerfile`**: Build multi-stage (Node.js + Nginx)
- **`docker-compose.yml`**: Configuración para producción
- **`docker-compose.dev.yml`**: Configuración para desarrollo
- **`nginx.conf`**: Configuración del servidor web
- **`.dockerignore`**: Archivos excluidos del build

### Puertos

- **Producción**: `3000:80` (puerto 3000 del host → puerto 80 del contenedor)
- **Desarrollo**: `5173:5173` (puerto de Vite)

### Variables de Entorno

- `NODE_ENV=production` (en producción)
- `NODE_ENV=development` (en desarrollo)

## 🐛 Problemas Conocidos y Soluciones

### "Las historias no avanzan"
**Solución aplicada**: Fix de stale closure en `useStoryViewer` usando refs

### "Storage lleno"
**Solución**: Las imágenes se comprimen automáticamente. Límite ~25 historias

### "Memory leaks"
**Solución**: Limpieza de timers en cleanup de useEffect

### "Error 500 o MIME type incorrecto"
**Solución**: Verificar que `nginx.conf` tenga los tipos MIME correctos configurados

## 📚 Documentación Adicional

- **Clase Magistral**: Ver `docs/CLASE_MAGISTRAL.md` para tutorial completo
- **Bitácora**: Ver `docs/bitacora.md` para proceso de desarrollo

## 📄 Licencia

MIT - Proyecto educativo de código abierto

## 🤝 Contribuciones

¡Las contribuciones son bienvenidas! 

1. Fork el proyecto
2. Crea tu feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add: nueva característica'`)
4. Push a la branch (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

---

⭐ **Si te gustó este proyecto, dale una estrella!**

🎓 **Perfecto para aprender**: React, TypeScript, APIs del navegador, Docker, y más.

