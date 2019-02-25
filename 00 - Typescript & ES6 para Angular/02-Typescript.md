# Typescript
## ¿Qué es esto?
Typescript no es un nuevo lenguaje super complicado que tengas que aprender, así que no tengas miedo. Typescript es un **superset** de Javascript que aporta tipado estático y diversas herramientas ya existentes en otros lenguajes a JS. Lo que significa que, a todo el Javascript que ya conoces, solo va a añadirle una serie de características extra para hacerle un lenguaje más potente, estricto y mantenible. Que sea un superset implica que **todo el código JS que desarrolles funcionará dentro de TS**, por lo que si simplemente quieres pasar de usarlo en Angular, sientete libre de hacerlo.

Typescript es un lenguaje que el navegador no entiende, por lo que antes de llegar al mismo necesita ser **compilado**. Básicamente **todo tu código Typescript pasará por el compilador TSC para transformarse en Javascript**. Esto incluye pasar de código ES6 a ES5, con lo que tu web también funcionará con navegadores viejos sin necesidad de utilizar también babel como paso extra. Para ello, Angular ya incorpora el compilador de Typescript a su cli, para hacernos transparente esa transformación. Puedes probar como funciona esa **transformación de código** en la [página de Typescript](https://www.typescriptlang.org/play/)

## ¿Y esto para qué?
Personalmente, me parece un acierto el uso de Typescript en frameworks orientados a componentes, como es el caso de Angular. Ten en cuenta que con este paradigma, múltiples desarrolladores van a trabajar con múltiples piezas que han desarrollado otras personas. Con Typescript podemos definir de forma sencilla los tipos de datos que requieren estas piezas y requerirlos antes de usarlos para validar que el formato de datos es el correcto. De la misma forma, vas a evitar un montón de errores tontos que se verían al llegar al navegador, ya que si TSC detecta una inconsistencia en los tipos, te avisará antes de llegar al navegador. Además, como ya incluye un transpilador de código además del compilador, nos permite hacer uso de características todavía no implementadas por los navegadores de forma cómoda.

Además, aprender Typescript no te servirá solo para usarlo con Angular, ya que cada vez más frameworks y librerías dan opciones en la implementación para trabajar de forma cómoda con Typescript

Aún así, **solo vamos a ver una pequeña parte de lo que aporta Typescript**. Es decisión tuya si quieres investigar más para sacarle todo el jugo posible :)

## Enseñame como va esto
### Tipado estático
Probablemente lo más importante y famoso de Typescript. Este lenguaje, nos permite dotar de tipos estáticos a las variables, lo que implica que **una vez definido un tipo para una variable, esta no podrá ser reasignada con un tipo distinto del definido inicialmente**. La forma de declarar tipos es muy sencilla

```TS
// Tipo Boolean (true/false)
let isDone:boolean = false;

// Tipo numerico (decimal/hexadecimal/octal...)
let counter:number = 5;
let hexCounter:number = 0xf000;
let binaryCounter:number = 0b1000;

// Tipo Texto
let text:string = 'Texto';

// Tipo Array
let numberArray:number[] = [1,2,3];
let numberArray:Array<number> = [1,2,3];

// Tipo comodin
let notSure:any = 'Im a string';
let notSure = false;

// Tipo indeciso
let notSure:number|string = 'Can be both';
```

Como veis la sintaxis es muy sencilla, simplemente pondremos : seguidos del tipo que queremos asociar a esa sentencia. Vamos a ver como aplicarlos a los parámetros de funciones, o a los valores de retorno de las mismas

```TS
// Tipos para los parámetros de la función
add(number1:number, number2:number) => {
    alert(number1 + number2);
}

// Tipos para el valor de retorno de la función
add(number1, number2):number => {
    return number1 + number2;
}

add(number1, number2):void => {
    console.log(number1 + number2);
}
```

### Interfaces
Las interfaces son un recurso **super útil** para definir tipos para objetos. Mediante los interfaces, podemos definir un "contrato" mediante el cual podemos asegurar que el objeto que estamos pasandole a un determinado componente se adecua a lo que el mismo necesita para funcionar. Por ejemplo, imaginemos que hemos hecho el componente "tarjeta-user", que requiere un objeto con la información del usuario: una imagen de avatar y un nombre.

```TS
interface User {
    name: string;
    imageSrc: string;
}
```

De esta forma, una vez tengo definido el interfaz, a la hora de pasarle a este tarjeta-user un objeto, se validará en tiempo de construcción que el objeto enviado realmente contiene esos campos

```
const user: User = {
    name: '...',
    imageSrc: '...',
};

tarjeta-user(myUser);

```

De la misma manera que puedo definir campos obligatorios, también puedo definirlos opcionales mediante el uso del caracter ?

```
interface User {
    name: string;
    imageSrc: string;
    email?: string;
}

```

### Decoradores

Los decoradores son uno de los recursos más usados en el core de Angular, y es la herramienta básica que nos ofrecen para la declaración de componentes, servicios y otras entidades propias de Angular. 

Un decorador, no es más que una función, que recibe como parámetros la función o elemento definida a continuación (en Angular normalmente una clase) y los pasados como paramétros normales. Esta función se ejecutará cuando la clase o función es instanciada/invocada.

Vamos a imaginar que tenemos la siguiente función

```TS 
function sealed(constructor: Function) {
    Object.seal(constructor);
    Object.seal(constructor.prototype);
}
```

La función Object.seal "sella" el objeto que recibe como parámetro para que no puedan añadirse nuevas propiedades. En este caso, la función sealed, recibirá una función, y sellará la función y su prototipo para hacerla inmutable

Para usarlo, unicamente tenemos que usar @ y el nombre de la función sobre lo que queremos "sellar".

```TS
@sealed()
class Greeter {
    greeting: string;

    constructor(message: string) {...}
    greet() {
        ...
    }
}
```

Una vez sellada la clase, ya no podríamos modificar sus propiedades ni su prototipo. Este caso es uno muy simple del uso de un decorador. En el caso de angular, se proporcionan decordadores tales como **@component**, que realiza todas las acciones necesarias para convertir nuestra logica, html y demás en un componente aislado y registrado en el navegador y dejarlo listo para su uso. Hay otros decoradores interesantes, como Input, Output en Angular que nos ayudaran con multiples funciones. No hace falta que declares nuevos decoradores, solo con conocer como funcionan te valrá de momento :) 

### Declaración de variables en clases

Existe también una sintaxis especial a la hora de trabajar con clases en typescript. En typescript podemos declarar variables de una clase, sin necesidad de definirlas en el constructor, si no que podemos colocarlas por encima para definirlas.

```TS
class Greeter {
    // No necesitamos hacer this.greeting = 'hola' en el constructor; 
    greeting: string = 'hola';

    constructor(private message: string) {...}
    greet() {
        ...
    }
}
```

Otra cosa importante, es que Typescript nos da la opción de definir el ámbito de nuestras variables a través de las palabras reservadas *private, public, protected*, cambiando de esta forma los privilegios de acceso de otros componentes a variables que son privadas o protegidas de los componentes.