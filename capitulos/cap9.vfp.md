# VENTANA TIPO WIZARD - VFP6 
-----------------
#### Instrucciones para crear una ventana tipo wizard

1. Crear formulario
```vfp
	create form nombreFormulario
```
1.1 Crear formulario en base a librerias MSL
```vfp
	create form nombreFormulario as formaMSL from msllib60
```
2. Dentro de la barra de herramientas para formularios (view / Form Controls Toolbar) seleccionar 'Page Frame' y colocar dentro de la ventana.

3. Clic derecho en PageFrame / Properties... / Layout / PageCount e ingresar el número de pestañas que se desean.

4. Por motivos de programación, se puede ingresar el titulo de las pestañas para distinguirlas durante el desarrollo:
	Clic derecho en PageFrame / Edit / Layout / Caption = 'Encabezado de pestaña'

5. Cambiar nombre de objeto de pestañas
	Clic derecho en PageFrame / Edit / Layout / Other / Name e ingresar el nombre con cual se identificara esta pestaña

#### Opciones dentro de formulario

- Deshabilitar marco del formulario
```vfp
	thisform.Marco.Visible = .F.
```
- Cambio de pestaña en el PageFrame
```vfp
	thisform.PageFrame.ActivePage = nNumeroPagina
```
- Seleccionar campo de otra pagina
```vfp
	thisform.PageFrame.Pag2.NombreCampo.SetFocus()
```
- Regresar al inicio
```vfp
	thisform.PageFrame.ActivePage = 1
```
- Deshabilitar pestañas de un PageFrame
```vfp
	thisform.PageFrame.Tabs = .F.
```
- Deshabilitar edición de campo de texto
```vfp
	thisform.Pageframe.Pag2.NombreCampo.Readonly = .T.
```
- Campo solo con mayúsculas
```vfp
	thisform.Pageframe.pag1.NombreCampo.Format = 'K!'
```
- Campo para contraseña
```vfp
	thisform.Pageframe.pag1.NombreCampo.Passwordchar = '*'
```