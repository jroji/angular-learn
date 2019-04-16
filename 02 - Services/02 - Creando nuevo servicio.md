# Creando nuestro primer servicio

## Generando desde el cli

El cli de Angular nos permite **generar los distintos elementos desde el terminal** de forma cómoda. Para hacerlo, vamos a utilizar el siguiente comando

```BASH
ng generate service users
# o abreviado
ng g s users
```

Si ejecutamos esto, y volvemos a nuestro IDE, podremos ver un nuevo servicio creado, y al lado, un archivo 'spec' para los test unitarios. Como verás, se crean en la raíz del proyecto, lo cual puede ser un jaleo a la hora de organizar nuestro código si el proyecto sigue creciendo. Puede ser recomendable crear una carpeta services donde almacenar los servicios que creemos. Esto se puede hacer moviendo los archivos, o desde el cli utilizando este comando en vez de el anterior.

```BASH
ng g s services/users
```

## Servicio de contactos

Vamos a ver que pinta tiene nuestro componente. Como comentaba anteriormente, un servicio es únicamente un elemento lógico, sin template ni vista que renderizar.  Por lo que únicamente tenemos que preocuparnos del archivo TS creado.

```TS
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class ContactsService {
  constructor() { }
}
```

Como se ve, un servicio es más sencillo en la declaración que un componente. El único parámetro que recibe, es un 'provideIn' que además es opcional. Este parámetro, introducido en la versión 6 de Angular, sirve para indicar a la aplicación en que módulo va a inyectarse como dependencia. Por defecto, se inyectará en el módulo root. Más adelante hablaremos de los módulos y para que lo utilizamos, pero de momento no hace falta que nos preocupemos por ello.

Un servicio no va a dejar de ser una clase en la que añadir propiedades y métodos que puedan invocarse desde los componentes. Los servicios pueden a su vez utilizar otros servicios. Vamos por ejemplo a añadir una serie de propiedades y funciones en nuestro componente.

```TS
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class ContactsService {
  private users = [
      'jnroji',
      'gody11',
      'canariasjs'
  ];

  constructor() { }

  public getUsers () {
      return this.users;
  }
}
```

Hemos creado una lista de usuarios que será recuperable desde los componentes a través del servicio. Mantener esta lógica aquí tiene sus ventajas. Por ejemplo, conseguimos que nuestros componentes sean agnosticos a la obtención de los datos. Pueden estar viniendo de una petición XHR, de una constante, etc... que a ellos les da igual, ya que simplemente va a consumir la información que el servicio les devuelva. 

Además, si varios componentes consumen esta información, centralizamos aquí la recuperación de estos datos, para tener que únicamente modificar este archivo si queremos cambiarlo.

### Utilizar un servicio

Una vez tenemos creado el servicio, simplemente tenemos que inyectarlo en uno de nuestros componentes. En este caso, voy a inyectarlo en mi app.component (si hiciste el ejercicio anterior, puedes evolucionarlo ahí :)

```TS
import { ContactsService } from '../services/messages.service';
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
  
  // Inyectamos el servicio como parámetro del constructor, indicandole en el tipo del parámetro que clase de servicio es
  constructor (private messages: ContactsService) {}
  
  ngOnInit() {}
}

```

Una vez lo tenemos inyectado, podemos acceder a sus propiedades públicas o invocar a sus funciones, a través de this.messages (o el nombre que le hayamos puesto a la hora de añadir el parámetro del constructor)

```TS
export class AppComponent implements OnInit {
  public users: string[];

  constructor (private messages: ContactsService) {}
  
  ngOnInit() {
      // Al instanciar el componente, almacenamos dentro del componente los usuarios que devuelve la función getUsers, declarada previamente en el servicio
      this.users = this.messages.getUsers();
  }
}

```

Una vez tenemos los usuarios en nuestro componente app, podemos pintarlos con un *ngFor

```HTML
<!-- app.component.html -->
<ul class="contacts">
    <li *ngFor="let user of users">{{ user }}</li>
</ul>
```

Si levantamos la aplicación y vamos al puerto 4200, deberíamos ver la lista de usuarios que hemos añadido :)
