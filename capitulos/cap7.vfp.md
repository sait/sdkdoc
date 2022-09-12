Capítulo 7 VFP
===================

Para hacer más fácil crear y modifciar un registros en una tabla es necesario hacer una consulta a los datos que contiene el registro existente sin tener que consultar toda la tabla.

Una manera sencilla es extrayendo los datos y colarlos en los campos de texto que se encuentran en el formulario. 

### Avance de aplicación

Primeramente es una recomendación que el numero identificador del registro sea autoincrementable, así podrá ayudar al usuario a no tener que verificar cual fue el último registro que se hizo.

```vfp
------En la clase
function cargarId()
	select max(idproducto) as maxid from productos into cursor temp
	nLastId = temp.maxid + 1
return (STR(nLastId))

-----Dentro del formulario
oClase = newObject('abcProductos','C:\Documents and Settings\LaptopXP\Mis documentos\Choferes v1.1\abcproductos.prg')
thisform.txtproducto.Value=oClase.cargarId()
```

Como se mencionó anteriormente,  suele ser útil que los campos dentro de los formularios se actualicen conforme el registro que se quiere modificar. En este ejemplo se puede ver que al momento en que la aplicación verifica que el registro existe, este extrae sus datos; De lo contrario el formulario se prepara para un nuevo registro.

```vfp
*Este código se implemento en el evento KeyPress de un cuadro de texto. 

LPARAMETERS nKeyCode, nShiftAltCtrl

IF nKeyCode = 13
	nIdproducto = VAL(thisform.txtproducto.Value)

	select nombre,descripcion,precio_compra,precio_venta,marca from productos where idproducto==nIdproducto into cursor temp
	if reccount('temp')>0
		thisform.txtnombre.value = temp.nombre
		thisform.txtdescripcion.value = temp.descripcion
		thisform.txtprecio.value = temp.precio_compra
		thisform.txtPreciov.value = temp.precio_venta
		thisform.txtmarca.value = temp.marca
	else
		wait wind "Registro no existe"
		oClase = newObject('abcProductos','C:\Documents and Settings\LaptopXP\Mis documentos\Choferes v1.1\abcproductos.prg')
		thisform.txtproducto.Value=oClase.cardarId()
	endif
endif
```
