# Guía de Despliegue - SchoolMood

Esta guía te ayudará a desplegar SchoolMood en diferentes plataformas.

## 🌐 Cloudflare Workers + D1 (Recomendado)

### Configuración Inicial

1. **Instalar Wrangler**
```bash
npm install -g wrangler
```

2. **Autenticar con Cloudflare**
```bash
wrangler auth login
```

3. **Crear Base de Datos D1**
```bash
wrangler d1 create schoolmood-production
```

4. **Actualizar wrangler.json**
```json
{
  "name": "schoolmood-production",
  "compatibility_date": "2024-01-01",
  "d1_databases": [
    {
      "binding": "DB",
      "database_name": "schoolmood-production",
      "database_id": "tu-database-id"
    }
  ]
}
```

### Migrar Base de Datos

```bash
# Crear tablas en producción
wrangler d1 execute schoolmood-production --remote --command="
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

wrangler d1 execute schoolmood-production --remote --command="
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

### Desplegar

```bash
# Compilar y desplegar
npm run build
wrangler deploy

# Tu app estará disponible en:
# https://schoolmood-production.tu-subdominio.workers.dev
```

### Dominio Personalizado

1. En Cloudflare Dashboard > Workers > tu-worker
2. Ve a "Triggers" > "Custom Domains"
3. Agrega tu dominio personalizado

## 🐳 Docker (Alternativo)

Si prefieres usar Docker para desarrollo local:

```dockerfile
# Dockerfile
FROM node:18-alpine

WORKDIR /app
COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

EXPOSE 5173
CMD ["npm", "run", "preview"]
```

```bash
# Compilar y ejecutar
docker build -t schoolmood .
docker run -p 5173:5173 schoolmood
```

## 🔒 Variables de Entorno en Producción

### Cloudflare Workers
```bash
# Configurar secrets
wrangler secret put MOCHA_USERS_SERVICE_API_KEY
wrangler secret put MOCHA_USERS_SERVICE_API_URL
```

### Verificar Deployment
```bash
# Ver logs en tiempo real
wrangler tail

# Probar endpoints
curl https://tu-dominio.com/api/students
```

## 📊 Monitoreo

### Cloudflare Analytics
- CPU time usage
- Requests per minute
- Error rates
- Edge locations

### Logs y Debugging
```bash
# Ver logs recientes
wrangler tail --format=pretty

# Logs específicos por fecha
wrangler tail --since=2024-01-01
```

## 🔄 CI/CD con GitHub Actions

Crea `.github/workflows/deploy.yml`:

```yaml
name: Deploy to Cloudflare Workers

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          
      - name: Install dependencies
        run: npm install
        
      - name: Build
        run: npm run build
        
      - name: Deploy to Cloudflare Workers
        uses: cloudflare/wrangler-action@v3
        with:
          apiToken: ${{ secrets.CLOUDFLARE_API_TOKEN }}
```

## 🛡️ Seguridad en Producción

1. **Configurar CORS apropiadamente**
2. **Usar HTTPS siempre**
3. **Validar todas las entradas**
4. **Limitar rate limiting**
5. **Monitorear logs de errores**

## 📈 Optimización de Performance

1. **Habilitar compresión**
2. **Usar CDN para assets estáticos**
3. **Implementar caching strategies**
4. **Optimizar queries de base de datos**

## 🆘 Troubleshooting

### Error: "Module not found"
```bash
rm -rf node_modules package-lock.json
npm install
```

### Base de datos no conecta
```bash
# Verificar configuración D1
wrangler d1 list
wrangler d1 info tu-database-name
```

### Worker no responde
```bash
# Verificar logs
wrangler tail
# Redeploy
wrangler deploy --force
```

¡Tu aplicación SchoolMood está lista para transformar la gestión educativa! 🚀
