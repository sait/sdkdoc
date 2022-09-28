# MODULO ESPECIAL - VFP6
-----------------

### Funciones adicionales

Las siguientes clases ayudan al desarrollo de aplicaciones, se encuentran en la librería de clase: MslLib60.vcx

#### Clases
- Búsqueda: Permite hacer una ventana de búsqueda
- LeeCve: Perite pedir una clave de catalogo, con opción a búsqueda
- FormaMsl: Permite hacer una forma con sesión privada, cerrar con Escape y cerrar las tablas al salir
- TextoFecha: Permite pedir una fecha en un TextBox

Cuando creas una forma, usa el comando:
```Create Form 'Nombre_editable' as 'Nombre_clase' From 'Nombre_libreria'```

Ejemplo:
```vfp
	Create Form VentanaClientes as FormaMsl From MslLib60
```
#### Clase TextoFecha

Permite leer una fecha en un text box adaptable a las ventanas de SAIT.

Para usarla ingrese a la barra de herramientas ```(View > Form Controls Toolbar)``` y usar la forma 'textofecha'. Agregue el control a cualquier formulario.

#### Propiedades
dValue: Contiene el valor en formato fecha leído
Value: Contiene el valor en formato string de la fecha. Ejemplo: 15-Sep-2001
SetValue: Método que permite fijar el valor de la fecha. Ejemplo : ThisForm.FechaCompra.SetValue(Date())

### Funciones internas de SAIT

Existen algunas funciones que se han ido desarrollando y que se pueden usar dentro de las formas de algun modulo especial en construcción, estas están clasificadas por categorías: 

#### Grupos
- Interfase de usuario: GetYN, GetNY, InitPje, ActPje, ClosePje, AddBar
- Manejo de fechas: StoD, MexDate, MexDate2, UsDate, SigMes, NomMes, cNomMes
- Impresión de Reportes: SendRep, ModiFto, DoCmd, InitVar, Letra, CampoArea
- Impresión en modo texto: DoblAnch, Elite,Pica,Octavos,Sextos, Bold,NoBold, Under,NoUnder, Cond, NoCond
- Funciones varias: SubLin, SigDoc, ValProp
- Almacenar variables: GetMsl, PutMsl, UserVal
- Manejo de DBFs: OpenDbf, OpenExcl, DelAlias
- Compresión de Archivos: MakeZip2, Unzip2, DynaZip
- Enlace con otros programas: OpenExcel

-----------------
### Funciones para la interfase de usuario

##### Aviso ( cMensaje )
Presenta un mensaje al usuario con el botón  de Aceptar/Ok con el signo de Aviso.

Ejemplo:
```vfp
	Aviso(‘Proceso ha sido terminado’)
```

##### Alerta ( cMensaje )
Presenta un mensaje al usuario con el botón  de Aceptar/Ok con el signo de Error.

Ejemplo:
```vfp
	Alerta(‘Proceso ha sido terminado’)
```

##### GetYN ( cMensaje )
Presenta una ventana con el mensaje indicado, preguntanto SI o NO, regresa .t. si el usuario presion SI, falso si el usuario presiono NO. El botón predeterminado es <SI>.

Ejemplo:
```vfp
	If not GetYN(‘Desea continuar?’)
		Return
	Endif
```

##### GetNY ( cMensaje )
Presenta una ventana con el mensaje indicado, preguntanto SI o NO, regresa .t. si el usuario presion SI, falso si el usuario presiono NO. El boton predeterminado es <NO>.Se debe usar para procesos delicados.

Ejemplo:
```vfp
	If not GetNY(‘Desea borrar la informacion?’)
		Return
	Endif
```

##### InitPje ( [cTitulo ] )
Activa una ventana en dónde se mostrará el porcentaje de avance de un proceso.

##### ActPje ( nPorcentaje )
Actualiza el porcentaje de avance en la barra. nPorcentaje debe ser un número entre 0 y 1. Es decir que 0.25 mostrara un avance de 25%   1.00 mostrara un avance del 100%.

##### ClosePje ()
Cierra la ventana dónde se muestra el porcentaje de avance

Ejemplo:
```vfp
	Use clientes
	InitPje(‘Procesando’)
	Scan
		ActPje(Recno() / Reccount() )
		Do Proceso
	EndScan
	ClosePje()
```

