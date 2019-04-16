# Desarrollo de componentes

A la hora de diseñar y crear componentes, debemos tener en cuenta un montón de conceptos. Hay una serie de cosas que debemos tener en cuenta cuando pensemos en si el fragmento que estamos desarrollando debe o no ser un componente independiente.

## Reutilización

El principio clave de los componentes es la reutilización. La idea principal y punto fuerte, es el poder tener la opción de utilizarnos en el resto de nuestra web. Para ello los dotaremos con unos estilos concretos, o unas funcionalidades concretas. 

A la hora de crear un componente muy simple (componente botón) está bien que dediquemos un momento a pensar en si nos compensa realmente el hecho de crear un componente para algo como eso. Muchas veces trabajando con este tipo de arquitecturas tendemos a sobrecomponentizar y comvertir en elementos todo lo que vemos.

## Estilado

Hay mucho que decir acerca de como hacer "reutilizables" los componentes en cuestión de estilos. Angular permite el uso de varias hojas de estilo en el mismo componente, pero esto no basta para poder dotar de diferentes estilos componentes que no querramos modificar en su definición.

Una buena manera de añadir reutilización de estilos, es el uso de custom properties de CSS para permitir la sobreescritura de estilos desde hojas de estilo "padres". Por ejemplo, si sabemos que el color del borde de un elemento puede cambiar cuando lo utilizemos en distintos lugares o proyectos, podemos prepararlo de la siguiente manera.

```CSS
my-button {
    border-color: var(--my-button-color, red);
}
```

Con esta sintaxis, lo que hacemos es crear una variable en css, que tendrá por valor por defecto el introducido tras la coma, en este caso "red". El día de mañana, podemos sobreescribir este valor desde hojas de estilo superiores de la siguiente manera:

```CSS
body {
    --my-button-color: blue;
}
```
## Abstracción de el contexto

Cuando empezamos a desarrollar un componente, tendemos a pensar en el componente como "la barra de busqueda" o "el que envia un comentario". Esto es peligroso, ya que tendemos a colocar más lógica de la que debemos en nuestros componentes de presentación.

 Los componentes funcionan mejor cuando su objetivo al ser desarrollados con un objetivo funcional. Por ejemplo, en vez de hacer el componente "barra de busqueda" voy a hacer el componente "que tenga un input y permita darle a un botón para enviar ese texto". Esto nos ayudará a no dotar de más responsabilidad al componente de la que debería tener, así como pensar que opciones extras deberíamos darle al componente para poder reutilizarlo más adelante (label del botón, tipo de evento, etc...)

**WORK IN PROGRESS**

