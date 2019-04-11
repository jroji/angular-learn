# Templating

Angular ofrece numerosos mecanismos para darnos más opciones para trabajar con elementos HTML y lógica javascript. A la hora de, por ejemplo, editar atributos, ocultar contenido o modificar clases, no vamos a trabajar como tradicionalmente se hace en Javascript, a través de [selecciones de elementos HTML](https://developer.mozilla.org/es/docs/Web/API/Document/querySelector), si no que tendremos una serie de herramientas para trabajar de forma más cómoda, y evitar el aumento de lógica en la clase de nuestro componente.

## Bindeo y data binding

El data binding, es la [formá más básica de uso de variables y funciones desde la vista](https://angular.io/guide/template-syntax). Puede servirnos para mostrar valores en pantalla, setear el valor de un atributo, etc... La forma más basica es el uso de doble "curly braces" / {{ }}, colocando el valor que deseamos "transferir" a la vista, dentro de las llaves. De esta forma, si tenemos el siguiente componente con la propiedad title definida.

```TS
// app.component.ts
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'components-project';
}
```

Podemos "llevarlo" al template, a través de la siguiente sintaxis
```HTML
<!-- app.component.html -->
<h1>{{ title }}</h1>
```
O podríamos pasar el valor a un atributo del elemento, como la clase, de la siguiente manera

```HTML
<h1 class="{{ title }}">{{ title }}</h1>

<!-- Podemos también utilizar esta sintaxis [nombreAtributo], para indicarle que el valor que tiene asignado corresponde a un valor de la clase. Esta sintaxis es muy cómoda a la hora de trabajar con atributos HTML -->
<h1 [class]="title">{{ title }}</h1>

```

Una vez tenemos conectados el template y la lógica, se crea una conexión de **One-way data-binding**, lo que implica que en el momento en el que el valor de la variable cambie, el template se renderizará de nuevo automáticamente, gracias al sistema de detección de cambios de Angular. Puedes comprobarlo añadiendo un setTimeout que actualize el valor de la variable.

```TS
// app.component.ts
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'components-project';

  constructor() {
      setTimeout(() => this.title = 'test', 3000);
  }
}
```

Este mecanismo es realmente útil y cómodo para modificar el contenido que vemos en pantalla de forma cómoda. Si trabajasemos con Vanilla Javascript, tendríamos que seleccionar el elemento con un querySelector, para modificar sus propiedades desde la parte de lógica. Angular nos hace la tarea un poco más sencilla gracias a este sistema.

Esta sintaxis no solo sirve para mostrar variables con valores estáticos. Siguiendo el mismo mecanismo, es posible invocar funciones definidas en la clase del componente para mostrar sus valores.

```TS
export class AppComponent {
  title = 'components-project';
  
  showTitle(prefix, title) {
    return `${prefix}${title}`;
  }
}
```
Y la invocamos desde el HTML
```HTML
<h1 [class]="title">{{ showTitle('Título: ', title) }}</h1>
```

**Nota**: *Si te fijas en la función showTitle, está recibiendo como parámetro un valor de la clase "title". En este caso, Angular creará un observador sobre la variable title, y llamará a la función showTitle cada vez que el valor cambie. Por eso es importante tener la simplicidad en mente a la hora de invocar funciones, ya que no queremos que procesos pesados se llamen cada vez que la variable title cambie.*

## Listeners de eventos

Angular también evita crear eventListeners directamente en la clase del componente. Nos permite [añadir escuchadores directamente en el HTML](https://angular.io/guide/user-input), evitando de esta manera el crear mas listeners de los necesarios o el tener que crearlos a través de selectores de elementos. Esto lo haremos a través del uso de paréntesis alrededor del evento que queramos escuchar, ya sea un evento nativo (click, hover...) o uno propio, como veremos más adelante.

```TS
export class AppComponent {  
  title = 'components-project';

  updateTitle(title) {
    this.title = title;
  }
}
```
```HTML
<h1 [class]="title">{{ showTitle('Título: ', title) }}</h1>
<!-- Escuchamos el evento 'click' -->
<button (click)="updateTitle('nuevo título')">Update</button>
```

Si queremos recoger la información que devuelve el evento (en el caso del click un objeto de tipo MouseEvent con el srcElement, target, etc...), podemos hacer uso de la variable auxiliar $event.


```TS
export class AppComponent {  
  title = 'components-project';

  updateTitle(title, event) {
    console.log(event);
    this.title = title;
  }
}
```

```HTML
<h1 [class]="title">{{ showTitle('Título: ', title) }}</h1>
<!-- Escuchamos el evento 'click' y recogemos la información a través de $event -->
<button (click)="updateTitle('nuevo título', $event)">Update</button>
```

## Directivas de templating condicional

Angular nos ofrece un montón de directivas que podemos usar en nuestros elementos HTML. Vamos a hablar de algunas de las más importantes a la hora de renderizar contenido en base a la lógica de nuestro componente

### ngIf

La [directiva ngIf](https://angular.io/guide/displaying-data#conditional-display-with-ngif), nos permite eliminar y añadir un elemento del DOM de una manera cómoda y eficiente, en base a una condición. Por ejemplo, con el siguiente trozo de HTML, podríamos mostrar o no un elemento solo cuando title está definido

```HTML
<h1 *ngIf="title != null"> {{ title }} </h1>
```
O podríamos llamar a una función que comprobase si es jueves, y devolviese un boolean
```HTML
<h1 *ngIf="isThursday()"> {{ title }} </h1>
```

Existe también otra directiva, [ngSwitch](https://angular.io/api/common/NgSwitch) que puede ser más útil para mostrar contenido condicional cuando tenemos más de una opción en base al valor de una propiedad.

### ngFor

[ngFor](https://angular.io/guide/displaying-data#conditional-display-with-ngif) es un recurso super útil cuando tenemos que renderizar listas de elementos. Por ejemplo, queremos mostrar todas las entradas de un blog, que hemos recuperado a través una petición XHR. Si tenemos un array definido en nuestra clase de Typescript, podemos utilizar ngFor en el template para pintar el html que nosotros definamos una vez por cada elemento del array.

Para utilizar ngFor, debemos seguir la siguiente sintaxis: 

'let elemento of elementos'

let [variable a través de la cual accedemos al valor de cada elemento del array en cada repetición] of [array de elementos a pintar];

```TS
export class AppComponent {  
    posts = [
        {
            id: 1,
            title: 'Angular intro',
            caption: 'Lo mejor para empezar'
        },
        {
            id: 2,
            title: 'Angular intermedio',
            caption: 'Lo mejor para continuar'
        },
        {
            id: 3,
            title: 'Angular Master',
            caption: 'Triunfa con Angular'
        },
    ];
}
```

```HTML
<div *ngFor="let post of posts" class="post" data-id="{{ post.id }}">
    <h1>{{ post.title }}</h1>
    <p>{{ post.caption }}</p>
</div>
```

Con este HTML, renderizariamos 3 divs con su h1 y su p. Uno por cada elemento del array, y a través de la variable *post*, podríamos acceder a su valor a la hora de renderizarlo.

### ngClass

NgClass es una directiva que no modifica el contenido que se renderiza en pantalla como las dos anteriores, pero si que añade o elimina clases de forma condicional en base a una condición. Para utilizarlo, debemos utilizar una sintaxis object-like, en la que la key será la clase a añadir, y el value, será la condición sobre la que se evalue. Por ejemplo, si en la situación anterior quiseramos destacar el post número 2 con unas reglas de css especiales, podríamos hacer lo siguiente

```HTML
<div [ngClass]="{'on-sale': post.id == 2}" *ngFor="let post of posts" data-id="{{ post.id }}">
    <h1>{{ post.title }}</h1>
    <p>{{ post.caption }}</p>
</div>
```

De esta forma, al cumplirse la condición que hemos colocado con el segundo libro del bucle, únicamente ese, tendría la clase 'on-sale' pudiendo destacarlo a través de un fondo distinto, o unos estilos concretos.

**Nota** *Siempre que aparezca un asterisco delante de una directiva, significa que Angular está creando 'under the hood' una etiqueta template, y clonando el contenido en base a la condición y el comportamiento de la directiva en concreto*