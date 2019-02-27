# Ciclo de vida y eventos

## Declaración de funciones

De la misma forma que podemos definir propiedades y variables propias de un determinado componente, podemos crear funciones que esten disponibles desde dentro.

Definir una función es muy sencillo, y al igual que las propiedades, la función estará disponible para ser invocada desde dentro del componente.

Vamos a crear por ejemplo una función para generar la url a partir del username. Lo primero, vamos a eliminare el Input de la propiedad image, ya que ya no será contribuida desde el componente padre, si no que la generaremos a partir del usuario. Recuerda eliminar tambien la información proporcionada en el atributo en el app.component.html

```TS
// avatar.component.ts
import { Component, OnInit, Input } from '@angular/core';

@Component({
  selector: 'app-avatar',
  templateUrl: './avatar.component.html',
  styleUrls: ['./avatar.component.css']
})
export class AvatarComponent implements OnInit {
  @Input() username: string = 'jnroji';

  constructor() { }

  ngOnInit() { }
}
``` 

Vamos ahora a definir una función para construir la url

```TS 
// avatar.component.ts
import { Component, OnInit, Input } from '@angular/core';

@Component({
  selector: 'app-avatar',
  templateUrl: './avatar.component.html',
  styleUrls: ['./avatar.component.css']
})
export class AvatarComponent implements OnInit {
  @Input() username: string = 'jnroji';

  constructor() { }

  ngOnInit() { }

  private getUrl (image) {
      return `https://twivatar.glitch.me/${image}`;
  }
}
``` 

Y ahora, actualizamos el HTML

```HTML
<img src="{{ getUrl(username) }}">
<h1>{{ username }}</h1>
```

Como se ve, podemos invocar a una función de la misma forma que cuando pintamos una propiedad. Esta función, se ejecuta una vez al renderizar el contenido y una vez por cada vez que cambie el parámetro que recibe la función. Es decir, en este caso, se está llamando a **getUrl(username)**. Esto implica que cada vez que username obtenga un nuevo valor, la función se ejecutará de nuevo y devolverá automáticamente un nuevo valor. Esto está genial, pero debemos tener cuidado a nivel rendimiento con los parámetros que pasamos a la función para que no esten ejecutandose funciones complejas con cada reasignación.

## Eventos

Hasta ahora, solo hemos usado la parte de presentación de la información. Es decir, hemos pintado valores de variables, pero aún no hemos hecho ningún tipo de interacción con el usuario. Para realizar esto, Angular permite escuchar eventos, tanto nativos como propios, de forma muy sencilla. En este caso la sintaxis cambia a (nombreEvento)
* { } => Presentar
* ( ) => Escuchar

Por lo tanto, si queremos escuchar por ejemplo un evento de click, solo debemos realizar lo siguiente. Vamos a añadir primero un botón en el HTML y a asociar un evento de click

```HTML
<img src="{{ getUrl(username) }}">
<h1>{{ username }}</h1>
<button (click)="sendMessage()">Enviar mensaje</button>
```

Con esto, estamos indicando a el componente, que cuando se haga click en el botón, se debe invocar a la función sendMessage, la cual aún no está definida, así que vamos a ello


```TS 
// avatar.component.ts
import { Component, OnInit, Input } from '@angular/core';

@Component({
  selector: 'app-avatar',
  templateUrl: './avatar.component.html',
  styleUrls: ['./avatar.component.css']
})
export class AvatarComponent implements OnInit {
  @Input() username: string = 'jnroji';

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

Si ahora hacemos click sobre el botón, debería aparecer el mensaje que hemos configurado.

En el caso de un evento nativo, el navegador envia un objeto con información sobre el target del evento, la posición clickada etc... En el caso de eventos custom, nosotros podremos enviar la información que deseemos. Para recoger este objeto, debemos hacer uso de la variable $event de angular, que hace que la función invocada reciba como argumento los datos asociados a el evento

```HTML
<img src="{{ getUrl(username) }}">
<h1>{{ username }}</h1>
<button (click)="sendMessage($event)">Enviar mensaje</button>
```

```TS 
// avatar.component.ts
import { Component, OnInit, Input } from '@angular/core';
...
  private sendMessage (event) {
      console.log(event);
      alert("Mensaje enviado");
  }
...
```

Si abrimos la consola, podremos ver la información que arroja el evento que hemos escuchado.