Capítulo 5 VFP
==============

En este capítulo se implementaron métodos dentro de clases, se mandan llamar dichos métodos esperando recibir parámetros definidos.

Esto se implementó en una interfaz gráfica haciendo uso de formas o formularios, con el fin de implementar los conocimientos en la creación de modúlos especiales para SAIT en la administración de entregas por pedidos.

### Avance de aplicación

```vfp
* 
* Se debe de definir el nombre de la clase junto con su objeto (custom).
*  

define class metodosPruebas as custom

* Se inicializan las propiedades de la clase que se ocuparan posteriormente en los métodos.
nIdchofer = 1
cNombre = ""
cCalle = ""
cCiudad = ""
cTelefono = ""
cNumero = ""
cColonia = ""
cObservacion = "" 

* Se declara el método con la palabra FUNCTION y entre paréntesis los parámetros que espera recibir.

function agregar(nIdchofer, cNombre, cCalle, cCiudad, cTelefono, cNumero, cColonia, cObservacion)

* Se ingresa los procesecimientos que se haran una vez que se active el método, este caso se hace una sentencia a una base de datos en sql.

	INSERT INTO choferes (idchofer, nombre, calle, ciudad, telefono, numero, colonia, observaciones) VALUES (nIdchofer, this.cNombre, this.cCalle, this.cCiudad, this.cTelefono, this.cNumero, this.cColonia, this.cObservacion)
		wait wind "Registro agregado"
	return

*Por ultimo se da por finalizada la definición de la clase.

enddefine

------Dentro del formulario
* En este sección se inicializaron las variables tomando el valor que contenían los TextBox dentro del formulario.

nIdchofer = VAL(thisform.txtNoChofer.value) 
* Se convirtió el texto a numérico.

cNombre = thisform.txtnombre.value
cCalle = thisform.txtcalle.value
cCiudad = thisform.txtciudad.value
cTelefono = thisform.txttelefono.value
cNumero = thisform.txtnumero.value
cColonia = thisform.txtcolonia.value
cObs = thisform.txtobs.value

* Se instancia la clase que contendrá los métodos en un objeto y se tiene que especificar la ruta de acceso.
  
oClase = newObject('metodosPruebas','C:\Documents and Settings\LaptopXP\Mis documentos\Choferes v1.1\metodos.prg')

* Se mandan a llamar los métodos dentro de la clase previamente instanciada, se ingresan los parámetros requeridos.

oClase.agregar(nIdchofer, cNombre, cCalle, cCiudad, cTelefono, cNumero, cColonia, cObs)
```
