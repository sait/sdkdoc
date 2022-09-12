# MODULO ESPECIAL - VFP6
-----------------
Uno de los aspectos más importantes para SAIT es la adaptabilidad a las empresas, por la experiencia que tenemos sabemos que existen giros de negocios con requerimientos especiales muy especificos, inclusive empresas del mismo giro manejan en forma muy distintas un mismo proceso, por eso contamos con un área exclusivamente para atender estas demandas.

Los módulos especiales son opciones, no muy grandes, que se agregan a SAIT para que se adapte mejor a la empresa.

Este documento contiene toda la información que usted como desarrollador necesita para realizar adecuaciones y funciones para mejorar la adaptabilidad de SAIT en la empresa o con su cliente.

Dentro de la creación de un catalogo adicional para integrar junto con un modulo especial deberá contar con estas herramientas para poder desarrollar aplicaciones y adecuaciones a SAIT:

- Visual FoxPro 6.3 (Aplicación necesaria para codificar, LINK DE DESCARGA)
- SAIT Software Administrativo (Para pruebas de integración, LINK DE DESCARGA)
- Librerías especiales (Herramientas necesarias adaptables, LINK DE DESCARGA)

Para el desarrollo de catálogos en Visual FoxPro, SAIT ha diseñado sus propias librerías para facilitarle a los programadores su proceso, unificando las propiedades que se requieren para que la apariencia sea la misma en todos los casos. 

Para su implementación es necesario seguir esta serie de pasos desde el programa VFP (Visual FoxPro):

SAIT desde sus inicios ha establecido un tamaño por default para los tamaños y calidad de ventanas, entonces si aún no está definido el píxel y área de trabajo seleccionamos lo siguiente del menú de VFP. 
Tools > Options > Forms 
SAIT Maneja un 5x5 Pixeles y un área de trabajo máximo de 1024x768.

Ahora, se tiene que proceder a integrar las librerías especiales de SAIT a VFP.
Desde Tools > Options > Controls
Se agregan las siguientes librerias
  
- Apolo.vcx
- catmsl.vcx
- msllib60.vcx

Al haber terminado estos dos pasos es recomendable seleccionar la opción ``` Set As Default ``` con la finalidad de que estos pasos no se tengan que volver a realizar al iniciar de nuevo vfp. Dar en aceptar para dar por terminado el proceso.

Desde la ventana de comandos (Window > Command Window) , nos colocamos sobre el directorio en el que se trabajará y donde se creará el sistema. 
```vfp
	CD F:\SAIT\DEMO
```

Y creamos la forma con las propiedades definidas de la librería.
```vfp
	create form choferescat as catmsl from Z:\sistemas\msllib60\catmsl.vcx
```
Básicamente se está ordenando que se cree una nueva ventana con las propiedades de la librería * CatMs * previamente añadida a vfp.

Muestra un grid con los registros actuales de la base de datos y presenta botones para Agregar/ Modificar y Borrar registros.

#### Catálogo de choferes

Ahora se procederá a crear el catalogo inicial que mostrara la lista de registros que sean añadidos a una determinada tabla. Dentro del método INIT del formulario se agregara el siguiente código:

Catmsl1.init
```vfp
	if not OpenDbf('Choferes', 'IDCHOFER')
		return .F. 
	EndIf
	*Permite abrir un archivo en modo compartido, con un índice predeterminado por cTag, y adicionalmente indicar bajo que Alias se desea identificar ese DBF. Regresa .t. si el DBF se pudo abrir,  .f. si hubo algún problema. Si no se pasa el 3er parámetro, entonces el nombre del archivo se considerara como Alias.

	* Alias donde se buscara la clave. Debe de estar activo el orden.
	this.cAlias = 'Choferes'
	* Nombre del catalogo. Usado para diferenciarlo de otros catálogos.
	this.cCatName = 'ChoferesCat'
	* Nombre de la forma a usar para hacer cambios en el catalogo.
	this.FormaCatalogo = 'ChoferesDat'
	* Nombre de la forma a usar para realizar una búsqueda. Debe ser modal y regresar la clave.
	this.FormaBusqueda = 'ListChofer()'
	* Ancho en píxeles, de las columnas.
	this.cAnchos = '80|100|100|50|100|100|100|100'
	* Títulos a desplegar en las columnas de la vista principal.
	this.cTitulos = 'clave|Nombre|Direccion|Número|Colonia|Ciudad|Telefono|Observaciones'
	* Campos a desplegar en la vista principal, separados con |
	this.cExprs = 'Choferes.IDCHOFER|Choferes.NOMBRE|Choferes.DIRECCION|Choferes.NUMERO|Choferes.COLONIA|Choferes.CIUDAD|Choferes.TELEFONO|Choferes.OBSERVACIONES'
```
Este es el código necesario dentro del formulario de catalogo. Ahora se debe proceder a la creación de una ventana para la edición de datos de registros.

Para crear el formulario de datos utilizamos el siguiente código en la ventana de comandos de vfp.
```vfp
	create form choferesdat as formamsl from Z:\sistemas\msllib60\msllib60.vcx
```

Para poder utilizar los objetos con las propiedades predeterminadas de la librería seleccionamos 'View Classes' de la barra de herramientas (View > Form Controls Toolbar) y seleccionamos la opción de 'Msllib60'.

Ya creado el formulario es importante declarar ciertos parámetros que se ocuparan mas adelante ya en la implementación del código, uno de ellos es la variable 'nModo', esta se declara desde vfp en 'Form > New Property...'.

Aparte se ocupa declarar dos métodos mas, los cuales son 'CargaInfo()' y 'SaveInfo()', esto desde 'Form > New Method...'.