##### AddBar (cPopUpName, cPrompt,  cComando, cKey )
Agrega una barra a un menú.

Ejemplo:
```vfp
	AddBar('PuntoDeVen','Reloj checador','Do Form Reloj','CTRL-R')
	AddBar('Inventario','Busqueda especial de articulos','Do BusMax in NumMotor')
```
-----------------
### Funciones para el manejo de fechas

##### StoD ( cFecha )
Convierte una fecha en formato string aaaammdd a formato tipo fecha.

Ejemplo:
```vfp
	? Stod(‘19701203’)  
	03/12/1970
```

##### MexDate ( dFecha )
Regresa la fecha pasada en formato string en el estilo: Lun.13/Sep/2003.

##### MexDate2 ( dFecha )
Regresa la fecha pasada en formato string en el estilo: 03/Dic/1970.

##### MexDate3 ( dFecha )
Regresa la fecha pasada en el formato string en el estilo: 03/Dic/70.

##### UsDate ( dFeha )
Regresa la fecha pasada en el formato string en el estilo:  03-Aug-1985.

##### SigMes ( cPeriodo )
Regresa el siguiente mes del período que se pasa.

Ejemplo:
```vfp
	SigMes('200212') = '200301'
	SigMes('200201') = '200202'
```

##### NomMes ( dFecha ) 
Regresa el nombre del mes de la fecha pasada.

Ejemplo:
```vfp
	? NomMes ( {^2003/05/31 } )  
	Mayo
```

##### cNomMes ( nMes )
Regresa el nombre del número de mes que se pasa.

Ejemplo:
```vfp
	cNomMes(1) = ‘Enero’
```

-----------------
### Funciones de Impresion de Reportes

##### SendRep (cFormato [, cDest [, cScope]]
Presenta la ventana para mandar el formato a: Pantalla, Impresora, Disco, Email. Ademas permite modificar el formato a imprimir. Opcionalmente permite especificar el destino a dónde se desea mandar el reporte. Así como el rango de registros a imprimir, según los indiados en Fox: Rest, Next nRec, While <lCond>, etc. Si se trata de un formato modo texto, debe contar con el archivo Imprime.bat en directorio actual.

Ejemplos:
```vfp
	SendRep(‘Nota’,’LPT1’)
	SendRep(‘RepVentas’)
	SendRep(‘Factura’,’’,’Rest’)
```

##### ModiFto (cFormato)
Activa el diseñador de reportes de fox, editando el formato que se manda como parámetro.

##### DoCmd (cComando)
Permite ejecutar cualquier comando dentro de un formato, util cuando quieres abrir otras tablas, o hacer un proceso especial.
Siempre regresa: ‘’

Ejemplo:
```vfp
	DoCmd(‘Use Arts in 0 Order NumArt’)
```

##### InitVar (cVarName, eExpresion)
Inicializa una variable como publica con el valor que se pasa como parámetro.

##### Letra (nImporte, cMoneda )
Regresa en forma de string, la cantidad con letra correspondiente al importe que se pasa como parámetro. cMoneda: ‘P’  Pesos  ‘D’ Dolares.

Ejemplo:  
```vfp
	Letra(Cxc.IMPORTE, Cxc.DIVISA)
```

##### LetBill(nImpore)
Regresa en forma de texto, la cantidad con letra correspondiente al importe que se pasa como parámetro. No incluye los centavos. No incluye el nombre de la moneda. Solo la cantidad con letra. Puede servir para hacer funciones para incluir el nombre de otras monedas.

##### CampoArea (eKey, cAlias, cNomCampo)
Busca en un área cierta llave y regresa el campo que se pasa como parámetro.

Ejemplo: 
```vfp
	CampoArea(Docum.NUMCLI, ‘Clientes’, ‘NOMCLI’)   
	* Busca en Clientes la expresión Docum.NUMCLI y regresa el nombre del cliente.
```

-----------------
### Funciones para impresión en modo texto

Estas funciones se usan en los formatos de modo texto, y son las secuencias de caracteres ESC que las impresoras Epson o compatibles, reconocen para cambiar el tipo de letra en modo texto. 
En formatos modo grafico, NO se usan.

##### Elite()
Regresa la cadena ESC para indicarle a la impresora que imprima en 12 ccp (caracteres por pulgada) cuando no esta condensado y 16 cpp cuando esta activado el condensado.

