Docker version 24.0.2, build cb74dfc

Para verificar si el grupo docker ya existe en tu sistema, puedes usar el siguiente comando:
getent group docker

Crear el grupo docker (si aún no existe):
sudo groupadd docker

Agregar tu usuario al grupo docker:
sudo usermod -aG docker $USER

Para borrar todas las imágenes que aparecen con docker images, puedes usar el siguiente comando:
docker rmi $(docker images -q)
Explicación:
docker images -q obtiene solo los IDs de todas las imágenes.
docker rmi elimina esas imágenes.

Si algunas imágenes están en uso por contenedores, es posible que el comando falle. 
Para forzar la eliminación, puedes hacer lo siguiente:
Eliminar todos los contenedores primero (si hay contenedores creados o en ejecución):
- Command Substitution -
docker rm -f $(docker ps -aq)
Luego, eliminar todas las imágenes forzadamente:
docker rmi -f $(docker images -q)

eliminar solo las específicas con:
docker rmi <IMAGE_ID>

Si necesitas ver el ID completo del contenedor, usa:
docker inspect --format="{{.Id}}" <NOMBRE_O_ID_DEL_CONTENEDOR>

También puedes listar todos los contenedores con su ID completo usando:
docker ps -a --no-trunc
Este comando mostrará los contenedores sin truncar los IDs.

Cuando borras la imagen con docker image rm ubuntu, la salida muestra tres tipos de información clave:

"Untagged: ubuntu:latest"

Se elimina la referencia ubuntu:latest de la imagen en tu sistema.
Esto significa que ya no puedes usar ubuntu como nombre de imagen, pero la imagen en sí todavía podría existir si tiene otros tags.
"Untagged: ubuntu@sha256:..."

También se elimina la referencia basada en el digest SHA256, que es una firma única de la imagen.
Docker usa estos valores para identificar imágenes específicas más allá de sus nombres.
"Deleted: sha256:..."

Finalmente, se eliminan las capas de la imagen del sistema.
Docker almacena las imágenes en capas y solo las borra completamente si no están siendo usadas por otro contenedor o imagen.
En este caso, sha256:a04dc4851cbc... y sha256:4b7c01ed0534... son capas que formaban parte de la imagen y fueron eliminadas.
💡 ¿Qué significa esto?

Si la imagen solo tenía el tag ubuntu:latest, ahora está completamente eliminada.
Si había otros tags apuntando a la misma imagen, solo se habrían "desetiquetado" en lugar de eliminarse.
Si algún contenedor en ejecución o detenido dependiera de la imagen, Docker no la habría eliminado sin la opción -f (docker image rm -f ubuntu).