Dentro del método INIT del formulario se agregara el siguiente codigo:

Formamsl1.init
```vfp 
	*nModo Almacena la forma en que va a mandar llamar la ventana
	*1 = Agregar
	*2 = Modificar
	*3 = Eliminar

	* Manda llamar la ventana según la acción que se va a realizar
	LParameter nModo, nRec
	if Not OpenDbf ('Choferes','IDCHOFER')
		Return .F.
	EndIf

	With thisform
	*La herramienta 'With thisform' elimina la necesidad de estar indicando en cada linea que los procesos que se están programando son para esta ventana.

		* Si no se recibe ningún parámetro, nModo = 1. Agregar
		if PCount()==0
			nModo =1
		EndIf
		
		* Asignamos a la propiedad nModo en que mandamos llamar la ventana
		.nModo = nModo
		
		Select Choferes
		* Si la forma de llamar la ventana es modificar o eliminar 
		IF(.nModo == 2 Or .nModo == 3)

			*Validamos que el número de registros que se recibió como parámetro exista
			if(nRec<=0 Or nRec>Reccount())
				Alerta('Registro no válido')
				Return .F.
			EndIf
			
			*Entonces es un número de registros válido. y me posiciono en ese registro
			GoTo nRec
		EndIf
		
		Do case
		Case .nModo==1
			* Agregar | Cambiar el titulo de la ventana y del botón 
			.Caption = 'Agregar Chofer'
			.Grabar.Caption='\<Agregar'
			Select Choferes
			Go Bott
			.Clave.value = PadL(Allt(SigDoc(IDCHOFER)),5)
		
		Case .nModo==2
			* Modificar | Cambiar el titulo de la ventana y del botón 
			.caption = 'Modificar chofer'
			.Grabar.Caption = '\<Modificar'
			
			.Clave.Enabled = .F.
			
			.CargarInfo()
		
		Case .nModo==3
			* Eliminar | Cambiar el titulo de la ventana y del botón 
			.Caption = 'Eliminar Chofer'
			.Grabar.Caption='\<Eliminar'
			
			.CargarInfo()
			.SetAll('enabled', .F.)
			
			.Grabar.Enabled = .T.
			.Cerrar.Enabled = .T.			
		endCase		
	EndWith
```

Dentro del formulario es necesario tener un campo de texto que mostrando y registrando la clave del registro. Es recomendable añadir la siguiente validación dentro del método 'Valid'.

IdChofer.Valid
```vfp
	* Alinear la clave del chofer
	this.Value=PadL(Allt(this.Value),5)
```

Como se menciono anteriormente, es necesario la implementación de dos métodos declarados por nosotros mismos, ahora se pasara a la codificación de cada uno.

Dentro del metodo 'SaveInfo()':

Formamsl.saveinfo
```vfp
	* Método para guardar la información
	With thisform
		select Choferes
		replace	IDCHOFER 	With 	.Clave.Value,;
				NOMBRE	 	With 	Allt(.Nombre.Value),;
				DIRECCION 	with 	Allt(.Direccion.Value),;
				NUMERO		With 	Allt(.Numero.Value),;
				COLONIA 	With	Allt(.Colonia.Value),;
				CIUDAD 		With 	Allt(.Ciudad.Value),;
				TELEFONO 	With 	Allt(.Telefono.Value),;
				OBSERVACIONES 	With 	Allt(.Observaciones.value)
	endwith
```
Dentro del metodo 'CargarInfo()':

Formammsl.cargarinfo
```vfp
	* Método para cargar la información del chofer
	with ThisForm
		.Clave.Value = Choferes.IDCHOFER
		.Nombre.Value = Choferes.NOMBRE
		.Direccion.Value = Choferes.DIRECCION
		.Numero.Value = Choferes.NUMERO
		.Colonia.Value = Choferes.COLONIA
		.Ciudad.Value  = Choferes.CIUDAD
		.Telefono.value = Choferes.TELEFONO
		.Observaciones.value = Choferes.OBSERVACIONES
		
	EndWith
```
Dentro del formulario se necesita un boton que ejecutara dichas acciones que se le integraran dentro del método 'Click()' que viene por default en la herramienta.

grabar.Click
```vfp
	* Graba la información 
	With Thisform
		
		*Manda llamar las validaciones, si estoy agregando o modificando
	if(.nModo<=2)

		If .nModo ==1
			
			* Validar que no se omita la clave del chofer		
			If Empty(.Clave.Value)
				Alerta('No se puede omitir la clave del chofer')
				.Clave.SetFocus()
				Return .F.
			EndIf
			
			* Validar que no exista otro chofer con la misma clave
			if seek(PadL(Allt(.Clave.value),5), 'Choferes')
				alerta('Ya existe un chofer con la misma clave')
				.Clave.setfocus()
				Return .F.
			EndIf
			
		EndIf
			
			* Validar que no se omita el nombre
			if empty (.Nombre.Value)
				Alerta('No se puede omitir el nombre')
				.Nombre.SetFocus()
				Return .F.
			EndIf
		EndIf
		
		Do case
			Case nModo==1
				Select Choferes
				Append blank
				
				* Manda llamar el método para guardar la información
				.SaveInfo()
				
				* Cerrar la ventana después de grabar 
				.Release
			
			Case .nModo==2
				* Manda llamar el método para guardar la información
				.SaveInfo()
				
				* Cerrar la ventana después de grabar 
				.Release
			
			Case nModo==3
				* Eliminar
				*Preguntar si realmente desea eliminar el chofer
				if GetNY('¿Desea eliminar el chofer')
					Select choferes
					Delete
				EndIf
				
				* Cerrar la ventana después de eliminar
				.release	
		EndCase
	EndWith
```
