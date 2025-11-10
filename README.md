# SchoolMood 🏫📊

**Sistema de Control y Análisis del Comportamiento Estudiantil con Emoticones**

SchoolMood es una aplicación web moderna diseñada para que maestros y administradores educativos puedan registrar, monitorear y analizar el comportamiento de los estudiantes de manera visual e intuitiva usando emoticones.

## ✨ Características Principales

- 📝 **Gestión de Estudiantes**: Registro completo con información demográfica
- 😊 **Seguimiento de Comportamiento**: Sistema de 5 niveles con emoticones intuitivos
- 📊 **Dashboard Analytics**: Estadísticas en tiempo real y distribución de comportamientos
- 🎯 **Búsqueda y Filtros**: Encuentra estudiantes rápidamente
- 📱 **Responsive Design**: Funciona perfectamente en móviles y escritorio
- 🔒 **Datos Seguros**: Base de datos SQLite con Cloudflare D1

## 🚀 Tecnologías Utilizadas

### Frontend
- **React 18** con TypeScript
- **Tailwind CSS** para estilos modernos
- **Vite** como bundler
- **React Router** para navegación
- **Lucide React** para iconos

### Backend
- **Hono** - Framework web ultrarrápido
- **Cloudflare Workers** - Edge computing
- **Cloudflare D1** - Base de datos SQLite distribuida
- **Zod** - Validación de esquemas

## 📋 Requisitos del Sistema

- **Node.js** 18+ 
- **npm** o **bun**
- Cuenta en **Cloudflare** (para D1 y Workers)
- **Git**

## 🛠️ Instalación

### 1. Clonar el Repositorio

```bash
git clone https://github.com/tu-usuario/schoolmood-app.git
cd schoolmood-app
```

### 2. Instalar Dependencias

```bash
# Con npm
npm install

# O con bun (recomendado)
bun install
```

### 3. Configurar Cloudflare

#### 3.1 Instalar Wrangler CLI
```bash
npm install -g wrangler
# O con bun
bun install -g wrangler
```

#### 3.2 Autenticar con Cloudflare
```bash
wrangler auth login
```

#### 3.3 Crear Base de Datos D1
```bash
wrangler d1 create schoolmood-db
```

Esto te dará una salida similar a:
```
[[d1_databases]]
binding = "DB"
database_name = "schoolmood-db"
database_id = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

#### 3.4 Actualizar wrangler.json
Actualiza el archivo `wrangler.json` con tu `database_id`:

```json
{
  "name": "schoolmood-app",
  "compatibility_date": "2024-01-01",
  "d1_databases": [
    {
      "binding": "DB",
      "database_name": "schoolmood-db",
      "database_id": "tu-database-id-aqui"
    }
  ]
}
```

### 4. Configurar la Base de Datos

#### 4.1 Crear las Tablas
```bash
# Crear tabla de estudiantes
wrangler d1 execute schoolmood-db --remote --command="
CREATE TABLE students (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL,
  grade TEXT NOT NULL,
  class_section TEXT,
  student_id TEXT UNIQUE,
  birth_date DATE,
  sex TEXT,
  is_active BOOLEAN DEFAULT 1,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);"

# Crear tabla de registros de comportamiento
wrangler d1 execute schoolmood-db --remote --command="
CREATE TABLE behavior_records (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  student_id INTEGER NOT NULL,
  behavior_type TEXT NOT NULL,
  emotion_icon TEXT NOT NULL,
  notes TEXT,
  teacher_name TEXT,
  date_recorded DATE NOT NULL,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);"
```

### 5. Ejecutar la Aplicación

#### Desarrollo Local
```bash
# Con npm
npm run dev

# Con bun
bun run dev
```

La aplicación estará disponible en `http://localhost:5173`

#### Compilar y Desplegar
```bash
# Compilar para producción
npm run build
# O con bun
bun run build

# Desplegar a Cloudflare Workers
wrangler deploy
```

## 🏗️ Estructura del Proyecto

```
schoolmood-app/
├── src/
│   ├── react-app/           # Frontend React
│   │   ├── components/      # Componentes reutilizables
│   │   │   ├── Dashboard.tsx
│   │   │   ├── StudentManager.tsx
│   │   │   ├── BehaviorTracker.tsx
│   │   │   └── Layout.tsx
│   │   ├── pages/          # Páginas principales
│   │   │   └── Home.tsx
│   │   └── main.tsx        # Punto de entrada React
│   ├── shared/             # Tipos y esquemas compartidos
│   │   └── types.ts
│   └── worker/             # Backend Hono
│       └── index.ts
├── public/                 # Archivos estáticos
├── index.html             # HTML principal
├── package.json
├── wrangler.json          # Configuración Cloudflare
├── tailwind.config.js     # Configuración Tailwind
└── vite.config.ts         # Configuración Vite
```

## 🎮 Cómo Usar la Aplicación

### 1. **Dashboard**
- Visualiza estadísticas generales
- Ve la distribución de comportamientos
- Revisa la actividad reciente

### 2. **Gestión de Estudiantes**
- Agrega nuevos estudiantes con información completa
- Busca estudiantes por nombre, grado o ID
- Elimina estudiantes (soft delete)

### 3. **Registro de Comportamiento**
- Selecciona un estudiante
- Escoge el nivel de comportamiento (😊 Excelente → 😰 Problemático)
- Agrega notas descriptivas
- Registra fecha y nombre del maestro

## 📊 Niveles de Comportamiento

| Emoji | Nivel | Descripción |
|-------|-------|-------------|
| 😊 | Excelente | Comportamiento ejemplar |
| 🙂 | Bueno | Comportamiento positivo |
| 😐 | Neutral | Comportamiento estándar |
| 😟 | Preocupante | Requiere atención |
| 😰 | Problemático | Necesita intervención inmediata |

## 🔧 Variables de Entorno

La aplicación puede usar las siguientes variables de entorno opcionales:

```bash
# Para funcionalidades futuras de autenticación
MOCHA_USERS_SERVICE_API_KEY=tu-api-key
MOCHA_USERS_SERVICE_API_URL=https://api.mocha.com
```

## 🤝 Contribuir

1. Fork el proyecto
2. Crea una rama para tu feature (`git checkout -b feature/nueva-caracteristica`)
3. Commit tus cambios (`git commit -am 'Agrega nueva característica'`)
4. Push a la rama (`git push origin feature/nueva-caracteristica`)
5. Abre un Pull Request

## 📝 Ideas para Mejoras Futuras

- [ ] Sistema de autenticación de maestros
- [ ] Notificaciones automáticas a padres
- [ ] Reportes en PDF
- [ ] Gráficos de tendencias temporales
- [ ] Integración con calendarios escolares
- [ ] API para aplicaciones móviles
- [ ] Sistema de roles (admin, maestro, director)

## 🐛 Reportar Problemas

Si encuentras algún bug o tienes sugerencias, por favor:

1. Revisa los [issues existentes](../../issues)
2. Crea un nuevo issue con detalles específicos
3. Incluye capturas de pantalla si es posible

## 📄 Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo `LICENSE` para más detalles.

## 👨‍💻 Autor

Desarrollado con ❤️ para la comunidad educativa.

---

**SchoolMood** - Transformando la gestión del comportamiento estudiantil, un emoticón a la vez. 🎓✨
