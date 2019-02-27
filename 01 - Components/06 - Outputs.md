# Outputs

De la misma manera que tenemos Inputs para proporcionar información a un componente, Angular **provee de un mecanismo llamado Output para el envío de eventos hacia los componentes padre**. Es recomendable que tengas en cuenta siempre el árbol que estás construyendo con tus componentes. En el caso del avatar que hemos utilizado antes, sería una jerarquía sencilla, en la que habría un nodo padre (app-component) y un nodo hijo que sale de el anterior (app-avatar).

Para que un componente de presentación tenga valor, no solo vale que muestre unos datos, también debe ser capaz de transmitir a la vista, o a un componente superior, que se ha realizado una determinada acción. 

Vamos a hacer que el componente avatar, envie el nombre de usuario del componente hacia arriba. De esta forma, desde el app-component, sabremos a que usuario queremos enviar la información.

Lo primero que vamos a hacer es importar el decorador Output y declarar una nueva variable con el.

```TS 
// avatar.component.ts
import { Component, OnInit, Input, Output } from '@angular/core';

@Component({
  selector: 'app-avatar',
  templateUrl: './avatar.component.html',
  styleUrls: ['./avatar.component.css']
})
export class AvatarComponent implements OnInit {
  @Input() username: string = 'jnroji';
  @Output() messageSent;

  constructor() { }

  ngOnInit() { }

  private getUrl (image) {
      return `https://twivatar.glitch.me/${image}`;
  }

  private sendMessage () {
      alert("Mensaje enviado");
  }
}
```

Las variables Output son un poco especiales comparadas con el resto. Debemos declararlas siempre instanciando la clase EventEmitter de Angular. Esta clase nos permite gestionar los eventos que queramos lanzar sin necesidad de hacer todo el código nosotros. Estos eventos se escucharán desde componentes superiores de la misma forma que se escucha por ejemplo el evento click, es decir, con paréntesis.

Vamos a declarar nuestro EventEmitter


```TS 
// avatar.component.ts
import { Component, OnInit, Input, Output } from '@angular/core';

@Component({
  selector: 'app-avatar',
  templateUrl: './avatar.component.html',
  styleUrls: ['./avatar.component.css']
})
export class AvatarComponent implements OnInit {
  @Input() username: string = 'jnroji';
  // Declaramos también el tipo de información que vamos a enviar en el evento, en este caso un objeto
  @Output() messageSent: EventEmitter<object> = new EventEmitter<object>();

  constructor() { }

  ngOnInit() { }

  private getUrl (image) {
      return `https://twivatar.glitch.me/${image}`;
  }

  private sendMessage () {
      // Llamamos a la función emit para enviar un evento hacia arriba con la información que deseemos. En este caso lo pongo como objeto pero podría ser simplemente un string o cualquier cosa
      messageSent.emit({ username: this.username });
      this.
  }
}
```

Una vez lo hemos declarado, podemos llamar a la función "emit" al realizar la acción que deseemos escuchar, en este caso el click del botón enviará el nombre del usuario al que queramos escribir.

Para escucharlo, vamos a irnos al app.component.html (desde donde se está usando el app-avatar) y escuchar el evento que viene desde abajo.


```HTML
<app-avatar></app-avatar>
<app-avatar (messageSent)="receiveMessage($event)" username="obimiau"></app-avatar>
<app-avatar (messageSent)="receiveMessage($event)" username="jnroji"></app-avatar>
```

Al hacer esto, podrémos declarar una función "receiveMessage" en nuestro app.component.ts, y recibir como parámetro la información contenida en el evento que hemos enviado desde el componente avatar.