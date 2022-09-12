# Capítulo 1 VFP

Visual Fox Pro o VFP es un lenguaje de programación por procedimientos, orientado a objetos derivado de DBase en los años 80s, comprado por Microsoft en 1992 y que dejó de existir en el 2007, su última versión en VFP 9.0.

La principal ventaja de VFP fue la velocidad en el manejo de datos y su ambiente de desarrollo rápido.

### Practicas comunes en VFP.
Es muy comun que al momento de leer código escrito FoxPro nos encontremos con lo siguiete:

```
    nAge = 19;
```

Encontrar este tipo de código es bastante normal ya que FoxPro es un lenguaje de tipado dinámico, por convención se coloca la inicial del tipo de dato que la variable va a almacenar.

### Tipos de datos en FoxPro
| Tipo     | Ejemplo  |
|----------|----------|
|Character | cName    |
|Numeric   | nAge     |
|Logic     | lValid |
|Date & DateTime | dBirthday|


Algunos comandos

El comando "?" se usa para imprimir texto en el contenedor MDI del entorno de Visual FoxPro

#### ? 
```vfp
? 'hola mundo', 10
? 2+3
```

#### function
```vfp
function areaTriangulo( nBase, nAltura)
local nArea
    nArea = nBase * nAltra / 2
return nArea
```



