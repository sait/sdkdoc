Capítulo 2 VFP
==============

Visual Fox Pro o VFP cuenta con un gran grupo de herramientas que vienen con el
objetivo de crear formas dentro de formularios con los que puede interactuar el
usuario.

Estas herramientas ayudan a asignar formas con funcionalidades especificas
dentro de la misma aplicación.

Visual Fox Pro ofrece una lista de eventos para asignarse a dichas formas, los
cuales si estos se activan tienden a ejecutar una accion al momento de que el
sistema de este ejecutando.

#### Ejemplos

```vfp
*Este es un botón programado para que grabe un registro en una tabla de una base de datos.

gather memvar
flush
thisform.refresh
wait wind "Registro agregado"

*La función de este botón es el modificar los datos ya existentes en el registro de la tabla.

gather memvar
thisform.refresh
wait wind "Registro modificado"

*Este botón elimina el registro de la tabla.

DELETE
wait wind "Registro eliminado"

*Este botón inicializa todos los TextBox para que queden limpios.

thisform.codigo.value=""
thisform.nombre.value=""
thisform.ciudad.value=""

*Este botón te posiciona en el primer registro y te refresca la ventana.

goto top
wait windows "Primer registro"
thisform.refresh

*Este botón te posiciona en el ultimo registro y te refresca la ventana.

goto bottom
wait windows "Ultimo registro registro"
thisform.refresh

*Este botón te regresa a un registro anterior previamente guardado y te refresca la ventana.

skip -1
if eof()
goto top
wait windows "Registro anterior"
endif
thisform.refresh

*Este botón te adelanta a un registro previamente guardado y te refresca la ventana.

skip 1
if eof()
goto top
wait windows "Registro superior"
endif
thisform.refresh

*Esta función hace que se cierren todas las ventanas abiertas, cerrando así la aplicación.

close all
release.thisform
```