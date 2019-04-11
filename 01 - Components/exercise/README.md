# Chat

Vamos a experimentar con lo que hemos visto hasta ahora en este capítulo. Realmente el desarrollo de componentes en Angular es, por así decir, el plato fuerte de este framework, ya que todo lo que hagamos visualmente será a través de componentes.

Vamos a construir una ventana de chat, con un componente que envíe texto. Esta carpeta contiene un proyecto "base" de Angular. Para arrancarlo, vamos a trabajar con el [cli de Angular](https://cli.angular.io/), que nos aporta bastante utilidad. Para instalarlo vamos a hacerlo globalmente desde una terminal de la siguiente manera.

```BASH
# Para instalar el cli
npm install -g @angular/cli

# Una vez lo tenemos instalado podemos lanzar el siguiente comando para lanzar el servidor de desarrollo en el puerto 4200
ng serve
```
El servidor que sirve la aplicación, basado en webpack-dev-server, está configurado con watchers y HMR, lo que significa que cada cambio que hagamos en los ficheros de la web, hará que se compile y sirva automáticamente en el navegador.


### Generar un componente avatar. 
  En este componente, visualizaremos la información de los contactos que vayamos utilizando. Podemos generar nuevos componentes utilizando el siguiente comando.

  ```
  ng generate component avatar
  ````

  Por defecto, nos generará un componente con el selector app-avatar. Si añadimos el html siguiente, visualizaremos en nuestra aplicación el mensaje app avatar works!

  ````HTML
  <app-avatar></app-avatar>
  `````
  Una vez tenemos el componente creado, podemos añadirle propiedades mediante Inputs. Consulta las slides si tienes dudas, pero acabaremos con algo asi en nuestro avatar.component.ts

  ````TS
  class AvatarComponent {
    @Input() imgSrc: string;
  }
  ````
 algo así en nuestro avatar.component.html
 
 ```HTML
  <img [src]="imgSrc">
```

 y así en nuestro app.component.html

 ```HTML
  <app-avatar imgSrc="<una url que os guste.jpg"></app-avatar>
 ```

Con esto, habremos creado nuestro primer componente con propiedades de entrada (Inputs)

  ### Generar un componente de introducción de texto.
  
  Ahora que ya sabemos crear un componente, y utilizar Inputs para pasarles propiedades, podemos crear un componente para enviar mensajes en nuestro chat. Este componente, va a utilizar NgModel para recoger la información que introduzcamos en un input. NgModel vincula el valor de un input con una propiedad. Para utilizarlo debemos seguir 3 pasos.
  
  Primero, vamos al app.module.ts para añadir el módulo de formularios de Angular. Angular no importa todos los módulos por defecto para dejarnos escoger que queremos y que no queremos usar. En este caso, vamos a añadir el modulo FormsModule al app.module.ts, añadiendo el import, e incluyendolo dentro del array de imports

```TS
import { FormsModule } from '@angular/forms';

@NgModule({
  ...
  imports: [
    BrowserModule,
    FormsModule
  ],
  ...
})
export class AppModule { }
```

Ahora ya podemos utilizar NgModel en nuestro componente. Vamos a empezar generando el componente

```BASH
ng g c text
```

Podemos incluirlo en el html del app.component.html utilizando el selector
```HTML
<app-text></app-text>
```

Ahora vamos a crear una variable "message" en nuestro text.component.ts para almacenar el valor introducido en el input por el usuario

```TS
export class TextComponent implements OnInit {
  message: string = '';
  
  constructor() {}
  ngOnInit() {}
}
```
Y vamos a añadir un input en nuestro text.component.html para que el usuario pueda introducir el mensaje que desea
```HTML
<input [(ngModel)]="message" type="text" placeholder="Message">
{{ message }}
```
Si introducimos un valor en el input, veremos que automaticamente se muestra debajo, gracias al NgModel, hemos creado una relación entre el input y la variable que hemos creado previamente. Cada vez que modifiquemos esta variable se verá reflejado en la vista.

Vamos a añadir un botón para confirmar el mensaje

```HTML
<input [(ngModel)]="message" type="text" placeholder="Message">
<button (click)="sendInfo()"> Enviar </button>
```

Mediante la notación (click), podemos escuchar el evento click en el botón y llamar a una función sendInfo que hayamos definido en nuestro componente. Vamos a mostrar el mensaje por un alert

```TS
export class TextComponent implements OnInit {
  message: string = '';
  
  constructor () {}
  ngOnInit () {}
  
  sendInfo () {
    alert(this.message);
  }
}
```

  ### Comunicando entre componentes
  
  Hemos creado un componente de introducción de texto, que nos servirá para introducir un nombre de usuario, un mensaje, o un comentario, dependiendo del contexto en el que lo utilizemos. Nuestro objetivo ahora es mostrar ese mensaje en el componente padre app.component.html. Para ello tendremos que usar los Output (Más info en las slides). Los outputs nos ayudan a definir "eventos propios del componente" que podremos escuchar como escuchamos los click, change y demás.
  
 ```TS
import { Component, OnInit, EventEmitter, Output } from '@angular/core';

@Component({
  selector: 'app-text',
  templateUrl: './text.component.html',
  styleUrls: ['./text.component.css']
})
export class TextComponent implements OnInit {
  @Output() written = new EventEmitter();
  message: string = '';
  
  constructor() { }

  ngOnInit() {
  }

  sendInfo() {
    this.written.emit(this.message);
  }
  
  ```
Como podéis ver, hemos definido el Output "written", y hemos modificado la función sendInfo para que, en vez de mostrar un mensaje alert por pantalla, emita la información a través de ese Output. De esta forma, podemos escuchar ese mensaje desde *app.component* con la notación de parentesis

````HTML
  <app-text (written)="receiveMessage($event)"></app-text>
````

Ahora, podemos definir esa función receiveMessage en nuestro app.component.ts para que guarde el valor del mensaje enviado en una variable. Como véis, hemos utilizado una variable especial $event en la llamada a la función, esto debe ir siempre que queramos recoger la información que nos envía el evento. En el caso de nuestro evento, recibiremos lo que hemos enviado en el emit de el Output

````TS

import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'chat';
  parentMessage: string;

  receiveMessage(message) {
    this.parentMessage = message;
  }
````
y mostrarlo en nuestro html
````HTML
<h3>Mensaje desde el hijo:</h3>
<p>{{ parentMessage }}</p>
<app-text (written)="receiveMessage($event)"></app-text>
````

Ahora que tenemos nuestro componente que envia mensajes,y hemos llevado la información al componente app.component, podemos crear nuestro nuevo componente para visualizar una lista de mensajes. Lo primero que vamos a hacer es cambiar como almacena app.component los mensajes. Hasta ahora, simplemente se mostraba el último mensaje enviado. Para ello, vamos a añadir cada mensaje a un array de mensajes en el componente app.

````TS
export class AppComponent {
  title = 'chat';
  // Cambiamos el tipo de string a array, y lo inicializamos para poder pushear mensajes
  parentMessage: string[] = [];

  receiveMessage(message) {
    // Cambiamos el envio de mensajes para añadirlos al array
    this.parentMessage.push(message);
  }
  ...
 }
````

Ahora que estamos guardando los mensajes en formato array, vamos a crear un nuevo componente BOARD que muestre esos mensajes.

````BASH
ng g c board
````

Para que el componente board, pueda recibir la información desde el componente app, debemos añadir una propiedad Input. Las propiedades inputs nos sirven para transmitir información a componentes hijos. Son aquellas propiedades que hacen nuestro componente reutilizable. Pensad en el atributo src de un componente img, o el atributo required de un input. En nuestro caso, board recibirá una lista de mensajes

````TS

import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-board',
  templateUrl: './board.component.html',
  styleUrls: ['./board.component.css']
})
export class AppComponent {
  @Input() messages;
}

````

Y en el html, vamos a utilizar una de las directivas que nos proporciona angular, ngFor, para pintar una lista de elementos desde un array, en este caso, la lista de mensajes. Para utilizarlo, debemos añadir * ngFor, seguido por let message of messages, donde message es la variable que declaramos por cada elemento del array, y messages el nombre de nuestra propiedad

````HTML
<p *ngFor="let message of messages"> {{ message }} </p>
````

Una vez tenemos creado el componente, podemos utilizarlo modificando el html de nuestro App.component, utilizando la propiedad input que hemos creado

````HTML
<app-board [messages]="messages"></app-board>
<app-text (written)="receiveMessage($event)"></app-text>
````