##### Pica()
Regresa la cadena ESC para indicarle a la impresora que imprima a 10 ccp en modo normal o 17 cpp en modo condensado. Es el default en las impresoras.

##### Sextos()
Regresa la cadena ESC para indicarle a la impresora que imprima en 6 lineas por pulgada (6lpp) que es el default = 66 lineas en una hoja tamano carta.

##### Octavos()
Regresa la cadena ESC para indicarle a la impresora que imprima en 8 lpp = 88 lineas en una hoja tamano carta.

##### SetPgLen( nLineas )
Regresa la cadena ESC para indicarle a la impresora que se estara usando una hoja diferente que el tamano carta.  nLineas es el numero de lineas que mide tu hoja.

Ejemplo:
```vfp
	SetPgLen(33)
	* Le indica a la impresora que estas usando una hoja media carta, de esta forma no va a saltar tanto cuando termine de imprimir el reporte.
```

##### Cond()
Regresa la cadena ESC para indicarle a la impresora que active el tipo de letra condensado.

##### NoCond()
Regresa la cadena ESC para indicarle a la impresora que cancele el tipo de letra condensado y regrese al normal.

##### Under()
Regresa la cadena ESC para indicarla a la impresora que active el tipo de letra subrayado.

##### NoUnder()
Regresa la cadena ESC para indicarle a la impresora que cancele el tipo de letra subrayado.

##### Bold()
Regresa la cadena ESC para indicarle a la impresora que active el tipo de letra Bold.

##### NoBold()
Regresa la cadena ESC para indicarle a la impresora que cancele el tipo de letra Bold.

-----------------
### Funciones Varias

##### SubLin  (cLista,  nItem  [,cSeparador] )
Permite obtener el elemento nItem, de una lista tipo string. Opcionalmente usted puede indicar el carácter usado como separador. Por default el separador es '^' .
Permite el manejo de lista en forma agíl y sencilla.

Ejemplo:
```vfp
	? SubLin('Juan^Perez’,2)
	Perez
	? SubLin(‘Ignacio,Elizabeth,Nicole’, 3 , ‘,’) 
	Nicole
	For i=1 to 3
		? SubLin(‘X,Y,Z’, i)
	Next
``` 

##### SigDoc (cFolioActual)
Regresa en formato string, el siguiente folio para ser usado en algún documento dependiendo del valor que se manda.
Usado para incrementar el campo que representa un folio.

Ejemplo:
```vfp		
	? SigDoc('A105') 
	'A106'
	? SigDoc('1054') 
	'1055'
``` 

##### ValProp (cMemo, cPropiedad)
Regresa el valor de una propiedad almacenada en un campo memo. Por ejemplo en el campo Clientes.OTROSDATOS se pueden almacenar otra información adicional que la empresa requiera.
De tal forma que se guarda en un campo memo con el siguiente formato:
```vfp
	<VarName> = <Valor>
	<VarName> = <Valor>
```
Por ejemplo el campo Clientes.OTROSDATOS puede contener:
```vfp
	COD=SI
	Mensaje=Avisar al gerente cuando se presente el cliente. Quedo de pagar una factura muy atrasada.
``` 
Para imprimir y usar el valor de esas variables use la funcion ValPro.
```vfp
	? ValProp( Clientes.OTROSDATOS, ‘COD’)  
	‘SI’
	? ValProp(Clientes.OTROSDATOS, “Mensaje”)
	‘Avisar al gerente cuando se presente el cliente. Quedo de pagar una factura muy atrasada.’
```

-----------------
### Funciones para almacenar variables

Estas funciones permite almacenar variables en 2 archivos:
- Config.MSL que se guarda en el directorio de la empresa.
- Apolo.INI que se guarda en el directorio de windows de la estacion.
Los parametros especificos a la estación se guardan en Apolo.INI y las variables comunes para todas las PCs se guardan en Config.MSL.

##### GetMsl ( cVarName )
Regresa en formato string, el valor actual de una variable almacenada en el archivo Config.Msl

Ejemplo:
```vfp
	? GetMsl(‘Decimales_en_Precio’)
```


##### PutMsl (cVarName, cValue)
Almacena una variable en el archivo Config.Msl
Regresa .t. si pudo grabarlo o .f. en caso contrario

