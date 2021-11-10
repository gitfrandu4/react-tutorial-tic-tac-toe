# Tutorial: Introducción a React

> https://es.reactjs.org/tutorial/tutorial.html

## Visión General

**¿Qué es React?**

React es una librería de JavaScript declarativa, eficiente y flexible para construir interfaces de usuario. Permite componer IUs complejas de pequeñas y aisladas piezas de código llamadas “componentes”.

React tiene pocos tipos diferentes de componentes, pero vamos a empezar con la subclase React.Component:

```javascript
class ShoppingList extends React.Component {
  render() {
    return (
      <div className="shopping-list">
        <h1>Lista de compras para {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}

// Uso de ejemplo: <ShoppingList name="Mark" />
```

Usamos componentes para decirle a React lo que queremos que se vea en la pantalla. Cuando nuestros datos cambian, React actualizará eficientemente y volverá a renderizar (re-render) nuestros componentes.

Aquí, ShoppingList es una clase de componente de React, ó tipo de componente de React. Un componente acepta parámetros, llamados props (abreviatura de “propiedades”), y retorna una jerarquía de vistas a mostrar a través del método render.

El método render retorna una descripción de lo que quieres ver en la pantalla. React toma la descripción y muestra el resultado. En particular, render retorna un elemento de React, el cuál es una ligera descripción de lo que hay que renderizar. La mayoría de desarrolladores de React usan una sintaxis especial llamada “JSX” que facilita la escritura de estas estructuras. La sintaxis <div /> es transformada en tiempo de construcción a React.createElement('div'). El ejemplo anterior es equivalente a:

```javascript
return React.createElement('div', {className: 'shopping-list'},
  React.createElement('h1', /* ... h1 children ... */),
  React.createElement('ul', /* ... ul children ... */)
);
```

JSX viene con todo el poder de JavaScript. Puedes poner cualquier expresión de JavaScript en el interior de las llaves dentro de JSX. Cada elemento de React es un objeto de JavaScript que puedes almacenar en una variable o pasar alrededor de tu programa.

El componente anterior ShoppingList solo renderiza componentes pre-construidos del DOM como `<div />` y `<li />`. Pero, también puedes componer y renderizar componentes personalizados de React. Por ejemplo, ahora podemos referirnos al listado de compras completo escribiendo `<ShoppingList />`. Cada componente de React está encapsulado y puede operar independientemente; esto te permite construir IUs complejas desde componentes simples.

### Inspeccionando el código inicial

Inspeccionando el código, notarás que tenemos 3 componentes de React:

* Square
* Board
* Game

El componente Square renderiza un simple <button> y el Board renderiza 9 cuadrados. El componente Game renderiza un table con valores de posición por defecto que modificaremos luego. Actualmente no hay componentes interactivos.

### Pasando datos a través de props

En el método renderSquare de Board, cambia el código para pasar una prop llamada value al Square:

```javascript
class Board extends React.Component {
  renderSquare(i) {
    return <Square value={i} />;
  }
}
```

Cambia el método render de Square para mostrar ese valor, reemplazando {/* TODO */} con {this.props.value}:

```javascript
class Square extends React.Component {
  render() {
    return (
      <button className="square">
        {this.props.value}
      </button>
    );
  }
}
```

¡Felicidades! Acabas de “pasar una prop” de un componente padre Board a un componente hijo Square. Pasando props es cómo la información fluye en apps de React, de padres a hijos.

### Haciendo un componente interactivo

Vamos a rellenar el componente de Square con una “X” cuando damos click en él. Primero, cambia la etiqueta button que es retornada del método render() del componente Square a esto:

```javascript
class Square extends React.Component {
 render() {
   return (
     <button className="square" onClick={() => console.log('click')}>
       {this.props.value}
     </button>
   );
 }
}
```

> Para continuar escribiendo código sin problemas y evitar el confuso comportamiento de this, vamos a usar la sintaxis de funciones flecha para manejar eventos aquí y más abajo
> Ten en cuenta cómo con `onClick={() => console.log('click')}`, estamos pasando una función como valor de la prop onClick. React solo llamará a esta función después de un click. Olvidar `() =>` y escribir `onClick={console.log('click')}` es un error común, y se ejecutaría cada vez que el componente se re-renderice.

