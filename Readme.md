# Funciones-SCSS

Funciones para generar clases predeterminadas y código CSS de forma rápida, anteriormente integrada en [CV_GEN](https://github.com/LuisPFdez/CV_GEN). Más información sobre el lenguaje en la página oficial.

## Librería
**Las dependencias del paquete son únicamente de desarrollo**. Para poder compilar los ejemplos, de normal no serán necesarias

El archivo *Funciones.scss* no necesita compilarse, no tiene código css, solo utilidades validas para scss. 

Para usar la librería, es posible copiar el fichero [*Funciones.scss*](./Funciones.scss ), o clonar el repositorio, con `git clone https://github.com/LuisPFdez/Funciones-SCSS`.

### Copiar librería
Si se copia el archivo, para usarlo en otro archivo scss, bastará con la regla [use](https://sass-lang.com/documentation/at-rules/use) seguido de la ruta relativa al archivo.
```SCSS
//Estando la librería  en el mismo fichero
@use "Funciones"

*{
  /*Código*/
}
```

### NPM 
Tras clonar el repositorio, npm ofrece un método para añadir librerías con `npm link`.

Dentro de la carpeta donde se ha clonado el repositorio, ejecuta **npm link**. Esto creara un link simbólico en la carpeta global de npm ( solo es necesario hacerlo una vez )

Luego, dentro de las carpetas donde se quiere usar, ejecuta **npm link nombre_paquete**. El nombre del paquete viene dado por el **name** dentro de el [package.json](./package.json) en este caso **funciones**

Es posible hacerlo todo en un paso, ejecutando `npm link /ruta/relativa/o/absoluta`. La ruta puede ser relativa o absoluta a la carpeta de la librería

```BASH
# Cambia a la carpeta home
cd ~
# Clona el repositorio
git clone https://github.com/LuisPFdez/Funciones-SCSS

# ---Primera forma---
# Mueve a la carpeta
cd Funciones-SCSS
# Ejecuta el primer comando
npm link
# Carpeta que necesitará la librería
cd ../otraCarpeta
# Ejecuta el segundo comando
npm link funciones

# ---Otra forma mas directa---
# Carpeta que necesitará la librería
cd ../otraCarpeta
# Ejecuta el segundo comando
npm link ../Funciones-SCSS
```

Para usar la funcion es necesario establecer la ruta en el comando de scss, con --load-path, (es recomendable establacer node_modules para permitir otras librerias)

```BASH
sass --no-source-map src/public/style.scss:dist/public/style.css --style compressed --load-path node_modules/
```

Y para importarlo
```SCSS
//
@use "funciones/Funciones"

*{
  /*Código*/
}
```

## Funciones

### Tipos de funciones
- mixin statement: Permite generar código scss de forma dinámica. Son usados para generar clases predeterminadas. Al ser llamados, pueden recibir como cuerpo otro mixin
- function: A diferencia del **mixin**, devuelve un valor.

### Estados
Al generar clases predeterminadas, estas pueden generarse con estados.
- normal: Estado por defecto, sin ninguna pseudo-clase adicional.
- hover: Estado con la pseudo-clase *hover*.
- active: Estado con la pseudo-clase *active*.
- focus: Estado con la pseudo-clase *focus*.

### Generar-estados ( mixin statement )
Permite generar múltiples clases para distintos estados admitidos. Si algún estado es distinto a los admitidos, este será ignorado

Los argumentos son:
1. Nombre de las clases (**obligatorio**)
2. Estados (**opcional**, por defecto se establecen en *normal* y *hover*), el parámetro ha de ser una lista, en caso de no ser una lista mostrará un error

Ejemplo:
```SCSS
@use "../funciones" as func;

@include func.generar-estados ("texto-negro", ("normal", "hover", "active", "focus")){
    color: #000;
}
```

Resultado CSS: 
```CSS
.texto-negro {
  color: #000;
}

.hover\:texto-negro:hover {
  color: #000;
}

.active\:texto-negro:active {
  color: #000;
}

.focus\:texto-negro:focus {
  color: #000;
}
```

### Generar-clases ( mixin statement ) 
Genera variantes de una clase a partir de un map. También permite generar distintos clases para distintos estados. Si algún estado es distinto a los admitidos, este será ignorado. 

Los argumentos son:
1. Nombre de las clases (**obligatorio**),
2. Valores (**obligatorio**, ha de ser un map, lanzará un error en caso contrario), map de donde obtendrá generará las clases, parte del nombre será compuesto por la clave del map.
3. Estados (**opcional**, por defecto se establecen en *normal* y *hover*), el parámetro ha de ser una lista, en caso de no ser una lista mostrará un error

Para obtener el valor del map acceder a la variable global, **$variableGlobal**

Ejemplo:
```SCSS
@use "../funciones" as func;

$porcentajes: (
    "100":  100%,
    "75":  75%,
    "50":  50%,
    "25":  25%
);

@include func.generar-clases("anchura", $porcentajes, [("normal")]){
    width: func.$variableGlobal;
    height: auto;
}

@include func.generar-clases("altura", $porcentajes, ("normal", "hover", "active", "focus")){
    height: func.$variableGlobal;
    width: auto;
}
```

Resultado CSS: 
```CSS
.anchura-100 {
  width: 100%;
  height: auto;
}
.anchura-75 {
  width: 75%;
  height: auto;
}
.anchura-50 {
  width: 50%;
  height: auto;
}
.anchura-25 {
  width: 25%;
  height: auto;
}
.altura-100 {
  height: 100%;
  width: auto;
}
.altura-75 {
  height: 75%;
  width: auto;
}
.altura-50 {
  height: 50%;
  width: auto;
}
.altura-25 {
  height: 25%;
  width: auto;
}
.hover\:altura-100:hover {
  height: 100%;
  width: auto;
}
.hover\:altura-75:hover {
  height: 75%;
  width: auto;
}
.hover\:altura-50:hover {
  height: 50%;
  width: auto;
}
.hover\:altura-25:hover {
  height: 25%;
  width: auto;
}
.active\:altura-100:active {
  height: 100%;
  width: auto;
}
.active\:altura-75:active {
  height: 75%;
  width: auto;
}
.active\:altura-50:active {
  height: 50%;
  width: auto;
}
.active\:altura-25:active {
  height: 25%;
  width: auto;
}
.focus\:altura-100:focus {
  height: 100%;
  width: auto;
}
.focus\:altura-75:focus {
  height: 75%;
  width: auto;
}
.focus\:altura-50:focus {
  height: 50%;
  width: auto;
}
.focus\:altura-25:focus {
  height: 25%;
  width: auto;
}
```

### degradado ( function ) 
Genera el código necesario para las funciones de degradados y lo devuelve.

Los argumentos son:
1. Colores (**obligatorio**, puede ser un map, el código lo convertirá a un list), listado de los colores del gradiente
2. Porcentajes (**opcional**, ha de ser un list, lanzará un error en caso contrario), por defecto list vacío, listado de porcentajes que corresponde a cada color. Si los porcentajes no tienen el mismo numero que colores, se omitirán los porcentajes faltantes.

Ejemplo:
```SCSS
@use "../funciones" as func;

$colores-1: (
    #56BC75,
    #4BA566, 
    #418D58,
    #367649
);

$colores-2: (
    "100": #CE3E58,
    "300": #B4364D, 
    "600": #9B2F42,
    "900": #812737
);

.porcentaje-1{
    background-color: linear-gradient(func.degradado($colores-1));
}

.porcentaje-2{
    background-color: linear-gradient(func.degradado($colores-2));
}

.porcentaje-3{
    background-color: linear-gradient(func.degradado($colores-1, (50%, 20%)));
}

.porcentaje-4{
    background-color: linear-gradient(func.degradado($colores-2, (50%, 20%)));
}

.porcentaje-5{
    background-color: linear-gradient(func.degradado($colores-1, (50%, 20%, 20%, 10%)));
}

.porcentaje-6{
    background-color: linear-gradient(func.degradado($colores-2, (50%, 20%, 20%, 10%)));
}
```

Resultado CSS: 
```CSS
.porcentaje-1 {
  background-color: linear-gradient(#56BC75 , #4BA566 , #418D58 , #367649 );
}
.porcentaje-2 {
  background-color: linear-gradient(#CE3E58 , #B4364D , #9B2F42 , #812737 );
}
.porcentaje-3 {
  background-color: linear-gradient(#56BC75 50%, #4BA566 20%, #418D58 , #367649 );
}
.porcentaje-4 {
  background-color: linear-gradient(#CE3E58 50%, #B4364D 20%, #9B2F42 , #812737 );
}
.porcentaje-5 {
  background-color: linear-gradient(#56BC75 50%, #4BA566 20%, #418D58 20%, #367649 10%);
}
.porcentaje-6 {
  background-color: linear-gradient(#CE3E58 50%, #B4364D 20%, #9B2F42 20%, #812737 10%);
}
```