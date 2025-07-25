
# 📚 Lineamientos de Desarrollo de Software &middot; [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com) [![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/josephefranco)

> Una colección completa de mejores prácticas y lineamientos para el desarrollo de software moderno.

## 📖 Acerca de este repositorio

Este repositorio contiene una lista cuidadosamente seleccionada de pautas, mejores prácticas y estándares que hemos recopilado y probado en proyectos reales. Estas guías están diseñadas para ayudar a equipos de desarrollo a mantener consistencia, calidad y eficiencia en sus proyectos.

**¿Quieres contribuir?** Si deseas compartir una mejor práctica o sugerir mejoras, [siéntete libre de crear un Pull Request](http://makeapullrequest.com).

## 📋 Tabla de Contenidos

### 🔧 Control de Versiones
- [1. Git](#git)
    - [1.1 Reglas fundamentales de Git](#some-git-rules)
    - [1.2 Git workflow](#git-workflow)
    - [1.3 Escribir buenos mensajes de commit](#writing-good-commit-messages)

### 📝 Desarrollo
- [2. Documentación](#documentation)
- [3. Ambientes](#environments)
    - [3.1 Entornos de desarrollo consistentes](#consistent-dev-environments)
- [4. Testing](#testing)
- [5. Estilo de código](#code-style)
    - [5.1 Pautas de estilo de código](#code-style-check)
    - [5.2 Hacer cumplir los estándares](#enforcing-code-style-standards)

### 🔍 Monitoreo y APIs
- [6. Logging](#logging)
- [7. API](#api)
    - [7.1 Diseño de API](#api-design)
    - [7.2 Seguridad de API](#api-security)
    - [7.3 Documentación de API](#api-documentation)

### 🚀 DevOps (Nuevo)
- [8. CI/CD](#cicd)
- [9. Containerización](#containerization)
- [10. Monitoreo y Observabilidad](#monitoring)

<a name="git"></a>
## 1. Git
<a name="some-git-rules"></a>

### 1.1 Algunas reglas de Git
Hay un conjunto de reglas a tener en cuenta:
* Realizar nuestro trabajo en una rama de `feature`
    
  _Por que?:_
    >Porque de esta manera todo el trabajo se realiza de forma aislada en una rama dedicada en lugar de la rama principal. Le permite enviar múltiples solicitudes de pull request sin confusión. Puede iterar sin contaminar la rama maestra con código potencialmente inestable e inacabado.

* Se ramifica desde `develop`
    
  _Por que?:_
    >De esta manera, puede asegurarse de que el código de la rama `master` (principal) casi siempre se compilará sin problemas, y se puede usar principalmente para releases (esto puede ser excesivo para algunos proyectos).

* Nunca haga push a la rama `develop` o` master`. Haga un Pull Request.
  _Por que?:_
    > Notifica a los miembros del equipo que han completado una función. También permite una fácil revisión (peer-review) del código y dedica una instancia para discutir la característica propuesta.

*  Actualice su rama local de `develop` y haga el rebasing antes del push de su `feature` y solicitar el Pull Request.
  _Por que?:_
    > Rebasing fusionará en la rama solicitada (`master` o` develop`) y aplicará los commits que ha realizado localmente a la parte superior del historial sin crear un merge commit (suponiendo que no haya conflictos). Dando por resultado historico de las versiones agradable y limpi0. [Lee mas ...](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

* Resolver conflictos potenciales con el rebasing antes de realizar un Pull Request. [Mira esto](https://medium.com/@MiguelCasas/diferencia-entre-git-rebase-y-git-merge-workshop-de-git-8622dedde2d7) y [esto](https://www.atlassian.com/git/tutorials/merging-vs-rebasing).

* Elimine las ramas locales y remotas después de reitegrar el codigo en la otra rama.
    
  _Por que?:_
    > Asegura que solo vuelva a fusionar la rama (`master` o` develop`) una vez. Las ramas de características solo deberían existir mientras el trabajo todavía está en progreso, en caso de no borrarlas sera inmanejable la cantidad de ramas que tendra en GIT.


* Antes de realizar un Pull Request, asegúrese de que la rama de feature compila correctamente y pasa todas las pruebas (incluidas las comprobaciones de estilo de código)
    
  _Por que?:_
    > Está a punto de agregar su código a una rama estable. Si el codigo de la rama 'feature' falla, existe una alta probabilidad de que la compilación de su ramificación de destino también falle. Además, debe aplicar la verificación de estilo de código antes de realizar un Pull Request. Ayuda a la legibilidad y reduce la posibilidad de que las correcciones de formato se mezclen con los cambios reales.

* Use un archivo `.gitignore` apropiado para su proyecto.
    
  _¿Por qué?:_
    > Evita que archivos del sistema, configuraciones locales y dependencias se suban accidentalmente al repositorio. Además, puede excluir la configuración de carpetas y archivos para los editores más utilizados, así como las carpetas de dependencia más comunes.
    
  _¿Cómo generarlo?:_
    > Utiliza herramientas como [gitignore.io](https://www.toptal.com/developers/gitignore) para generar un archivo `.gitignore` personalizado según las tecnologías de tu proyecto

* Proteja sus ramas `develop` y `master`.
  
  _¿Por qué?:_
    > Protege tus ramas de producción de recibir cambios inesperados e irreversibles. Las ramas protegidas garantizan que el código crítico pase por un proceso de revisión adecuado.
    
  _¿Cómo configurarlo?:_
    > **GitHub**: Settings → Branches → Add rule → Require pull request reviews
    > **GitLab**: Settings → Repository → Protected branches
    > **Bitbucket**: Repository settings → Branch permissions
    
  _Configuración recomendada:_
    > - Requerir revisión de PR antes de fusionar
    > - Descartar aprobaciones obsoletas cuando se pushean nuevos commits
    > - Requerir que las ramas estén actualizadas antes de fusionar
    > - Incluir administradores en las restricciones

<a name="git-workflow"></a>
### 1.2 GitFlow

Debido a la mayoría de las razones anteriores, utilizamos [Feature-branch-workflow] (https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow) con [Interactive Rebasing] (https: //www.atlassian.com/git/tutorials/merging-vs-rebasing#the-golden-rule-of-rebasing) y algunos elementos de [Gitflow] (https://www.atlassian.com/git/tutorials/comparating-workflows#gitflow-workflow).  

* Verificamos que contamos con git flow.

    ```sh
    git flow
    ```

  _No lo tengo, que hago?:_
   > Procedemos a instalar el paquete.
  ```sh
  sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys E1DD270288B4E6030699E45FA1715D88E1DF1F24
  sudo su -c "echo 'deb http://ppa.launchpad.net/git-core/ppa/ubuntu trusty main' > /etc/apt/sources.list.d/git.list"

  sudo apt-get update 
  sudo apt-get install git-flow
    ```

* Creacion de un proyeto nuevo, en este ejemplo es `gitflow-empty`.

    ```sh
    $ mkdir gitflow-empty
    $ cd gitflow-empty
    $ echo "# gitflow-empty" >> README.md
    $ git init
    Initialized empty Git repository in C:/Repos/gitflow-empty/.git/
    $ git add README.md
    warning: LF will be replaced by CRLF in README.md.
    The file will have its original line endings in your working directory
    $ git commit -m "first commit"
    [master (root-commit) af85f5b] first commit
    1 file changed, 1 insertion(+)
    create mode 100644 README.md
    $ git remote add origin https://github.com/jefrnc/gitflow-empty.git
    $ git push -u origin master
    Enumerating objects: 3, done.
    Counting objects: 100% (3/3), done.
    Writing objects: 100% (3/3), 230 bytes | 230.00 KiB/s, done.
    Total 3 (delta 0), reused 0 (delta 0)
    To https://github.com/jefrnc/gitflow-empty.git
    * [new branch]      master -> master
    Branch 'master' set up to track remote branch 'master' from 'origin'.
    ```
    Ahora generamos el branch de develop
    ```sh
    $ git branch develop
    $ git push -u origin develop
    Total 0 (delta 0), reused 0 (delta 0)
    remote:
    remote: Create a pull request for 'develop' on GitHub by visiting:
    remote:      https://github.com/jefrnc/gitflow-empty/pull/new/develop
    remote:
    To https://github.com/jefrnc/gitflow-empty.git
    * [new branch]      develop -> develop
    Branch 'develop' set up to track remote branch 'develop' from 'origin'.

    ```

  Tras ejecutar este comando, git-flow me pregunta una serie de cuestiones sobre la configuración del repositorio, usamos la configuracion por defecto.

    ```sh
    $ git flow init

    Which branch should be used for bringing forth production releases?
      - develop
      - master
    Branch name for production releases: [master]

    Which branch should be used for integration of the "next release"?
      - develop
    Branch name for "next release" development: [develop]

    How to name your supporting branch prefixes?
    Feature branches? [feature/]
    Bugfix branches? [bugfix/]
    Release branches? [release/]
    Hotfix branches? [hotfix/]
    Support branches? [support/]
    Version tag prefix? []
    Hooks and filters directory? [C:/Repos/gitflow-empty/.git/hooks]
    ```

  _Tip:_
    > git-flow configura el repositorio Git usando el comando "git config". git-flow obtiene los valores introducidos al ejecutar este comando y, usando el comando "git config", lo almacena en el archivo ".git/config".


* Como inicializar git flow en un repositorio existente.

    ```sh
    git clone https://github.com/jefrnc/gitflow-empty.git
    git branch -a -v  #verificamos si existen o no los branch de git flow
    git flow init
    ```

  _Tip:_
    > Si ejecuto el comando git flow init vuelvo a indicar cuáles son las ramas y los prefijos que usaré para este proyecto. En este caso uso los valores por defecto sugeridos por git-flow.

* Para verificar la lista de features que estan actualmente podemos verificarlo de la siguiente manera.

    ```sh
    $ git flow feature list
    No feature branches exist.

    You can start a new feature branch:

    git flow feature start <name> [<base>]
    ```
* Cuando tenemos que desarrollar un nuevo feature, debemos proceder de la siguiente manera. Supongamos que es una nueva funcionalidad llamada new-license-control.

    ```sh
    $  git flow feature start new-license-control
    Switched to a new branch 'feature/new-license-control'

    Summary of actions:
    - A new branch 'feature/new-license-control' was created, based on 'develop'
    - You are now on branch 'feature/new-license-control'

    Now, start committing on your feature. When done, use:

    git flow feature finish new-license-control
    ```

  _Por que?:_
    > TBD

  _Alternativa:_
    > Se puede generar sin las extensiones de git flow, y se podria usar directamente git.
    ```sh
    $  git checkout -b new-license-control develop
    ```

* En caso de necesitar guardar nuestros cambios, procedemos a hacer un push del feature.

    ```sh
    $ git flow feature publish new-license-control
    Enumerating objects: 2, done.
    Counting objects: 100% (2/2), done.
    Writing objects: 100% (2/2), 169 bytes | 169.00 KiB/s, done.
    Total 2 (delta 0), reused 0 (delta 0)
    To https://github.com/jefrnc/gitflow-empty.git
    * [new branch]      feature/new-license-control -> feature/new-license-control
    Branch 'feature/new-license-control' set up to track remote branch 'feature/new-license-control' from 'origin'.
    Already on 'feature/new-license-control'
    Your branch is up to date with 'origin/feature/new-license-control'.

    Summary of actions:
    - The remote branch 'feature/new-license-control' was created or updated
    - The local branch 'feature/new-license-control' was configured to track the remote branch
    - You are now on branch 'feature/new-license-control'
    ```

  _Por que?:_
    > TBD
 
* Si otro desarrollador necesita bajar el feature nuestro, debemos seguir los siguientes pasos.

    ```sh
    $ git clone https://github.com/jefrnc/gitflow-empty.git
    Cloning into 'gitflow-empty'...
    remote: Enumerating objects: 6, done.
    remote: Counting objects: 100% (6/6), done.
    remote: Compressing objects: 100% (3/3), done.
    remote: Total 6 (delta 0), reused 6 (delta 0), pack-reused 0
    Unpacking objects: 100% (6/6), done.
    $ cd gitflow-empty
    $ git flow init

    Which branch should be used for bringing forth production releases?
      - master
    Branch name for production releases: [master]
    Branch name for "next release" development: [develop]

    How to name your supporting branch prefixes?
    Feature branches? [feature/]
    Bugfix branches? [bugfix/]
    Release branches? [release/]
    Hotfix branches? [hotfix/]
    Support branches? [support/]
    Version tag prefix? []
    Hooks and filters directory? [/home/jsfrnc/Repos/gitflow-empty/.git/hooks]
    $ git flow feature track new-license-control
    Branch 'feature/new-license-control' set up to track remote branch 'feature/new-license-control' from 'origin'.
    Switched to a new branch 'feature/new-license-control'

    Summary of actions:
    - A new remote tracking branch 'feature/new-license-control' was created
    - You are now on branch 'feature/new-license-control'
    $ git flow feature pull origin new-license-control
    The command 'git flow feature pull' will be deprecated per version 2.0.0. Use 'git flow feature track' instead.
    Pulled origin's changes into feature/new-license-control.
    $ git branch -a
      develop
    * feature/new-license-control
      master
      remotes/origin/HEAD -> origin/master
      remotes/origin/develop
      remotes/origin/feature/new-license-control
      remotes/origin/master
    ```

  _Por que?:_
    > TBD

* Cuando finalizamos el trabajo en el feature, procedemos a cerrar este branch.

    ```sh
    $ git flow feature finish new-license-control
    Switched to branch 'develop'
    Your branch is up to date with 'origin/develop'.
    Updating af85f5b..52044ef
    Fast-forward
    licencia.txt | 1 +
    1 file changed, 1 insertion(+)
    create mode 100644 licencia.txt
    To https://github.com/jefrnc/gitflow-empty.git
    - [deleted]         feature/new-license-control
    Deleted branch feature/new-license-control (was 52044ef).

    Summary of actions:
    - The feature branch 'feature/new-license-control' was merged into 'develop'
    - Feature branch 'feature/new-license-control' has been locally deleted; it has been remotely deleted from 'origin'
    - You are now on branch 'develop'
    ```

    Este comando realiza las siguientes operaciones:
    - La rama 'feature/new-license-control' es fusionada en la rama 'develop'
    - La rama local 'feature/new-license-control' se elimina.
    - La rama remota 'feature/new-license-control' se elimina del 'origin'.
    - La rama activa pasa a ser 'develop'.

     
    ```sh
    $ git branch -a -v
    * develop                52044ef [ahead 1] subimos licencia
      master                 af85f5b first commit
      remotes/origin/develop af85f5b first commit
      remotes/origin/master  af85f5b first commit

    $ git push
    Enumerating objects: 4, done.
    Counting objects: 100% (4/4), done.
    Delta compression using up to 8 threads
    Compressing objects: 100% (2/2), done.
    Writing objects: 100% (3/3), 291 bytes | 291.00 KiB/s, done.
    Total 3 (delta 0), reused 0 (delta 0)
    To https://github.com/jefrnc/gitflow-empty.git
      af85f5b..52044ef  develop -> develop
    ```
    Verificamos y hacemos un push.

  _Tip:_
    > Como veran, las extensiones de git flow simplifican la operatoria.


  _Alternativa:_
    > Se puede generar sin las extensiones de git flow, y se podria usar directamente git. El trabajo es incorporado en la rama new-license-control mediante los correspondientes commits. Una vez finalizado el trabajo, me cambio a la rama develop, que es la que va a recibir la nueva funcionalidad, borro la rama y la subo al repositorio remoto.
    ```sh
    $  git checkout develop 
    $ git merge --no-ff new-license-control 
    $ git branch -d new-license-control 
    $ git push origin develop
    ```
  _Tip:_
    > El parámetro "--no-ff" en la fusión de las dos ramas fuerza la creación de un nuevo commit aunque la fusión se pudiera haber hecho mediante fast-forward (si se hubiera llevado a cabo de forma recursiva este nuevo commit se crea siempre). Este commit permite en un futuro localizar donde se ha incorporado una determinada funcionalidad (un conjunto de commits) y poder revertirla.


* Como genero un Release?

    Voy a suponer que la versión estable que se encuentra en producción es la 2.5.1 y que he decidido publicar la versión 2.6 en la siguiente release (siguiendo, por ejemplo, la convención de versionamiento semántico).

    ```sh
    $ git checkout -b release-2.6 develop
    ```

    Cuando la release está lista para ser publicada, llevo esta rama de release a la rama master y etiqueto este commit para poder conocer posteriormente qué commit ha sido enviado a producción con esa versión concreta.

    ```sh
    $ git checkout master 
    $ git merge --no-ff release-2.6 
    $ git tag -a 2.6
    ```

    Los cambios que se han producido en la rama de release también tienen que ser llevados a la rama de develop

    ```sh
    $ git checkout develop $ 
    $ git merge --no-ff release-2.6
    ```

    Tras llevar los cambios a las ramas master y develop, ahora puedo borrar la rama de release
    ```sh
    $ git branch -d release-2.6
    ```
 
* Como genero un Hotfix?
    Voy a suponer que he encontrado en la release anterior, etiquetada con el nombre "2.6" un error. Lo primero que hago es crear una nueva rama de hotfix a partir de la rama master.

    ```sh
    $ git checkout -b hotfix-2.6.1 master
    ```

    Cuando la release está lista para ser publicada, llevo esta rama de bugfix a la rama master y etiqueto este commit para poder conocer posteriormente qué commit ha sido enviado a producción con esa versión concreta.

    ```sh
    $ git checkout master $ 
    $ git merge --no-ff bugfix-2.6.1 
    $ git tag -a 2.6.1
    ```
    Los cambios que se han producido en la rama de bugfix también tienen que ser llevados a la rama de develop

    ```sh
    $ git checkout develop 
    $ git merge --no-ff bugfix-2.6.1
    ```
    Tras llevar los cambios a las ramas master y develop, ahora puedo borrar la rama de bugfix

    ```sh
    $ git branch -d bugfix-2.6.1
    ```
 
* Depurar ramas muertas de nuestra carpeta local.

    ```sh
    $ git checkout master; 
    $ git pull origin master; 
    $ git fetch --all -p; 
    $ git branch -vv | grep ": gone]" | awk '{ print $1 }' | xargs -n 1 git branch -D
    ```

  _Por que?:_
    > Se puede dar que otros companeros de trabajo generaron branch los cuales caducaron y generar estos cadaveres en nuestro equipo, es bueno tener todo al dia.
 
 
* Comitear un cambio.
    ```sh
    git add <file1> <file2> ...
    git commit
    ```
  _Por que?:_
    > `git add <file1> <file2> ... ` - solo debe agregar archivos que constituyan un cambio pequeño y coherente.
    
    > `git commit`  iniciará un editor que le permite separar el tema del cuerpo
    
    _Tip:_
    > En su lugar, puede usar `git add -p`, que le dará la oportunidad de revisar todos los cambios introducidos uno por uno, y decidir si incluirlos en el commit o no.

* Actualiza tu rama con los cambios que han hecho en la rama superior 

    ```sh
    git checkout develop
    git pull
    ```
    
  _Por que?:_    
    > Esto le dará la oportunidad de lidiar con conflictos en tu máquina y no en el rebasing (más adelante) en lugar de crear un Pull Request que contenga conflictos.

* Actualice su rama de `feature` con los últimos cambios desde el desarrollo mediante rebase interactivo.
    ```sh
    git checkout <branchname>
    git rebase -i --autosquash develop
    ```
    
  _Por que?:_
    > Puede usar --autosquash para realizar una sola confirmación. . [read more...](https://robots.thoughtbot.com/autosquashing-git-commits)
      
* Si no tiene conflictos, omita este paso. Si tiene conflictos, [resuélvalos](https://help.github.com/articles/resolving-a-merge-conflict-using-the-command-line/) y continúe rebase.

    ```sh
    git add <file1> <file2> ...
    git rebase --continue
    ```

* Push tu rama. Rebase cambiará el historial, por lo que tendrá que usar '-f' para forzar los cambios en la rama remota. Si alguien más está trabajando en su rama, use el menos destructivo `--force-with-lease`.

    ```sh
    git push -f
    ```
    
  _Por que?:_
    > Cuando hace un rebase, está cambiando el historial en su rama de características. Como resultado, Git rechazará el `git push` normal. En su lugar, deberá usar el indicador -f o --force. [leer más ...](https://developer.atlassian.com/blog/2015/04/force-with-lease/)
    
* Genere un Pull Request.
* El Pull Request aceptada, fusionada y cerrada por un aprovador (reviewer).
* Elimine su rama de `feature` local si ha terminado.

  ```sh
  git branch -d <branchname>
  ```
  para eliminar todas las ramas que ya no están en el repositorio remoto

  ```sh
  git fetch -p && for branch in `git branch -vv --no-color | grep ': gone]' | awk '{print $1}'`; do git branch -D $branch; done
  ```

<a name="writing-good-commit-messages"></a>
### 1.3 Escribir mensajes descriptivos en el commit

Tener una buena guía para crear commits y cumplirla hace que trabajar con Git y colaborar con otros sea mucho más fácil. Aquí hay algunas reglas generales ([fuente](https://chris.beams.io/posts/git-commit/#seven-rules)):

 * Separe el sujeto del cuerpo con una nueva línea entre los dos.

  _Por que?:_
    > Git es lo suficientemente inteligente como para distinguir la primera línea de su mensaje de confirmación como su resumen. De hecho, si prueba git shortlog, en lugar de git log, verá una larga lista de mensajes de confirmación, que consta de la identificación de la confirmación y solo el resumen.

 * Limite la línea de asunto a 50 caracteres y ajuste el cuerpo a 72 caracteres.

  _Por que?:_
    > Los commits deben ser lo más precisos y enfocados posible, no es el lugar para ser detallado. [Lee mas...](https://medium.com/@preslavrachev/what-s-with-the-50-72-rule-8a906f61f09c)

 * Capitalizar la línea de asunto.
 * No termine la línea de asunto con un punto.
 * Use modo [imperativo](https://en.wikipedia.org/wiki/Imperative_mood) en la línea de asunto.

  _Por que?:_
    > En lugar de escribir mensajes que digan lo que ha hecho, es mejor considerar estos mensajes como las instrucciones de lo que se hará después de que se aplique la confirmación en el repositorio. Osea el valor o funcionalidad que aporta nuestro aporte en el repositorio.  


* Use el cuerpo para explicar **qué** y **por qué** en lugar de **cómo**.

 <a name="documentation"></a>
## 2. Documentacion

* Use esta [plantilla] (./README.sample.md) para `README.md`, siéntase libre de agregar secciones.
* Para proyectos con más de un repositorio, proporcione enlaces a ellos en sus respectivos archivos `README.md`.
* Mantenga `README.md` actualizado a medida que evoluciona un proyecto.
* Comenta tu código. Trate de dejar lo más claro posible lo que pretende con cada sección principal.
* Si hay una discusión abierta sobre github o stackoverflow sobre el código o enfoque que está utilizando, incluya el enlace en su comentario.
* No utilice comentarios como excusa para un código incorrecto. Mantén tu código limpio.
* Mantenga los comentarios relevantes a medida que evoluciona su código.

<a name="environments"></a>
## 3. Ambientes/Environments

* Defina entornos separados de `desarrollo`,` prueba` y `producción` si es necesario.

  _Por que?:_
    > Es posible que se necesiten diferentes datos, tokens, API, puertos, etc en diferentes entornos. Es posible que desee un modo de "desarrollo" aislado que llame a una API falsa que devuelva datos predecibles, lo que facilita mucho las pruebas automáticas y manuales. O puede que desee habilitar Google Analytics solo en `producción` y así sucesivamente. [Lee mas...](https://stackoverflow.com/questions/8332333/node-js-setting-up-environment-specific-configs-to-be-used-with-everyauth)


* Cargue sus configuraciones específicas de implementación desde variables de entorno y nunca las agregue a la base de código como constantes.

  _Por que?:_
    > Tiene tokens, contraseñas y otra información valiosa allí. Su configuración debe estar correctamente separada de las partes internas de la aplicación como si la base de código pudiera hacerse pública en cualquier momento.

    _Como?:_
    > Archivos `.env` para almacenar sus variables y agregarlas a` .gitignore` para excluirlas. En su lugar, suba un archivo `.env.example` que sirva de guía para los desarrolladores. [Leer mas](https://medium.com/@rafaelvidaurre/managing-environment-variables-in-node-js-2cb45a55195f)

* Se recomienda validar las variables de entorno antes de que se inicie la aplicación. [Un ejemplo](./configWithTest.sample.js) usando `joi` para validar los valores proporcionados.
    
  _Por que?:_
    > Puede salvar a otros de horas de resolución de problemas.

    ***TBD:: TODO // FALTA IMPLEMENTAR ALGUN EJEMPLO EN JAVA / NET***

<a name="consistent-dev-environments"></a>
### 3.1 Entornos de desarrollo consistentes:
* Establezca la versión de su nodo en `engine` en` package.json`.
    
  _Por que?:_
    > Les permite a otros saber la versión de node en el que trabaja el proyecto. [Lee mas...](https://docs.npmjs.com/files/package.json#engines)

* Además, use `nvm` y cree un` .nvmrc` en la raíz de su proyecto. No olvides mencionarlo en la documentación.

  _Por que?:_
    > Cualquiera que use `nvm` puede simplemente usar` nvm use` para cambiar a la versión de nodo adecuada. [read more...](https://github.com/creationix/nvm)

* Es una buena idea configurar un script `preinstall` que verifique las versiones de node y npm.

  _Por que?:_
    > Algunas dependencias pueden fallar cuando son instaladas por versiones más nuevas de npm.
    
* Use Docker si es posible.

  _Por que?:_
    > Puede brindarle un entorno consistente en todo el flujo de trabajo. Sin mucha necesidad de jugar con dependencias o configuraciones. [Lee mas...](https://hackernoon.com/how-to-dockerize-a-node-js-application-4fbab45a0c19)



<a name="testing"></a>
## 5. Testing

* Tenga un entorno de `test` si es necesario, en realidad deberia ser mandatorio a mi opinion.

  _Por que?:_
    > Si bien a veces las pruebas de punta a punta (regresion) en `producción` pueden parecer suficientes, hay algunas excepciones: un ejemplo es que es posible que no desee habilitar el test en este ambiente para no producir problemas de consistencia de datos o modificar informacion vital para el negocio. El otro ejemplo es que su API puede tener límites de velocidad en `producción` y bloquea sus llamadas de prueba después de una cierta cantidad de solicitude en caso de tratarse de una API, por lo general los ambientes mas cercanos a produccion cuenta con restricciones de seguridad informatica.


* Coloque sus archivos de prueba adicionales en una carpeta de prueba separada para evitar confusiones.

  _Por que?:_
    > Algunos archivos de prueba no se relacionan particularmente con ningún archivo de implementación específico. Debe colocarlo en una carpeta que otros desarrolladores puedan encontrar: la carpeta `__test__`. Este nombre: `__test__` también es estándar ahora y la mayoría de los framework de testing por ejemplo de JavaScript lo utilizan.


* Ejecute pruebas localmente antes de realizar cualquier Pull Request para `develop`.

  _Por que?:_
    > No querrás ser el responsable que provocó el fracaso de la compilación de la rama superior. Ejecute sus pruebas después de su `rebase` y antes de llevar su rama de feature-branch a un repositorio remoto

* Documente sus pruebas, incluidas las instrucciones en la sección correspondiente de su archivo `README.md`.

  _Por que?:_
    > Es una nota útil que deja para otros desarrolladores/DevOp/QA o cualquier persona que tenga la suerte de trabajar en su código.
 
<a name="code-style"></a>
## 7. Estilo de código

<a name="code-style-check"></a>
### 7.1 Algunas pautas de estilo de código

* Dependiendo del tamaño de la tarea, use `// TODO:` comentarios o abra un ticket.

  _Por que?:_
    > Entonces puede recordarse a sí mismo y a los demás acerca de una tarea pequeña (como refactorizar una función o actualizar un comentario). Para tareas más grandes, use `// TODO (# 3456)`, que se aplica mediante una regla de lint y el número es un ticket abierto.

* Siempre comente y manténgalos relevantes a medida que cambia el código. Eliminar los bloques de código comentados.
    
  _Por que?:_
    > Su código debe ser lo más legible posible, debe deshacerse de cualquier cosa que lo distraiga. Si refactorizó una función, no solo comente la anterior, elimínela.

* Evite comentarios, registros o nombres irrelevantes o divertidos.

  _Por que?:_
    > Si bien su proceso de compilación puede (debería) deshacerse de ellos, a veces su código fuente puede entregarse a otra empresa / cliente y es posible que no compartan las mismas bromas.

* Haga que sus nombres sean buscables con distinciones significativas para evitar nombres abreviados. Para las funciones, use nombres largos y descriptivos. El nombre de una función debe ser un verbo o una frase verbal, y debe comunicar su intención.
  _Por que?:_
    > Hace que sea más natural leer el código fuente.


<a name="enforcing-code-style-standards"></a>
### 7.2 Hacer cumplir los estándares de estilo de código

* Utilice un archivo [.editorconfig] (http://editorconfig.org/) que ayuda a los desarrolladores a definir y mantener estilos de codificación consistentes entre diferentes editores e IDE en el proyecto.

  _Por que?:_
    > El proyecto EditorConfig consta de un formato de archivo para definir estilos de codificación y una colección de complementos de editor de texto que permiten a los editores leer el formato de archivo y adherirse a los estilos definidos. Los archivos EditorConfig son fáciles de leer y funcionan muy bien con los sistemas de control de versiones.

* Haga que su editor le notifique sobre los errores de estilo de código. Utilice [eslint-plugin-prettier] (https://github.com/prettier/eslint-plugin-prettier) y [eslint-config-prettier] (https://github.com/prettier/eslint-config-prettier) con su configuración existente de ESLint. [leer más ...] (https://github.com/prettier/eslint-config-prettier#installation)

* Considera usar Git hooks.

  _Por que?:_
    > Los Git hooks aumentan enormemente la productividad de un desarrollador. Realice cambios, comitee y haga push a stagging o produccion sin temor a romper las compilaciones. [Lee mas...](http://githooks.com/)

* Use Prettier con un precommit hook.

  _Por que?:_
    > Si bien `prettier` en sí mismo puede ser muy poderoso, no es muy productivo ejecutarlo simplemente como una tarea npm solo cada vez que formatea el código. Aquí es donde entran en juego `lint-staged` (y` husky`). Lea más sobre la configuración de `lint-staged` [aquí] (https://github.com/okonet/lint-staged#configuration) y sobre la configuración de` husky` [aquí](https://github.com/typicode/husky).


<a name="logging"></a>
## 8. Logging

* Evite los registros de la consola del lado del cliente en producción

  _Por que?:_
    > Aunque su proceso de compilación puede (debería) deshacerse de ellos, asegúrese de que su verificador de estilo de código le advierta sobre los registros de consola restantes.

* Producir registros de producción legibles. Idealmente, use componentes de loggin para usar en modo de producción (como [winston] (https://github.com/winstonjs/winston) o [nodo-bunyan] (https://github.com/trentm/node-bunyan)).

  _Por que?:_
    > Hace que su solución de problemas sea menos desagradable con la coloración, las marcas de tiempo, el registro en un archivo además de la consola o incluso el registro en un archivo que fracciona diariamente. [Lee mas...](https://blog.risingstack.com/node-js-logging-tutorial/)


<a name="api"></a>
## 9. API
<a name="api-design"></a>

### 9.1 API design

_Por que?:_
> Porque tratamos de forzar el desarrollo de interfaces RESTful construidas de manera sensata, que los miembros del equipo y los clientes puedan consumir de manera simple y consistente.

_Por que?:_
> La falta de consistencia y simplicidad puede aumentar enormemente los costos de integración y mantenimiento. Es por eso que el "diseño API" está incluido en este documento.


* Principalmente seguimos un diseño orientado a los recursos. Tiene tres factores principales: recursos, colecciones y URL.
    * Un recurso tiene datos, se anida y existen métodos que operan en su contra.
    * Un grupo de recursos se llama colección.
    * URL identifica la ubicación en línea del recurso o colección.
    
  _Por que?:_
    > Este es un diseño muy conocido para los desarrolladores (sus principales consumidores de API). Además de la legibilidad y la facilidad de uso, nos permite escribir bibliotecas y conectores genéricos sin siquiera saber de qué se trata la API.

* use kebab-case para las URL. TBD // BUSCAR EJEMPLO
* use camelCase para los parámetros de la QueryString o en los campos de recursos.
* use plural kebab-case para nombres de recursos en URL.

* Utilice siempre un nombre plural para nombrar una url que apunta a una colección: `/users`.

  _Por que?:_
    > Básicamente, se lee mejor y mantiene las URL consistentes. [Lee mas...](https://apigee.com/about/blog/technology/restful-api-design-plural-nouns-and-concrete-names)


* Siempre use un concepto singular que comience con una colección y termine con un identificador:

    ```
    /students/245743
    /airports/kjfk
    ```
* Evite URL como esta:
    ```
    GET /blogs/:blogId/posts/:postId/summary
    ```

  _Por que?:_
    > Esto no apunta a un recurso sino a una propiedad en su lugar. Puede pasar la propiedad como parámetro para recortar su respuesta

* Mantenga los verbos fuera de las URL de sus recursos.

  _Por que?:_
    > Porque si usa un verbo para cada operación de recursos, pronto tendrá una gran lista de URL y ningún patrón consistente que dificulte el aprendizaje de los desarrolladores. Además, usamos verbos para otra cosa.

* Use verbos para non-resources. En este caso, su API no devuelve ningún recurso. En su lugar, ejecuta una operación y devuelve el resultado. Estas **no son** operaciones CRUD (crear, recuperar, actualizar y eliminar)

    ```
    /translate?text=Hallo
    ```

  _Por que?:_
    > Porque para CRUD usamos métodos HTTP en URL de `resource` o` collection`. Los verbos de los que estábamos hablando son en realidad `Controllers`. Usualmente no desarrollas muchos de estos. [Lee mas...](https://byrondover.github.io/post/restful-api-guidelines/#controller)
 

* El cuerpo de la solicitud o el tipo de respuesta es JSON, entonces siga `camelCase` para los nombres de propiedad` JSON` para mantener la coherencia.
  _Por que?:_
    > Se use que muchas de las integraciones que tenemos son en JavaScript, donde se supone que el lenguaje de programación para generar y analizar JSON.

* Aunque un recurso es un concepto singular que es similar a una instancia de objeto o registro de base de datos, no debe usar su `table_name` para un nombre de recurso y una propiedad de recurso` column_name`.
  _Por que?:_
    > Debido a que su intención es exponer Recursos, no los detalles de su esquema de base de datos. La idea es no generar un hueco de seguridad, mientras menos informacion exista de nuestro esquema real mucho mejor.

* Nuevamente, solo use sustantivos en su URL cuando nombre sus recursos y no intente explicar su funcionalidad.

  _Por que?:_
    > Solo use sustantivos en sus URL de recursos, evite puntos finales como `/addNewUser` o` /updateUser`. También evite enviar operaciones de recursos como parámetro.

* Implemente las funcionalidades CRUD utilizando métodos HTTP:

    _Como?:_
    > `GET`: Para obtener un resources.

    > `POST`: Para crear un nuevo resources o sub-resources.
   
    > `PUT`: Para actualizar un resources.
    
    > `PATCH`: Para actualizar un resources. Solo se actualiza los campos que se envian, el resto se ignoran. 
    
    > `DELETE`:	Para borrar un resources.


* Para recursos anidados, use la relación entre ellos en la URL. Por ejemplo, usar 'id' para relacionar a un empleado con una empresa.

  _Por que?:_
    > Esta es una forma natural de hacer que los recursos sean explorables.

    _Como?:_

    > `GET      /schools/2/students	` , debe obtener la lista de todos los estudiantes de la escuela 2.

    > `GET      /schools/2/students/31`	, debe obtener los detalles del estudiante 31, que pertenece a la escuela 2.

    > `DELETE   /schools/2/students/31`	, debe eliminar el estudiante 31, que pertenece a la escuela 2.

    > `PUT      /schools/2/students/31`	, debe actualizar la información del estudiante 31, usar PUT solo en la URL del recurso, no en el resource.

    > `POST     /schools` , debe crear una nueva escuela y devolver los detalles de la nueva escuela creada. Utilice POST en URL de colección.

* Use un número ordinal simple para una versión con un prefijo `v` (v1, v2). Muévalo completamente hacia la izquierda en la URL para que tenga el alcance más alto:

    ```
    http://api.domain.com/v1/schools/3/students	
    ```

  _Por que?:_
    > Cuando sus API son públicas para otros terceros, la actualización de las API con algunos cambios importantes también conduciría a la ruptura de los productos o servicios existentes que utilizan sus API. El uso de versiones en su URL puede evitar que eso suceda. [Lee mas...](https://apigee.com/about/blog/technology/restful-api-design-tips-versioning)


* Los mensajes de respuesta deben ser autodescriptivos. Una buena respuesta de mensaje de error podría verse así:
    ```json
    {
        "code": 1234,
        "message" : "Something bad happened",
        "description" : "More details"
    }
    ```
    o para errores de validación:
    ```json
    {
        "code" : 2314,
        "message" : "Validation Failed",
        "errors" : [
            {
                "code" : 1233,
                "field" : "email",
                "message" : "Invalid email"
            },
            {
                "code" : 1234,
                "field" : "password",
                "message" : "No password provided"
            }
          ]
    }
    ```

  _Por que?:_
    > los desarrolladores dependen de errores bien diseñados en los momentos críticos cuando están solucionando problemas y resolviendo problemas después de que las aplicaciones que han creado utilizando sus API están en manos de sus usuarios.


    _Nota: Mantenga los mensajes de excepción de seguridad tan genéricos como sea posible. Por ejemplo, en lugar de decir "contraseña incorrecta", puede responder de nuevo diciendo "nombre de usuario o contraseña no válidos" para que no informemos al usuario sin saberlo que el nombre de usuario era correcto y solo la contraseña era incorrecta._

* Use estos códigos de estado (http codes) para enviar con su respuesta para describir si **todo funcionó**,La **aplicación cliente hizo algo mal** o La **API hizo algo mal**.
    
    _Cuales?:_
    > `200 OK` representa que la operacion funciono correctamente para los verbos `GET`, `PUT` o `POST` .

    > `201 Created` en la creacion de una nuevo objetco, en caso de usar un verbo POST si todo funciono correctamente deberia retorar este codigo http.

    > `204 No Content` la respuesta representa el éxito pero no hay contenido para enviar en la respuesta. Úselo cuando la operación `DELETE` tenga éxito.

    > `304 Not Modified` la respuesta es minimizar la transferencia de información cuando el destinatario ya tiene representaciones en caché.

    > `400 Bad Request` para cuando la solicitud no se procesó, ya que el servidor no pudo entender lo que el cliente solicita.

    > `401 Unauthorized` para cuando la solicitud carece de credenciales válidas y debe volver a solicitarla con las credenciales requeridas.

    > `403 Forbidden` significa que el servidor entendió la solicitud pero se niega a autorizarla.

    > `404 Not Found` indica que no se encontró el recurso solicitado.

    > `500 Internal Server Error` indica que la solicitud es válida, pero el servidor no pudo cumplirla debido a alguna condición inesperada.

  _Por que?:_
    > La mayoría de los proveedores de API utilizan un pequeño subconjunto de códigos de estado HTTP. Por ejemplo, la API de Google GData usa solo 10 códigos de estado, Netflix usa 9 y Digg, solo 8. Por supuesto, estas respuestas contienen un cuerpo con información adicional. Hay más de 70 códigos de estado HTTP. Sin embargo, la mayoría de los desarrolladores no tienen todos los 70 memorizados. Por lo tanto, si elige códigos de estado que no son muy comunes, obligará a los desarrolladores de aplicaciones a no construir sus aplicaciones y a Wikipedia para averiguar qué está tratando de decirles. [Lee mas...](https://apigee.com/about/blog/technology/restful-api-design-what-about-errors)

* Proveedor el total de numeros de resources de tu respuesta.
* Aceptar los parametros `limit` y `offset` para limitar la respuesta.

* La cantidad de datos que expone el recurso también debe tenerse en cuenta. El consumidor de API no siempre necesita la representación completa de un recurso. Use un parámetro de consulta de campos que tome una lista de campos separados por comas para incluir:

    ```
    GET /student?fields=id,name,age,class
    ```
* La paginación, el filtrado y la clasificación no necesitan ser compatibles desde el principio para todos los recursos. Documente los recursos que ofrecen filtrado y clasificación.

<a name="api-security"></a>
### 9.2 Seguridad de la API
Estas son algunas de las mejores prácticas básicas de seguridad:

* No use la autenticación básica a menos que sea a través de una conexión segura (HTTPS). Los tokens de autenticación no deben transmitirse en la URL: `GET /users/123?token=asdf....`

  _Por que?:_
    > Debido a que el token, o ID de usuario y contraseña se pasan a través de la red como texto sin cifrar (está codificado en base64, pero base64 es una codificación reversible), el esquema de autenticación básico no es seguro. [Lee mas...](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication)

* Los tokens deben transmitirse utilizando el encabezado de autorización en cada solicitud: `Authorization: Bearer xxxxxx, Extra yyyyy`.

* El código de autorización debe ser de corta duración.

* Rechace cualquier solicitud que no sea TLS, evite cualquier intercambio de datos inseguro. Responda a las solicitudes HTTP por `403 Forbidden`.

* Considere usar un Rate Limiting.

  _Por que?:_
    > Para proteger sus API de amenazas de bot que llaman a su API miles de veces por hora. Debería considerar implementar un límite de tasa desde el principio.

* Establecer los encabezados HTTP de manera adecuada puede ayudar a bloquear y proteger su aplicación web. [Lee mas...](https://github.com/helmetjs/helmet)

* Su API debe convertir los datos recibidos a su forma canónica o rechazarlos. Devuelva un codigo de error 400 en caso de datos incorrectos o faltantes.

* Todos los datos intercambiados con la API REST deben ser validados por la API.

* Serialize su JSON.

  _Por que?:_
    > Es importante validar la mensajeria json, par evitar cualquier tipo de injeccion de codigo malicioso.

* Valide que el content-type use `application/*json` (Content-Type header).
    
  _Por que?:_
    > Por ejemplo, aceptar el tipo mime `application / x-www-form-urlencoded` le permite al atacante crear un formulario y activar una simple solicitud POST. El servidor nunca debe asumir el tipo de contenido. La falta de un encabezado de tipo de contenido o un encabezado inesperado de tipo de contenido debe hacer que el servidor rechace el contenido con una respuesta `4XX`.

* Revise este proyecto API Security Checklist. [Ver mas](https://github.com/shieldfy/API-Security-Checklist)

<a name="api-documentation"></a>
### 9.3 Documentación de la API
* Complete la sección `Referencia de API` en [README.md template](./README.sample.md).
* Describa los métodos de autenticación de API con una muestra de código.s
* Explique la estructura de URL (solo ruta, sin URL raíz) incluyendo el tipo de solicitud (Method).

Para cada endpoint, explique:
* Parámetros de URL: Si existen parámetros de URL, especifíquelos de acuerdo con el nombre mencionado en la sección de URL:

    ```
    Required: id=[integer]
    Optional: photo_id=[alphanumeric]
    ```

* Si el tipo de solicitud es POST, proporcione ejemplos de trabajo. Las reglas de los parámetros de URL también se aplican aquí. Separe la sección en Opcional y Requerido.
* En caso de que la respuesta sea correcta, ¿Cuál debería ser el código de estado? ¿Hay algún dato de devolución? Esto es útil cuando las personas necesitan saber qué deben esperar sus devoluciones de llamada:

    ```
    Code: 200
    Content: { id : 12 }
    ```

* Respuesta de error?, la mayoría de los puntos finales tienen muchas formas de fallar. Desde el acceso no autorizado a parámetros erróneos, etc. Todos estos deben estar listados aquí. Puede parecer repetitivo, pero ayuda a evitar que se hagan suposiciones. Por ejemplo
    ```json
    {
        "code": 403,
        "message" : "Authentication failed",
        "description" : "Invalid username or password"
    }   
    ```

* Use herramientas de diseño de API. Hay muchas herramientas de código abierto para una buena documentación, como [API Blueprint](https://apiblueprint.org/) y [Swagger](https://swagger.io/).

<a name="cicd"></a>
## 8. CI/CD (Integración y Despliegue Continuo)

### 8.1 Principios básicos de CI/CD

* Configure pipelines de CI/CD desde el inicio del proyecto.

  _¿Por qué?:_
    > Detecta errores tempranamente, automatiza tareas repetitivas y asegura que el código en producción siempre esté en un estado desplegable.

* Ejecute todas las pruebas en cada commit a las ramas principales.

  _¿Cómo?:_
    > Use herramientas como GitHub Actions, GitLab CI, Jenkins o CircleCI:
    ```yaml
    # Ejemplo de GitHub Actions
    name: CI
    on: [push, pull_request]
    jobs:
      test:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v2
          - run: npm install
          - run: npm test
          - run: npm run lint
    ```

* Automatice el versionado semántico.

  _¿Por qué?:_
    > Mantiene un historial claro de cambios y facilita el rollback si es necesario.

### 8.2 Mejores prácticas de deployment

* Use estrategias de deployment como Blue-Green o Canary.
* Implemente feature flags para activar/desactivar funcionalidades.
* Mantenga los secretos fuera del código usando variables de entorno o servicios de gestión de secretos.

<a name="containerization"></a>
## 9. Containerización

### 9.1 Docker

* Siempre use imágenes base oficiales y específicas.

  ```dockerfile
  # ❌ Evitar
  FROM node:latest
  
  # ✅ Preferir
  FROM node:18.17-alpine3.18
  ```

* Optimice las capas del Dockerfile.

  _¿Por qué?:_
    > Reduce el tamaño de la imagen y mejora los tiempos de construcción aprovechando la caché de Docker.

  ```dockerfile
  # Copiar dependencias primero para aprovechar caché
  COPY package*.json ./
  RUN npm ci --only=production
  
  # Luego copiar el código fuente
  COPY . .
  ```

* Use multi-stage builds para reducir el tamaño final.

  ```dockerfile
  # Etapa de construcción
  FROM node:18-alpine AS builder
  WORKDIR /app
  COPY package*.json ./
  RUN npm ci
  COPY . .
  RUN npm run build
  
  # Etapa de producción
  FROM node:18-alpine
  WORKDIR /app
  COPY --from=builder /app/dist ./dist
  COPY --from=builder /app/node_modules ./node_modules
  CMD ["node", "dist/index.js"]
  ```

### 9.2 Docker Compose

* Use Docker Compose para desarrollo local con múltiples servicios.
* Defina redes personalizadas para mejor aislamiento.
* Use archivos `.env` para configuración (no olvide agregarlos a `.gitignore`).

<a name="monitoring"></a>
## 10. Monitoreo y Observabilidad

### 10.1 Los tres pilares de la observabilidad

* **Logs**: Use structured logging con niveles apropiados.
  
  ```javascript
  // Ejemplo con Winston
  logger.info('Usuario autenticado', {
    userId: user.id,
    timestamp: new Date().toISOString(),
    action: 'login'
  });
  ```

* **Métricas**: Implemente métricas de negocio y técnicas.
  - Latencia de respuesta
  - Tasa de errores
  - Uso de recursos
  - Métricas de negocio específicas

* **Trazas**: Use distributed tracing para sistemas complejos.
  - OpenTelemetry
  - Jaeger
  - Zipkin

### 10.2 Alertas

* Configure alertas basadas en SLOs (Service Level Objectives).
* Use la regla de las 4 golden signals:
  - **Latency**: Tiempo de respuesta
  - **Traffic**: Volumen de solicitudes
  - **Errors**: Tasa de errores
  - **Saturation**: Uso de recursos

### 10.3 Dashboards

* Cree dashboards para diferentes audiencias:
  - **Técnico**: Métricas de infraestructura y aplicación
  - **Negocio**: KPIs y métricas de usuario
  - **Ejecutivo**: Vista de alto nivel del estado del sistema

<a name="security"></a>
## 11. Seguridad - Mejores Prácticas Actualizadas

### 11.1 Seguridad en el Código

* **Nunca hardcodees credenciales o secretos**
  
  ```javascript
  // ❌ NUNCA hagas esto
  const apiKey = "sk-1234567890abcdef";
  
  // ✅ Usa variables de entorno
  const apiKey = process.env.API_KEY;
  ```

* **Valida y sanitiza TODAS las entradas de usuario**
  
  ```javascript
  // Ejemplo con Express y express-validator
  app.post('/user', [
    body('email').isEmail().normalizeEmail(),
    body('age').isInt({ min: 0, max: 120 }),
    body('name').trim().escape()
  ], (req, res) => {
    // Procesar solicitud
  });
  ```

* **Usa prepared statements para consultas SQL**
  
  ```javascript
  // ❌ Vulnerable a SQL injection
  const query = `SELECT * FROM users WHERE id = ${userId}`;
  
  // ✅ Usa parámetros preparados
  const query = 'SELECT * FROM users WHERE id = ?';
  db.query(query, [userId]);
  ```

### 11.2 Dependencias y Vulnerabilidades

* **Audita regularmente las dependencias**
  
  ```bash
  # Node.js
  npm audit
  npm audit fix
  
  # Python
  pip-audit
  safety check
  
  # Java
  mvn dependency-check:check
  ```

* **Mantén las dependencias actualizadas**
  - Usa herramientas como Dependabot, Renovate o Snyk
  - Revisa los changelogs antes de actualizar versiones mayores
  - Ten un proceso para parchear vulnerabilidades críticas rápidamente

### 11.3 Autenticación y Autorización

* **Implementa autenticación robusta**
  - Usa bibliotecas probadas (Passport.js, Spring Security, etc.)
  - Implementa MFA cuando sea posible
  - Usa tokens con expiración (JWT con refresh tokens)

* **Principio de menor privilegio**
  ```javascript
  // Ejemplo de middleware de autorización
  function authorize(roles = []) {
    return (req, res, next) => {
      if (!req.user || !roles.includes(req.user.role)) {
        return res.status(403).json({ message: 'Forbidden' });
      }
      next();
    };
  }
  
  // Uso
  app.get('/admin', authorize(['admin']), adminController);
  ```

### 11.4 Comunicación Segura

* **Usa HTTPS en todos los ambientes**
  ```javascript
  // Forzar HTTPS en Express
  app.use((req, res, next) => {
    if (!req.secure && process.env.NODE_ENV === 'production') {
      return res.redirect('https://' + req.headers.host + req.url);
    }
    next();
  });
  ```

* **Implementa headers de seguridad**
  ```javascript
  // Usando helmet.js
  const helmet = require('helmet');
  app.use(helmet({
    contentSecurityPolicy: {
      directives: {
        defaultSrc: ["'self'"],
        styleSrc: ["'self'", "'unsafe-inline'"]
      }
    },
    hsts: {
      maxAge: 31536000,
      includeSubDomains: true,
      preload: true
    }
  }));
  ```

### 11.5 Manejo de Datos Sensibles

* **Encripta datos sensibles en reposo**
  ```javascript
  const crypto = require('crypto');
  
  function encrypt(text) {
    const algorithm = 'aes-256-gcm';
    const key = Buffer.from(process.env.ENCRYPTION_KEY, 'hex');
    const iv = crypto.randomBytes(16);
    const cipher = crypto.createCipheriv(algorithm, key, iv);
    
    let encrypted = cipher.update(text, 'utf8', 'hex');
    encrypted += cipher.final('hex');
    
    const authTag = cipher.getAuthTag();
    
    return {
      encrypted,
      authTag: authTag.toString('hex'),
      iv: iv.toString('hex')
    };
  }
  ```

* **Implementa rate limiting**
  ```javascript
  const rateLimit = require('express-rate-limit');
  
  const limiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutos
    max: 100, // límite de solicitudes
    message: 'Demasiadas solicitudes, intente más tarde'
  });
  
  // Límite más estricto para login
  const loginLimiter = rateLimit({
    windowMs: 15 * 60 * 1000,
    max: 5,
    skipSuccessfulRequests: true
  });
  
  app.use('/api/', limiter);
  app.use('/api/login', loginLimiter);
  ```

### 11.6 Logging y Monitoreo de Seguridad

* **Registra eventos de seguridad**
  ```javascript
  // Eventos a registrar
  logger.security({
    event: 'failed_login',
    userId: attemptedUserId,
    ip: req.ip,
    userAgent: req.get('user-agent'),
    timestamp: new Date().toISOString()
  });
  ```

* **NO registres información sensible**
  - Nunca loguees contraseñas, tokens o datos de tarjetas
  - Sanitiza los logs antes de enviarlos a servicios externos
  - Usa niveles de log apropiados (error, warn, info, debug)

### 11.7 Checklist de Seguridad para Deployment

- [ ] Todas las variables de entorno están configuradas
- [ ] HTTPS está habilitado y forzado
- [ ] Headers de seguridad están configurados
- [ ] Rate limiting está implementado
- [ ] Logs no contienen información sensible
- [ ] Dependencias están actualizadas y sin vulnerabilidades conocidas
- [ ] Backups automáticos están configurados
- [ ] Monitoreo de seguridad está activo
- [ ] Plan de respuesta a incidentes está documentado

---
## 📚 Recursos adicionales

### Fuentes originales:
- [elsewhencode/project-guidelines](https://github.com/elsewhencode/project-guidelines)
- [RisingStack Engineering](https://blog.risingstack.com/)
- [Mozilla Developer Network](https://developer.mozilla.org/)
- [Heroku Dev Center](https://devcenter.heroku.com)
- [Airbnb/javascript](https://github.com/airbnb/javascript)
- [Atlassian Git tutorials](https://www.atlassian.com/git/tutorials)
- [Apigee](https://apigee.com/about/blog)
- [Wishtack](https://blog.wishtack.com)

### Recursos adicionales recomendados:
- [12 Factor App](https://12factor.net/es/)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [Google SRE Books](https://sre.google/books/)
- [Martin Fowler's Blog](https://martinfowler.com/)

Icons by [icons8](https://icons8.com/)


## Contribuyendo 🖇️

Por favor lee el [CONTRIBUTING.md]([CONTRIBUTING.md) para detalles de nuestro código de conducta, y el proceso para enviarnos pull requests.

 
## Licencia 📄

Mira el archivo [LICENSE.md](LICENSE.md) para detalles

## Expresiones de Gratitud 🎁

* Comenta a otros sobre este proyecto 📢
* Invita una cerveza 🍺 o un café ☕ a alguien del equipo. 
* Da las gracias públicamente 🤓.

---
⌨️ con ❤️ por [jose-franco](https://github.com/jefrnc) 😊