Como un siguiente paso, queremos que el componente Square “recuerde” que fue clickeado, y se rellene con una marca de “X”. Para “recordar” cosas, los componentes usan **estado**.

Los componentes de React pueden tener estado estableciendo `this.state` en sus constructores. **`this.state`** debe ser considerado como privado para un componente de React en el que es definido. Vamos a almacenar el valor actual de un cuadrado en this.state, y cambiarlo cuando el cuadrado es clickeado.

Primero, vamos a agregar el constructor a la clase para inicializar el estado:

```javascript
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button className="square" onClick={() => console.log('click')}>
        {this.props.value}
      </button>
    );
  }
}
```

> En las [clases de JavaScript](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Classes), necesitas siempre llamar super cuando defines el constructor de una subclase. Todas las clases de componentes de React que tienen un constructor deben empezar con una llamada a super(props).

Ahora vamos a cambiar el método render de Square para mostrar el valor del estado actual cuando es clickeado:

```javascript
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button
        className="square"
        onClick={() => this.setState({value: 'X'})}
      >
        {this.state.value}
      </button>
    );
  }
}
```

Llamando a this.setState desde el manejador onClick en el método render de Square, decimos a React que re-renderice el cuadrado siempre que su <button> es clickeado. Luego de la actualización, el this.state.value del cuadrado será 'X', así que veremos X en el tablero de juego.

Cuando llamas `setState` en un componente, React actualiza automáticamente los componentes hijos dentro del mismo también.

### Herramientas de desarrollo

La extensión de **React Devtools** para Chrome y Firefox te permite inspeccionar el árbol de componentes de React con tus herramientas de desarrollo del navegador.

React Devtools
React DevTools te permite revisar las props y el estado de tus componentes de React.

Después de instalar React DevTools, puedes hacer click derecho en cualquier elemento de la página, click en “Inspeccionar elemento” para abrir las herramientas de desarrollo, y la pestaña de React aparecerá como la última pestaña a la derecha. Usa “⚛️ Components” para inspeccionar el árbol de componentes.


## Completando el juego

Ahora tenemos los bloques de construcción básicos para nuestro juego tic-tac-toe. Para completar el juego, necesitamos alternar colocando “X” y “O” en el tablero, y necesitas una forma de determinar el ganador.

### Elevando el estado

Actualmente, cada componente Square mantiene el estado del juego. Para determinar un ganador, necesitamos mantener el valor de cada uno de los 9 cuadrados en un solo lugar.

Podemos pensar que el tablero debería solo preguntar a cada cuadrado por su estado. Aunque este enfoque es posible en React, te incentivamos a que no lo uses porque el código se vuelve difícil de ententer, susceptible a errores, y difícil de refactorizar. En su lugar, el mejor enfoque es almacenar el estado del juego en el componente padre Board en vez de cada componente Square. El componente Board puede decirle a cada cuadrado que mostrar pasándole una prop tal cual hicimos cuando pasamos un número a cada cuadrado.

**Para recopilar datos de múltiples hijos, o tener dos componentes hijos comunicados entre sí, necesitas declarar el estado compartido en su componente padre. El componente padre puede pasar el estado hacia los hijos usando props; esto mantiene los componentes hijos sincronizados entre ellos y con su componente padre.**

Elevar el estado al componente padre es común cuando componentes de React son refactorizados, vamos a tomar esta oportunidad para intentarlo.

Añade un constructor al Board y establece el estado inicial de Board para contener un arreglo con 9 valores null. Estos 9 nulls corresponden a los 9 cuadrados:

```javascript
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
    };
  }

  renderSquare(i) {
    return <Square value={i} />;
  }
```

Cuando rellenemos el tablero luego, el arreglo this.state.squares se verá algo así:

```javascript
[
  'O', null, 'X',
  'X', 'X', 'O',
  'O', null, null,
]
```

El método renderSquare del componente Board actualmente se ve así:

```javascript
renderSquare(i) {
    return <Square value={i} />;
  }
```

Ahora usaremos la prop pasando el mecanismo otra vez. Modificaremos el Board para instruir cada Square acerca de su valor actual ('X', 'O', ó null). Ya tenemos definido el arreglo squares en el constructor del Board, y modificaremos el método renderSquare para que lo lea desde allí:

