Capítulo 5 VFP
==============

En este capitulo se implemento el uso de métodos dentro de clases, en donde se hacían llamar dichas funciones esperando recibir parámetros definidos.

Esto se implemento en el uso de interfaz mediante el uso de  formas.

Todo esto fue con el fin de implementarse los conocimientos en la creación de modulo especial para SAIT en la administración de entregas por pedidos.


###Avance de aplicación

```vfp
------En la clase
*Se debe de definir el nombre de la clase junto con su objeto (custom).
  
define class metodosPruebas as custom

*Se inicializan las clases que se ocuparan posteriormente en los métodos.
nIdchofer = 1
cNombre = ""
cCalle = ""
cCiudad = ""
cTelefono = ""
cNumero = ""
cColonia = ""
cObservacion = "" 

*Se declara el método con la palabra FUNCTION y entre paréntesis los parámetros a esperar.

function agregar(nIdchofer, cNombre, cCalle, cCiudad, cTelefono, cNumero, cColonia, cObservacion)

*Se ingresa los procesecimientos que se haran una vez que se active el método, este caso se hace una sentencia a una base de datos en sql.

	INSERT INTO choferes (idchofer, nombre, calle, ciudad, telefono, numero, colonia, observaciones) VALUES (nIdchofer, this.cNombre, this.cCalle, this.cCiudad, this.cTelefono, this.cNumero, this.cColonia, this.cObservacion)
		MESSAGEBOX ("Registro agregado")
	return

*Por ultimo se da por finalizada la definición de la clase.

enddefine

------Dentro del formulario
*En este sección se inicializo las variables tomando el valor que contenían los TextBox dentro del formulario.

nIdchofer = VAL(thisform.txtNoChofer.value) 
*Se convirtió el texto a numérico.

cNombre = thisform.txtnombre.value
cCalle = thisform.txtcalle.value
cCiudad = thisform.txtciudad.value
cTelefono = thisform.txttelefono.value
cNumero = thisform.txtnumero.value
cColonia = thisform.txtcolonia.value
cObs = thisform.txtobs.value

*Se instancia la clase que contendrá los métodos en un objeto y se tiene que especificar la ruta de acceso.
  
oClase = newObject('metodosPruebas','C:\Documents and Settings\LaptopXP\Mis documentos\Choferes v1.1\metodos.prg')

*Se mandan a llamar los métodos dentro de la clase previamente instanciada, se ingresan los parámetros requeridos.

oClase.agregar(nIdchofer, cNombre, cCalle, cCiudad, cTelefono, cNumero, cColonia, cObs)
```
