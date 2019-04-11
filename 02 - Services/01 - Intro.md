# Servicios

## ¿Qué es un servicio?

Un servicio, es otro tipo de elementos que nos permite crear Angular. En este caso, es una pieza únicamente lógica y no visual.

Además de esto, hay otra diferencia respecto a como se instancian los componentes en la aplicación, y es que los servicios implementan el patrón Singleton, es decir, que se utiliza una única instancia por módulo para todos los componentes que lo utilizen. Esto significa que si un componente realiza una modificación en un servicio (modificar una propiedad por ejemplo), este cambio estará disponible en el servicio si el mismo se invoca desde otro componente.

En resumen, todos comparte una única instancia del servicio. Esto nos viene genial, ya que los componentes no deberían ser los encargados de pedir o guardar la información de forma directa, ya que su función es presentar información visualmente y delegar esa lógica en los servicios.

** WIP DIAGRAMA Y EXPLICAR COMUNICACIÓN **

## ¿Como lo uso?

Para crear un servicio en Angular, utilizaremos un *decorador* distinto al @Component que hemos visto. En este caso utilizaremos el decorador @Injectable, que indica que la clase definida justo debajo, va a participar en el proceso de inyección de dependencias de Angular (hablaremos de esto más adelante).

Una vez tengamos definido nuestro servicio, o queramos "inyectar" en el componente un servicio de Angular, como el httpClient, el router, etc... Debemos indicarlo en el constructor de el componente. Es decir, que si quiero acceder al servicio del Router, lo haré de la siguiente manera (No modifiques el componente de momento)

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