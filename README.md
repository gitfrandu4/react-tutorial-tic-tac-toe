# Tutorial: Introducción a React

> https://es.reactjs.org/tutorial/tutorial.html

## Visión General

**¿Qué es React?**

React es una librería de JavaScript declarativa, eficiente y flexible para construir interfaces de usuario. Permite componer IUs complejas de pequeñas y aisladas piezas de código llamadas “componentes”.

React tiene pocos tipos diferentes de componentes, pero vamos a empezar con la subclase React.Component:

´´´javascript
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

Square
Board
Game
El componente Square renderiza un simple <button> y el Board renderiza 9 cuadrados. El componente Game renderiza un table con valores de posición por defecto que modificaremos luego. Actualmente no hay componentes interactivos.

### Pasando datos a través de props

### Haciendo un componente interactivo

### Herramientas de desarrollo

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

