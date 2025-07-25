# Diagrama de Git Flow

## Flujo Visual de Git Flow

```
                     PRODUCCIÓN (master/main)
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
        │              [TAG v1.0]          [TAG v1.1]
        │                   │                   │
    ════╪═══════════════════╪═══════════════════╪════════> 
        │                   │                   │
        │                   │              ┌────┴────┐
        │                   │              │ HOTFIX  │
        │                   │              │  1.0.1  │
        │                   │              └────┬────┘
        │                   │                   │
        │              ┌────┴────┐              │
        │              │ RELEASE │              │
        │              │   1.0   │              │
        │              └────┬────┘              │
        │                   │                   │
    ════╪═══════════════════╪═══════════════════╪════════>  DESARROLLO (develop)
        │                   │                   │
        │         ┌─────────┴─────────┐         │
        │         │                   │         │
        │    ┌────┴────┐         ┌───┴────┐    │
        │    │ FEATURE │         │FEATURE │    │
        │    │    A    │         │   B    │    │
        │    └─────────┘         └────────┘    │
        │                                       │
        └───────────────────────────────────────┘

```

## Descripción de las Ramas

### 🏷️ Master/Main (Producción)
- Contiene código en producción
- Cada merge representa una nueva versión
- Se etiqueta con números de versión (v1.0, v1.1, etc.)

### 🚧 Develop (Desarrollo)
- Rama de integración para nuevas funcionalidades
- Contiene las últimas características desarrolladas
- Base para crear ramas de feature y release

### ✨ Feature (Funcionalidad)
- Se crea desde: `develop`
- Se fusiona en: `develop`
- Nomenclatura: `feature/nombre-descriptivo`
- Ejemplo: `feature/login-social`

### 📦 Release (Lanzamiento)
- Se crea desde: `develop`
- Se fusiona en: `master` y `develop`
- Nomenclatura: `release/X.Y.Z`
- Para preparación de nuevas versiones

### 🚨 Hotfix (Corrección urgente)
- Se crea desde: `master`
- Se fusiona en: `master` y `develop`
- Nomenclatura: `hotfix/X.Y.Z`
- Para correcciones críticas en producción

## Flujo de Trabajo Típico

### 1. Desarrollo de Nueva Funcionalidad
```
develop → feature/nueva-funcionalidad → develop
```

### 2. Preparación de Release
```
develop → release/1.2.0 → master + develop
                          ↓
                      tag v1.2.0
```

### 3. Corrección de Bug en Producción
```
master → hotfix/1.2.1 → master + develop
                        ↓
                    tag v1.2.1
```

## Comandos Esenciales

```bash
# Ver estado actual
git flow status

# Listar todas las ramas
git branch -a

# Ver historial con gráfico
git log --graph --pretty=oneline --abbrev-commit --all
```

## Mejores Prácticas

1. **No hacer commits directos** a `master` o `develop`
2. **Usar Pull Requests** para revisar código antes de fusionar
3. **Mantener features pequeños** y enfocados
4. **Actualizar frecuentemente** desde develop para evitar conflictos
5. **Etiquetar todas las releases** con versionado semántico

## Versionado Semántico

```
MAJOR.MINOR.PATCH

1.2.3
│ │ └── Correcciones de bugs (hotfix)
│ └──── Nuevas funcionalidades (release)
└────── Cambios incompatibles con versiones anteriores
```