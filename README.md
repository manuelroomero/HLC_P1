## Práctica 1: Git y GitHub
![Git](https://img.shields.io/badge/git-%23F05033.svg?style=for-the-badge&logo=git&logoColor=white)
![GitHub](https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge&logo=github&logoColor=white)

En esta práctica vamos a utilizar diversos comandos y funciones de Git y GitHub. Para ello, trabajaremos con el uso de ramas en este mismo repositorio.

### Repositorio HLC

1. El primer paso es clonar este repositorio usando el comando `git clone`. Este comando establece una copia local del repositorio completo, incluyendo el historial, todas las ramas y los archivos de trabajo actuales.
   
~~~
git clone https://github.com/apaemau686/HLC_P1.git
~~~

2. Tras ello, crearemos una rama llamada `<nombre_apellidos_v1>` en la que vamos a trabajar.

~~~
git checkout -b <nombre_apellidos_v1>
~~~

## Commit inicial

1. Crearemos un fichero llamado `command_history.txt`, y añadiremos la secuencia de comandos utilizados hasta ahora.
2. Realizaremos un commit con estos cambios con el mensaje `Add used commands`.
3. Para terminar subiremos los cambios al repositorio remoto.

~~~
git add .
git commit -m "Add used commands"
git push
~~~

4. Si la rama está creada en local pero no en remoto, Git nos indicará que es necesario subirla al repositorio remoto. 

~~~
git push --set-upstream origin <nombre_apellidos_v1>
~~~

## Provocando un conflicto

1. Desde la web de GitHub, vamos a cambiar la primera línea del fichero `command_history.txt` por cualquier texto y después lo subiremos con el mensaje `Update history from website`.
2. Volvemos al repositorio local y **sin actualizar** la rama vamos a cambiar de nuevo la primera línea del fichero `command_history.txt`. Esta vez el texto debe ser diferente al del paso 1, y el mensaje del commit será `Update history from command line`.
3. Desde consola, commitearemos los cambios y los subiremos.


~~~
git add .
git commit -m "Update history from command line"
git push
~~~

4. Git mostrará un mensaje indicando que la rama no está actualizada, por lo cual realizaremos la actualización de la rama.

~~~
git pull origin <nombre_apellidos_v1>
~~~

5. Tras actualizar la rama intentaremos subir los cambios de nuevo. 

~~~
git push
~~~

6. Git nos informará de que hay un conflicto en el fichero y nos pedirá resolverlo. Tras hacerlo, volveremos a crear un commit con el conflicto resuelto y lo subiremos.

~~~
git add .
git commit -m "Solve conflicts"
git push
~~~

## Merge entre ramas

1. Creamos otra rama desde `main` llamada `<nombre_apellidos_v2>`

~~~
git checkout -b <nombre_apellidos_v2>
~~~

2. Añadimos un fichero `command_history_2.txt` y lo subimos a la rama con el mensaje `Add newer commands.`

~~~
git add .
git commit -m "Add newer commands"
git push 
~~~

3. Hacemos un merge de la rama `<nombre_apellidos_v1>` en la rama `<nombre_apellidos_v2>`.

~~~
git merge `<nombre_apellidos_v1>` -m "Merge v1"
~~~

4. Hacemos push de los cambios. Observamos que no es necesario crear un nuevo commit con los cambios del merge porque Git ya lo hace automáticamente.

~~~
git push
~~~

5. Vamos a visualizar el historial de commits, y comprobamos que los commits que introducimos en la rama v1 se encuentran tal cual en la rama v2 mediante `git log`. Este comando muestra el historial de commits en orden cronológico inverso (del más reciente al más antiguo). Para salir del visualizador, basta con pulsar la tecla `q`.

~~~
git log
~~~

6. El comando `git log` cuenta con varias opciones:

~~~
git log -n <número>                                   # Limita la cantidad de commits mostrados
git log --author=<nombre-autor>                       # Filtra los commits por autor
git log --oneline                                     # Muestra cada commit en una línea única
git log --since=<fecha-inicial> --until=<fecha-final> # Filtra por rango de fechas
git log --graph                                       # Muestra un gráfico de ramas y merges
git log --grep="<texto-a-buscar>"                     # Busca commits por mensaje
git log -- <nombre-archivo>                           # Filtra commits por archivo
~~~

## Stash y amend

1. Volvemos a la rama v1 y actualizamos el fichero `command_history.txt` añadiendo en la última línea el mensaje "Text to revert".

2. Visualizamos el estado de los ficheros y observamos que Git nos indica que hay cambios que no han sido guardados.

~~~
git status
~~~

3. En lugar de realizar commit, vamos a guardar temporalmente el cambio en un stash. El stash es útil cuando necesitas cambiar de rama o realizar otras operaciones sin comprometer tus cambios actuales. Tras realizarlo, observamos que el cambio ha desaparecido. 

~~~
git stash
~~~

4. Vamos a visualizar la lista de stashes y a volver a aplicar el último cambio. Observamos que el cambio ha vuelto a aparecer.

~~~
git stash list
git stash apply "stash@{0}"
~~~

5. Subimos el cambio al repositorio remoto con el mensaje `Add text`.

~~~
git add .
git commit -m "Add text"
git push 
~~~

6. Por último, vamos a editar el mensaje del commit que acabamos de publicar. Para ello vamos a usar el flag `--amend` y escribiremos como nuevo mensaje de commit el texto `Add text to revert`. Este flag se utiliza junto con el comando `git commit` para realizar modificaciones en el commit más reciente. Permite modificar el mensaje del commit, agregar cambios olvidados o combinar nuevos cambios en un commit ya realizado.

~~~
git commit --amend -m "Add text to revert"
~~~

7. Para sustituir el nuevo commit por el anterior es necesario forzar el push mediante el flag `-f`. De esta forma le indicamos a Git que sobrescriba el historial remoto con nuestro historial local, incluso si eso implica reescribir la historia de commits del repositorio. Por defecto, Git no permite actualizar el historial remoto de manera predeterminada si eso implica cambios en la historia. Hay que tener en cuenta que esto también afectaría a los ficheros si se hubiesen realizado cambios con los peligros que eso conlleva, por lo que hay que utilizar el forzado con precaución.

~~~
git push -f
~~~

Si no hubiésemos hecho push del commit anterior, habría bastado con hacer el amend en local, ya que no implicaría ningún cambio en la historia del repositorio.

## Revert

1. Vamos a revertir el último commit realizado en la rama v1. Para ello, visualizamos el historial de commits y copiamos el **id** del último commit realizado. Es necesario conocer este id para poder indicarle al comando `revert` qué commit debe revertir. A diferencia de `git reset`, que elimina los commits y puede reescribir el historial, `git revert` no modifica el historial existente; en su lugar, agrega nuevos commits que deshacen los cambios de un commit anterior.

~~~
git log
~~~

2. Realizamos un revert al commit y lo pusheamos. Tras ejecutar el comando se abrirá un vim donde podremos editar el mensaje del commit. Para salir sin realizar ningún cambio, pulsamos `:q`. Si hemos realizado algún cambio y queremos que se guarde, pulsamos `:wq`. Observamos que no es necesario crear un nuevo commit ya que el revert lo hace automáticamente.

~~~
git revert <id>
git push
~~~

## Cherry Pick

1. Creamos una nueva rama llamada `<nombre_apellidos_v3>` desde `main`.

~~~
git checkout main
git checkout -b <nombre_apellidos_v3>
~~~

2. Utilizamos cherry-pick para aplicar un commit específico de la rama <nombre_apellidos_v1> en la rama <nombre_apellidos_v3>. Este comando permite seleccionar y aplicar commits específicos en otra rama, lo que puede ser útil cuando solo queremos aplicar cambios específicos de una rama a otra, sin necesidad de fusionar todas las ramas o realizar un merge completo.

~~~
git log
git cherry-pick <id>
~~~

3. Tras ello, subimos los cambios al repositorio remoto.

~~~
git add .
git commit -m "Additional changes in v3"
git push
~~~

## Crear Pull-request

1. Desde la web de GitHub, hacemos una Pull Request para incorporar la rama v2 en la v1. Para ello, seleccionamos `Open pull request` en el desplegable `Contribute`. Dejamos la PR vacía y la publicamos.

   ![image](https://github.com/creyaro/HLC_P1/assets/152985662/06572a10-c09c-492e-a90c-53f347124b76)
![image](https://github.com/creyaro/HLC_P1/assets/152985662/56cfcbe0-211d-4bc3-9ef9-5452ae553115)


3. Tras ello, aceptamos nosotros mismos la PR e incorporamos los cambios de v2 en v1 mediante el botón `Merge`.

![image](https://github.com/creyaro/HLC_P1/assets/152985662/6cd32fec-5690-4078-8245-3bbd5573e1e4)

## Listado y borrado de ramas

1. En primer lugar vamos a listar las ramas del repositorio.

~~~
git branch
~~~

2. A continuación, vamos a eliminar las rama v2 y v3. Para ello tenemos que estar en otra rama que no sea ninguna de ellas, como v1.

~~~
git checkout v1
git branch -d <nombre_apellidos_v2>
git push origin --delete <nombre_apellidos_v2>
git branch -d <nombre_apellidos_v3>
git push origin --delete <nombre_apellidos_v3>
~~~

