Capítulo 8 VFP
===================
Dentro del sistema se han considerado varias condiciones que se tienen que cumplir en determinados formularios; Dichas condiciones son lo que harán que estos formularios se distingan de los demás.  

### Avance de aplicación

Esta condición verifica el numero de resultados obtenidos en una consulta a una tabla, dependiendo el resultado final ejecutara una acción.

```vfp
* Las siguientes lineas de código están dentro de un evento KeyPress de un campo de texto.

LPARAMETERS nKeyCode, nShiftAltCtrl

IF nKeyCode = 13
	nIdchofer = VAL(thisform.txtChofer.Value)
	
	select reccount() from Pedidos where idchofer = nIdChofer into cursor temp

	if reccount('temp') >= 5
		* Si los resultados obtenidos superan o igualan a 5, el sistema te mandara el siguiente mensaje.
		
		wait wind "Chofer no disponible por exceso de pedidos."
		thisform.txtChofer.value = ""
		thisform.Label10.Caption= ""
	else
		* De lo contrario el sistema dará paso a buscar los datos de dicho ID que se buscó inicialmente.
  
		select nombre from Choferes where idchofer==nIdchofer into cursor temp
		if reccount('temp')>0
			thisform.Label10.Caption = temp.nombre 
		else
		* En caso de que no haya datos obtenidos se dará por entendido que dicho registro no existe.

			wait wind "Chofer no existe"
			thisform.txtChofer.value = ""
			thisform.Label10.Caption= ""
		endif
	endif
endif
```
