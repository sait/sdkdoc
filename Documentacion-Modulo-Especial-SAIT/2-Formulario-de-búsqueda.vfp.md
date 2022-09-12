# MODULO ESPECIAL - VFP6 
-----------------
#### Búsqueda dentro de catálogos

Para realizar una búsqueda dentro del algunos de los módulos que se desarrollan, se utiliza una forma predeterminada de las librerías de SAIT para realizar dicha acción. Esta permite hacer una ventana de búsqueda, en forma rápida y sencilla

Las ventanas de búsqueda sirven de ayuda al usuario para que en caso que no recuerde la clave de algún registro, dicha ventana hace la busqueda del registro por medio de palabras claves y sí el usuario lo desea traerá a la ventana inicial de vuelta la clave del registro que buscó.

Para la creación de formulario de búsqueda, dentro de la ventana de comandos ```(Window > Command Window)``` de vfp se ordena:
```vfp
	   create form busqueda as Busqueda from E:\vfp\MSLLIB60\msllib60.vcx
```

Solo se necesita configurar algunas propiedades para que funcione correctamente ya sea directamente desde el cuadro de propiedades o en el INIT de la forma. 

```vfp
	busqueda.init 

	* Ancho en píxeles, de las columnas
	this.anchos = '200,100'
	* Campos en donde se buscara la información capturada por el usuario.
	this.ccamposbuscar = 'NOMBRE'
	* Campos a seleccionar en el Select.
	this.ccamposelect = 'NOMBRE, IDCHOFER'
	* Expresiones a desplegar en cada columna de la lista.
	this.cexprs = 'NOMBRE, IDCHOFER'
	* Tabla en donde buscaremos.
	this.frontable = 'CHOFERES'
	* Campo que usaremos para ordenar el resultado del Select.
	this.orderby = ''
	* Nombre del campo a regresar como resultado de la búsqueda.
	this.retval = 'IDCHOFER'
	* SQL a realizar para la búsqueda.
	this.csql = ''
	```

Dentro del formulario de catálogos, se requiere escoger una de las herramientas que se encuentra en la librería 'MSllib60' la cuál se llama leecve que nos ayuda a abrir la forma de búsqueda y recibir la clave y nombre en ella, solo se modifica algunas de sus propiedades: 

```vfp
	* Alias de la base de datos principal del catalogo.
	this.alias = 'choferes'
	* Expresion que me devuelve el nombre a desplegar.
	this.cexprnombre = 'choferes.NOMBRE'
	* Nombre de la forma que realizara la búsqueda por nombre.
	this.cformabusqueda = 'listachoferes'
	* Indica si la clave capturada por el usuario existe en la tabla.
	this.lexiste = '.T.'
```

La herramienta 'leecve' cuenta ya con un campo de texto el cual ofrece mostrar algún dato del registro de la clave que se trajo de vuelta por el ventana de búsqueda.

En el método setValue() de la misma herramienta se agrega el siguiente código:
```vfp

	leecve.setvalue()

	LPARAMETERS cvalor 

	Select Nombre from choferes where idchofer = cValor into cursor temp

	if reccount('temp')>0
		this.Nombre.value = temp.Nombre
	else
		this.Nombre.value = ''
		this.Clave.Value = ''
	EndIf
```
