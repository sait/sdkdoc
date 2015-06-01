# CAPITULO 4 VFP
----------

Visual fox pro ofrece las herramientas necesarias que el usuario necesita para el manejo de datos de forma rápida y sencilla. Para ello, es importante conocer el uso de algunos comandos básicos en VFP.

####COMANDOS

Comando | use
----------- | ----
CD          | Se usa para cambiar de directorio en vfp
USE         | Permite abrir o cerrar una tabla
Shared 	    | Permite compartir una tabla (a varios usuarios)
Exclusive   | Permite abrir una tabla a un sólo usuario
GO TOP 	    | Se posiciona al inicio de una tabla
GO BOTTOM   | Se posiciona al final de una tabla
RECNO()  | Devuelve el número de registros actual de una tabla
GO TO N 	| Se coloca en el registro N de una tabla
RECCOUNT    | Devuelve el número de registros de una tabla
SELECT nAreaTrabajo/ cAliasTabla | Selecciona una BD que se encuentre abierta 
SELECT SQL  | Permite realizar consultas de una o más tablas
SET FILTER TO | Crea un filtro a los registros de una tabla
BROWSE      | Muestra los registros de una tabla
SEEK        | Localiza un registro
LOCATE... FOR | Busca el primer registro de una tabla que coincida con la expresión dada
DELETE      | Marca como borrado un registro
PACK       	| Elimina los registros marcados como borrados
ZAP 	   	| Elimina los registros de una tabla
MODIFY STRUCTURE | Permite modificar la estructura de una tabla
DISPLAY STRUCTURE | Muestra la estructura de una tabla 
MODIFY COMMAND| Crea o permite modificar un programa
CLEAR 		| Limpia la ventana de comandos
= 			| Permite asignar un valor a una variable
== 			| Permite comparar dos expresiones (variables o registros) 
SET RELATION TO | Crea una relación entre dos o más tablas
ALLTRIM() 	| Permite eliminar espacios al inicio y al final de cada cadena
IIF (Expresión, Expresión1, Expresión2) | Devuelve uno de los dos valores dependiendo del valor de una expresión lógica
SUM(nExpresión) |Realiza la suma de la expresión contenida dentro de la función 
MODIFY FORM | Crear o modificar un formulario
INSERT... VALUES | Almacena información en un registro