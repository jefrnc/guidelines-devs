# 📁 Estructura del Repositorio

## Directorios

```
guidelines-devs/
├── docs/                    # Documentación adicional
│   └── diagrams/           # Diagramas y visualizaciones
│       └── gitflow-diagram.md
│
├── examples/               # Ejemplos de código y configuración
│   ├── api/               # Ejemplos de APIs
│   │   └── openapi-spec.yml
│   ├── ci-cd/             # Pipelines y configuraciones CI/CD
│   │   └── github-actions-nodejs.yml
│   ├── docker/            # Dockerfiles y docker-compose
│   │   ├── docker-compose.yml
│   │   └── nodejs-dockerfile.example
│   └── git/               # Comandos y flujos de Git
│       └── gitflow-commands.md
│
└── resources/             # Recursos y plantillas
    └── templates/         # Plantillas reutilizables
        └── .env.example

```

## Archivos Principales

- **README.md** - Documentación principal con todas las guías y mejores prácticas
- **CONTRIBUTING.md** - Guía para contribuir al repositorio
- **LICENSE.md** - Licencia del proyecto
- **DEVELOPER_GUIDE.md** - Guía para desarrolladores trabajando con este repositorio
- **config.sample.js** - Ejemplo básico de configuración Node.js
- **configWithTest.sample.js** - Ejemplo avanzado con validación usando Joi
- **.gitignore** - Archivo completo para múltiples tecnologías

## ¿Cómo usar este repositorio?

1. **Para consulta rápida**: Lee el README.md que contiene todas las guías principales
2. **Para ejemplos específicos**: Navega a la carpeta `examples/` correspondiente
3. **Para plantillas**: Usa los archivos en `resources/templates/` como base
4. **Para contribuir**: Sigue las instrucciones en CONTRIBUTING.md

## Nuevas Secciones Agregadas

- ✅ **CI/CD**: Integración y despliegue continuo con ejemplos prácticos
- ✅ **Containerización**: Docker y Docker Compose con mejores prácticas
- ✅ **Monitoreo y Observabilidad**: Logs, métricas y trazas
- ✅ **Seguridad Actualizada**: Prácticas modernas de seguridad
- ✅ **Ejemplos de Código**: Configuraciones y pipelines listos para usar