```javascript
  renderSquare(i) {
    return <Square value={this.state.squares[i]} />;
  }
```

Cada Square ahora recibirá una prop value que será `'X'`, `'O'`, ó `null` para cuadrados vacíos.

Luego, necesitamos cambiar lo que sucede cuando un cuadrado es clickeado. El componente Board ahora mantiene qué cuadrados están rellenos. Necesitamos crear una forma para que el cuadrado actualice el estado del componente Board. ==Debido a que el estado es considerado privado al componente que lo define, no podemos actualizar el estado de Board directamente desde Square==.

En cambio, pasaremos una función como prop desde Board a Square y haremos que Square llame a esa función cuando un cuadrado sea clickeado. Cambiaremos el método `renderSquare` en Board a:

```javascript
renderSquare(i) {
    return (
      <Square
        value={this.state.squares[i]}
        onClick={() => this.handleClick(i)}
      />
    );
  }
```

Ahora estamos pasando dos props desde Board a Square: value y onClick. la prop onClick es una función que Square puede llamar cuando sea clickeado. Haremos los siguientes cambios a Square:

* Reemplazar `this.state.value` por `this.props.value` en el método render de Square
* Reemplazar `this.setState()` por `this.props.onClick()` en el método render de Square
* Eliminar el `constructor` de Square porque el componente ya no hace seguimiento del estado del juego

Luego de estos cambios, el componente Square se ve algo así:

```javascript
class Square extends React.Component {
  render() {
    return (
      <button
        className="square"
        onClick={() => this.props.onClick()}
      >
        {this.props.value}
      </button>
    );
  }
}
```

Cuando un cuadrado es clickeado, la función `onClick` provista por el componente Board es llamada. Aquí un repaso de cómo esto fue logrado:

1. La prop `onClick` en el componente pre-construido del DOM `<button>` le dice a React para establecer un escuchador del evento click.

2. Cuando el botón es clickeado, React llamará al manejador de evento onClick que está definido en el método render() de Square.

3. Este manejador de evento llama a this.props.onClick(). la prop onClick del componente Square fue especificado por el componente Board.

4. Debido a que el Board pasó onClick={() => this.handleClick(i)} a Square, el componente Square llama al handleClick(i) de Board cuando es clickeado.

5. No tenemos definido el método `handleClick()` aun, así que nuestro código falla. Si haces click ahora verás una pantalla roja de error que dice algo como “this.handleClick is not a function” (this.handleClick no es una función).

Nota: 

> El atributo `onClick` del elemento `<button>` del DOM tiene un significado especial para React porque es un componente pre-construido. Para componentes personalizados como Square, la nomenclatura la decides tú. Podríamos darle cualquier nombre al prop `onClick` de Square o al método `handleClick` de Board, y el código funcionaría de la misma forma. En React, sin embargo, es una convención usar los nombres `on[Evento]` para props que representan eventos y `handle[Evento]` para los métodos que manejan los eventos.

Cuando intentamos clickear un cuadrado, deberíamos obtener un error porque no hemos definido handleClick aun. Vamos ahora a agregar handleClick a la clase Board:

```javascript
handleClick(i) {
    const squares = this.state.squares.slice();
    squares[i] = 'X';
    this.setState({squares: squares});
  }
```

Luego de estos cambios, podemos nuevamente clickear en los cuadrados para rellenarlos de la misma forma que lo hicimos antes. Sin embargo, ahora el estado está almacenado en el componente Board en lugar de cada componente Square. Cuando el estado del Board cambia, los componentes Square se re-renderiza automáticamente. Mantener el estado de todos los cuadrados en el componente Board nos permitirá determinar el ganador en el futuro.

Debido a que el componente Square ahora no mantiene estado, los componentes Square reciben valores del componente Board e informan al mismo cuando son clickeados. ==En términos de React, los componentes Square ahora son **componentes controlados**. El componente Board tiene control completo sobre ellos==.

Notar cómo en `handleClick`, llamamos `.slice()` para crear una copia del array de squares para modificarlo en vez de modificar el array existente. Ahora explicaremos porqué crear una copia del array squares en la siguiente sección.