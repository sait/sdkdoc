# MODULO ESPECIAL - VFP6 
-----------------

Para el desarrollo de catálogos en VisualFoxPro, SAIT ha diseñado sus propias librerias para facilitarle a los programadores su proceso, unificando las propiedades que se requieren para que la apariencia sea la misma en todos los casos. Para ello es necesario seguir una serie de pasos.

Si aún no está definido el pixel y área de trabajo seleccionamos lo siguiente del menú de vfp. 
Tools > Options > Forms 
SAIT Maneja un 5x5 px y un área de trabajo máximo de 1024x768

en Tools > Options > Controls
Se agregan las siguientes librerias
  
```vfp
	Apolo
	catmsl
	msllib60
```

desde el commando, nos colocamos sobre el directorio en el que se trabajará y donde se creará el sistema. 
```vfp
	CD F:\SAIT\DEMO
``

Y creamos la forma con las propiedades definidas de la libreria 
```vfp
	create form choferescat as catmsl from Z:\sistemas\msllib60\catmsl.vcx
```

#### Catálogo de choferes

Catmsl1.init
```vfp
if not OpenDbf('Choferes', 'IDCHOFER')
	return .F. 
EndIf

* Alias se refiere al nombre de la tabla
this.cAlias = 'Choferes'
* CatName se refiere al nombre del catálogo
this.cCatName = 'ChoferesCat'
this.FormaCatalogo = 'ChoferesDat'
this.cAnchos = '80|100|100|50|100|100|100|100'
this.cTitulos = 'clave|Nombre|Direccion|Número|Colonia|Ciudad|Telefono|Observaciones'
this.cExprs = 'Choferes.IDCHOFER|Choferes.NOMBRE|Choferes.DIRECCION|Choferes.NUMERO|Choferes.COLONIA|Choferes.CIUDAD|Choferes.TELEFONO|Choferes.OBSERVACIONES'
```

Para crear el formulario de datos utilizamos el siguiente código en el commando de vfp
```vfp
	create form choferesdat as formamsl from Z:\sistemas\msllib60\msllib60.vcx
```

Para poder utilizar los objetos con las propiedades predeterminadas de la libreria seleccionamos 'view Classes' de la barra de herramientas y seleccionamos la opción de Msllib60

Formamsl1.init
```vfp 
*nModo Almacena la forma en que va a mandar llamar la ventana
*1 = Agregar
*2 = Modificar
*3 = Eliminar

* Manda llamar la ventana segun la accion que se va a realizar
LParameter nModo, nRec
if Not OpenDbf ('Choferes','IDCHOFER')
	Return .F.
EndIf

With thisform 
	* Si no se recibe ningun parámetro, nModo = 1. Agregar
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
		* Agregar | Cambiar el titulo de la ventana y del boton 
		.Caption = 'Agregar Chofer'
		.Grabar.Caption='\<Agregar'
		Select Choferes
		Go Bott
		.Clave.value = PadL(Allt(SigDoc(IDCHOFER)),5)
	
	Case .nModo==2
		* Modificar | Cambiar el titulo de la ventana y del boton 
		.caption = 'Modificar chofer'
		.Grabar.Caption = '\<Modificar'
		
		.Clave.Enabled = .F.
		
		.CargarInfo()
	
	Case .nModo==3
		* Eliminar | Cambiar el titulo de la ventana y del boton 
		.Caption = 'Eliminar Chofer'
		.Grabar.Caption='\<Eliminar'
		
		.CargarInfo()
		.SetAll('enabled', .F.)
		
		.Grabar.Enabled = .T.
		.Cerrar.Enabled = .T.
		
	endCase
	
EndWith
```

Textbox idchofer dentro del método valid
```vfp
* Alinear la clave del chofer
this.Value=PadL(Allt(this.Value),5)
```

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