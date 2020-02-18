
# Project Guidelines &middot; [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

Aquí hay una lista de pautas que hemos encontrado, escrito y recopilado que (creemos) funciona muy bien con la mayoría de los proyectos.
Si desea compartir una mejor práctica, o cree que se debe eliminar una de estas pautas, [sientete libre de compartirlo con nosotros](http://makeapullrequest.com).

<hr>

- [Git](#git)
    - [Algunas reglas de Git](#some-git-rules)
    - [Git workflow](#git-workflow)
    - [Escribir buenos mensajes en el commit](#writing-good-commit-messages)
- [Documentacion](#documentation)
- [Ambientes](#environments)
    - [Entornos de desarrollo consistentes](#consistent-dev-environments)
    - [Dependencias consistentes](#consistent-dependencies)
- [Dependencias](#dependencies)
- [Testing](#testing)
- [Estructura y nomenclatura](#structure-and-naming)
- [Estilo de código](#code-style)
    - [Algunas pautas de estilo de código](#code-style-check)
    - [Hacer cumplir los estándares de estilo de código](#enforcing-code-style-standards)
- [Logging](#logging)
- [API](#api)
    - [Diseño del API](#api-design)
    - [Seguridad de la API](#api-security)
    - [Documentación de la API](#api-documentation)

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

* Use [este](./.gitignore) `.gitignore` archivo.
    
  _Por que?:_
    > Ya tiene una lista de archivos del sistema que no deben enviarse con su código a un repositorio remoto. Además, puede excluir la configuración de carpetas y archivos para los editores más utilizados, así como las carpetas de dependencia más comunes. Existe paginas que permite auto-generar este archivo, un ejemplo es [gitignore.io](https://www.gitignore.io)

* Proteja su `develop` y `master` branch.
  
  _Por que?:_
    > It protects your production-ready branches from receiving unexpected and irreversible changes. read more... [Github](https://help.github.com/articles/about-protected-branches/), [Bitbucket](https://confluence.atlassian.com/bitbucketserver/using-branch-permissions-776639807.html) and [GitLab](https://docs.gitlab.com/ee/user/project/protected_branches.html)
    > En caso de utilizar ramas de `release`, proteja las msimas de recibir cambios inesperados e irreversibles. Leer más ... [Github] (https://help.github.com/articles/about-protected-branches/), [Bitbucket] (https://confluence.atlassian.com/bitbucketserver/using-branch-permissions -776639807.html) y [GitLab] (https://docs.gitlab.com/ee/user/project/protected_branches.html)

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
    $ git remote add origin https://github.com/jose-franco/gitflow-empty.git
    $ git push -u origin master
    Enumerating objects: 3, done.
    Counting objects: 100% (3/3), done.
    Writing objects: 100% (3/3), 230 bytes | 230.00 KiB/s, done.
    Total 3 (delta 0), reused 0 (delta 0)
    To https://github.com/jose-franco/gitflow-empty.git
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
    remote:      https://github.com/jose-franco/gitflow-empty/pull/new/develop
    remote:
    To https://github.com/jose-franco/gitflow-empty.git
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
    git clone https://github.com/jose-franco/gitflow-empty.git
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
    To https://github.com/jose-franco/gitflow-empty.git
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
    $ git clone https://github.com/jose-franco/gitflow-empty.git
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
    To https://github.com/jose-franco/gitflow-empty.git
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
    To https://github.com/jose-franco/gitflow-empty.git
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
    $  git checkout master; git pull origin master; git fetch --all -p; git branch -vv | grep ": gone]" | awk '{ print $1 }' | xargs -n 1 git branch -D
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
    >
    Archivos `.env` para almacenar sus variables y agregarlas a` .gitignore` para excluirlas. En su lugar, suba un archivo `.env.example` que sirva de guía para los desarrolladores. [Leer mas](https://medium.com/@rafaelvidaurre/managing-environment-variables-in-node-js-2cb45a55195f)

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


<a name="consistent-dependencies"></a>
### 3.2 Consistent dependencies:

* Make sure your team members get the exact same dependencies as you.

  _Por que?:_
    > Because you want the code to behave as expected and identical in any development machine [read more...](https://kostasbariotis.com/consistent-dependencies-across-teams/)

    _how:_
    > Use `package-lock.json` on `npm@5` or higher

    _I don't have npm@5:_
    > Alternatively you can use `Yarn` and make sure to mention it in `README.md`. Your lock file and `package.json` should have the same versions after each dependency update. [read more...](https://yarnpkg.com/en/)

    _I don't like the name `Yarn`:_
    > Too bad. For older versions of `npm`, use `—save --save-exact` when installing a new dependency and create `npm-shrinkwrap.json` before publishing. [read more...](https://docs.npmjs.com/files/package-locks)

<a name="dependencies"></a>
## 4. Dependencias

* Keep track of your currently available packages: e.g., `npm ls --depth=0`. [read more...](https://docs.npmjs.com/cli/ls)
* See if any of your packages have become unused or irrelevant: `depcheck`. [read more...](https://www.npmjs.com/package/depcheck)
    
  _Por que?:_
    > Puede incluir una biblioteca no utilizada en su código y aumentar el tamaño del paquete de producción. Encuentra dependencias no utilizadas y deshazte de ellas.

* Before using a dependency, check its download statistics to see if it is heavily used by the community: `npm-stat`. [read more...](https://npm-stat.com/)
    
  _Por que?:_
    > More usage mostly means more contributors, which usually means better maintenance, and all of these result in quickly discovered bugs and quickly developed fixes.

* Before using a dependency, check to see if it has a good, mature version release frequency with a large number of maintainers: e.g., `npm view async`. [read more...](https://docs.npmjs.com/cli/view)

  _Por que?:_
    > Having loads of contributors won't be as effective if maintainers don't merge fixes and patches quickly enough.

* If a less known dependency is needed, discuss it with the team before using it.
* Always make sure your app works with the latest version of its dependencies without breaking: `npm outdated`. [read more...](https://docs.npmjs.com/cli/outdated)

  _Por que?:_
    > Dependency updates sometimes contain breaking changes. Always check their release notes when updates show up. Update your dependencies one by one, that makes troubleshooting easier if anything goes wrong. Use a cool tool such as [npm-check-updates](https://github.com/tjunnone/npm-check-updates).

* Check to see if the package has known security vulnerabilities with, e.g., [Snyk](https://snyk.io/test?utm_source=risingstack_blog).


<a name="testing"></a>
## 5. Testing

* Have a `test` mode environment if needed.

  _Por que?:_
    > While sometimes end to end testing in `production` mode might seem enough, there are some exceptions: One example is you may not want to enable analytical information on a 'production' mode and pollute someone's dashboard with test data. The other example is that your API may have rate limits in `production` and blocks your test calls after a certain amount of requests. 

* Place your test files next to the tested modules using `*.test.js` or `*.spec.js` naming convention, like `moduleName.spec.js`.

  _Por que?:_
    > You don't want to dig through a folder structure to find a unit test. [read more...](https://hackernoon.com/structure-your-javascript-code-for-testability-9bc93d9c72dc)
    

* Put your additional test files into a separate test folder to avoid confusion.

  _Por que?:_
    > Some test files don't particularly relate to any specific implementation file. You have to put it in a folder that is most likely to be found by other developers: `__test__` folder. This name: `__test__`  is also standard now and gets picked up by most JavaScript testing frameworks.

* Write testable code, avoid side effects, extract side effects, write pure functions

  _Por que?:_
    > You want to test a business logic as separate units. You have to "minimize the impact of randomness and nondeterministic processes on the reliability of your code". [read more...](https://medium.com/javascript-scene/tdd-the-rite-way-53c9b46f45e3)
    
    > A pure function is a function that always returns the same output for the same input. Conversely, an impure function is one that may have side effects or depends on conditions from the outside to produce a value. That makes it less predictable. [read more...](https://hackernoon.com/structure-your-javascript-code-for-testability-9bc93d9c72dc)

* Use a static type checker 

  _Por que?:_
    > Sometimes you may need a Static type checker. It brings a certain level of reliability to your code. [read more...](https://medium.freecodecamp.org/why-use-static-types-in-javascript-part-1-8382da1e0adb)


* Run tests locally before making any pull requests to `develop`.

  _Por que?:_
    > You don't want to be the one who caused production-ready branch build to fail. Run your tests after your `rebase` and before pushing your feature-branch to a remote repository.

* Document your tests including instructions in the relevant section of your `README.md` file.

  _Por que?:_
    > It's a handy note you leave behind for other developers or DevOps experts or QA or anyone who gets lucky enough to work on your code.

<a name="structure-and-naming"></a>
## 6. Structure and Naming
 
* Organize your files around product features / pages / components, not roles. Also, place your test files next to their implementation.


    **Bad**

    ```
    .
    ├── controllers
    |   ├── product.js
    |   └── user.js
    ├── models
    |   ├── product.js
    |   └── user.js
    ```

    **Good**

    ```
    .
    ├── product
    |   ├── index.js
    |   ├── product.js
    |   └── product.test.js
    ├── user
    |   ├── index.js
    |   ├── user.js
    |   └── user.test.js
    ```

  _Por que?:_
    > Instead of a long list of files, you will create small modules that encapsulate one responsibility including its test and so on. It gets much easier to navigate through and things can be found at a glance.

* Put your additional test files to a separate test folder to avoid confusion.

  _Por que?:_
    > It is a time saver for other developers or DevOps experts in your team.

* Use a `./config` folder and don't make different config files for different environments.

  _Por que?:_
    >When you break down a config file for different purposes (database, API and so on); putting them in a folder with a very recognizable name such as `config` makes sense. Just remember not to make different config files for different environments. It doesn't scale cleanly, as more deploys of the app are created, new environment names are necessary.
    Values to be used in config files should be provided by environment variables. [read more...](https://medium.com/@fedorHK/no-config-b3f1171eecd5)
    

* Put your scripts in a `./scripts` folder. This includes `bash` and `node` scripts.

  _Por que?:_
    >It's very likely you may end up with more than one script, production build, development build, database feeders, database synchronization and so on.
    

* Place your build output in a `./build` folder. Add `build/` to `.gitignore`.

  _Por que?:_
    >Name it what you like, `dist` is also cool. But make sure that keep it consistent with your team. What gets in there is most likely generated  (bundled, compiled, transpiled) or moved there. What you can generate, your teammates should be able to generate too, so there is no point committing them into your remote repository. Unless you specifically want to. 

<a name="code-style"></a>
## 7. Code style

<a name="code-style-check"></a>
### 7.1 Some code style guidelines

* Use stage-2 and higher JavaScript (modern) syntax for new projects. For old project stay consistent with existing syntax unless you intend to modernise the project.

  _Por que?:_
    > This is all up to you. We use transpilers to use advantages of new syntax. stage-2 is more likely to eventually become part of the spec with only minor revisions. 

* Include code style check in your build process.

  _Por que?:_
    > Breaking your build is one way of enforcing code style to your code. It prevents you from taking it less seriously. Do it for both client and server-side code. [read more...](https://www.robinwieruch.de/react-eslint-webpack-babel/)

* Use [ESLint - Pluggable JavaScript linter](http://eslint.org/) to enforce code style.

  _Por que?:_
    > We simply prefer `eslint`, you don't have to. It has more rules supported, the ability to configure the rules, and ability to add custom rules.

* We use [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) for JavaScript, [Read more](https://www.gitbook.com/book/duk/airbnb-javascript-guidelines/details). Use the javascript style guide required by the project or your team.

* We use [Flow type style check rules for ESLint](https://github.com/gajus/eslint-plugin-flowtype) when using [FlowType](https://flow.org/).

  _Por que?:_
    > Flow introduces few syntaxes that also need to follow certain code style and be checked.

* Use `.eslintignore` to exclude files or folders from code style checks.

  _Por que?:_
    > You don't have to pollute your code with `eslint-disable` comments whenever you need to exclude a couple of files from style checking.

* Remove any of your `eslint` disable comments before making a Pull Request.

  _Por que?:_
    > It's normal to disable style check while working on a code block to focus more on the logic. Just remember to remove those `eslint-disable` comments and follow the rules.

* Depending on the size of the task use  `//TODO:` comments or open a ticket.

  _Por que?:_
    > So then you can remind yourself and others about a small task (like refactoring a function or updating a comment). For larger tasks use `//TODO(#3456)` which is enforced by a lint rule and the number is an open ticket.


* Always comment and keep them relevant as code changes. Remove commented blocks of code.
    
  _Por que?:_
    > Your code should be as readable as possible, you should get rid of anything distracting. If you refactored a function, don't just comment out the old one, remove it.

* Avoid irrelevant or funny comments, logs or naming.

  _Por que?:_
    > While your build process may(should) get rid of them, sometimes your source code may get handed over to another company/client and they may not share the same banter.

* Make your names search-able with meaningful distinctions avoid shortened names. For functions use long, descriptive names. A function name should be a verb or a verb phrase, and it needs to communicate its intention.

  _Por que?:_
    > It makes it more natural to read the source code.

* Organize your functions in a file according to the step-down rule. Higher level functions should be on top and lower levels below.

  _Por que?:_
    > It makes it more natural to read the source code.

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

* Always use a plural nouns for naming a url pointing to a collection: `/users`.

  _Por que?:_
    > Basically, it reads better and keeps URLs consistent. [read more...](https://apigee.com/about/blog/technology/restful-api-design-plural-nouns-and-concrete-names)

* In the source code convert plurals to variables and properties with a List suffix.

    _Why_:
    > Plural is nice in the URL but in the source code, it’s just too subtle and error-prone.

* Always use a singular concept that starts with a collection and ends to an identifier:

    ```
    /students/245743
    /airports/kjfk
    ```
* Avoid URLs like this: 
    ```
    GET /blogs/:blogId/posts/:postId/summary
    ```

  _Por que?:_
    > This is not pointing to a resource but to a property instead. You can pass the property as a parameter to trim your response.

* Keep verbs out of your resource URLs.

  _Por que?:_
    > Because if you use a verb for each resource operation you soon will have a huge list of URLs and no consistent pattern which makes it difficult for developers to learn. Plus we use verbs for something else.

* Use verbs for non-resources. In this case, your API doesn't return any resources. Instead, you execute an operation and return the result. These **are not** CRUD (create, retrieve, update, and delete) operations:

    ```
    /translate?text=Hallo
    ```

  _Por que?:_
    > Because for CRUD we use HTTP methods on `resource` or `collection` URLs. The verbs we were talking about are actually `Controllers`. You usually don't develop many of these. [read more...](https://byrondover.github.io/post/restful-api-guidelines/#controller)

* The request body or response type is JSON then please follow `camelCase` for `JSON` property names to maintain the consistency.
    
  _Por que?:_
    > This is a JavaScript project guideline, where the programming language for generating and parsing JSON is assumed to be JavaScript.

* Even though a resource is a singular concept that is similar to an object instance or database record, you should not use your `table_name` for a resource name and `column_name` resource property.

  _Por que?:_
    > Because your intention is to expose Resources, not your database schema details.

* Again, only use nouns in your URL when naming your resources and don’t try to explain their functionality.

  _Por que?:_
    > Only use nouns in your resource URLs, avoid endpoints like `/addNewUser` or `/updateUser` .  Also avoid sending resource operations as a parameter.

* Explain the CRUD functionalities using HTTP methods:

    _How:_
    > `GET`: To retrieve a representation of a resource.
    
    > `POST`: To create new resources and sub-resources.
   
    > `PUT`: To update existing resources.
    
    > `PATCH`: To update existing resources. It only updates the fields that were supplied, leaving the others alone.
    
    > `DELETE`:	To delete existing resources.


* For nested resources, use the relation between them in the URL. For instance, using `id` to relate an employee to a company.

  _Por que?:_
    > This is a natural way to make resources explorable.

    _How:_

    > `GET      /schools/2/students	` , should get the list of all students from school 2.

    > `GET      /schools/2/students/31`	, should get the details of student 31, which belongs to school 2.

    > `DELETE   /schools/2/students/31`	, should delete student 31, which belongs to school 2.

    > `PUT      /schools/2/students/31`	, should update info of student 31, Use PUT on resource-URL only, not collection.

    > `POST     /schools` , should create a new school and return the details of the new school created. Use POST on collection-URLs.

* Use a simple ordinal number for a version with a `v` prefix (v1, v2). Move it all the way to the left in the URL so that it has the highest scope:
    ```
    http://api.domain.com/v1/schools/3/students	
    ```

  _Por que?:_
    > When your APIs are public for other third parties, upgrading the APIs with some breaking change would also lead to breaking the existing products or services using your APIs. Using versions in your URL can prevent that from happening. [read more...](https://apigee.com/about/blog/technology/restful-api-design-tips-versioning)



* Response messages must be self-descriptive. A good error message response might look something like this:
    ```json
    {
        "code": 1234,
        "message" : "Something bad happened",
        "description" : "More details"
    }
    ```
    or for validation errors:
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
    > developers depend on well-designed errors at the critical times when they are troubleshooting and resolving issues after the applications they've built using your APIs are in the hands of their users.


    _Note: Keep security exception messages as generic as possible. For instance, Instead of saying ‘incorrect password’, you can reply back saying ‘invalid username or password’ so that we don’t unknowingly inform user that username was indeed correct and only the password was incorrect._

* Use these status codes to send with your response to describe whether **everything worked**,
The **client app did something wrong** or The **API did something wrong**.
    
    _Which ones:_
    > `200 OK` response represents success for `GET`, `PUT` or `POST` requests.

    > `201 Created` for when a new instance is created. Creating a new instance, using `POST` method returns `201` status code.

    > `204 No Content` response represents success but there is no content to be sent in the response. Use it when `DELETE` operation succeeds.

    > `304 Not Modified` response is to minimize information transfer when the recipient already has cached representations.

    > `400 Bad Request` for when the request was not processed, as the server could not understand what the client is asking for.

    > `401 Unauthorized` for when the request lacks valid credentials and it should re-request with the required credentials.

    > `403 Forbidden` means the server understood the request but refuses to authorize it.

    > `404 Not Found` indicates that the requested resource was not found. 

    > `500 Internal Server Error` indicates that the request is valid, but the server could not fulfill it due to some unexpected condition.

  _Por que?:_
    > Most API providers use a small subset HTTP status codes. For example, the Google GData API uses only 10 status codes, Netflix uses 9, and Digg, only 8. Of course, these responses contain a body with additional information. There are over 70 HTTP status codes. However, most developers don't have all 70 memorized. So if you choose status codes that are not very common you will force application developers away from building their apps and over to wikipedia to figure out what you're trying to tell them. [read more...](https://apigee.com/about/blog/technology/restful-api-design-what-about-errors)


* Provide total numbers of resources in your response.
* Accept `limit` and `offset` parameters.

* The amount of data the resource exposes should also be taken into account. The API consumer doesn't always need the full representation of a resource. Use a fields query parameter that takes a comma separated list of fields to include:
    ```
    GET /student?fields=id,name,age,class
    ```
* Pagination, filtering, and sorting don’t need to be supported from start for all resources. Document those resources that offer filtering and sorting.

<a name="api-security"></a>
### 9.2 API security
These are some basic security best practices:

* Don't use basic authentication unless over a secure connection (HTTPS). Authentication tokens must not be transmitted in the URL: `GET /users/123?token=asdf....`

  _Por que?:_
    > Because Token, or user ID and password are passed over the network as clear text (it is base64 encoded, but base64 is a reversible encoding), the basic authentication scheme is not secure. [read more...](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication)

* Tokens must be transmitted using the Authorization header on every request: `Authorization: Bearer xxxxxx, Extra yyyyy`.

* Authorization Code should be short-lived.

* Reject any non-TLS requests by not responding to any HTTP request to avoid any insecure data exchange. Respond to HTTP requests by `403 Forbidden`.

* Consider using Rate Limiting.

  _Por que?:_
    > To protect your APIs from bot threats that call your API thousands of times per hour. You should consider implementing rate limit early on.

* Setting HTTP headers appropriately can help to lock down and secure your web application. [read more...](https://github.com/helmetjs/helmet)

* Your API should convert the received data to their canonical form or reject them. Return 400 Bad Request with details about any errors from bad or missing data.

* All the data exchanged with the REST API must be validated by the API.

* Serialize your JSON.

  _Por que?:_
    > A key concern with JSON encoders is preventing arbitrary JavaScript remote code execution within the browser... or, if you're using node.js, on the server. It's vital that you use a proper JSON serializer to encode user-supplied data properly to prevent the execution of user-supplied input on the browser.

* Validate the content-type and mostly use `application/*json` (Content-Type header).
    
  _Por que?:_
    > For instance, accepting the `application/x-www-form-urlencoded` mime type allows the attacker to create a form and trigger a simple POST request. The server should never assume the Content-Type. A lack of Content-Type header or an unexpected Content-Type header should result in the server rejecting the content with a `4XX` response.

* Check the API Security Checklist Project. [read more...](https://github.com/shieldfy/API-Security-Checklist)

<a name="api-documentation"></a>
### 9.3 API documentation
* Fill the `API Reference` section in [README.md template](./README.sample.md) for API.
* Describe API authentication methods with a code sample.
* Explaining The URL Structure (path only, no root URL) including The request type (Method).

For each endpoint explain:
* URL Params If URL Params exist, specify them in accordance with name mentioned in URL section:

    ```
    Required: id=[integer]
    Optional: photo_id=[alphanumeric]
    ```

* If the request type is POST, provide working examples. URL Params rules apply here too. Separate the section into Optional and Required.

* Success Response, What should be the status code and is there any return data? This is useful when people need to know what their callbacks should expect:

    ```
    Code: 200
    Content: { id : 12 }
    ```

* Error Response, Most endpoints have many ways to fail. From unauthorized access to wrongful parameters etc. All of those should be listed here. It might seem repetitive, but it helps prevent assumptions from being made. For example
    ```json
    {
        "code": 403,
        "message" : "Authentication failed",
        "description" : "Invalid username or password"
    }   
    ```


* Use herramientas de diseño de API. Hay muchas herramientas de código abierto para una buena documentación, como [API Blueprint](https://apiblueprint.org/) y [Swagger](https://swagger.io/).

---
Sources:
[elsewhencode/project-guidelines](https://github.com/elsewhencode/project-guidelines),
[RisingStack Engineering](https://blog.risingstack.com/),
[Mozilla Developer Network](https://developer.mozilla.org/),
[Heroku Dev Center](https://devcenter.heroku.com),
[Airbnb/javascript](https://github.com/airbnb/javascript),
[Atlassian Git tutorials](https://www.atlassian.com/git/tutorials),
[Apigee](https://apigee.com/about/blog),
[Wishtack](https://blog.wishtack.com)

Icons by [icons8](https://icons8.com/)