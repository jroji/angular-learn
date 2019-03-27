# Comunicación entre componentes

## Estado de componentes

Cuando pensamos en componentes independientes, debemos trabajar con el concepto de 'estado'. Cada componente tiene un estado, unas propiedades con unos determinados valores, así como unas funciones definidas en el mismo. Podemos distinguir principalmente dos estados. Uno será el estado de la aplicación, y otro tipo, cada uno de los estados de cada componente.

Una de los principales problemas o puntos de dolor que nos encontramos a la hora de trabajar con componentes es la comunicación entre propiedades del 'estado' de un componente. Cada componente tiene su estados que se modifica en base a acciones de usuarios y demás. Estos cambios muchas veces deben transmitirse para presentarse en otro componente. Un buen ejemplo de esto es cuando en un ecommerce añadimos un producto a el carrito. Esta información se añade en un componente "producto" y se muestra en el componente "carrito".

A través de los mecanismos como Inputs y Outputs que hemos visto anteriormente, podemos crear este tipo de comunicación. Podemos enviar el evento de click a través de un **Output** y transmitirlo a través de data binding a componentes que estemos utilizando.

Vamos a probar esta comunicación en conjunto con el ejercicio de este tema.