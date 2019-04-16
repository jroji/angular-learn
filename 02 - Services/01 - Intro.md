# Servicios

## ¿Qué es un servicio?

Un servicio, es otro tipo de elementos que nos permite crear Angular. En este caso, es una pieza únicamente lógica y no visual, que nos va a permitir declarar lógica y funciones comunes para que los componentes puedan usar, o guardar información para que sea compartida por los componentes. Esto se debe a que durante nuestro uso de la web, solo existirá una instancia de cada servicio, que compartirán los componentes

 La diferencia respecto a como se instancian los componentes en la aplicación, es que los servicios implementan el patrón Singleton, es decir, que se utiliza una única instancia por módulo para todos los componentes que lo utilizen en el mismo. Esto significa que si un componente realiza una modificación en un servicio (modificar una propiedad por ejemplo), este cambio estará disponible en el servicio si el mismo se invoca desde otro componente.

En resumen, todos comparten una única instancia del servicio. Esto nos viene genial, ya que los componentes no deberían ser los encargados de pedir o guardar la información de forma directa, ya que su función es presentar información visualmente y delegar esa lógica en los servicios.

** WIP DIAGRAMA Y EXPLICAR COMUNICACIÓN **


Para crear un servicio en Angular, utilizaremos un *decorador* distinto al @Component que hemos visto. En este caso utilizaremos el decorador @Injectable, que indica que la clase definida justo debajo, va a participar en el proceso de inyección de dependencias de Angular (hablaremos de esto más adelante).

## ¿Como lo uso?

Una vez tengamos definido nuestro servicio, o queramos "inyectar" en el componente un servicio de Angular, como el httpClient, el router, etc... Debemos indicarlo en el constructor de el componente. Angular ya nos proporciona numerosos servicios, como el router, el cliente de HTTP, y otras muchas utilidades. Si quiero acceder por ejemplo al servicio del Router, lo haré de la siguiente manera (No modifiques el componente de momento)

```TS
import { Component, OnInit } from '@angular/core';
import { Router } from '@angular/router';

@Component({
  selector: 'app-avatar',
  templateUrl: './avatar.component.html',
  styleUrls: ['./avatar.component.css']
})
export class AvatarComponent implements OnInit {

  // Añadimos el servicio que queremos utilizar como parámetro en el constructor.
  constructor (private router: Router) { }

  ngOnInit() { }

}
```

Una vez lo incluímos en el constructor, entra en juego la **Inyección de dependecias** de Angular, que resolverá por nosotros lo que necesite el constructor del servicio, y nos devolverá la instancia que ya esté creada o creará una nueva.

Una vez declarada, podemos acceder al nuevo servicio como si fuera una propiedad más, a través de this, en este caso, this.router. Se podría decir que lo que hace Angular por debajo es lo siguiente

```TS
 // No exactamente así, por que nos devolverá la instancia que ya está creada y no una nueva, pero para que se entienda que se crea un this.router dentro de la clase :)
  constructor() {
      this.router = new Router(param1..., param2...)
  }
```

