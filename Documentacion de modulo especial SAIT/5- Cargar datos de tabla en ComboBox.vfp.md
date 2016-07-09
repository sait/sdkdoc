# CARGAR DATOS DE TABLA EN COMBOBOX - VFP6 
-----------------
#### Instrucciones para cargar en un ComboBox la columna de un .DBF utilizando librerias MSL

### Crear y modificar método dentro del form para cargar los datos al ComboBox
1. Ir a Form / New Method...
2. Dentro del campo de 'Name' ingresar el nombre de referencia a este método
3. Para ingresar al método, ingresamos a un evento de cualquier objeto dentro del formulario
4. En el campo desplegable de 'Object' seleccionamos la ventana (FormaMsl)
5. En el campo desplegable de 'Procedure' nos vamos hasta el ultimo elemento, donde se encontraran los métodos creados
6. Al dar clic en el método ya podremos ingresar las instrucciones a este método

El siguiente método realizara una conulsta a la tabka desea con el fin de almacenar el resultado de la consulta en el ComboBox.
Posteriormente este método lo podemos hacer llamar desde el arranque inicial del formulario.

```vfp
*
* CargarTipos()
*
* Cargar columna a ComboBox
*
with thisform
	.cbo_tipo.RowSourse = ''

	* Se crea la consulta que traera consigo las columnas que se guardaran en el temporal
	cSql = 'Select ID, TIPO form TablaOrigen'

	* Ejecuta la consulta declarada anteriormente
	if not SqlMsl('Temporal','cSql')
		return
	endif

	* Si el resultaod de la consulta tiene 0 resultados, dejara en blanco el ComboBox y lo inhavilitara
	if reccount()==0
		.cbo_tipo.value = ''
		.cbo_tipo.enabled = .f.
	* De lo contrario, actualizara los datos del ComboBox con una de la columna almancenada en el temporal
	else
		Select Temporal
		.cbo_tipo.RowSourse = 'Temporal.TIPO'
		.cbo_tipo.Value = Temporal.TIPO
		.cbo_tipo.Enabled = .T.
	endif
endwith
```
-------------------------------------------------
Las siguientes instrucciones se deberan ingresar en el método INIT del formulario, de esta manera de cuando se inicie la vetana se ejecutaran estas instrucciones.

```vfp
*
* Forma.INIT
*

* Revisa si la tabla de donde se creara el tempora exista, o bien, si se puede tener acceso a ella
if Not OpenDbf('Tabla','ID')
	return .f.
endif

* Crear temporal para almacenar los datos del ComboBox
Create cursor Temporal(;
	ID 		c(3),;
	TIPO 	c(30))

* Se manda a llamar el método para cargar la info. en el ComboBox
thisform.CargarTipos()
* Se deja en blanco el valor de ComboBox
thisform.cbo_tipo.Value = ''
```
-------------------------------------------------
Al dar clic en el objeto, este caso en un botón, se ejecutara las instrucciones para guardar el valor del ComboBox en una tabla
```vfp
*
* Guardar.CLICK
*

* Registrar en la tabla (Registros) la clave referente al valor seccionado en el ComboBox (Temporal)
Select Registro
Go bott
Append blank
Replace TIPO = Temporal.ID
```