Ejemplo:
```vfp
	PutMsl(‘Decimales_en_precio’, ‘2’)
```

##### UserVal ( cVarName [,cValue] )
Tiene 2 funciones:
1)	Grabar una variable en C:\Windows\Apolo.Ini
2)	Obtener el valor de una variable de C:\Windows\Apolo.Ini
Si no se pasa el parámetro cValue, entonces la función regresa en formato tipo string, el valor de una variable almacenada en Apolo.INI.
Si se pasan los 2 parámetros, entonces la función, graba la variable que se pasa, en el archivo Apolo.INI. El valor almacenado es el del 2do parámetro. Estas variables son útiles, cuando se trata de parametros especificos a la computadora del usuario.

Ejemplo:
```vfp
	? UserVal(‘BitMap’)
	UserVal(‘BitMap’, ‘C:\SAIT\FONDO.BMP’)
```

-----------------
### Funciones para el manejo de DBFs

##### OpenDbf  ( cDbf  [,cTag  [,cAliasaUsar]] )
Permite abrir un archivo en modo compartido, con un índice predeterminado por cTag, y adicionalmente indicar bajo que Alias se desea identificar ese DBF. Regresa .t. si el DBF se pudo abrir, .f. si hubo algun problema. Si no se pasa el 3er parametro, entonces el nombre del archivo se considerara como Alias.

Ejemplo:
```vfp
	If not OpenDbf(‘Cleintes’,’NumCli’) 
		Aviso(‘No se pudo abrir’)
	Endif
```

##### OpenExcl ( cListaDbfs )
Permite abrir en modo exclusivo todos los archivos que se pasan como parámetro
Regresa .t. si todos los archivos se pudieron abrir, .f. si hubo algun problema

Ejemplo:
```vfp  
	If OpenExcl(‘Clientes,Arts, Vend’)
		Do Process...
	Endif
```

##### DelAlias ( cAlias )
Borra el archivo asociado con un alias actual.
CUIDADO esta funcion debe usarse con mucho cuidado.
BORRA el archivo DBF especificado en el alias que se pasa como parámetro.
Sirve para borrar DBFs temporales que se usan en alguna consulta.
NUNCA usarla sobre archivos de datos. DelAlias(‘Clientes’) borrara completamente del disco el archivo de clientes

Ejemplo:
```vfp
	DelAlias(‘Temp’)
```

-----------------
### Funciones de compresion de archivos

##### MakeZip2  ( cZipFileToCreate, cFilesToInclude)
Crea un archivo zip, que contiene los archivos especificados. No muestra avances, solo genera el archivo.
IMPORTANTE: Se debe incluir toda la ruta de todos los archivos

Ejemplo
```vfp
	MakeZip2( ‘A:\Envio.Zip’, ‘C:\SAIT\CIA001\Docum.DBF  C:\SAIT\CIA001\Docum.FPT  C:\SAIT\CIA001\*.FR?’)
```

##### DynaZip ( cZipFileToCreate, cFilesToInclude)
Crea un archivo zip, que contiene los archivos especificados. Muestra el porcentaje de avance.
IMPORTANTE: Se debe incluir toda la ruta de todos los archivos

Ejemplo
```vfp
	MakeZip2( ‘A:\Envio.Zip’, ‘C:\SAIT\CIA001\Docum.DBF  C:\SAIT\CIA001\Docum.FPT  C:\SAIT\CIA001\*.FR?’)
```

##### UnZip2  ( cZipFileName )
Descomprime el archivo indicado en el directorio actual.
IMPORTANTE: Debe especificar toda la ruta del archivo.

Ejemplo:
```vfp
	Unzip2(‘A:\ENVIO.ZIP’)
```

-----------------
### Funciones de enlace con otros programas

##### OpenExcel ( cTit1, cTit2, cTitulos, cMascaras)
El area activa la pasa a excel. Todos los campos.
- cTit1: Título 1 generalmente el nombre de la empresa.
- cTit2: Título 2, generalmente el nombre del reporte.
- cTitulos: Es una lista con los títulos a colocar en cada columna. Usando el separador ^.
- cMascaras: Es una lista con la mascara que debe usar excel. Usar el separador ^.  
		
Ejemplos de mascaras:
```vfp
	9,999,999.99
	C   (Para campos tipo carácter)
	D   (Para campos tipo fecha)
```
