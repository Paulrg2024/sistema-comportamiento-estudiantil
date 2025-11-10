# Guía de Contribución - SchoolMood

¡Gracias por tu interés en contribuir a SchoolMood! 🎉

## 🤝 Cómo Contribuir

### Reportar Bugs

1. **Verifica** que el bug no haya sido reportado antes en [Issues](../../issues)
2. **Crea un nuevo issue** con:
   - Título descriptivo
   - Pasos para reproducir el bug
   - Comportamiento esperado vs actual
   - Capturas de pantalla (si aplica)
   - Información del navegador/dispositivo

### Sugerir Mejoras

1. **Abre un issue** con la etiqueta "enhancement"
2. **Describe claramente** la mejora propuesta
3. **Explica por qué** sería útil para la comunidad educativa

### Enviar Código

1. **Fork** el repositorio
2. **Crea una rama** para tu cambio:
   ```bash
   git checkout -b feature/nueva-caracteristica
   ```
3. **Sigue las convenciones** de código del proyecto
4. **Escribe commits** descriptivos en español
5. **Prueba** tus cambios localmente
6. **Envía un Pull Request**

## 📝 Convenciones de Código

### TypeScript/React
- Usa **TypeScript estricto**
- Nombra componentes en **PascalCase**
- Usa **camelCase** para variables y funciones
- Prefiere **function components** sobre class components
- Usa **hooks** apropiadamente

### CSS/Tailwind
- Usa **Tailwind CSS** para estilos
- Mantén clases ordenadas por función
- Prefiere **utility classes** sobre CSS custom

### Backend/API
- Sigue **REST principles**
- Valida entrada con **Zod schemas**
- Maneja errores apropiadamente
- Usa **async/await** sobre promises

## 🧪 Testing

- Asegúrate de que `npm run build` pase sin errores
- Prueba la funcionalidad en diferentes navegadores
- Verifica la responsividad en móviles

## 📦 Estructura de Commits

Usa commits descriptivos en español:

```
feat: agrega funcionalidad de exportar reportes
fix: corrige error en cálculo de edad
docs: actualiza instrucciones de instalación
style: mejora espaciado en dashboard
refactor: optimiza consultas de base de datos
```

## 🏷️ Labels de Issues

- `bug` - Error en funcionalidad existente
- `enhancement` - Nueva característica
- `documentation` - Mejoras en documentación
- `help wanted` - Se necesita ayuda
- `good first issue` - Ideal para nuevos contribuidores

## 👥 Código de Conducta

- Sé **respetuoso** con otros colaboradores
- Usa **lenguaje inclusivo**
- Enfócate en **soluciones constructivas**
- Recuerda que esto es para **beneficiar la educación**

## ❓ Preguntas

Si tienes dudas, puedes:
- Abrir un issue con la etiqueta "question"
- Revisar issues existentes
- Contactar a los mantenedores

¡Gracias por hacer SchoolMood mejor para la comunidad educativa! 🎓✨
