# CAPITULO 3 VFP
----------

Para la elaboración de un módulo de un sistema se requieren de clases y algunos métodos importantes para su correcto funcionamiento.

Se le conoce como método a una serie de sentencias que permite llevar a cabo una acción. 

####Ejemplo

Se define una clase en donde se incluirán los métodos
```vfp 
define class clientes as custom
	id = 0
	nombre = ''
	telef  = ''
	ciudad = ''
```
Estos son algunos de los métodos necesarios para la creación de un módulo
```vfp
crearTabla() &&Crea la tabla de clientes, que es donde almacenaramos la informacion
	
function crearTabla()
		create table clientes ( id n(5), nombre c(50), telefono c(30), ciudad c(50))
		index clientes on id tag id
return
```

```vfp
abrirTabla() &&Abre la tabla de cliente o la crea en caso de ser necesario
	
function abrirTabla()
	if not file('clientes.dbf')
		this.crearTabla()
		return
	endif
	use clientes in 0 order id
return
```

```vfp
agregar() &&Agrega un cliente a la base de datos, con la información contenida en las propiedades del objeto

function agregar()
//obtener el ultimo id existente en la tabla
	select max(id) as maxid from clientes into cursor temp
		nLastId = temp.maxid + 1
		
		//agregar un registro a la tabla
		insert into clientes (id, nombre, telef, ciudad) values ;
		(this.id, this.nombre, this.telef, this.ciudad)
return
```

```vfp
actualizar() &&Actualiza la información del cliente, almacenándola en la base de datos
	
function actualizar()
	select clientes
	update clientes set ;
		nombre	= this.nombre,;
		telef	= this.telef,;
		ciudad	= this.ciudad ;
	where id == this.id
return
``` 

```vfp 
eliminar()
	&&Elimina el registro del cliente con id
		
function eliminar()
	delete from clientes where id==this.id
return
```

```vfp
cargar(nId) &&Carga el objeto con los datos del cliente que tiene el id que se manda como parametro
	
function cargar(nId)
	this.initValues()
	select * from clientes where id==nId into cursor temp
	if reccount('temp')>0
		this.id		= temp.id
		this.nombre	= temp.nombre
		this.telef	= temp.telef
		this.ciudad = temp.ciudad
	endif
return
```

```vfp
initValues() &&Inicializa las propiedades del objeto, con su valor default

function initValues
	this.id = 0
	this.nombre = ''
	this.telefono = ''
	this.ciudad = ''
return
```

```vfp
listarTodos()
	&&Lista todos los clientes actuales
	
function listarTodos()
	select clientes
	scan
		? clientes.id, clientes.nombre, clientes.telef, clientes.ciudad
	endscan
return
```
```vfp 
listarAlguns(nIdIni, nIdFin)
	&&Lista los clientes que tiene id entre el rango nIdInicial y nIdFinal que se manda como parametro	
function listarAlgunos(nIdIni, nIdFin)
	select * from clientes where between(clientes.id, nIdIni, nIdFin) into cursor temp
	selec temp
	scan
		? temp.id, temp.nombre, temp.telef, temp.ciudad
	endscan
return
```
Ese código define la finalización de una clase
```vfp
enddefine

```