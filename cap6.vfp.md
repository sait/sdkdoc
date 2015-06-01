# CAPITULO 6 VFP
----------
Dentro del desarrollo del módulo se requiere de ciertas búsquedas necesarias para la obtención de datos específicos. 

####Código

Código necesario para la búsqueda sensible en un Grid desde un campo de texto.
```vfp 
x = ALLTRIM (This.value)
thisform.Grid1.RecordSource = "SELECT * FROM choferes WHERE nombre = x INTO CURSOR pro"
```
Para mostrar todos los datos de una tabla en el Grid es necesario de esta sentencia: 
```vfp
SELECT * FROM choferes 
```
Botón de borrar. Se instancia la clase para poder invocar el método de eliminar que se encuentra en ella. Se le envía como parámetro el ID del registro que se desea eliminar. Y como confirmación devuelve el Id que ha sido eliminado. 
```vfp
nIdchofer = VAL(thisform.txtNoChofer.value)

oClase = newObject('metodosPruebas','C:\Documents and Settings\LaptopXP\Mis documentos\Choferes v1.1\metodos.prg')

oClase.eliminar(nIdchofer)

wait wind (STR(nIdchofer) + "Registro eliminado")
```
Método eliminar que se encuentra en la clase 'métodosPruebas'.
```vfp
function eliminar(nIdchofer)
	SET DELETED ON
	delete from choferes where idchofer = nIdchofer
return
```

Método modificar que se encuentra en la clase 'métodosPruebas'.
```vfp
function actualizar(nIdchofer, cNombre, cCalle, cCiudad, cTelefono, cNumero, cColonia, cObservacion)
	select choferes
	update choferes set ;
	nombre	= cNombre,;
	calle	= cCalle,;
	ciudad	= cCiudad,;
	telefono = cTelefono,;
	numero =  cNumero,;
	colonia = cColonia,;
	observaciones = cObservacion; 
	where idchofer = nIdchofer
	wait wind "Registro modificado"
return
```

Botón Limpiar. Para limpiar los campos de textos de un formulario es necesario inicializar los valores. 
```vfp
thisform.txtNoChofer.value = ''
thisform.txtnombre.value = ''
thisform.txtcalle.value = ''
thisform.txtciudad.value = ''
thisform.txttelefono.value = '' 
thisform.txtnumero.value = ''
thisform.txtcolonia.value =''
thisform.txtobs.value = ''
```
Botón de cerrar/salir. Este cierra el formulario en uso. 
```vfp
thisform.release
