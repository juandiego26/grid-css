ref: <https://css-tricks.com/snippets/css/complete-guide-grid/>

# Una guía completa de Grid

Publicado

6 de noviembre de 2016

Actualizado

3 de abril de 2019

CSS Grid Layout es el sistema de diseño más poderoso disponible en CSS. Es un sistema bidimensional, lo que significa que puede manejar columnas y filas, a diferencia de flexbox, que es en gran parte un sistema unidimensional. Usted trabaja con el Grid Layout aplicando reglas CSS tanto a un elemento padre (que se convierte en el  Grid Container) como a [...]

Patrocinador mensual

##### Gracias, Keen!

Envíe métricas orientadas al cliente en una tarde con [Keen](https://synd.co/2HtkHDF). Recopile, almacene, consulte y visualice analíticas integradas a la perfección. No se necesitan iframes.


CSS Grid Layout es el sistema de diseño más poderoso disponible en CSS. Es un sistema bidimensional, lo que significa que puede manejar columnas y filas, a diferencia de [flexbox,](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) que es en gran parte un sistema unidimensional. Trabaja con Grid Layout aplicando reglas de CSS tanto a un elemento padre (que se convierte en el Grid Container) como a los elementos secundarios de ese elemento (que se convierten en Elementos de cuadrícula).


Este artículo fue presentado originalmente por [la guía Chris House](http://chris.house/blog/a-complete-guide-css-grid-layout/) , y desde entonces ha sido actualizado por el personal de CSS-Tricks y los escritores de pago.

### [Introducción](https://css-tricks.com/snippets/css/complete-guide-grid/#grid-introduction)

CSS Grid Layout (también conocido como "Grid"), es un sistema de diseño bidimensional basado en grid que tiene como objetivo hacer nada menos que cambiar por completo la forma en que diseñamos las interfaces de usuario basadas en grid. CSS siempre se ha utilizado para diseñar nuestras páginas web, pero nunca ha hecho un buen trabajo. Primero, usamos tablas, luego floats, posicionamiento y `inline-block`, pero todos estos métodos fueron esencialmente pirateados y dejaron de lado muchas funcionalidades importantes (centrado vertical, por ejemplo). Flexbox ayudó, pero está pensado para diseños unidimensionales más simples, no complejos bidimensionales (Flexbox y Grid en realidad funcionan muy bien juntos). Grid es el primer módulo CSS creado específicamente para resolver los problemas de diseño que todos hemos estado haciendo a lo largo de la piratería desde que creamos sitios web.

Hay dos cosas principales que me inspiraron a crear esta guía. El primero es el impresionante libro de Rachel Andrew, [Get Ready for CSS Grid Layout. ](http://abookapart.com/products/get-ready-for-css-grid-layout)Es una introducción completa y clara a Grid y es la base de todo este artículo. Le *recomiendo* que lo compre y lo lea. Mi otra gran inspiración es la [Guía completa para Flexbox de Chris Coyier](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) , que ha sido mi recurso de consulta para todo lo relacionado con flexbox. Ha ayudado a un montón de personas, evidente por el hecho de que es el mejor resultado cuando se realiza Google "flexbox". Notarás muchas similitudes entre su publicación y la mía, porque ¿por qué no robar lo mejor?

Mi intención con esta guía es presentar los conceptos de Grid tal como existen en la última versión de la especificación. Por lo tanto, no cubriré la sintaxis obsoletas de IE  y haré todo lo posible para actualizar esta guía periódicamente a medida que la especificación madure.

### [Fundamentos y soporte del navegador](https://css-tricks.com/snippets/css/complete-guide-grid/#grid-browser-support)

Para comenzar, debe definir un elemento contenedor como una cuadrícula (grid) con `display: grid`, establecer los tamaños de columna y fila con `grid-template-columns` y `grid-template-rows`, y luego colocar sus elementos hijos en la cuadrícula (grid) con `grid-column` y `grid-row`. De manera similar a flexbox, el orden de origen de los elementos de la cuadrícula no importa. Su CSS puede colocarlos en cualquier orden, lo que hace que sea muy fácil reorganizar su cuadrícula (grid) con media queries. Imagine definir el diseño de toda la página y luego reorganizarlo por completo para adaptarse a un ancho de pantalla diferente, todo con solo un par de líneas de CSS. Grid es uno de los módulos CSS más poderosos jamás presentados.

A partir de marzo de 2017, la mayoría de los navegadores proporcionaron soporte nativo, sin prefijo para CSS Grid: Chrome (incluido en Android), Firefox, Safari (incluido en iOS) y Opera. Internet Explorer 10 y 11, por otro lado, lo admiten, pero es una implementación antigua con una sintaxis obsoleta. El tiempo para construir con grid es ahora!

Esta información de soporte del navegador es de [Caniuse](http://caniuse.com/#feat=css-grid) , que tiene más detalles. Un número indica que el navegador admite la función en esa versión y más.

#### Escritorio

| Chrome | Opera | Firefox | IE   | Edge      | Safari |
| :----- | :---- | :------ | :--- | :-------- | :----- |
| 57     | 44    | 52      | 11 * | dieciséis | 10.1   |

#### Móvil / Tablet

| iOS Safari | Opera Mobile | Opera Mini | Android | Android Chrome | Android Firefox |
| :--------- | :----------- | :--------- | :------ | :------------- | :-------------- |
| 10.3       | 46           | No         | 67      | 74             | 66              |

### [Terminología importante](https://css-tricks.com/snippets/css/complete-guide-grid/#grid-terminology)

Antes de sumergirse en los conceptos de Grid, es importante entender la terminología. Ya que los términos involucrados aquí son todos conceptualmente similares, es fácil confundirlos entre sí, si no memoriza primero sus significados definidos por la especificación Grid. Pero no te preocupes, no hay muchos de ellos.

#### Grid container

El elemento sobre el que `display: grid` se aplica. Es el padre directo de todos los elementos de la cuadrícula (grid items). En este ejemplo `container`  es el grid container.

```html
<div class="container">
  <div class="item item-1"></div>
  <div class="item item-2"></div>
  <div class="item item-3"></div>
</div>
```

#### Grid Item

Los hijos (es decir, descendientes *directos* ) del grid container. Aquí los  elementos `item`  son elementos de la cuadrícula (grid), pero `sub-item` no lo es.

```html
<div class="container">
  <div class="item"></div> 
  <div class="item">
  	<p class="sub-item"></p> <!-- sub-item es hijo de item osea container es su abuelo-->
  </div>
  <div class="item"></div>
</div>
```

#### Grid line

Las líneas divisorias que conforman la estructura de la cuadrícula (grid). Pueden ser verticales ("grid lines en columna") u horizontales ("grid lines en fila") y residir en cualquier lado de una fila o columna. Aquí la línea amarilla es un ejemplo de una grid line de columna.

![img](https://css-tricks.com/wp-content/uploads/2018/11/terms-grid-line.svg)

#### Grid Track

El espacio entre dos grid lines adyacentes. Puedes pensar en ellos como las columnas o filas de la grid. Aquí está el grid Track entre las grid lines de la segunda y tercera fila.

![img](https://css-tricks.com/wp-content/uploads/2018/11/terms-grid-track.svg)

#### Grid Cell

El espacio entre dos filas adyacentes y dos grid lines de columnas adyacentes. Es una sola "unidad" de el grid. Aquí está la celda de el grid entre las grid lines de fila 1 y 2, y las grid lines de columna 2 y 3.

![img](https://css-tricks.com/wp-content/uploads/2018/11/terms-grid-cell.svg)

#### Grid Area

El espacio total rodeado de cuatro grid lines. Un área de cuadrícula puede estar compuesta por cualquier número de celdas de cuadrícula (grid cell). Aquí está el área de cuadrícula (grid area) entre las grid lines de fila 1 y 3, y las grid lines de columna 1 y 3.

![img](https://css-tricks.com/wp-content/uploads/2018/11/terms-grid-area.svg)

### [Propiedades de la cuadrícula Tabla de contenidos](https://css-tricks.com/snippets/css/complete-guide-grid/#grid-table-of-contents)

Propiedades para el Grid container

- [display](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-display)
- [grid-template-columns](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-grid-template-columns-rows)
- [grid-template-rows](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-grid-template-columns-rows)
- [grid-template-areas](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-grid-template-areas)
- [grid-template](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-grid-template)
- [grid-colum-gap](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-grid-column-row-gap)
- [grid-row-gap](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-grid-column-row-gap)
- [grid-gap](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-grid-gap)
- [justify-items](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-justify-items)
- [align-items](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-align-items)
- [place-items](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-place-items)
- [justify-content](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-justify-content)
- [align-content](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-align-content)
- [place-content](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-place-content)
- [grid-auto-columns](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-grid-auto-columns-rows)
- [grid-auto-rows](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-grid-auto-columns-rows)
- [grid-auto-flow](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-grid-auto-flow)
- [grid](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-grid)

Propiedades de los Grid Items

- [grid-column-start](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-grid-column-row-start-end)
- [grid-column-end](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-grid-column-row-start-end)
- [grid-row-start](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-grid-column-row-start-end)
- [grid-row-end](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-grid-column-row-start-end)
- [grid-column](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-grid-column-row)
- [grid-row](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-grid-column-row)
- [grid-area](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-grid-area)
- [justify-self](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-justify-self)
- [align-self](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-align-self)
- [place-self](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-place-self)

## Propiedades para el padre  (Grid Container)

#### Display

Define el elemento como un grid container y establece un nuevo *contexto de formato de grid* para su contenido.

Valores:

- **grid** - genera una grid a nivel de bloque block-level
- **inline-grid** - genera una grid de nivel en línea inline-level

```css
.container {
  display: grid | inline-grid;
}
```

Nota: La capacidad de pasar los parámetros de la grid a través de elementos anidados (también conocidos como subgrids) se ha movido al [nivel 2 de la especificación de la cuadrícula de CSS. ](https://www.w3.org/TR/css-grid-2/#subgrids)Aquí hay [una explicación rápida](https://css-tricks.com/grid-level-2-and-subgrid/) .

#### grid-template-columns  grid-template-rows

Define las columnas y filas de la grid con una lista de valores separados por espacios. Los valores representan el track-size y el espacio entre ellos representa la línea de la cuadrícula (grid line).

Valores:

- **<track-size>** : puede ser una longitud, un porcentaje o una fracción del espacio libre en la grid (usando la unidad `fr` )
- **<line-name>** - un nombre arbitrario de su elección

```css
.container {
  grid-template-columns: <track-size> ... | <line-name> <track-size> ...;
  grid-template-rows: <track-size> ... | <line-name> <track-size> ...;
}
```

Ejemplos:

Cuando deja un espacio vacío entre los valores de track, las líneas de la cuadrícula(grid lines) se les asignan automáticamente números positivos y negativos:

```css
.container {
  grid-template-columns: 40px 50px auto 50px 40px;
  grid-template-rows: 25% 100px auto;
}
```

![img](https://css-tricks.com/wp-content/uploads/2018/11/template-columns-rows-01.svg)

Pero puedes elegir nombrar explícitamente las líneas. Note la sintaxis del corchete para los nombres de línea:

```css
.container {
  grid-template-columns: [first] 40px [line2] 50px [line3] auto [col4-start] 50px [five] 40px [end];
  grid-template-rows: [row1-start] 25% [row1-end] 100px [third-line] auto [last-line];
}
```

![Cuadrícula con líneas nombradas por el usuario](https://css-tricks.com/wp-content/uploads/2018/11/template-column-rows-02.svg)

Tenga en cuenta que una línea puede tener más de un nombre. Por ejemplo, aquí la segunda línea tendrá dos nombres: row1-end y row2-start:

```css
.container {
  grid-template-rows: [row1-start] 25% [row1-end row2-start] 25% [row2-end];
}
```

Si su definición contiene partes que se repiten, puede usar la notación `repeat()` para simplificar las cosas:

```css
.container {
  grid-template-columns: repeat(3, 20px [col-start]);
}
```

Lo que es equivalente a esto:

```css
.container {
  grid-template-columns: 20px [col-start] 20px [col-start] 20px [col-start];
}
```

Si varias líneas comparten el mismo nombre, se puede hacer referencia a ellas por su nombre y número de línea.

```css
.item {
  grid-column-start: col-start 2;
}
```

La unidad `fr` le permite establecer el tamaño de una pista (track) como una fracción del espacio libre del grid container. Por ejemplo, esto establecerá cada elemento en un tercio del ancho del contenedor de la cuadrícula:

```css
.container {
  grid-template-columns: 1fr 1fr 1fr;
}
```

El espacio libre se calcula *después de* los elementos no flexibles. En este ejemplo, la cantidad total de espacio libre disponible para las unidades  `fr` no incluye los 50px:

```css
.container {
  grid-template-columns: 1fr 50px 1fr 1fr;
}
```

#### grid-template-areas

Define una grid template haciendo referencia a los nombres de las áreas de el grid que se especifican con la propiedad `grid-area`. La repetición del nombre de un grid area hace que el contenido abarque esas celdas. Un punto (.) significa una celda vacía. La propia sintaxis proporciona una visualización de la estructura del grid.

Valores:

- **<grid-area-name>** : el nombre de un área de el grid especificada con `grid-area`
- **.** - un punto significa una celda de grid está vacía
- **none** - no se definen áreas de la grid

```css
.container {
  grid-template-areas: 
    "<grid-area-name> | . | none | ..."
    "...";
}
```

Ejemplo:

```css
.item-a {
  grid-area: header;
}
.item-b {
  grid-area: main;
}
.item-c {
  grid-area: sidebar;
}
.item-d {
  grid-area: footer;
}

.container {
  display: grid;
  grid-template-columns: 50px 50px 50px 50px;
  grid-template-rows: auto;
  grid-template-areas: 
    "header header header header"
    "main main . sidebar"
    "footer footer footer footer";
}
```

Eso creará una grid de cuatro columnas de ancho por tres filas de alto. La fila superior completa estará compuesta por el área del **header** . La fila central estará compuesta de dos áreas **principales** , una celda vacía y un área de **sidebar**. La última fila es todo el **pie de página** .

![Ejemplo de cuadrícula-áreas-plantilla](https://css-tricks.com/wp-content/uploads/2018/11/dddgrid-template-areas.svg)

Cada fila en su declaración debe tener el mismo número de celdas.

Puede utilizar cualquier número de puntos (.) adyacentes para declarar una sola celda vacía. Mientras los puntos (.) no tengan espacios entre ellos, representan una sola celda.

Observe que no está nombrando líneas con esta sintaxis, solo áreas. Cuando usas esta sintaxis, las líneas en cualquiera de los extremos de las áreas se están nombrando automáticamente. Si el nombre de su grid area es **foo** , el nombre de la línea de la fila de inicio del área y la línea de la columna de inicio será **foo -start** , y el nombre de su última línea de fila y la última línea de la columna será **foo-end** . Esto significa que algunas líneas pueden tener varios nombres, como la línea del extremo izquierdo en el ejemplo anterior, que tendrá tres nombres: header-start, main-start y footer-start.

#### grid-template

Un shorthand para establecer `grid-template-rows`, `grid-template-columns` y `grid-template-areas` en una sola declaración.

Valores:

- **none** - establece las tres propiedades a sus valores iniciales
- **<grid-template-rows> / <grid-template-columns>** - establece `grid-template-columns` y `grid-template-rows` a los valores especificados, respectivamente, y establece `grid-template-areas` en `none`

```css
.container {
  grid-template: none | <grid-template-rows> / <grid-template-columns>;
}
```

También acepta una sintaxis más compleja pero bastante práctica para especificar los tres. Aquí hay un ejemplo:

```css
.container {
  grid-template:
    [row1-start] "header header header" 25px [row1-end]
    [row2-start] "footer footer footer" 25px [row2-end]
    / auto 50px auto;
}
```

Eso es equivalente a esto:

```css
.container {
  grid-template-rows: [row1-start] 25px [row1-end row2-start] 25px [row2-end];
  grid-template-columns: auto 50px auto;
  grid-template-areas: 
    "header header header" 
    "footer footer footer";
}
```

Dado que `grid-template` no restablece las propiedades *implícitas* de la grid ( `grid-auto-columns`, `grid-auto-rows` y `grid-auto-flow`), que es probablemente lo que desea hacer en la mayoría de los casos, se recomienda usar la propiedad `grid` en lugar de `grid-template`.

#### grid-column-gap  grid-row-gap

Especifica el tamaño de las líneas de la cuadrícula. Puede pensarlo como establecer el ancho de los canales entre las columnas/filas.

Valores:

- **<line-size>** - un valor de longitud

```css
.container {
  grid-column-gap: <line-size>;
  grid-row-gap: <line-size>;
}
```

Ejemplo:

```css
.container {
  grid-template-columns: 100px 50px 100px;
  grid-template-rows: 80px auto 80px; 
  grid-column-gap: 10px;
  grid-row-gap: 15px;
}
```

![Ejemplo de grilla-columna-brecha y rejilla-fila-brecha](https://css-tricks.com/wp-content/uploads/2018/11/dddgrid-gap.svg)

Los canales solo se crean *entre* las columnas/filas, no en los bordes exteriores.

Nota: el prefijo `grid-` se eliminará y su nombre `grid-column-gap` y `grid-row-gap` cambiará a `column-gap` y `row-gap`. Las propiedades sin prefijo ya son compatibles con Chrome 68+, Safari 11.2 Release 50+ y Opera 54+.

#### grid-gap

Un shothand para `grid-row-gap` y `grid-column-gap`

Valores:

- **<grid-row-gap> <grid-column-gap>** - valores de longitud

```css
.container {
  grid-gap: <grid-row-gap> <grid-column-gap>;
}
```

Ejemplo:

```css
.container {
  grid-template-columns: 100px 50px 100px;
  grid-template-rows: 80px auto 80px; 
  grid-gap: 15px 10px;
}
```

Si no se especifica ningún `grid-row-gap` , se establece en el mismo valor que `grid-column-gap`

Nota: El prefijo `grid-` se eliminará y se cambiará su nombre  `grid-gap` a `gap`. La propiedad sin prefijo ya es compatible con Chrome 68+, Safari 11.2 Release 50+ y Opera 54+.

#### justify-items

Alinea los elementos del grid a lo largo del eje en *línea (fila)* (en oposición a `align-items` el cual se alinea a lo largo del eje de *bloque (columna)* ). Este valor se aplica a todos los elementos del grid dentro del contenedor.

Valores:

- **start** : alinea los elementos para estar al ras con el borde inicial de su celda.
- **end** : alinea los elementos para que queden al ras con el borde final de su celda
- **centro** - alinea elementos en el centro de su celda
- **stretch** : rellena todo el ancho de la celda (este es el valor predeterminado)

```css
.container {
  justify-items: start | end | center | stretch;
}
```

Ejemplos:

```css
.container {
  justify-items: start;
}
```

![Ejemplo de elementos de justificación establecidos para iniciar](https://css-tricks.com/wp-content/uploads/2018/11/justify-items-start.svg)

```css
.container {
  justify-items: end;
}
```

![Ejemplo de elementos de justificación establecidos para finalizar](https://css-tricks.com/wp-content/uploads/2018/11/justify-items-end.svg)

```css
.container {
  justify-items: center;
}
```

![Ejemplo de elementos de justificación establecidos para centrar](https://css-tricks.com/wp-content/uploads/2018/11/justify-items-center.svg)

```css
.container {
  justify-items: stretch;
}
```

![Ejemplo de elementos de justificación establecidos para estirar](https://css-tricks.com/wp-content/uploads/2018/11/justify-items-stretch.svg)

Este comportamiento también se puede establecer en elementos individuales del grid a través de la propiedad `justify-self`.

#### align-items

Alinea los elementos de la cuadrícula a lo largo del eje del *bloque (columna)* (en oposición a `justify-items` que se alinea a lo largo del eje en *línea (fila)* ). Este valor se aplica a todos los elementos del grid dentro del contenedor.

Valores:

- **start** : alinea los elementos para estar al ras con el borde inicial de su celda.
- **end** : alinea los elementos para que queden estar al ras con el borde final de su celda
- **center** - alinea elementos en el centro de su celda
- **stretch** : rellena toda la altura de la celda (este es el valor predeterminado)

```css
.container {
  align-items: start | end | center | stretch;
}
```

Ejemplos:

```css
.container {
  align-items: start;
}
```

![Ejemplo de conjunto de elementos de alineación para iniciar](https://css-tricks.com/wp-content/uploads/2018/11/align-items-start.svg)

```css
.container {
  align-items: end;
}
```

![Ejemplo de elementos de alineación establecidos para finalizar](https://css-tricks.com/wp-content/uploads/2018/11/align-items-end.svg)

```css
.container {
  align-items: center;
}
```

![Ejemplo de elementos de alineación establecidos para centrar](https://css-tricks.com/wp-content/uploads/2018/11/align-items-center.svg)

```css
.container {
  align-items: stretch;
}
```

![Ejemplo de alineación de elementos establecidos para estirar](https://css-tricks.com/wp-content/uploads/2018/11/align-items-stretch.svg)

Este comportamiento también se puede establecer en elementos individuales del grid a través de la propiedad  `align-self` .

#### place-items

`place-items` establece las propiedades `align-items` y `justify-items` en una sola declaración.

Valores:

- **<align-items> / <justify-items>** - El primer valor establece `align-items`, el segundo valor `justify-items`. Si se omite el segundo valor, el primer valor se asigna a ambas propiedades.

Todos los principales navegadores, excepto Edge, admiten la propiedad shorthand `place-items`.

Para más detalles, vea [align-items](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-align-items) y [justify-items](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-justify-items) .

#### justify-content

A veces, el tamaño total de su grid puede ser menor que el tamaño de su contenedor grid. Esto podría suceder si todos los elementos de su grid se dimensionan con unidades no flexibles como `px`. En este caso, puede establecer la alineación del grid dentro del contenedor del grid. Esta propiedad alinea el grid a lo largo del eje de *línea (fila)* (a diferencia de lo que hace `align-content` que alinea la cuadrícula a lo largo del eje de *bloque (columna)* ).

Valores:

- **start** : alinea la grid para que quede al ras con el borde inicial del contenedor de grid
- **end** : alinea la grid para que quede al ras con el borde del extremo del contenedor de la grid
- **center** : alinea la grid en el centro del contenedor de cuadrícula
- **stretch** : cambia el tamaño de los elementos de la grid para permitir que la grid llene todo el ancho del contenedor de la grid.
- **space-around** : coloca una cantidad uniforme de espacio entre cada elemento de la grid, con espacios de tamaño medio en los extremos.
- **space-beetween** : coloca una cantidad uniforme de espacio entre cada elemento de la cuadrícula, sin espacio en los extremos.
- **space-evenly** : coloca una cantidad uniforme de espacio entre cada elemento de la cuadrícula, incluidos los extremos.

```css
.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;	
}
```

Ejemplos:

```css
.container {
  justify-content: start;
}
```

![Ejemplo de conjunto de justificación de contenido para iniciar](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-start.svg)

```css
.container {
  justify-content: end;	
}
```

![Ejemplo de conjunto de justificación de contenido para finalizar](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-end.svg)

```css
.container {
  justify-content: center;	
}
```

![Ejemplo de justify-content set to center](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-center.svg)

```css
.container {
  justify-content: stretch;	
}
```

![Ejemplo de conjunto de justificación de contenido para estirar](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-stretch.svg)

```css
.container {
  justify-content: space-around;	
}
```

![Ejemplo de justify-content set to space-around](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-space-around.svg)

```css
.container {
  justify-content: space-between;	
}
```

![Ejemplo de justify-content set to space-between](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-space-between.svg)

```css
.container {
  justify-content: space-evenly;	
}
```

![Ejemplo de justify-content set to space-uniformly](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-space-evenly.svg)

#### align-content

A veces, el tamaño total de su grid puede ser menor que el tamaño de su contenedor de grid. Esto podría suceder si todos los elementos de su grid se dimensionan con unidades no flexibles como `px`. En este caso, puede establecer la alineación de la grid dentro del contenedor de grid. Esta propiedad alinea la cuadrícula a lo largo del eje de *bloque (columna)* (a diferencia de lo que hace `justify-content` que alinea la cuadrícula a lo largo del eje en *línea (fila)* ).

Valores:

- **start** : alinea la cuadrícula para que quede al ras con el borde inicial del contenedor del grid
- **end** : alinea la cuadrícula para que quede al ras con el borde del extremo del contenedor del grid
- **center** : alinea la cuadrícula en el centro del contenedor de cuadrícula.
- **stretch** : cambia el tamaño de los elementos de la cuadrícula para permitir que la cuadrícula llene toda la altura del contenedor de la cuadrícula.
- **space-around** : coloca una cantidad uniforme de espacio entre cada elemento de la cuadrícula, con espacios de tamaño medio en los extremos
- **space-between** : coloca una cantidad uniforme de espacio entre cada elemento de la cuadrícula, sin espacio en los extremos.
- **espacio evenly** : coloca una cantidad uniforme de espacio entre cada elemento de la cuadrícula, incluidos los extremos

```css
.container {
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;	
}
```

Ejemplos:

```css
.container {
  align-content: start;	
}
```

![Ejemplo de conjunto de contenido de alineación para iniciar](https://css-tricks.com/wp-content/uploads/2018/11/align-content-start.svg)

```css
.container {
  align-content: end;	
}
```

![Ejemplo de conjunto de contenido de alineación para finalizar](https://css-tricks.com/wp-content/uploads/2018/11/align-content-end.svg)

```css
.container {
  align-content: center;	
}
```

![Ejemplo de alinear el contenido en el centro](https://css-tricks.com/wp-content/uploads/2018/11/align-content-center.svg)

```css
.container {
  align-content: stretch;	
}
```

![Ejemplo de alineación de contenido establecido para estirar](https://css-tricks.com/wp-content/uploads/2018/11/align-content-stretch.svg)

```css
.container {
  align-content: space-around;	
}
```

![Ejemplo de alineación de contenido para espacio-alrededor](https://css-tricks.com/wp-content/uploads/2018/11/align-content-space-around.svg)

```css
.container {
  align-content: space-between;	
}
```

![Ejemplo de alineación de contenido en espacio entre](https://css-tricks.com/wp-content/uploads/2018/11/align-content-space-between.svg)

```css
.container {
  align-content: space-evenly;	
}
```

![Ejemplo de alineación de contenido establecido en el espacio de manera uniforme](https://css-tricks.com/wp-content/uploads/2018/11/align-content-space-evenly.svg)

#### place-content

`place-content` establece las propiedades `align-content` y `justify-content` en una sola declaración.

Valores:

- **<align-content> / <justify-content>** - El primer valor establece `align-content`, el segundo valor `justify-content`. Si se omite el segundo valor, el primer valor se asigna a ambas propiedades.

Todos los principales navegadores, excepto Edge, admiten la propiedad shorthand `place-content`.

Para más detalles, vea [align-content](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-align-content) y [justify-content](https://css-tricks.com/snippets/css/complete-guide-grid/#prop-justify-content) .

#### grid-auto-columns  grid-auto-rows

Especifica el tamaño los grid tracks (camino o pistas) de cuadrícula generadas automáticamente (también conocidas como *tracks de cuadrícula implícitas* ). Los tracks implícitos se crean cuando hay más elementos de cuadrícula (grid items) que celdas en la cuadrícula o cuando un elemento de cuadrícula (grid item) se coloca fuera de la cuadrícula (grid) explícita. (ver [La diferencia entre cuadrículas explícitas e implícitas](https://css-tricks.com/difference-explicit-implicit-grids/) )

Valores:

- **<track-size>** : puede ser una longitud, un porcentaje o una fracción del espacio libre en la cuadrícula (usando la unidad `fr`)

```css
.container {
  grid-auto-columns: <track-size> ...;
  grid-auto-rows: <track-size> ...;
}
```

Para ilustrar cómo se crean las grid track implícitas, piense en esto:

```css
.container {
  grid-template-columns: 60px 60px;
  grid-template-rows: 90px 90px
}
```

![Ejemplo de cuadrícula 2x2.](https://css-tricks.com/wp-content/uploads/2018/11/grid-auto-columns-rows-01.svg)

Esto crea una cuadrícula de 2 x 2.

Pero ahora imagina que usas `grid-column` y `grid-row` posicionas tus grid items de esta manera:

```css
.item-a {
  grid-column: 1 / 2;
  grid-row: 2 / 3;
}
.item-b {
  grid-column: 5 / 6;
  grid-row: 2 / 3;
}
```

![Ejemplo de pistas implícitas.](https://css-tricks.com/wp-content/uploads/2018/11/grid-auto-columns-rows-02.svg)

Le dijimos a .item-b que comenzara en la línea 5 de la columna y terminara en la línea 6 de la columna, *pero nunca definimos una línea 5 o 6 de la columna* . Debido a que hicimos referencia a las líneas que no existen, las pistas (grid tracks) implícitas con anchos de 0 se crean para llenar los huecos (gaps). Podemos usar `grid-auto-columns` y `grid-auto-rows` para especificar los anchos de estas pistas (tracks) implícitas:

```css
.container {
  grid-auto-columns: 60px;
}
```

![cuadrícula-auto-columnas-filas](https://css-tricks.com/wp-content/uploads/2018/11/grid-auto-columns-rows-03.svg)

#### grid-auto-flow

Si tiene elementos de cuadrícula que no coloca explícitamente en la cuadrícula, el *algoritmo de colocación* automática se activa para colocar automáticamente los elementos. Esta propiedad controla cómo funciona el algoritmo de colocación automática auto-placement.

Valores:

- **row** : le dice al algoritmo de ubicación automática que complete cada fila sucesivamente, agregando nuevas filas según sea necesario (predeterminado)
- **column** : le dice al algoritmo de ubicación automática que complete cada columna sucesivamente, agregando nuevas columnas según sea necesario
- **dense** : le dice al algoritmo de colocación automática que intente rellenar los orificios antes en la cuadrícula si los elementos más pequeños aparecen más adelante

```css
.container {
  grid-auto-flow: row | column | row dense | column dense
}
```

Tenga en cuenta que **Dense** solo cambia el orden visual de sus elementos y puede hacer que aparezcan fuera de orden, lo que es malo para la accesibilidad.

Ejemplos:

Considera este HTML:

```html
<section class="container">
  <div class="item-a">item-a</div>
  <div class="item-b">item-b</div>
  <div class="item-c">item-c</div>
  <div class="item-d">item-d</div>
  <div class="item-e">item-e</div>
</section>
```

Usted define una cuadrícula con cinco columnas y dos filas, y establece `grid-auto-flow` en `row` (que también es el predeterminado):

```css
.container {
  display: grid;
  grid-template-columns: 60px 60px 60px 60px 60px;
  grid-template-rows: 30px 30px;
  grid-auto-flow: row;
}
```

Al colocar los elementos en la cuadrícula, solo debe especificar puntos para dos de ellos:

```css
.item-a {
  grid-column: 1;
  grid-row: 1 / 3;
}
.item-e {
  grid-column: 5;
  grid-row: 1 / 3;
}
```

Debido a que configuramos `grid-auto-flow` a `row`, nuestra cuadrícula se verá así. Observe cómo los tres elementos que no colocamos ( **item-b** , **item-c** y **item-d** ) fluyen a través de las filas disponibles:

![Ejemplo de grid-auto-flow establecido en fila](https://css-tricks.com/wp-content/uploads/2018/11/grid-auto-flow-01.svg)

Si, por el contrario, establecemos `grid-auto-flow` en `column`, **item-b** , **item-c** y **item-d** fluyen por las columnas:

```css
.container {
  display: grid;
  grid-template-columns: 60px 60px 60px 60px 60px;
  grid-template-rows: 30px 30px;
  grid-auto-flow: column;
}
```

![Ejemplo de grid-auto-flow configurado a columna](https://css-tricks.com/wp-content/uploads/2018/11/grid-auto-flow-02.svg)

#### grid

Una forma rápida (shorthand) para establecer todas las siguientes propiedades en una sola declaración: `grid-template-rows`, `grid-template-columns`, `grid-template-areas`, `grid-auto-rows`, `grid-auto-columns`, y `grid-auto-flow` (Nota: Sólo se puede especificar las propiedades de los grids explícitos o implícitos en una sola declaración de la grid).

Valores:

- **none** : establece todas las sub-propiedades en sus valores iniciales.
- **<grid-template>** - funciona igual que el shorthand `grid-template`.
- **<grid-template-rows> / [auto-flow && dense? ] <grid-auto-columns>?** - Establece `grid-template-rows` en el valor especificado. Si la keyword `auto-flow` está a la derecha de la barra diagonal, se establece `grid-auto-flow` en `column`. Si la palabra clave `dense` se especifica adicionalmente, el algoritmo de colocación automática utiliza un algoritmo empaquetamiento "dense". Si `grid-auto-columns` se omite, se establece en `auto`.
- **[auto-flow && dense? ] <grid-auto-rows>? / <grid-template-columns>** : establece `grid-template-columns` al valor especificado. Si la keywork `auto-flow` está a la izquierda de la barra (slash), se establece `grid-auto-flow` en `row`. Si la keyword `dense` se especifica adicionalmente, el algoritmo de colocación automática utiliza un algoritmo de empaquetamiento "dense". Si `grid-auto-rows` se omite, se establece en `auto`.

Ejemplos:

Los siguientes dos bloques de código son equivalentes:

```css
.container {
    grid: 100px 300px / 3fr 1fr;
  }
.container {
    grid-template-rows: 100px 300px;
    grid-template-columns: 3fr 1fr;
  }
```

Los siguientes dos bloques de código son equivalentes:

```css
.container {
    grid: auto-flow / 200px 1fr;
  }
.container {
    grid-auto-flow: row;
    grid-template-columns: 200px 1fr;
  }
```

Los siguientes dos bloques de código son equivalentes:

```css
.container {
    grid: auto-flow dense 100px / 1fr 2fr;
  }
.container {
    grid-auto-flow: row dense;
    grid-auto-rows: 100px;
    grid-template-columns: 1fr 2fr;
  }
```

Y los siguientes dos bloques de código son equivalentes:

```css
.container {
    grid: 100px 300px / auto-flow 200px;
  }
.container {
    grid-template-rows: 100px 300px;
    grid-auto-flow: column;
    grid-auto-columns: 200px;
  }
```

También acepta una sintaxis más compleja pero bastante práctica para configurar todo a la vez. Usted especifica `grid-template-areas`, `grid-template-rows` y `grid-template-columns`, y todas las demás sub-propiedades se establecen en sus valores iniciales. Lo que está haciendo es especificar los nombres de línea y los tamaños de pista (track) en línea con sus respectivas áreas de cuadrícula (grid areas). Esto es más fácil de describir con un ejemplo:

```css
.container {
    grid: [row1-start] "header header header" 1fr [row1-end]
          [row2-start] "footer footer footer" 25px [row2-end]
          / auto 50px auto;
  }
```

Eso es equivalente a esto:

```css
.container {
    grid-template-areas: 
      "header header header"
      "footer footer footer";
    grid-template-rows: [row1-start] 1fr [row1-end row2-start] 25px [row2-end];
    grid-template-columns: auto 50px auto;    
  }
```

## Propiedades para los niños  (elementos de cuadrícula)

**Nota:**
`float` , `display: inline-block`, `display: table-cell`, `vertical-align` y `column-*`  son propiedades no tienen ningún efecto sobre un elemento de la cuadrícula (grid item).

#### grid-column-start  grid-column-end  grid-row-start  grid-row-end

Determina la ubicación de un grid item dentro de la cuadrícula haciendo referencia a líneas de cuadrícula específicas. `grid-column-start`/ `grid-row-start` es la línea donde comienza el elemento y `grid-column-end`/ `grid-row-end` es la línea donde termina el elemento.

Valores:

- **<line>** : puede ser un número para referirse a una línea de cuadrícula numerada, o un nombre para referirse a una línea de cuadrícula nombrada.
- **span <number>** : el elemento abarcará el número proporcionado de pistas de cuadrícula (grid tracks)
- **span <name>** : el elemento se extenderá hasta que llegue a la siguiente línea con el nombre provisto
- **auto** : indica la ubicación automática auto-placement, un intervalo (span) automático o un intervalo (span) predeterminado de uno

```css
.item {
  grid-column-start: <number> | <name> | span <number> | span <name> | auto;
  grid-column-end: <number> | <name> | span <number> | span <name> | auto;
  grid-row-start: <number> | <name> | span <number> | span <name> | auto;
  grid-row-end: <number> | <name> | span <number> | span <name> | auto;
}
```

Ejemplos:

```css
.item-a {
  grid-column-start: 2;
  grid-column-end: five;
  grid-row-start: row1-start
  grid-row-end: 3;
}
```

![Ejemplo de fila-fila / columna-inicio / final](https://css-tricks.com/wp-content/uploads/2018/11/grid-column-row-start-end-01.svg)

```css
.item-b {
  grid-column-start: 1;
  grid-column-end: span col4-start;
  grid-row-start: 2
  grid-row-end: span 2
}
```

![Ejemplo de fila-fila / columna-inicio / final](https://css-tricks.com/wp-content/uploads/2018/11/grid-column-row-start-end-02.svg)

Si no se declara `grid-column-end`/ `grid-row-end`, el item abarcará (span) 1 track de forma predeterminada.

Los elementos pueden superponerse entre sí. Puede utilizar `z-index` para controlar su orden de apilamiento.

#### grid-column  grid-row

Shorthand para `grid-column-start` + `grid-column-end`, y `grid-row-start` + `grid-row-end`, respectivamente.

Valores:

- **<start-line> / <end-line>** - cada uno acepta todos los mismos valores que la versión longhand, incluido el intervalo (span)

```css
.item {
  grid-column: <start-line> / <end-line> | <start-line> / span <value>;
  grid-row: <start-line> / <end-line> | <start-line> / span <value>;
}
```

Ejemplo:

```css
.item-c {
  grid-column: 3 / span 2;
  grid-row: third-line / 4;
}
```

![Ejemplo de rejilla-columna / rejilla-fila](https://css-tricks.com/wp-content/uploads/2018/11/grid-column-row.svg)

Si no se declara ningún valor de la línea final, el elemento abarcará 1 (track) de forma predeterminada.

#### grid-area

Da a un elemento un nombre para que pueda ser referenciado por una plantilla (templete) creada con la propiedad `grid-template-areas`. Alternativamente, esta propiedad puede usarse como una abreviatura aún más corta para `grid-row-start` + `grid-column-start` + `grid-row-end` + `grid-column-end`.

Valores:

- **<name>** - un nombre de su elección.
- **<row-start> / <column-start> / <row-end> / <column-end>** - pueden ser números o líneas con nombre

```css
.item {
  grid-area: <name> | <row-start> / <column-start> / <row-end> / <column-end>;
}
```

Ejemplos:

Como una forma de asignar un nombre a un item:

```css
.item-d {
  grid-area: header;
}
```

Como la shorthand de `grid-row-start` + `grid-column-start` + `grid-row-end` + `grid-column-end`:

```css
.item-d {
  grid-area: 1 / col4-start / last-line / 6;
}
```

![Ejemplo de área de cuadrícula](https://css-tricks.com/wp-content/uploads/2018/11/grid-area.svg)

#### justify-self

Alinea un elemento de cuadrícula (grid item) dentro de una celda a lo largo del eje de *línea (fila)* (a diferencia de lo `align-self` que se alinea a lo largo del eje de *bloque (columna)* ). Este valor se aplica a un elemento de la cuadrícula dentro de una sola celda.

Valores:

- **start** : alinea el elemento de la cuadrícula para  quedar a ras con el borde inicial de la celda.
- **end** : alinea el elemento de la cuadrícula para que quede al ras con el borde final de la celda
- **center** : alinea el elemento de la cuadrícula en el centro de la celda
- **stretch** : rellena todo el ancho de la celda (este es el valor predeterminado)

```css
.item {
  justify-self: start | end | center | stretch;
}
```

Ejemplos:

```css
.item-a {
  justify-self: start;
}
```

![Ejemplo de justify-self set to start](https://css-tricks.com/wp-content/uploads/2018/11/justify-self-start.svg)

```css
.item-a {
  justify-self: end;
}
```

![alt = "Ejemplo](https://css-tricks.com/wp-content/uploads/2018/11/justify-self-end.svg)

```css
.item-a {
  justify-self: center;
}
```

![Ejemplo de auto-justificarse en el centro.](https://css-tricks.com/wp-content/uploads/2018/11/justify-self-center.svg)

```css
.item-a {
  justify-self: stretch;
}
```

![Ejemplo de auto-justificación configurada para estirar.](https://css-tricks.com/wp-content/uploads/2018/11/justify-self-stretch.svg)

Para establecer la alineación de *todos* los elementos en una cuadrícula grid, este comportamiento también se puede establecer en el contenedor de cuadrícula (grid) a través de la propiedad `justify-items`.

#### align-self

Alinea un elemento de la cuadrícula dentro de una celda a lo largo del eje del *bloque (columna)* (a diferencia del `justify-self` que se alinea a lo largo del eje de *línea (fila)*). Este valor se aplica al contenido dentro de un solo elemento de cuadrícula.

Valores:

- **start** : alinea el elemento de la cuadrícula para  que quede a ras con el borde inicial de la celda.
- **end** : alinea el elemento de la cuadrícula para que quede al ras con el borde final de la celda
- **center** : alinea el elemento de la cuadrícula en el centro de la celda
- **stretch** : rellena toda la altura de la celda (este es el valor predeterminado)

```css
.item {
  align-self: start | end | center | stretch;
}
```

Ejemplos:

```css
.item-a {
  align-self: start;
}
```

![Ejemplo de alineación auto configurada para comenzar](https://css-tricks.com/wp-content/uploads/2018/11/align-self-start.svg)

```css
.item-a {
  align-self: end;
}
```

![Ejemplo de alineación-self configurado para finalizar](https://css-tricks.com/wp-content/uploads/2018/11/align-self-end.svg)

```css
.item-a {
  align-self: center;
}
```

![Ejemplo de alineación auto ajustada al centro](https://css-tricks.com/wp-content/uploads/2018/11/align-self-center.svg)

```css
.item-a {
  align-self: stretch;
}
```

![Ejemplo de alineación auto configurada para estirar](https://css-tricks.com/wp-content/uploads/2018/11/align-self-stretch.svg)

Para alinear *todos* los elementos en una cuadrícula, este comportamiento también se puede establecer en el contenedor de cuadrícula a través de la propiedad `align-items`.

#### place-self

`place-self` establece las propiedades `align-self` y `justify-self` en una sola declaración.

Valores:

- **auto** - La alineación "predeterminada" para el modo de diseño.
- **<align-self> / <justify-self>** - El primer valor establece `align-self`, el segundo valor `justify-self`. Si se omite el segundo valor, el primer valor se asigna a ambas propiedades.

Ejemplos:

```css
.item-a {
  place-self: center;
}
```

![Coloca el auto configurado en el centro](https://css-tricks.com/wp-content/uploads/2018/11/place-self-center.svg)

```css
.item-a {
  place-self: center stretch;
}
```

![coloque el conjunto ajustado al centro del estiramiento](https://css-tricks.com/wp-content/uploads/2018/11/place-self-center-stretch.svg)

Todos los principales navegadores, excepto Edge, admiten la propiedad shorthand `place-self` .



### Funciones especiales y palabras clave

- Al dimensionar filas y columnas, se pueden utilizar todas las [longitudes](https://css-tricks.com/the-lengths-of-css/) que se utilizan para, como `px`, rem,%, etc., pero también hay palabras clave como `min-content`, `max-content`, `auto`, y tal vez las unidades más útiles, fracciones. `grid-template-columns: 200px 1fr 2fr min-content;`
- También tiene acceso a una función que puede ayudar a establecer límites para unidades flexibles. Por ejemplo, para configurar una columna para que sea 1fr, pero no se reduzca más de 200px: `grid-template-columns: 1fr minmax(200px, 1fr);`
- Hay una función `repeat()` que guarda algo de escritura, como hacer 10 columnas: `grid-template-columns: repeat(10, 1fr);`
- Combinar todas estas cosas puede ser extremadamente poderoso, como `grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));`

<https://codepen.io/chriscoyier/pen/ecff0af160b3fd32cf67841b1eb6cfc3>

<iframe name="cp_embed_1" src="https://codepen.io/chriscoyier/embed/ecff0af160b3fd32cf67841b1eb6cfc3?height=395&amp;theme-id=1&amp;default-tab=result&amp;user=chriscoyier&amp;slug-hash=ecff0af160b3fd32cf67841b1eb6cfc3&amp;pen-title=That%20Grid%20Thing%20Everybody%20Loves%20%27Cause%20It%27s%20Awesome&amp;name=cp_embed_1" scrolling="no" frameborder="0" height="395" allowtransparency="true" allowfullscreen="true" allowpaymentrequest="true" title="Esa cosa de la rejilla que todo el mundo ama porque es increíble" class="cp_embed_iframe " id="cp_embed_ecff0af160b3fd32cf67841b1eb6cfc3" style="box-sizing: border-box; max-width: 100%; display: block; width: 1256px; overflow: hidden; height: 395px;"></iframe>



### [Animación](https://css-tricks.com/snippets/css/complete-guide-grid/#grid-animation)

De acuerdo con la especificación del Nivel 1 del Módulo CSS Grid Layout, hay 5 propiedades de cuadrícula animables:

- `grid-gap`, `grid-row-gap`, `grid-column-gap` Como la longitud, porcentaje, o calc.
- `grid-template-columns`, `grid-template-rows` como una simple lista de longitud, porcentaje o cálculo (calc), siempre que las únicas diferencias sean los valores de longitud, porcentaje o los componentes de cálculo (calc) en la lista.

#### Soporte del navegador de las propiedades de CSS Grid

A partir de hoy (7º de mayo de, 2018) solamente la animación de `(grid-)gap`, `(grid-)row-gap`, `(grid-)column-gap`se implementa en cualquiera de los navegadores probados.

| Navegador                               | `(grid-)gap`, `(grid-)row-gap`,`(grid-)column-gap` | `grid-template-columns` | `grid-template-rows` |
| :-------------------------------------- | :------------------------------------------------- | :---------------------- | :------------------- |
| Firefox                                 | apoyado✅ 53+                                       | apoyado✅ 66+            | apoyado✅ 66+         |
| Safari 12.0                             | no soportado❌                                      | no soportado❌           | no soportado❌        |
| Cromo                                   | apoyado✅ 66+                                       | no soportado❌           | no soportado❌        |
| Chrome para Android 66+, Opera Mini 33+ | apoyado✅                                           | no soportado❌           | no soportado❌        |
| Borde                                   | apoyado✅ 16+                                       | no soportado❌           | no soportado❌        |

[CSS Grid Layout: Demo de animación](https://codepen.io/matuzo/pen/rmQvMG)

**Comentarios**

Entonces, ¿cómo se puede hacer para crear colores de fondo de ancho completo en los headers y footers mientras se mantiene un ancho máximo/mínimo en el contenido (centrado)? 
Ese es un patrón de diseño muy común; ¿Me gustaría verlo reproducido con grillas?

Tal vez este artículo sea útil:<https://cloudfour.com/thinks/breaking-out-with-css-grid-layout/>

<https://codepen.io/rustyrobison/pen/MEjLYV>



Este llegó a ser uno de los mejores recursos que existen para las cuadrículas CSS, pero carece de una de las características más importantes: la función de **autocompletar** , que se utiliza para agregar automáticamente columnas al diseño de acuerdo con el ancho del contenedor.

Esto es extremadamente útil, y combinado con la `minmax()`función, proporciona una alternativa perfecta a la flexbox, incluso "arreglando" el comportamiento de la última fila en un `justify-content: space-between / space-around`contenedor flexible.

```css
.grid{
  /*sets the container as a grid with variable number of columns*/
  display:grid;
  grid-template-columns: repeat(auto-fill, minmax(150px,1fr));
}
```

Aquí hay [un ejemplo de trabajo en mi CodePen](https://codepen.io/facundocorradini/pen/XVMLEq)

<https://codepen.io/facundocorradini/pen/XVMLEq>

Y por supuesto, [un enlace a la especificación.](https://drafts.csswg.org/css-grid/#valdef-repeat-auto-fill)

¡Hola! También depende de lo que esté sucediendo en el HTML, pero mirando este ejemplo, parece que .grid es un hijo de .grid-container. Si ese es el caso, .grid ocupará las cuatro columnas completas que se definen en .grid.container. Si planea poner hijos dentro de .grid, entonces querrá establecer las propiedades de la cuadrícula allí para que los niños puedan colocarse en las pistas de la cuadrícula en consecuencia.

IE puede ser complicado. Aquí hay una serie completa que podría ayudar:

<https://css-tricks.com/css-grid-in-ie-debunking-common-ie-grid-misconceptions/>



