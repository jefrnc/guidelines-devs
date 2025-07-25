# Comandos de Git Flow - Guía Rápida

## Inicialización

```bash
# Inicializar git flow en un repositorio existente
git flow init

# Configuración típica:
# Branch de producción: master
# Branch de desarrollo: develop
# Prefijo feature: feature/
# Prefijo release: release/
# Prefijo hotfix: hotfix/
```

## Features (Funcionalidades)

```bash
# Listar features
git flow feature list

# Iniciar un nuevo feature
git flow feature start nombre-del-feature

# Publicar un feature al repositorio remoto
git flow feature publish nombre-del-feature

# Obtener un feature publicado por otro desarrollador
git flow feature track nombre-del-feature

# Finalizar un feature (merge a develop)
git flow feature finish nombre-del-feature
```

## Releases

```bash
# Iniciar una release
git flow release start 1.2.0

# Publicar una release
git flow release publish 1.2.0

# Finalizar una release (merge a master y develop, crea tag)
git flow release finish 1.2.0
```

## Hotfixes

```bash
# Iniciar un hotfix desde master
git flow hotfix start 1.2.1

# Finalizar un hotfix (merge a master y develop, crea tag)
git flow hotfix finish 1.2.1
```

## Comandos Git equivalentes (sin git-flow)

### Feature
```bash
# Iniciar feature
git checkout -b feature/nombre-del-feature develop

# Finalizar feature
git checkout develop
git merge --no-ff feature/nombre-del-feature
git branch -d feature/nombre-del-feature
git push origin develop
```

### Release
```bash
# Iniciar release
git checkout -b release/1.2.0 develop

# Finalizar release
git checkout master
git merge --no-ff release/1.2.0
git tag -a 1.2.0
git checkout develop
git merge --no-ff release/1.2.0
git branch -d release/1.2.0
```

### Hotfix
```bash
# Iniciar hotfix
git checkout -b hotfix/1.2.1 master

# Finalizar hotfix
git checkout master
git merge --no-ff hotfix/1.2.1
git tag -a 1.2.1
git checkout develop
git merge --no-ff hotfix/1.2.1
git branch -d hotfix/1.2.1
```

## Tips adicionales

1. **Siempre sincroniza antes de empezar**: 
   ```bash
   git checkout develop
   git pull origin develop
   ```

2. **Usa `--no-ff` para preservar la historia**:
   Esto crea un commit de merge incluso cuando se podría hacer fast-forward

3. **Limpia branches locales obsoletos**:
   ```bash
   git remote prune origin
   git branch -vv | grep ': gone]' | awk '{print $1}' | xargs -n 1 git branch -d
   ```