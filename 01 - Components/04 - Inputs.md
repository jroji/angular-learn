# Inputs

## Ámbito del componente

Ahora mismo, nuestro componente Avatar tiene "dentro de él" una serie de propiedades, como la imagen o el nombre de usuario, que utiliza para visualizar. La cosa es que como esas propiedades están dentro de el componente, no podemos modificarlas de una forma tan sencilla.

Una forma de **ver las propiedades que tiene cada componente**, es mediante la extensión de navegador [Augury](https://chrome.google.com/webstore/detail/augury/elgalmkoelokbchhkhacckoklkejnhcd?hl=es-419nnn). Esta lista en una especie de "DOM de componentes Angular" los componentes que estás utilizando, así como sus propiedades y relaciones si los seleccionas en el árbol.

## Input

Para resolver este problema, Angular pone a nuestra disposición un **mecanismo de transmisión de información unidireccional, llamado Input.**

Un Input, no es más que otro decorador de Angular para dotar de superpoderes a **una variable que esté declarada dentro de un componente, permitiendo que su valor sea sobreescrito desde fuera del componente.**

Vamos a ver como utilizarlo desde nuestro avatar.component.ts. Lo primero es importar el decorador Input desde @angular/core. Si tenéis un IDE con auto-import, os facilitará mucho la vida trabajando con sistemas modulares.

```TS
import { Component, OnInit, Input } from '@angular/core';
```
Y ahora, vamos a aplicar el decorador sobre la propiedad, o propiedades, que queramos que se puedan modificar.

```TS
import { Component, OnInit, Input } from '@angular/core';
...
export class AvatarComponent implements OnInit {
  @Input() username: string = 'jnroji';
  @Input() image: string = 'https://twivatar.glitch.me/jnroji';
  ...
}
```
Con esto **ya estaría activada la transmisión de datos hacia este componente**. Pero hasta ahora no hemos dicho como se realiza esta transmisión.**El decorador Input va a registrar las propiedades username e image como atributos** de la etiqueta app-avatar, de tal forma que, **cuando esos atributos estén presentes, su valor se guardará** dentro de las variables de el componente. Vamos a ver un ejemplo.

```HTML
<!-- 
    No tiene atributos username ni image 
     - Valor de username: (valor por defecto) 'jnroji'
     - Valor de image: (valor por defecto)  'https://twivatar.glitch.me/jnroji'
-->
<app-avatar></app-avatar>

<!-- 
    Tiene atributo username, no image 
     - Valor de username: (valor del atributo) Maria
     - Valor de image: (valor por defecto)  'https://twivatar.glitch.me/jnroji'
-->
<app-avatar username="Maria"></app-avatar>

<!-- 
    Tiene atributo username e image 
     - Valor de username: (valor del atributo) Maria
     - Valor de image: (valor del atributo 'https://twivatar.glitch.me/obimiau'
-->
<app-avatar username="Maria" image="https://twivatar.glitch.me/obimiau"></app-avatar>
```
Prueba a poner estos 3 avatares en el app.component.html y observar como se comportan.

Como punto extra, **el decorador Input acepta un parámetro de tipo string, con el que podemos indicarle el nombre del atributo que aportará el valor** para la variable si queremos que estos sean distintos. Es decir, que si mi variable se llama image, pero yo quiero que el atributo que aporte el valor se llame avatar, puedo usar el Input de la siguiente manera:

```TS
# avatar.component.ts

@Input('avatar') image: string = 'https://twivatar.glitch.me/jnroji';
```
```HTML
# app.component.html

<app-avatar avatar="https://twivatar.glitch.me/obimiau"></app-avatar>
```

Gracias a este sencillo mecanismo, podemos crear componentes que tengan una lógica y un aspecto definido solo una vez, y aportarle los valores que puedan cambiar mediante Inputs. Puedes dedicar un rato a entrar en Youtube, e intentar modificar componentes e Inputs, para ver que información necesitaría o cambiaría cada componente.