REACT conceptos principales

- [1. Hello World](#Hello-World)
- [2. Introducing JSX](#Introducing-JSX)
- [3. Rendering Elements](#Rendering-Elements)
- [4. Components and Props](#Components-and-Props)
- [5. State and Lifecycle](#State-and-Lifecycle)
- [6. Handling Events](#Handling-Events)
- [7. Conditional Rendering](#Conditional-Rendering)
- [8. Lists and Keys](#Lists-and-Keys)
- [9. Forms](#Forms)
- [10. Lifting State Up](#Lifting-State-Up)
- [11. Composition vs Inheritance](#Composition-vs-Inheritance)
- [12. Thinking In React](#Thinking-In-React)


# Hello World

    El ejemplo más pequeño de React se ve así:

```js
ReactDOM.render (
   <h1> ¡Hola, mundo! </ h1>,
   document.getElementById ('root')
);
```

Muestra un encabezado que dice "¡Hola, mundo!" En la página.

[Pruébalo en CodePen](https://reactjs.org/redirect-to-codepen/hello-world)

Haga clic en el enlace de arriba para abrir un editor en línea. 
Siéntete libre de hacer algunos cambios y ver cómo afectan la salida. 
La mayoría de las páginas de esta guía tendrán ejemplos editables como este.

# Introducing JSX

Considere esta declaración de variable:

```js
const element = <h1> ¡Hola, mundo! </ h1>;
```

Esta sintaxis de etiqueta divertida no es ni una cadena ni HTML.

Se llama JSX y es una extensión de sintaxis de JavaScript. 
Recomendamos usarlo con React para describir cómo debería ser la interfaz de usuario. 
JSX puede recordarte un lenguaje de plantilla, pero viene con todo el poder de JavaScript.

JSX produce React "elements". Exploraremos su representación en DOM en la siguiente sección. A continuación, puede encontrar los conceptos básicos de JSX necesarios para que pueda comenzar.


**Por qué JSX?**

React adopta el hecho de que la lógica de renderizado está inherentemente acoplada con otra lógica de UI: cómo se manejan los eventos, cómo cambia el estado a lo largo del tiempo y cómo se preparan los datos para su visualización.

En lugar de separar artificialmente las tecnologías al poner marcado y lógica en archivos separados, React separa las preocupaciones con unidades débilmente acopladas llamadas "componentes" que contienen ambos. 

Volveremos a los componentes en una sección adicional, pero si todavía no se siente cómodo poniendo marcado en JS, esta conversación podría convencerlo de lo contrario.

React no requiere el uso de JSX, pero la mayoría de las personas lo encuentran útil como ayuda visual cuando se trabaja con la IU dentro del código JavaScript. También permite a React mostrar mensajes de error y advertencia más útiles.

¡Con eso fuera del camino, empecemos!

**Incrustemos expresiones en JSX**

En el ejemplo siguiente, declaramos una variable llamada nombre y luego la usamos dentro de JSX envolviéndola con llaves:

```js
const name = 'Josh Perez';
const element = <h1> Hola, {nombre} </ h1>;

ReactDOM.render (
   elemento,
   document.getElementById ('root')
);
```

Puede poner cualquier expresión de JavaScript válida dentro de las llaves en JSX. Por ejemplo, 2 + 2, user.firstName o formatName (user) son expresiones de JavaScript válidas.

En el siguiente ejemplo, incorporamos el resultado de llamar a una función de JavaScript, formatName (usuario), en un elemento ``` <h1> ```

```js
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, 
{formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

Dividimos JSX en varias líneas para facilitar la lectura. 
Si bien no es necesario, al hacer esto, también recomendamos envolverlo entre paréntesis para evitar los inconvenientes de la inserción automática de punto y coma.

**JSX es una expresión también**

Después de la compilación, las expresiones JSX se convierten en llamadas regulares a funciones de JavaScript y se evalúan en objetos JavaScript.

Esto significa que puede usar JSX dentro de sentencias if y para bucles, asignarlo a variables, aceptarlo como argumentos y devolverlo desde funciones:

```js
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

**Especificación de atributos con JSX**

Puede usar comillas para especificar literales de cadena como atributos:

```js 
const element = <div tabIndex = "0"> </ div>;
```

También puede usar llaves para insertar una expresión de JavaScript en un atributo:

```js 
const element = <img src = {user.avatarUrl}> </ img>;
```

No coloque comillas alrededor de las llaves cuando incorpore una expresión de JavaScript en un atributo. Debería usar comillas (para valores de cadena) o llaves (para expresiones), pero no ambas en el mismo atributo.


**Warning:**
```
Dado que JSX está más cerca de JavaScript que de HTML, React DOM utiliza la convención de nombres de propiedad camelCase en lugar de nombres de atributos HTML.

Por ejemplo, la clase se convierte en className en JSX y tabindex se convierte en tabIndex. 

background-color -> backgroundColor, 
font-size -> fontSize etc.
```

**Especificando Children con JSX**

Si una etiqueta está vacía, puede cerrarla inmediatamente con />, como XML:

```js
const element = <img src={user.avatarUrl} />;
Las etiquetas JSX pueden contener niños:

elemento const = (
   <div>
     <h1> ¡Hola! </ h1>
     <h2> Me alegro de verte aquí. </ h2>
   </ div>
);
```
**JSX previene los ataques de inyección**

Es seguro insertar la información del usuario en JSX:

```js
const title = response.potentiallyMaliciousInput;
// Esto es seguro:
const element = <h1> {title} </ h1>;
```

De forma predeterminada, React DOM escapa cualquier valor incrustado en JSX antes de representarlos. Por lo tanto, garantiza que nunca podrá inyectar nada que no esté escrito explícitamente en su aplicación. Todo se convierte en una cadena antes de ser renderizado. Esto ayuda a prevenir ataques XSS **(cross-site-scripting).**

**JSX representa objetos**

Babel compila JSX hasta llamadas a React.createElement ().

Estos dos ejemplos son idénticos:

```js
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```javascript
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

React.createElement () realiza algunas comprobaciones para ayudarte a escribir código libre de errores pero esencialmente crea un objeto como este:

```js
// Nota: esta estructura esta simplificada
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

Estos objetos se llaman "React Elements". Puedes pensar en ellos como descripciones de lo que quieres ver en la pantalla. React lee estos objetos y los usa para construir el DOM y mantenerlo actualizado.

Exploraremos la representación de los elementos de React en el DOM en la siguiente sección.



# Rendering Elements

Muestra "Hola, mundo" en la página.

Actualizar el elemento renderizado
Los elementos de reacción son inmutables. Una vez que crea un elemento, no puede cambiar sus elementos secundarios o atributos. Un elemento es como un único fotograma en una película: representa la UI en un momento determinado.

Con nuestro conocimiento hasta ahora, la única forma de actualizar la UI es crear un nuevo elemento y pasarlo a ReactDOM.render ().

Considere este ejemplo de reloj que hace tictac:

```js
función tick() {
  const element = (
    <div>
      <h1>¡Hola, mundo!</ h1>
      <h2> Es {new Date().toLocaleTimeString() }.</ h2>
    </ div>
  );
  ReactDOM.render (element, document.getElementById ('root'));
}

setInterval(tick, 1000);
```

[Pruébalo en CodePen](https://reactjs.org/redirect-to-codepen/rendering-elements/update-rendered-element)

Llama a ReactDOM.render () cada segundo desde una devolución de llamada setInterval ().

**Nota:**
```
En la práctica, la mayoría de las aplicaciones de React solo llaman a ReactDOM.render () una vez. En las siguientes secciones, aprenderemos cómo dicho código se encapsula en componentes con estado.
```
Recomendamos que no se salte temas porque se complementan entre sí.


**React solo actualiza Lo que es necesario**

React DOM compara el elemento y sus elementos secundarios con el anterior, y solo aplica las actualizaciones DOM necesarias para llevar el DOM al estado deseado.

Puede verificar inspeccionando el último ejemplo con las herramientas del navegador:

Inspector DOM que muestra actualizaciones granulares

A pesar de que creamos un elemento que describe todo el árbol de la interfaz de usuario en cada tic, solo el nodo de texto cuyo contenido ha cambiado se actualiza por Reaccionar DOM.

En nuestra experiencia, pensar en cómo debe verse la IU en un momento dado en lugar de cómo cambiarla a lo largo del tiempo elimina toda una clase de errores.


![alt text][logo]

[logo]: https://reactjs.org/granular-dom-updates-c158617ed7cc0eac8f58330e49e48224.gif "Ejemplo de Reloj"


# Components and Props

Los componentes le permiten dividir la IU en piezas independientes y reutilizables, y pensar en cada pieza aisladamente.
Esta página proporciona una introducción a la idea de los componentes. Aquí puede encontrar una referencia de componente API detallada.

Conceptualmente, los componentes son como las funciones de JavaScript. Aceptan entradas arbitrarias (llamadas "apoyos") y devuelven elementos React que describen lo que debería aparecer en la pantalla.

**Componentes funcionales y de clase**

La forma más sencilla de definir un componente es escribir una función de JavaScript:

```js
función de bienvenida (accesorios) {
  return <h1> Hola, {props.name} </h1>;
}
```

Esta función es un componente React válido porque acepta un único argumento de objeto "props" (que significa propiedades) con datos y devuelve un elemento React. 
Llamamos a dichos componentes "funcionales" porque son, literalmente, funciones de JavaScript.

También puede usar una clase ES6 para definir un componente:

```js
La clase Welcome extiende React.Component {
  render () {
    return <h1> Hola, {this.props.name} </h1>;
  }
}
```

Los dos componentes anteriores son equivalentes desde el punto de vista de React.

Las clases tienen algunas características adicionales que discutiremos en las siguientes secciones. Hasta entonces, usaremos componentes funcionales por su concisión.

**Renderizar un componente**

Anteriormente, solo encontrábamos elementos React que representan etiquetas DOM.

```js
const element = <div/>;
```

Sin embargo, los elementos también pueden representar componentes definidos por el usuario:

```js
const element = <Welcome name = "Sara" />;
```

Cuando React ve un elemento que representa un componente definido por el usuario, pasa atributos JSX a este componente como un solo objeto. Llamamos a este objeto **"props".**

Por ejemplo, este código muestra "Hola, Sara" en la página:

```js
function welcome (props) {
  return <h1> Hola, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

[Pruébalo en CodePen](https://reactjs.org/redirect-to-codepen/components-and-props/rendering-a-component)

Repasemos lo que sucede en este ejemplo:

Llamamos a ```ReactDOM.render()``` con el elemento ```<Welcome name="Sara" />```.
React llama al componente Bienvenido con ```{name: 'Sara'}``` como los accesorios.
Nuestro componente Bienvenido devuelve un elemento ```<h1>``` Hola, Sara ```</h1> ``` como resultado.
React DOM actualiza de manera eficiente el DOM para que coincida con ```<h1>``` Hola, Sara ```</h1>```.
Nota: siempre comience los nombres de los componentes con una letra mayúscula.

React trata los componentes comenzando con letras minúsculas como etiquetas DOM. 
Por ejemplo, ```<div/>``` representa una etiqueta div HTML, pero ```<Welcome/>``` representa un componente y requiere que Welcome esté dentro del alcance.

Puede leer más sobre el razonamiento detrás de esta convención aquí.

**Componiendo componentes**

Los componentes pueden referirse a otros componentes en su salida. Esto nos permite usar la misma abstracción de componentes para cualquier nivel de detalle. Un botón, un formulario, un diálogo, una pantalla: en las aplicaciones Reaccionar, todos esos se expresan comúnmente como componentes.

Por ejemplo, podemos crear un componente de la aplicación que rinda Bienvenida muchas veces:

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

[Pruébalo en CodePen](https://reactjs.org/redirect-to-codepen/components-and-props/composing-components)


Por lo general, las nuevas aplicaciones de React tienen un solo componente de aplicación en la parte superior. Sin embargo, si integra React en una aplicación existente, puede comenzar de abajo hacia arriba con un pequeño componente como Botón y gradualmente ir hacia la parte superior de la jerarquía de vista.

**Extracción de componentes**

No tengas miedo de dividir los componentes en componentes más pequeños.

Por ejemplo, considere este componente Comentario:


```js 
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

[Pruébalo en CodePen](https://reactjs.org/redirect-to-codepen/components-and-props/composing-components)

Acepta autor (un objeto), texto (una cadena) y fecha (una fecha) como props, y describe un comentario en un sitio web de redes sociales.

Este componente puede ser difícil de cambiar debido a todo el anidamiento, y también es difícil reutilizar partes individuales de él. Vamos a extraer algunos componentes de él.

Primero, vamos a extraer Avatar:


```js 
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />

  );
}
```


El Avatar no necesita saber que se está procesando dentro de un Comentario. Es por eso que le hemos dado a su **prop** un nombre más genérico: usuario en lugar de autor.

Recomendamos el nombramiento de **props** desde el punto de vista del componente en lugar del contexto en el que se utiliza.

Ahora podemos simplificar el comentario un poquito: 

```js
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        
      <Avatar user={props.author} />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

A continuación, extraeremos un componente UserInfo que representa un Avatar junto al nombre del usuario:

```js
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```

Esto nos permite simplificar el comentario aún más:

```js 
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

[Pruébalo en CodePen](https://reactjs.org/redirect-to-codepen/components-and-props/extracting-components-continued)

La extracción de componentes puede parecer un trabajo pesado al principio, pero tener una paleta de componentes reutilizables vale la pena en aplicaciones más grandes. Una buena regla general es que si una parte de su UI se usa varias veces (Button, Panel, Avatar), o es lo suficientemente compleja por sí misma (App, FeedStory, Comment), es un buen candidato para ser un componente reutilizable.

**Los props son de solo lectura**

Ya sea que declare un componente como una función o una clase, nunca debe modificar sus propios accesorios. Considera esta función de suma:

```js
function sum(a, b) {
  return a + b;
}
```

Tales funciones se llaman "puras" porque no intentan cambiar sus entradas, y siempre devuelven el mismo resultado para las mismas entradas.

Por el contrario, esta función es impura porque cambia su propia entrada:

```js
function withdraw(account, amount) {
  account.total -= amount;
}
```

React es bastante flexible pero tiene una sola regla estricta:
Todos los componentes de React deben actuar como funciones puras con respecto a sus accesorios.

Por supuesto, las IU de aplicaciones son dinámicas y cambian con el tiempo. En la siguiente sección, presentaremos un nuevo concepto de "estado". Estado permite que los componentes React cambien su salida a lo largo del tiempo en respuesta a acciones del usuario, respuestas de red y cualquier otra cosa, sin violar esta regla.

# State and Lifecycle

**Esta página presenta el concepto de estado y ciclo de vida en un componente React. Aquí puede encontrar una referencia de componente API detallada.**

Considere el ejemplo de reloj de ```tic-tac``` de una de las secciones anteriores. En Rendering Elements, solo hemos aprendido una forma de actualizar la UI. Llamamos a ```ReactDOM.render()``` para cambiar el resultado representado:

```js
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

En esta sección, aprenderemos a hacer que el componente Clock sea realmente reutilizable y encapsulado. Configurará su propio temporizador y se actualizará cada segundo.

Podemos comenzar por encapsular cómo se ve el reloj:

```js
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
      </div>
  );
}

function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

Sin embargo, se pierde un requisito crucial: el hecho de que el reloj configure un temporizador y actualice la interfaz de usuario cada segundo debe ser un detalle de implementación del reloj.

Idealmente, queremos escribir esto una vez y tener la actualización del reloj en sí:

```js
ReactDOM.render (
   <Reloj />,
   document.getElementById ('root')
);
```

Para implementar esto, necesitamos agregar "state" al componente Reloj.

El estado es similar a los accesorios, pero es privado y está completamente controlado por el componente.

Anteriormente mencionamos que los componentes definidos como clases tienen algunas características adicionales. El estado local es exactamente eso: una característica disponible solo para las clases.

**Converting a Function to a Class**

**Convirtiendo una función en una clase**

Puede convertir un componente funcional como Reloj en una clase en cinco pasos:

- 1. Cree una clase ES6, con el mismo nombre, que extienda React.Component.
- 2. Agregue un solo método vacío llamado ```render().```
- 3. Mueva el cuerpo de la función al método ```render().```
- 4. Reemplace los accesorios con ```this.props``` en el cuerpo de ```render().```
- 5. Borre la declaración de la función vacía restante.

```js
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

[Try it on CodePen](http://codepen.io/gaearon/pen/zKRGpo?editors=0010)


Clock ahora se define como una clase en lugar de una función.

Se llamará al método de representación cada vez que se produzca una actualización, pero siempre que hagamos que <Clock /> en el mismo nodo DOM, solo se utilizará una instancia única de la clase Clock. Esto nos permite usar funciones adicionales como el estado local y los ganchos de ciclo de vida.

Agregar estado local a una clase
Pasaremos la fecha de los accesorios al estado en tres pasos:

- 1. Reemplace ```this.props.date``` con ```this.state.date``` en el método ```render():```

```js 
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        
<h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

- 2. Add a ```class constructor``` that assigns the initial ```this.state:```

```js 
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

Tenga en cuenta cómo pasamos los accesorios al constructor base:

```js
constructor(props) {
  super(props);
  this.state = {date: new Date()};
}
```

Los componentes de la clase siempre deben llamar al constructor base con **props.**

- 3. Elimine el prop de fecha del elemento ```<Reloj />:```

```js
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

Posteriormente agregaremos el código del temporizador al componente en sí.

El resultado es así:

```js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

[Pruébalo en CodePen](http://codepen.io/gaearon/pen/KgQpJd?editors=0010)

A continuación, haremos que el reloj configure su propio temporizador y se actualice cada segundo.


**Agregar métodos de ciclo de vida a una clase**

En aplicaciones con muchos componentes, es muy importante liberar recursos tomados por los componentes cuando se destruyen.

Queremos configurar un temporizador cada vez que el reloj se representa en el DOM por primera vez. Esto se llama "montaje" en REACT.

También queremos borrar ese temporizador siempre que se elimine el DOM producido por el reloj. Esto se llama "desmontar" en REACT.

Podemos declarar métodos especiales en la clase de componente para ejecutar algún código cuando un componente se monta y desmonta:

```js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    //Metodo que se ejecuta cuando el componente se ha montado
  }

  componentWillUnmount() {
    //Metodo que se ejecuta cuando el componente se ha desmontado
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
} 
```

Estos métodos se denominan "ganchos de ciclo de vida".

El gancho ```componentDidMount()``` se ejecuta después de que la salida del componente se haya procesado en el DOM. Este es un buen lugar para configurar un temporizador:

```js
 componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
```

Tenga en cuenta cómo guardamos la ID del temporizador en este.

Mientras que ```this.props``` está configurado por React y ```this.state``` tiene un significado especial, puede añadir campos adicionales a la clase manualmente si necesita almacenar algo que no participa en el flujo de datos **(como una ID de temporizador).** )

Desmontaremos el temporizador en el gancho del ciclo de vida ```componentWillUnmount()```:

```js 
componentWillUnmount() {
    clearInterval(this.timerID);
}
```

Finalmente, implementaremos un método llamado ```tick()``` que el componente Clock se ejecutará cada segundo.

Utilizará ```this.setState()``` para programar actualizaciones en el estado local del componente:

```js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

[Pruébalo en CodePen](https://codepen.io/gaearon/pen/amqdNA?editors=0010)

Ahora el reloj marca cada segundo.

Repasemos rápidamente lo que está sucediendo y el orden en que se llaman los métodos:

- 1. Cuando ```<Clock />``` se pasa a ```ReactDOM.render()```, React llama al constructor del componente Clock. Como Clock necesita mostrar la hora actual, inicializa this.state con un objeto que incluye la hora actual. Más adelante actualizaremos este estado.

- 2. React luego llama al método ```render()``` del componente Clock. Así es como React aprende lo que debe mostrarse en la pantalla. React luego actualiza el DOM para que coincida con la salida de render del Reloj.

- 3. Cuando la salida del reloj se inserta en el DOM, React llama al gancho del ciclo de vida ```componentDidMount()```. Dentro de él, el componente Reloj le pide al navegador que configure un temporizador para llamar al método ```tick()``` del componente una vez por segundo.

- 4. Cada segundo el navegador llama al método ```tick()```. Dentro de él, el componente Clock programa una actualización de UI llamando a ```setState()``` con un objeto que contiene la hora actual. Gracias a la llamada a ```setState()```, React sabe que el estado ha cambiado y llama al método de ```render()``` para aprender qué debería estar en la pantalla. Esta vez, ```this.state.date``` en el método ```render()``` será diferente, por lo que la salida del renderizado incluirá la hora actualizada. React actualiza el DOM en consecuencia.

- 5.  Si el componente Clock se elimina alguna vez del DOM, React llama al hook del ciclo de vida ```componentWillUnmount()``` para que el temporizador se detenga.


**Usar el estado correctamente**

Hay tres cosas que debe saber sobre setState ().

**No modifique el estado directamente**

Por ejemplo, esto no volverá a representar un componente:

```js 
// Incorrecto
this.state.comment = 'Hola';
```

En su lugar, use ```setState()```:

```js
//Correcto
this.setState ({comment: 'Hello'});
```

El único lugar donde puede asignar this.state es el constructor.

Las actualizaciones de estado pueden ser asíncronas
React puede agrupar varias llamadas ```setState()``` en una sola actualización para el rendimiento.

Como ```this.props``` y ```this.state``` se pueden actualizar de forma asincrónica, no debe confiar en sus valores para calcular el siguiente estado.

Por ejemplo, este código puede no actualizar el contador:

```js
// Incorrecto
this.setState ({
  counter: this.state.counter + this.props.increment,
});
```

Para solucionarlo, use una segunda forma de setState() que acepte una función en lugar de un objeto. Esa función recibirá el estado anterior como primer argumento, y los puntales en el momento en que se aplica la actualización como segundo argumento:

```js
// Correcto
this.setState ((state, props) => ({
  counter: state.counter + props.increment
}));
```

Usamos una función de flecha arriba, pero también funciona con funciones regulares:

```js
// Correcto
this.setState (function (state, props) {
  return {
    counter: state.counter + props.increment
  };
});
```

Las actualizaciones de estado se fusionan

Cuando llama a ```setState()```, React fusiona el objeto que proporciona en el estado actual.

Por ejemplo, su estado puede contener varias variables independientes:

```js 
constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
```

Luego puede actualizarlos de manera independiente con llamadas setState () separadas:

```js
 componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```

La fusión es superficial, por lo que ```this.setState({comments})``` deja intactos a ```this.state.posts```, pero reemplaza completamente a ```this.state.comments```.

### La información fluye hacia abajo

Ni los componentes primarios ni secundarios pueden saber si un componente determinado es stateless o statefull, y no deberían importar si se define como una función o una clase.


Esta es la razón por la cual el ```state``` a menudo se llama local o encapsulado. No es accesible a ningún componente que no sea el que posee y lo establece.

Un componente puede elegir pasar su estado como props a sus componentes hijos:

```js 
  <h2> Es {this.state.date.toLocaleTimeString ()}. </ h2>
```

Esto también funciona para los componentes definidos por el usuario:

```js
<FormattedDate date = {this.state.date} />
```

El componente ```FormattedDate``` recibiría la fecha en sus accesorios y no sabría si proviene del estado del reloj, de los puntales del reloj o si se escribió a mano:

```js
function FormattedDate(props) {
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}
```

[Pruébalo en CodePen](http://codepen.io/gaearon/pen/zKRqNB?editors=0010)

Esto se conoce comúnmente como flujo de datos "descendente" o "unidireccional". Cualquier estado siempre es propiedad de algún componente específico, y cualquier dato o IU derivado de ese estado solo puede afectar los componentes "debajo" de ellos en el árbol.

Si imagina un árbol de componentes como una cascada de **props**, el estado de cada componente es como una fuente de agua adicional que lo une en un punto arbitrario pero también fluye hacia abajo.

Para mostrar que todos los componentes están verdaderamente aislados, podemos crear un componente de la aplicación que represente tres <Clock> s:

```js
function App() {
  return (
    <div>
      <Clock />
      <Clock />
      <Clock />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

[Pruébalo en CodePen](http://codepen.io/gaearon/pen/vXdGmd?editors=0010)


Cada reloj configura su propio temporizador y actualizaciones de forma independiente.

En las aplicaciones de React, si un componente es con estado o sin estado se considera un detalle de implementación del componente que puede cambiar con el tiempo. Puede usar componentes sin estado dentro de componentes con estado, y viceversa.

# Handling Events

Manejar eventos con elementos React es muy similar a manejar eventos en elementos DOM. Hay algunas diferencias sintácticas:

Los eventos React se nombran usando camelCase, en lugar de minúsculas.
Con JSX pasas una función como el controlador de eventos, en lugar de una cadena.
Por ejemplo, el HTML:

```html
<button onclick="activateLasers()">
    Activar láseres
</button>
```

es ligeramente diferente en React:

```html 
<button onClick={activateLasers} >
   Activar láseres
</button>
```

Otra diferencia es que no puede devolver falso para evitar el comportamiento predeterminado en REACT. 
Debe llamar preventDefault explícitamente. Por ejemplo, con HTML simple, para evitar el comportamiento de enlace predeterminado de abrir una página nueva, puede escribir:

```html
<a href="#" onclick="console.log('El vínculo se hizo clic.'); return false">
  Haz click en mi
</a>
```

En React, esto podría ser:

```js
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```

Aquí, e es un evento sintético. React define estos eventos sintéticos de acuerdo con la especificación W3C, por lo que no tiene que preocuparse por la compatibilidad entre navegadores. Consulte la guía de referencia SyntheticEvent para obtener más información.

Cuando se usa React, generalmente no necesita llamar a addEventListener para agregar oyentes a un elemento DOM después de haber sido creado. En su lugar, solo proporcione un oyente cuando el elemento se represente inicialmente.

Cuando define un componente utilizando una clase ES6, un patrón común es que un controlador de eventos sea un método en la clase. Por ejemplo, este componente de alternancia representa un botón que le permite al usuario alternar entre los estados "ON" y "OFF":


```js
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // Este binding es necesario para hacer que `this` trabaje en el callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

[Pruébalo en CodePen](http://codepen.io/gaearon/pen/xEmzGg?editors=0010)

Debe tener cuidado con el significado de esto en las devoluciones de llamada JSX. 
En JavaScript, los métodos de clase no están vinculados por defecto. Si olvida vincular ```this.handleClick``` y pasarlo a ```onClick```, esto no estará definido cuando realmente se llame a la función.

Esto no es un comportamiento específico de React; es una parte de cómo funcionan las funciones en `JavaScript`. En general, si se refiere a un método sin () después de él, como `onClick = {this.handleClick}`, debe vincular ese método.

Si llamar a bind le molesta, hay dos maneras de evitarlo. Si está utilizando la sintaxis experimental de campos de clase pública, puede usar los campos de clase para enlazar correctamente las devoluciones de llamada:

```js 
class LoggingButton extends React.Component {
  // Esta sintaxis asegura que `this` está vinculado dentro de handleClick.
  // Advertencia: esta es la sintaxis * experimental *.
  handleClick = () => {
    console.log('this is:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
```

Esta sintaxis está habilitada por defecto en la aplicación Create React.

Si no usa la sintaxis de los campos de clase, puede utilizar una función de flecha en la devolución de llamada:

```js
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // Esta sintaxis asegura que `this` está enlazado dentro de handleClick
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
}
```

El problema con esta sintaxis es que se crea una devolución de llamada diferente cada vez que se representa el LoggingButton. En la mayoría de los casos, esto está bien. Sin embargo, si esta devolución de llamada se pasa como un prop a los componentes inferiores, esos componentes podrían hacer una nueva representación adicional. En general, recomendamos enlazar en el constructor o usar la sintaxis de los campos de clase, para evitar este tipo de problema de rendimiento.

Pasar argumentos a los controladores de eventos
Dentro de un ciclo, es común querer pasar un parámetro adicional a un controlador de eventos. 

Por ejemplo, si id es la identificación de la fila, cualquiera de los siguientes funcionaría:

```html
<button onClick={(e) => this.deleteRow(id, e)}>
  Delete Row
</button>

<button onClick={this.deleteRow.bind(this, id)}>
  Delete Row
</button>
```

Las dos líneas anteriores son equivalentes, y usan las funciones de flecha y `Function.prototype.bind` respectivamente.


En ambos casos, el argumento e que representa el evento React se pasará como un segundo argumento después del ID. Con una función de flecha, tenemos que pasarla explícitamente, pero con el enlace cualquier argumento adicional se reenvía automáticamente.

# Conditional Rendering

En React, puedes crear componentes distintos que encapsulan el comportamiento que necesitas. Luego, puede procesar solo algunos de ellos, dependiendo del estado de su aplicación.

La representación condicional en React funciona de la misma forma que las condiciones funcionan en JavaScript. 
Utilice operadores JavaScript como if o el operador condicional para crear elementos que representen el estado actual, y permita que React actualice la interfaz de usuario para que coincida con ellos.

Considere estos dos componentes:

```js
function UserGreeting(props) {
  return <h1>Welcome back!</h1>;
}

function GuestGreeting(props) {
  return <h1>Please sign up.</h1>;
}
```


Crearemos un componente de Saludo que muestre cualquiera de estos componentes dependiendo de si un usuario está conectado:

```js
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}

ReactDOM.render(
  // Try changing to isLoggedIn={true}:
  <Greeting isLoggedIn={false} />,
  document.getElementById('root')
);
```

[Pruébalo en CodePen](https://codepen.io/gaearon/pen/ZpVxNq?editors=0011)


Este ejemplo muestra un saludo diferente según el valor de isLoggedIn prop.

**Variables de elemento**

Puede usar variables para almacenar elementos. Esto puede ayudarlo a representar condicionalmente una parte del componente mientras que el resto de la salida no cambia.

Considere estos dos nuevos componentes que representan los botones Cerrar sesión y Iniciar sesión:

```js
function LoginButton(props) {
  return (
    <button onClick={props.onClick}>
      Login
    </button>
  );
}

function LogoutButton(props) {
  return (
    <button onClick={props.onClick}>
      Logout
    </button>
  );
}
```

En el siguiente ejemplo, crearemos un componente con estado llamado LoginControl.

Presentará <LoginButton /> o <LogoutButton /> dependiendo de su estado actual. También renderizará un <Saludo /> del ejemplo anterior:


```js
class LoginControl extends React.Component {
  constructor(props) {
    super(props);
    this.handleLoginClick = this.handleLoginClick.bind(this);
    this.handleLogoutClick = this.handleLogoutClick.bind(this);
    this.state = {isLoggedIn: false};
  }

  handleLoginClick() {
    this.setState({isLoggedIn: true});
  }

  handleLogoutClick() {
    this.setState({isLoggedIn: false});
  }

  render() {
    const isLoggedIn = this.state.isLoggedIn;
    let button;

    if (isLoggedIn) {
      button = <LogoutButton onClick={this.handleLogoutClick} />;
    } else {
      button = <LoginButton onClick={this.handleLoginClick} />
    }

    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {button}
      </div>
    );
  }
}

ReactDOM.render(
  <LoginControl />,
  document.getElementById('root')
);
```

[Pruébalo en CodePen](https://codepen.io/gaearon/pen/QKzAgB?editors=0010)

Si bien declarar una variable y usar una instrucción if es una forma excelente de representar un componente condicionalmente, a veces es posible que desee utilizar una sintaxis más corta. Hay algunas formas de condiciones en línea en JSX, explicadas a continuación.

**En línea si con Logical && Operator**

Puede incrustar cualquier expresión en JSX enrollándolas con llaves. Esto incluye el operador lógico de JavaScript &&. Puede ser útil para incluir de manera condicional un elemento:

```js
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}

const messages = ['React', 'Re: React', 'Re:Re: React'];
ReactDOM.render(
  <Mailbox unreadMessages={messages} />,
  document.getElementById('root')
);
```

[Pruébalo en CodePen](https://codepen.io/gaearon/pen/ozJddz?editors=0010)

Funciona porque en JavaScript, la expresión ```verdadera && expression``` siempre se evalúa como expresión, y la expresión ``èxpressio && false``` siempre evalúa como `falsa`.

Por lo tanto, si la condición es `true`, el elemento justo después de `&&` aparecerá en la salida. Si es `false`, React lo ignorará y lo omitirá.

**Inline If-Else con operador condicional**

Otro método para representar elementos en línea de manera condicional es usar la condición de operador condicional de JavaScript. `condition ? true : false`

En el ejemplo siguiente, lo usamos para representar condicionalmente un pequeño bloque de texto.

```js
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
    </div>
  );
}
```

También se puede usar para expresiones más grandes, aunque es menos obvio lo que está sucediendo:

```js
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn ? (
        <LogoutButton onClick={this.handleLogoutClick} />
      ) : (
        <LoginButton onClick={this.handleLoginClick} />
      )}
    </div>
  );
}
```

Al igual que en JavaScript, depende de usted elegir un estilo apropiado según lo que usted y su equipo consideren más legible. También recuerde que cuando las condiciones se vuelvan demasiado complejas, podría ser un buen momento para extraer un componente.

**Evitar que los componentes se rendericen**

En casos excepcionales, es posible que desee que un componente se oculte a pesar de que fue procesado por otro componente. Para hacer esto, devuelve null en lugar de su salida de renderizado.

En el siguiente ejemplo, el `<WarningBanner />` se representa según el valor del **prop** llamado warn. Si el valor de prop es falso, entonces el componente no representa:

```js
function WarningBanner(props) {
  if (!props.warn) {
    return null;
  }

  return (
    <div className="warning">
      Warning!
    </div>
  );
}

class Page extends React.Component {
  constructor(props) {
    super(props);
    this.state = {showWarning: true};
    this.handleToggleClick = this.handleToggleClick.bind(this);
  }

  handleToggleClick() {
    this.setState(state => ({
      showWarning: !state.showWarning
    }));
  }

  render() {
    return (
      <div>
        <WarningBanner warn={this.state.showWarning} />
        <button onClick={this.handleToggleClick}>
          {this.state.showWarning ? 'Hide' : 'Show'}
        </button>
      </div>
    );
  }
}

ReactDOM.render(
  <Page />,
  document.getElementById('root')
);
```

[Pruébalo en CodePen](https://codepen.io/gaearon/pen/Xjoqwm?editors=0010)

La devolución nula del método de renderizado de un componente no afecta la activación de los métodos del ciclo de vida del componente. Por ejemplo, todavía se llamará a componentDidUpdate.

# Lists and Keys

Primero, repasemos cómo transforma las listas en JavaScript.

Dado el siguiente código, usamos la función `map()` para tomar una matriz de números y duplicar sus valores. Asignamos la nueva matriz devuelta por `map()` a la variable duplicada y la registramos:

```js
const numbers = [1, 2, 3, 4, 5];
const double = numbers.map ((number) => number * 2);
console.log (duplicado);
```

Este código registra [2, 4, 6, 8, 10] en la consola.

En React, la transformación de matrices en listas de elementos es casi idéntica.

Representación de múltiples componentes
Puede crear colecciones de elementos e incluirlos en JSX con llaves `{}`.

A continuación, recorremos la matriz de números usando la función JavaScript map (). Devolvemos un elemento `<li>` para cada artículo. Finalmente, asignamos la matriz resultante de elementos a listItems:

```js
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map ((number) =>
  <li> {number} </ li>
);
```

Incluimos todo el conjunto listItems dentro de un elemento `<ul>` y lo renderizamos al DOM:

```js
ReactDOM.render(
  <ul>{listItems}</ul>,
  document.getElementById('root')
);
```

[Pruébalo en CodePen](https://codepen.io/gaearon/pen/GjPyQr?editors=0011)

Este código muestra una lista de números entre 1 y 5.

Componente de lista básica
Por lo general, usted debe presentar listas dentro de un componente.

Podemos refactorizar el ejemplo anterior en un componente que acepte una matriz de números y genere una lista desordenada de elementos.

```js
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li>{number}</li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```


Cuando ejecuta este código, se le dará una advertencia de que se debe proporcionar una clave para los elementos de la lista. Una "clave" es un atributo de cadena especial que debe incluir al crear listas de elementos. Discutiremos por qué es importante en la siguiente sección.

Asignemos una clave a los elementos de nuestra lista dentro de `numbers.map()` y solucionemos el problema de la clave faltante.

```js
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>
      {number}
    </li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

[Pruébalo en CodePen](https://codepen.io/gaearon/pen/jrXYRR?editors=0011)

## Keys (llaves)

Las llaves ayudan a REACT a identificar qué elementos han cambiado, se han agregado o se han eliminado. Las llaves se deben dar a los elementos dentro de la matriz para dar a los elementos una identidad estable:

```js
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li key={number.toString()}>
    {number}
  </li>
);
```

La mejor forma de elegir una clave es usar una cadena que identifique de forma única un elemento de la lista entre sus hermanos. Muy a menudo usaría ID de sus datos como llaves:

```js
const todoItems = todos.map((todo) =>
  <li key={todo.id}>
    {todo.text}
  </li>
);
```

Cuando no tiene ID estables para los elementos representados, puede usar el índice del elemento como llave como último recurso:

```js
const todoItems = todos.map((todo, index) =>
  // Only do this if items have no stable IDs
  <li key={index}>
    {todo.text}
  </li>
);
```

No recomendamos utilizar índices para claves si el orden de los elementos puede cambiar. Esto puede afectar negativamente el rendimiento y puede causar problemas con el estado del componente. Consulte el artículo de Robin Pokorny para obtener una explicación detallada sobre los impactos negativos del uso de un índice como clave. Si opta por no asignar una clave explícita para enumerar los elementos, React utilizará los índices como claves de forma predeterminada.

Aquí hay una explicación detallada sobre por qué las claves son necesarias si está interesado en aprender más.

Extracción de componentes con llaves
Las claves solo tienen sentido en el contexto de la matriz circundante.

Por ejemplo, si extrae un componente ListItem, debe mantener la clave en los elementos <ListItem /> en la matriz en lugar de en el elemento <li> en el ListItem mismo.

Ejemplo: Uso de clave incorrecto

```js
function ListItem(props) {
  const value = props.value;
  return(
    // Wrong! There is no need to specify the key here:
    <li key={value.toString()}>
      {value}
    </li>
  );
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // Wrong! The key should have been specified here:
    <ListItem value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```


Ejemplo: uso correcto de la clave

```js
function ListItem(props) {
  // Correct! There is no need to specify the key here:
  return <li>{props.value}</li>;
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // Correct! Key should be specified inside the array.
    <ListItem key={number.toString()}
              value={number} />

  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

[Pruebalo en codePen](https://codepen.io/gaearon/pen/ZXeOGM?editors=0010)

Una buena regla general es que los elementos dentro de la llamada al map() necesitan keys.

Las llaves deben ser únicas entre los hermanos

Las claves utilizadas en las matrices deben ser únicas entre sus hermanos. Sin embargo, no necesitan ser globalmente únicos. Podemos usar las mismas teclas cuando producimos dos matrices diferentes:

```js
function Blog(props) {
  const sidebar = (
    <ul>
      {props.posts.map((post) =>
        <li key={post.id}>
          {post.title}
        </li>
      )}
    </ul>
  );
  const content = props.posts.map((post) =>
    <div key={post.id}>
      <h3>{post.title}</h3>
      <p>{post.content}</p>
    </div>
  );
  return (
    <div>
      {sidebar}
      <hr />
      {content}
    </div>
  );
}

const posts = [
  {id: 1, title: 'Hello World', content: 'Welcome to learning React!'},
  {id: 2, title: 'Installation', content: 'You can install React from npm.'}
];
ReactDOM.render(
  <Blog posts={posts} />,
  document.getElementById('root')
);
```


[Pruébalo en CodePen](https://codepen.io/gaearon/pen/NRZYGN?editors=0010)


Las claves sirven como una sugerencia para React, pero no pasan a sus componentes. Si necesita el mismo valor en su componente, páselo explícitamente como un accesorio con un nombre diferente:

```js
const content = posts.map((post) =>
  <Post
    key={post.id}
    id={post.id}
    title={post.title} />
);
```
Con el ejemplo anterior, el componente Publicar puede leer props.id, pero no props.key.

Incrustar map() en JSX
En los ejemplos anteriores, declaramos una variable listItems separada y la incluimos en JSX:

```js
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <ListItem key={number.toString()}
              value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}
```

JSX permite incrustar cualquier expresión en llaves para que podamos alinear el resultado del `map()`:

```js
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) =>
        <ListItem key={number.toString()}
                  value={number} />

      )}
    </ul>
  );
}
```

[Pruébalo en CodePen](https://codepen.io/gaearon/pen/BLvYrB?editors=0010)

A veces, esto da como resultado un código más claro, pero también se puede abusar de este estilo. Al igual que en JavaScript, depende de usted decidir si vale la pena extraer una variable para poder leerla. Tenga en cuenta que si el cuerpo del map() está demasiado anidado, podría ser un buen momento para extraer un componente.

# Forms

Los elementos de formulario HTML funcionan de forma un poco diferente de otros elementos DOM en React, porque los elementos de forma mantienen naturalmente un cierto estado interno. Por ejemplo, este formulario en HTML simple acepta un solo nombre:

```js
<form>
  <label>
    Name:
    <input type="text" name="name" />
  </label>
  <input type="submit" value="Submit" />
</form>
```

Este formulario tiene el comportamiento de forma HTML predeterminado de navegación a una nueva página cuando el usuario envía el formulario. Si quieres este comportamiento en React, simplemente funciona. Pero en la mayoría de los casos, es conveniente tener una función de JavaScript que maneje el envío del formulario y tenga acceso a los datos que el usuario ingresó en el formulario. La forma estándar de lograr esto es con una técnica llamada "componentes controlados".

Componentes controlados
En HTML, los elementos de formulario como `<input>`, `<textarea` y `<select>` suelen mantener su propio estado y actualizarlo en función de la entrada del usuario. En React, el estado mutable generalmente se mantiene en la propiedad estatal de los componentes, y solo se actualiza con `setState()`.

Podemos combinar los dos haciendo que el estado Reaccionar sea la "única fuente de la verdad". Luego, el componente Reaccionar que representa un formulario también controla lo que sucede en ese formulario en la posterior entrada del usuario. Un elemento de formulario de entrada cuyo valor es controlado por Reaccionar de esta manera se denomina "componente controlado".

Por ejemplo, si queremos hacer que el ejemplo anterior registre el nombre cuando se envía, podemos escribir el formulario como un componente controlado:

```js
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

Dado que el atributo de valor se establece en nuestro elemento de formulario, el valor mostrado siempre será this.state.value, lo que hará que React indique la fuente de la verdad. Dado que handleChange se ejecuta con cada pulsación de tecla para actualizar el estado Reaccionar, el valor mostrado se actualizará a medida que el usuario escriba.

Con un componente controlado, cada mutación de estado tendrá una función de controlador asociada. Esto hace que sea sencillo modificar o validar la entrada del usuario. Por ejemplo, si quisiéramos hacer cumplir que los nombres están escritos con todas las letras mayúsculas, podríamos escribir handleChange como:

```js
handleChange(event) {
  this.setState({value: event.target.value.toUpperCase()});
}
```

La etiqueta `textarea`
En HTML, un elemento `<textarea>` define su texto por sus hijos:

```html
<textarea>
  Hola, este es un texto en un área de texto
</textarea>
```

En React, un `<textarea>` usa un atributo de valor en su lugar. De esta forma, un formulario que utiliza un `<textarea>` se puede escribir de forma muy similar a un formulario que utiliza una entrada de una sola línea:

```js
class EssayForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: 'Please write an essay about your favorite DOM element.'
    };

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('An essay was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Essay:
          <textarea value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

Observe que this.state.value se inicializa en el constructor, de modo que el área de texto comience con algo de texto.

**La etiqueta de selección**

En HTML, `<select>` crea una lista desplegable. Por ejemplo, este HTML crea una lista desplegable de sabores:

```html
<select>
  <option value="grapefruit">Grapefruit</option>
  <option value="lime">Lime</option>
  <option selected value="coconut">Coconut</option>
  <option value="mango">Mango</option>
</select>
```

Tenga en cuenta que la opción Coconut se selecciona inicialmente, debido al atributo seleccionado. Reaccionar, en lugar de usar este atributo seleccionado, usa un atributo de valor en la etiqueta de selección de raíz. Esto es más conveniente en un componente controlado porque solo necesita actualizarlo en un solo lugar. Por ejemplo:

```js
class FlavorForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: 'coconut'};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('Your favorite flavor is: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Pick your favorite flavor:
          <select value={this.state.value} onChange={this.handleChange}>
            <option value="grapefruit">Grapefruit</option>
            <option value="lime">Lime</option>
            <option value="coconut">Coconut</option>
            <option value="mango">Mango</option>
          </select>
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```


[Pruébalo en CodePen](https://codepen.io/gaearon/pen/JbbEzX?editors=0010)

En general, esto hace que `<input type = "text">`, `<textarea>` y `<select>` funcionen de manera muy similar; todos aceptan un atributo de valor que puede usar para implementar un componente controlado.

**Nota**

Puede pasar una matriz al atributo de valor, lo que le permite seleccionar varias opciones en una etiqueta de selección:

```html
  <select multiple = {true} value = {['B', 'C']}>
```

La etiqueta de entrada de archivo
En HTML, un `<input type = "file">` le permite al usuario elegir uno o más archivos del almacenamiento de su dispositivo para ser cargados en un servidor o manipulados por JavaScript a través de la API de archivos.
```html
<input type = "file" />
```

Como su valor es de solo lectura, es un componente no controlado en React. Se analiza junto con otros componentes no controlados más adelante en la documentación.

Manejo de entradas múltiples
Cuando necesite manejar múltiples elementos de entrada controlados, puede agregar un atributo de nombre a cada elemento y dejar que la función del manejador elija qué hacer en función del valor de `event.target.name.`

Por ejemplo:

```js
class Reservation extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isGoing: true,
      numberOfGuests: 2
    };

    this.handleInputChange = this.handleInputChange.bind(this);
  }

  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          Is going:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          Number of guests:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```

[Pruébalo en CodePen](https://codepen.io/gaearon/pen/wgedvV?editors=0010)

Tenga en cuenta cómo usamos la sintaxis del nombre de propiedad computada ES6 para actualizar la clave de estado correspondiente al nombre de entrada dado:

```js
this.setState({
  [name]: value
});
```

Es equivalente a este código ES5:

```js
var partialState = {};
partialState[name] = value;
this.setState(partialState);
```

Además, como setState () fusiona automáticamente un estado parcial en el estado actual, solo necesitamos llamarlo con las partes modificadas.

## Valor Nulo de Entrada Controlada

Especificar el valor prop en un componente controlado evita que el usuario cambie la entrada a menos que así lo desee. Si ha especificado un valor pero la entrada aún se puede editar, puede haber configurado accidentalmente el valor como indefinido o nulo.

El siguiente código demuestra esto. (La entrada se bloquea al principio pero se puede editar después de un breve retraso).

```js
ReactDOM.render(<input value="hi" />, mountNode);

setTimeout(function() {
  ReactDOM.render(<input value={null} />, mountNode);
}, 1000);
```


## Alternativas a los componentes controlados

A veces puede ser tedioso usar componentes controlados, porque necesita escribir un controlador de eventos para cada forma en que sus datos pueden cambiar y canalizar todo el estado de entrada a través de un componente Reaccionar. Esto puede ser particularmente molesto cuando convierte una base de código preexistente en Reaccionar o integra una aplicación Reaccionar con una biblioteca que no es de React. En estas situaciones, es posible que desee verificar los componentes no controlados, una técnica alternativa para implementar formularios de entrada.

# Lifting State Up

A menudo, varios componentes deben reflejar los mismos datos cambiantes. Recomendamos elevar el estado compartido a su ancestro común más cercano. Veamos cómo funciona esto en acción.

En esta sección, crearemos una calculadora de temperatura que calcula si el agua hierve a una temperatura dada.

Comenzaremos con un componente llamado `BoilingVerdict`. Acepta la temperatura celsius como un accesorio, e imprime si es suficiente para hervir el agua:

```js
function BoilingVerdict(props) {
  if (props.celsius >= 100) {
    return <p>The water would boil.</p>;
  }
  return <p>The water would not boil.</p>;
}
```

A continuación, crearemos un componente llamado `Calculator`. Presta una `<input>` que le permite ingresar la temperatura, y mantiene su valor en `this.state.temperature`.

Además, representa el BoilingVerdict para el valor de entrada actual.

```js
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {temperature: ''};
  }

  handleChange(e) {
    this.setState({temperature: e.target.value});
  }

  render() {
    const temperature = this.state.temperature;
    return (
      <fieldset>
        <legend>Enter temperature in Celsius:</legend>
        
        <input
          value={temperature}
          onChange={this.handleChange} />

        <BoilingVerdict
          celsius={parseFloat(temperature)} />

      </fieldset>
    );
  }
}
```

[Pruébalo en CodePen](https://codepen.io/gaearon/pen/ZXeOBm?editors=0010)

**Agregar una segunda entrada**

Nuestro nuevo requisito es que, además de una entrada Celsius, proporcionamos una entrada Fahrenheit, y se mantienen sincronizados.

Podemos comenzar extrayendo un componente TemperatureInput de la Calculadora. Añadiremos un nuevo soporte de escala que puede ser "c" o "f":

```js
const scaleNames = {
  c: 'Celsius',
  f: 'Fahrenheit'
};

class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {temperature: ''};
  }

  handleChange(e) {
    this.setState({temperature: e.target.value});
  }

  render() {
    const temperature = this.state.temperature;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={temperature}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}
```

Ahora podemos cambiar la Calculadora para representar dos entradas de temperatura separadas:

```js
class Calculator extends React.Component {
  render() {
    return (
      <div>
        <TemperatureInput scale="c" />
        <TemperatureInput scale="f" />
      </div>
    );
  }
}
```

[Pruébalo en CodePen](https://codepen.io/gaearon/pen/jGBryx?editors=0010)


Tenemos dos entradas ahora, pero cuando ingresas la temperatura en una de ellas, la otra no se actualiza. Esto contradice nuestro requisito: queremos mantenerlos sincronizados.

Tampoco podemos mostrar el BoilingVerdict de la Calculadora. La calculadora no conoce la temperatura actual porque está oculta dentro de la entrada de temperatura.

## Escribiendo funciones de conversión

Primero, escribiremos dos funciones para convertir de Celsius a Fahrenheit y viceversa:

```js
function toCelsius(fahrenheit) {
  return (fahrenheit - 32) * 5 / 9;
}

function toFahrenheit(celsius) {
  return (celsius * 9 / 5) + 32;
}
```

Estas dos funciones convierten números. Vamos a escribir otra función que tome una temperatura de cadena y una función de convertidor como argumentos y devuelve una cadena. Lo usaremos para calcular el valor de una entrada basada en la otra entrada.

Devuelve una cadena vacía en una temperatura no válida y mantiene el resultado redondeado al tercer decimal:

```js
function tryConvert(temperature, convert) {
  const input = parseFloat(temperature);
  if (Number.isNaN(input)) {
    return '';
  }
  const output = convert(input);
  const rounded = Math.round(output * 1000) / 1000;
  return rounded.toString();
}
```

Por ejemplo, `tryConvert('abc', toCelsius)` devuelve una cadena vacía, y `tryConvert('10 .22 ', toFahrenheit)` devuelve `'50 .396'`.

**Estado de elevación**

Actualmente, ambos componentes de TemperatureInput guardan independientemente sus valores en el estado local:

```js
class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {temperature: ''};
  }

  handleChange(e) {
    this.setState({temperature: e.target.value});
  }

  render() {
    const temperature = this.state.temperature;
    // ...  
```

Sin embargo, queremos estos dos insumos para ser sincronizados con cada uno. Cuando actualiza el Celsius input, el Fahrenheit input debería reflejar la temperatura de conversión, y viceversa.

En React, el recurso compartido se realiza desplazando hacia el común común ancestor de los componentes que lo necesitan. Esto se llama "lifting state up". Nosotros eliminará el estado actual de la temperaturaInput y lo mueve en la Calculadora en lugar.

Si la calculadora tiene el estado compartido, se realiza el "origen de la verdad" para la temperatura actual en los insumos. Puede indicar que ambos tienen valores que coinciden con cada uno. En el caso de los dos componentes, los dos insumos de cada uno de los componentes de ambos componentes se incluyen en el mismo componente de la calculadora de componente.

Lea cómo hacer este trabajo paso a paso.

En primer lugar, se reemplazará esto.state.temperature con this.props.temperature en el componente de temperaturaInput. En el caso de que se trate de una versión anterior,

```js
 render() {
    // Before: const temperature = this.state.temperature;
    const temperature = this.props.temperature;
    // ...
```
Sabemos que los **props** son de sólo lectura. Cuando la temperatura se encuentra en el estado actual, el `TemperatureInput` no podría llamar a `this.setState()` para cambiarlo. Sin embargo, ahora que la temperatura está llegando desde los padres a la prop, el `TemperaturaInput` tiene en el control sobre él.

En React, esto se suele solvencia mediante la elaboración de un componente controlado. En el caso de que se produzca un cambio en el valor de los valores de los valores,

Ahora, cuando el TemperaturaInput quiere actualizar su temperatura, se llama this.props.onTemperatureChange:

```js
handleChange(e) {
    // Before: this.setState({temperature: e.target.value});
    this.props.onTemperatureChange(e.target.value);
    // 
```

**tenga en cuenta:**

```
No hay un significado especial en la temperatura o en el entorno de los nombres de los usuarios. En el caso de que se trate de un problema,
```

La `onTemperatureChange` **prop** se proporcionará junto con la temperatura propuesta por el componente de cálculo de la cabeza. Se manejará el cambio por modificar su propio estado local, que re-renderizará ambos insumos con los nuevos valores. Se verá en la nueva calculadora de lanzamiento.

Antes de buce en los cambios en la calculadora, se deshacer los cambios en el componente de temperatura. Hemos eliminado el estado actual de él, y en lugar de leer this.state.temperature, ahora read this.props.temperature. En lugar de llamar a this.setState () cuando desea realizar un cambio, ahora debe llamar a esta.props.onTemperatureChange () que se proporcionará mediante la calculadora:

```js
class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange(e) {
    this.props.onTemperatureChange(e.target.value);
  }

  render() {
    const temperature = this.props.temperature;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={temperature}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}
```

Ahora dejen el turn to the Calculator component.

Nosotros almacenamos la temperatura actual y la escala en su estado actual. Este es el estado de la vida de los insumos, y que servirá a la "fuente de la verdad" de ambos. Es la mínima representación de toda la fecha que necesitamos para hacer los insumos.

Por ejemplo, si se escribe 37 en el punto de entrada, el estado del cálculo de la calculadora será:

```js
{
  temperature: '37',
  scale: 'c'
}
```

Si se modifica el campo Fahrenheit para ser 212, el estado de la calculadora será:

```js
{
  En la mayoría de los casos,
  scale: 'f'
}
```

Se puede haber almacenado el valor de ambos insumos, pero no es necesario que sea innecesario. Es suficiente para almacenar el valor del último input entrantes y la escala que representa. A continuación, se puede inferir el valor del otro input basado en la temperatura actual y de forma independiente.

Los insumos permanecen en sincronización debido a que los valores se computa desde el mismo estado:

```js
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleCelsiusChange = this.handleCelsiusChange.bind(this);
    this.handleFahrenheitChange = this.handleFahrenheitChange.bind(this);
    this.state = {temperature: '', scale: 'c'};
  }

  handleCelsiusChange(temperature) {
    this.setState({scale: 'c', temperature});
  }

  handleFahrenheitChange(temperature) {
    this.setState({scale: 'f', temperature});
  }

  render() {
    const scale = this.state.scale;
    const temperature = this.state.temperature;
    const celsius = scale === 'f' ? tryConvert(temperature, toCelsius) : temperature;
    const fahrenheit = scale === 'c' ? tryConvert(temperature, toFahrenheit) : temperature;

    return (
      <div>
        <TemperatureInput
          scale="c"
          temperature={celsius}
          onTemperatureChange={this.handleCelsiusChange} />

        <TemperatureInput
          scale="f"
          temperature={fahrenheit}
          onTemperatureChange={this.handleFahrenheitChange} />

        <BoilingVerdict
          celsius={parseFloat(celsius)} />

      </div>
    );
  }
}
```

Ahora, no importa qué entrada usted edit, this.state.temperature y this.state.scale en el Calculator get actualizado. Una de las entradas se obtiene el valor de las is, no se incluye ninguna entrada de usuario, y el valor de entrada se vuelve a calcular ahora.

¿Qué es lo que sucede cuando edita una entrada:

React llama a la función especificada en el DOM `<input>`. En este caso, este es el método de manejo del método en el componente de temperatura.
El identificador de método en el componente de las centrales de energía se llama a `this.props.onTemperatureChange()` con el nuevo valor deseado. 
Sus props, incluyendo `onTemperatureChange`, fueron proporcionadas por su componente primario, la Calculadora.
Cuando se ha renderizado, la calculadora ha especificado que la escala de temperatura de la temperatura de la temperatura es el método de cálculo del controlador de la función de control de pH y de la escala de temperatura del equipo. En cualquiera de estos dos métodos de cálculo se llama dependiendo de qué input se ha editado.
En estos métodos, la calculadora de componente de componente React se vuelve a representar independientemente de llamar a este.setState () con el valor nuevo de entrada y la escala actual de la entrada se ha editado.
React llamando a la calculadora de componentes de la representación de método para aprender qué la interfaz de usuario debería parecer. Los valores de ambos insumos se recomputed basados ​​en la temperatura actual y la escala activa. La temperatura de conversión se realiza aquí.
React llama a los métodos de procesamiento de los componentes individuales de componentes individuales con sus nuevos props especificado por la calculadora. ¿Qué es lo que tu UI debería mirar como.
React llamando al método de reproducción del BoilingVerdict componente, pasando la temperatura en su propio props.
React DOM actualiza el DOM con el boletín veredicto y para coincidir con los valores de entrada especificados. La entrada que acaba de edificar recibe su valor actual y el otro input se actualiza a la temperatura después de la conversión.
Cada actualización va a través de los mismos pasos con los constantes de entrada en sincronización.

Lecciones aprendidas
No debería haber un único "source of truth" para cualquier fecha que los cambios en una aplicación React. Usually, el estado se agrega primero al componente que necesita para renderizar. A continuación, si otros componentes también lo necesitan, puede ascender a su común common ancestor. En lugar de intentar sincronizar el estado entre los diferentes componentes, debe ir en el flujo de datos de flujo de datos.

En el caso de que se produzca un error en el sistema, se debe tener en cuenta que la mayoría de las veces, En el caso de que se produzca un error en el sistema operativo, Además, se puede implementar una lógica personalizada para rejec o transformar el usuario de entrada.

Si algo se puede derivar de un estado o de un estado, es probable que no esté en el estado. Por ejemplo, en lugar de tanques `celsiusValue` y `fahrenheitValue`, guardamos la última temperatura y su escala. El valor de la entrada entera se puede calcular de ellos en el método (). Esto dejará que se despegue o se aplique alrededor del otro campo sin perder ninguna precisión en el usuario de entrada.

Cuando ve algo incorrecto en la interfaz de usuario, puede utilizar React Developer Tools para inspeccionar los props y mover el árbol hasta que encuentre el componente responsable para actualizar el estado. Esto le permite trazar los bugs a su fuente:



![alt text][logo2]

[logo2]: https://reactjs.org/react-devtools-state-ef94afc3447d75cdc245c77efb0d63be.gif "Supervisión del estado en React DevTools"

# Composition vs Inheritance

React tiene un modelo de plantilla potente, y se recomienda utilizar la composición en lugar de la herencia para reutilizar el código entre los componentes.

En esta sección, usted considerará algunos problemas donde los desarrolladores de nuevo a React a menudo concurrentes para la herencia, y cómo se puede solucionar con la composición.

**contención (Containment)**
Algunos componentes no saben sus niños de edad. Esto es especialmente común para los componentes como Sidebar o Dialog que representan genérico "boxes".

Se recomienda que tales componentes utilicen los niños especiales para pasar a los elementos secundarios directamente en su salida:

```js
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}
```

Esto deja que otros componentes pasan arbitrarios hijos a ellos mediante el jsx:

```js
function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecraft!
      </p>
    </FancyBorder>
  );
}
```

[Try it on CodePen](https://codepen.io/gaearon/pen/ozqNOV?editors=0010)

Anything dentro de la `<FancyBorder>` JSX etiqueta se pasa en el FancyBorder component as a children prop. En el caso de que se produzca un error,

En el caso de que se trate de un problema, En tal caso usted puede iniciar con su propia entidad en lugar de utilizar niños:


```js
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}
      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane
      left={
        <Contacts />
      }
      right={
        <Chat />
      } />
  );
}
```

[Try it on CodePen](https://codepen.io/gaearon/pen/gwZOJp?editors=0010)


React elements como `<Contacts />` y `<Chat />` son objetos justos, usted puede pasar los props como cualquier otra fecha. Este enfoque puede deducir las "ranuras" en otras bibliotecas pero no están en las restricciones en qué puede pasar los props in React.

**especialización**

A veces pensamos en los componentes que se trata de casos especiales de otros componentes. Por ejemplo, podemos decir que el WelcomeDialog es un caso especial de Dialog.

En React, también se obtiene una combinación de composición, donde hay más "específico" de componentes de rendimiento a más "genérico" y configures con props:

```js
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
    </FancyBorder>
  );
}

function WelcomeDialog() {
  return (
    <Dialog
      title="Welcome"
      message="Thank you for visiting our spacecraft!" />

  );
}
```

[Try it on CodePen](https://codepen.io/gaearon/pen/kkEaOZ?editors=0010)

Composición de las estaciones de trabajo de las clases:

```js
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
      {props.children}
    </FancyBorder>
  );
}

class SignUpDialog extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.handleSignUp = this.handleSignUp.bind(this);
    this.state = {login: ''};
  }

  render() {
    return (
      <Dialog title="Mars Exploration Program"
              message="How should we refer to you?">
        <input value={this.state.login}
               onChange={this.handleChange} />

        <button onClick={this.handleSignUp}>
          Sign Me Up!
        </button>
      </Dialog>
    );
  }

  handleChange(e) {
    this.setState({login: e.target.value});
  }

  handleSignUp() {
    alert(`Welcome aboard, ${this.state.login}!`);
  }
}
```
Try it on CodePen

¿Qué acerca de la herencia?
En Facebook, utilizamos React en miles de componentes, y no hemos encontrado ningún uso de casos en los que se recomienda crear componentes hereditarios jerárquicos.

Props y compila darle toda la flexibilidad que necesita para personalizar el aspecto de la relación y el comportamiento en una forma explícita y segura. Recuerde que los componentes pueden aceptar arbitrary props, incluyendo primitive values, React elements, or functions.

Si desea reutilizar la interfaz de usuario no-UI entre los componentes, se trata de extracción en un módulo de JavaScript. Los componentes pueden importar y utilizar aquella función, objeto, o la clase, sin embargo.

# Thinking In React


This project was bootstrapped with [Create React App](https://github.com/facebookincubator/create-react-app).

Below you will find some information on how to perform common tasks.<br>
You can find the most recent version of this guide [here](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md).

## Table of Contents

- [Updating to New Releases](#updating-to-new-releases)
- [Sending Feedback](#sending-feedback)
- [Folder Structure](#folder-structure)
- [Available Scripts](#available-scripts)
  - [npm start](#npm-start)
  - [npm test](#npm-test)
  - [npm run build](#npm-run-build)
  - [npm run eject](#npm-run-eject)
- [Supported Browsers](#supported-browsers)
- [Supported Language Features and Polyfills](#supported-language-features-and-polyfills)
- [Syntax Highlighting in the Editor](#syntax-highlighting-in-the-editor)
- [Displaying Lint Output in the Editor](#displaying-lint-output-in-the-editor)
- [Debugging in the Editor](#debugging-in-the-editor)
- [Formatting Code Automatically](#formatting-code-automatically)
- [Changing the Page `<title>`](#changing-the-page-title)
- [Installing a Dependency](#installing-a-dependency)
- [Importing a Component](#importing-a-component)
- [Code Splitting](#code-splitting)
- [Adding a Stylesheet](#adding-a-stylesheet)
- [Post-Processing CSS](#post-processing-css)
- [Adding a CSS Preprocessor (Sass, Less etc.)](#adding-a-css-preprocessor-sass-less-etc)
- [Adding Images, Fonts, and Files](#adding-images-fonts-and-files)
- [Using the `public` Folder](#using-the-public-folder)
  - [Changing the HTML](#changing-the-html)
  - [Adding Assets Outside of the Module System](#adding-assets-outside-of-the-module-system)
  - [When to Use the `public` Folder](#when-to-use-the-public-folder)
- [Using Global Variables](#using-global-variables)
- [Adding Bootstrap](#adding-bootstrap)
  - [Using a Custom Theme](#using-a-custom-theme)
- [Adding Flow](#adding-flow)
- [Adding a Router](#adding-a-router)
- [Adding Custom Environment Variables](#adding-custom-environment-variables)
  - [Referencing Environment Variables in the HTML](#referencing-environment-variables-in-the-html)
  - [Adding Temporary Environment Variables In Your Shell](#adding-temporary-environment-variables-in-your-shell)
  - [Adding Development Environment Variables In `.env`](#adding-development-environment-variables-in-env)
- [Can I Use Decorators?](#can-i-use-decorators)
- [Fetching Data with AJAX Requests](#fetching-data-with-ajax-requests)
- [Integrating with an API Backend](#integrating-with-an-api-backend)
  - [Node](#node)
  - [Ruby on Rails](#ruby-on-rails)
- [Proxying API Requests in Development](#proxying-api-requests-in-development)
  - ["Invalid Host Header" Errors After Configuring Proxy](#invalid-host-header-errors-after-configuring-proxy)
  - [Configuring the Proxy Manually](#configuring-the-proxy-manually)
  - [Configuring a WebSocket Proxy](#configuring-a-websocket-proxy)
- [Using HTTPS in Development](#using-https-in-development)
- [Generating Dynamic `<meta>` Tags on the Server](#generating-dynamic-meta-tags-on-the-server)
- [Pre-Rendering into Static HTML Files](#pre-rendering-into-static-html-files)
- [Injecting Data from the Server into the Page](#injecting-data-from-the-server-into-the-page)
- [Running Tests](#running-tests)
  - [Filename Conventions](#filename-conventions)
  - [Command Line Interface](#command-line-interface)
  - [Version Control Integration](#version-control-integration)
  - [Writing Tests](#writing-tests)
  - [Testing Components](#testing-components)
  - [Using Third Party Assertion Libraries](#using-third-party-assertion-libraries)
  - [Initializing Test Environment](#initializing-test-environment)
  - [Focusing and Excluding Tests](#focusing-and-excluding-tests)
  - [Coverage Reporting](#coverage-reporting)
  - [Continuous Integration](#continuous-integration)
  - [Disabling jsdom](#disabling-jsdom)
  - [Snapshot Testing](#snapshot-testing)
  - [Editor Integration](#editor-integration)
- [Debugging Tests](#debugging-tests)
  - [Debugging Tests in Chrome](#debugging-tests-in-chrome)
  - [Debugging Tests in Visual Studio Code](#debugging-tests-in-visual-studio-code)
- [Developing Components in Isolation](#developing-components-in-isolation)
  - [Getting Started with Storybook](#getting-started-with-storybook)
  - [Getting Started with Styleguidist](#getting-started-with-styleguidist)
- [Publishing Components to npm](#publishing-components-to-npm)
- [Making a Progressive Web App](#making-a-progressive-web-app)
  - [Opting Out of Caching](#opting-out-of-caching)
  - [Offline-First Considerations](#offline-first-considerations)
  - [Progressive Web App Metadata](#progressive-web-app-metadata)
- [Analyzing the Bundle Size](#analyzing-the-bundle-size)
- [Deployment](#deployment)
  - [Static Server](#static-server)
  - [Other Solutions](#other-solutions)
  - [Serving Apps with Client-Side Routing](#serving-apps-with-client-side-routing)
  - [Building for Relative Paths](#building-for-relative-paths)
  - [Azure](#azure)
  - [Firebase](#firebase)
  - [GitHub Pages](#github-pages)
  - [Heroku](#heroku)
  - [Netlify](#netlify)
  - [Now](#now)
  - [S3 and CloudFront](#s3-and-cloudfront)
  - [Surge](#surge)
- [Advanced Configuration](#advanced-configuration)
- [Troubleshooting](#troubleshooting)
  - [`npm start` doesn’t detect changes](#npm-start-doesnt-detect-changes)
  - [`npm test` hangs on macOS Sierra](#npm-test-hangs-on-macos-sierra)
  - [`npm run build` exits too early](#npm-run-build-exits-too-early)
  - [`npm run build` fails on Heroku](#npm-run-build-fails-on-heroku)
  - [`npm run build` fails to minify](#npm-run-build-fails-to-minify)
  - [Moment.js locales are missing](#momentjs-locales-are-missing)
- [Alternatives to Ejecting](#alternatives-to-ejecting)
- [Something Missing?](#something-missing)

## Updating to New Releases

Create React App is divided into two packages:

* `create-react-app` is a global command-line utility that you use to create new projects.
* `react-scripts` is a development dependency in the generated projects (including this one).

You almost never need to update `create-react-app` itself: it delegates all the setup to `react-scripts`.

When you run `create-react-app`, it always creates the project with the latest version of `react-scripts` so you’ll get all the new features and improvements in newly created apps automatically.

To update an existing project to a new version of `react-scripts`, [open the changelog](https://github.com/facebookincubator/create-react-app/blob/master/CHANGELOG.md), find the version you’re currently on (check `package.json` in this folder if you’re not sure), and apply the migration instructions for the newer versions.

In most cases bumping the `react-scripts` version in `package.json` and running `npm install` in this folder should be enough, but it’s good to consult the [changelog](https://github.com/facebookincubator/create-react-app/blob/master/CHANGELOG.md) for potential breaking changes.

We commit to keeping the breaking changes minimal so you can upgrade `react-scripts` painlessly.

## Sending Feedback

We are always open to [your feedback](https://github.com/facebookincubator/create-react-app/issues).

## Folder Structure

After creation, your project should look like this:

```
my-app/
  README.md
  node_modules/
  package.json
  public/
    index.html
    favicon.ico
  src/
    App.css
    App.js
    App.test.js
    index.css
    index.js
    logo.svg
```

For the project to build, **these files must exist with exact filenames**:

* `public/index.html` is the page template;
* `src/index.js` is the JavaScript entry point.

You can delete or rename the other files.

You may create subdirectories inside `src`. For faster rebuilds, only files inside `src` are processed by Webpack.<br>
You need to **put any JS and CSS files inside `src`**, otherwise Webpack won’t see them.

Only files inside `public` can be used from `public/index.html`.<br>
Read instructions below for using assets from JavaScript and HTML.

You can, however, create more top-level directories.<br>
They will not be included in the production build so you can use them for things like documentation.

## Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the development mode.<br>
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.<br>
You will also see any lint errors in the console.

### `npm test`

Launches the test runner in the interactive watch mode.<br>
See the section about [running tests](#running-tests) for more information.

### `npm run build`

Builds the app for production to the `build` folder.<br>
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.<br>
Your app is ready to be deployed!

See the section about [deployment](#deployment) for more information.

### `npm run eject`

**Note: this is a one-way operation. Once you `eject`, you can’t go back!**

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (Webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.

You don’t have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature. However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.

## Supported Browsers

By default, the generated project uses the latest version of React.

You can refer [to the React documentation](https://reactjs.org/docs/react-dom.html#browser-support) for more information about supported browsers.

## Supported Language Features and Polyfills

This project supports a superset of the latest JavaScript standard.<br>
In addition to [ES6](https://github.com/lukehoban/es6features) syntax features, it also supports:

* [Exponentiation Operator](https://github.com/rwaldron/exponentiation-operator) (ES2016).
* [Async/await](https://github.com/tc39/ecmascript-asyncawait) (ES2017).
* [Object Rest/Spread Properties](https://github.com/sebmarkbage/ecmascript-rest-spread) (stage 3 proposal).
* [Dynamic import()](https://github.com/tc39/proposal-dynamic-import) (stage 3 proposal)
* [Class Fields and Static Properties](https://github.com/tc39/proposal-class-public-fields) (part of stage 3 proposal).
* [JSX](https://facebook.github.io/react/docs/introducing-jsx.html) and [Flow](https://flowtype.org/) syntax.

Learn more about [different proposal stages](https://babeljs.io/docs/plugins/#presets-stage-x-experimental-presets-).

While we recommend using experimental proposals with some caution, Facebook heavily uses these features in the product code, so we intend to provide [codemods](https://medium.com/@cpojer/effective-javascript-codemods-5a6686bb46fb) if any of these proposals change in the future.

Note that **the project only includes a few ES6 [polyfills](https://en.wikipedia.org/wiki/Polyfill)**:

* [`Object.assign()`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) via [`object-assign`](https://github.com/sindresorhus/object-assign).
* [`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) via [`promise`](https://github.com/then/promise).
* [`fetch()`](https://developer.mozilla.org/en/docs/Web/API/Fetch_API) via [`whatwg-fetch`](https://github.com/github/fetch).

If you use any other ES6+ features that need **runtime support** (such as `Array.from()` or `Symbol`), make sure you are including the appropriate polyfills manually, or that the browsers you are targeting already support them.

Also note that using some newer syntax features like `for...of` or `[...nonArrayValue]` causes Babel to emit code that depends on ES6 runtime features and might not work without a polyfill. When in doubt, use [Babel REPL](https://babeljs.io/repl/) to see what any specific syntax compiles down to.

## Syntax Highlighting in the Editor

To configure the syntax highlighting in your favorite text editor, head to the [relevant Babel documentation page](https://babeljs.io/docs/editors) and follow the instructions. Some of the most popular editors are covered.

## Displaying Lint Output in the Editor

>Note: this feature is available with `react-scripts@0.2.0` and higher.<br>
>It also only works with npm 3 or higher.

Some editors, including Sublime Text, Atom, and Visual Studio Code, provide plugins for ESLint.

They are not required for linting. You should see the linter output right in your terminal as well as the browser console. However, if you prefer the lint results to appear right in your editor, there are some extra steps you can do.

You would need to install an ESLint plugin for your editor first. Then, add a file called `.eslintrc` to the project root:

```js
{
  "extends": "react-app"
}
```

Now your editor should report the linting warnings.

Note that even if you edit your `.eslintrc` file further, these changes will **only affect the editor integration**. They won’t affect the terminal and in-browser lint output. This is because Create React App intentionally provides a minimal set of rules that find common mistakes.

If you want to enforce a coding style for your project, consider using [Prettier](https://github.com/jlongster/prettier) instead of ESLint style rules.

## Debugging in the Editor

**This feature is currently only supported by [Visual Studio Code](https://code.visualstudio.com) and [WebStorm](https://www.jetbrains.com/webstorm/).**

Visual Studio Code and WebStorm support debugging out of the box with Create React App. This enables you as a developer to write and debug your React code without leaving the editor, and most importantly it enables you to have a continuous development workflow, where context switching is minimal, as you don’t have to switch between tools.

### Visual Studio Code

You would need to have the latest version of [VS Code](https://code.visualstudio.com) and VS Code [Chrome Debugger Extension](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome) installed.

Then add the block below to your `launch.json` file and put it inside the `.vscode` folder in your app’s root directory.

```json
{
  "version": "0.2.0",
  "configurations": [{
    "name": "Chrome",
    "type": "chrome",
    "request": "launch",
    "url": "http://localhost:3000",
    "webRoot": "${workspaceRoot}/src",
    "sourceMapPathOverrides": {
      "webpack:///src/*": "${webRoot}/*"
    }
  }]
}
```
>Note: the URL may be different if you've made adjustments via the [HOST or PORT environment variables](#advanced-configuration).

Start your app by running `npm start`, and start debugging in VS Code by pressing `F5` or by clicking the green debug icon. You can now write code, set breakpoints, make changes to the code, and debug your newly modified code—all from your editor.

Having problems with VS Code Debugging? Please see their [troubleshooting guide](https://github.com/Microsoft/vscode-chrome-debug/blob/master/README.md#troubleshooting).

### WebStorm

You would need to have [WebStorm](https://www.jetbrains.com/webstorm/) and [JetBrains IDE Support](https://chrome.google.com/webstore/detail/jetbrains-ide-support/hmhgeddbohgjknpmjagkdomcpobmllji) Chrome extension installed.

In the WebStorm menu `Run` select `Edit Configurations...`. Then click `+` and select `JavaScript Debug`. Paste `http://localhost:3000` into the URL field and save the configuration.

>Note: the URL may be different if you've made adjustments via the [HOST or PORT environment variables](#advanced-configuration).

Start your app by running `npm start`, then press `^D` on macOS or `F9` on Windows and Linux or click the green debug icon to start debugging in WebStorm.

The same way you can debug your application in IntelliJ IDEA Ultimate, PhpStorm, PyCharm Pro, and RubyMine. 

## Formatting Code Automatically

Prettier is an opinionated code formatter with support for JavaScript, CSS and JSON. With Prettier you can format the code you write automatically to ensure a code style within your project. See the [Prettier's GitHub page](https://github.com/prettier/prettier) for more information, and look at this [page to see it in action](https://prettier.github.io/prettier/).

To format our code whenever we make a commit in git, we need to install the following dependencies:

```sh
npm install --save husky lint-staged prettier
```

Alternatively you may use `yarn`:

```sh
yarn add husky lint-staged prettier
```

* `husky` makes it easy to use githooks as if they are npm scripts.
* `lint-staged` allows us to run scripts on staged files in git. See this [blog post about lint-staged to learn more about it](https://medium.com/@okonetchnikov/make-linting-great-again-f3890e1ad6b8).
* `prettier` is the JavaScript formatter we will run before commits.

Now we can make sure every file is formatted correctly by adding a few lines to the `package.json` in the project root.

Add the following line to `scripts` section:

```diff
  "scripts": {
+   "precommit": "lint-staged",
    "start": "react-scripts start",
    "build": "react-scripts build",
```

Next we add a 'lint-staged' field to the `package.json`, for example:

```diff
  "dependencies": {
    // ...
  },
+ "lint-staged": {
+   "src/**/*.{js,jsx,json,css}": [
+     "prettier --single-quote --write",
+     "git add"
+   ]
+ },
  "scripts": {
```

Now, whenever you make a commit, Prettier will format the changed files automatically. You can also run `./node_modules/.bin/prettier --single-quote --write "src/**/*.{js,jsx,json,css}"` to format your entire project for the first time.

Next you might want to integrate Prettier in your favorite editor. Read the section on [Editor Integration](https://prettier.io/docs/en/editors.html) on the Prettier GitHub page.

## Changing the Page `<title>`

You can find the source HTML file in the `public` folder of the generated project. You may edit the `<title>` tag in it to change the title from “React App” to anything else.

Note that normally you wouldn’t edit files in the `public` folder very often. For example, [adding a stylesheet](#adding-a-stylesheet) is done without touching the HTML.

If you need to dynamically update the page title based on the content, you can use the browser [`document.title`](https://developer.mozilla.org/en-US/docs/Web/API/Document/title) API. For more complex scenarios when you want to change the title from React components, you can use [React Helmet](https://github.com/nfl/react-helmet), a third party library.

If you use a custom server for your app in production and want to modify the title before it gets sent to the browser, you can follow advice in [this section](#generating-dynamic-meta-tags-on-the-server). Alternatively, you can pre-build each page as a static HTML file which then loads the JavaScript bundle, which is covered [here](#pre-rendering-into-static-html-files).

## Installing a Dependency

The generated project includes React and ReactDOM as dependencies. It also includes a set of scripts used by Create React App as a development dependency. You may install other dependencies (for example, React Router) with `npm`:

```sh
npm install --save react-router
```

Alternatively you may use `yarn`:

```sh
yarn add react-router
```

This works for any library, not just `react-router`.

## Importing a Component

This project setup supports ES6 modules thanks to Babel.<br>
While you can still use `require()` and `module.exports`, we encourage you to use [`import` and `export`](http://exploringjs.com/es6/ch_modules.html) instead.

For example:

### `Button.js`

```js
import React, { Component } from 'react';

class Button extends Component {
  render() {
    // ...
  }
}

export default Button; // Don’t forget to use export default!
```

### `DangerButton.js`


```js
import React, { Component } from 'react';
import Button from './Button'; // Import a component from another file

class DangerButton extends Component {
  render() {
    return <Button color="red" />;
  }
}

export default DangerButton;
```

Be aware of the [difference between default and named exports](http://stackoverflow.com/questions/36795819/react-native-es-6-when-should-i-use-curly-braces-for-import/36796281#36796281). It is a common source of mistakes.

We suggest that you stick to using default imports and exports when a module only exports a single thing (for example, a component). That’s what you get when you use `export default Button` and `import Button from './Button'`.

Named exports are useful for utility modules that export several functions. A module may have at most one default export and as many named exports as you like.

Learn more about ES6 modules:

* [When to use the curly braces?](http://stackoverflow.com/questions/36795819/react-native-es-6-when-should-i-use-curly-braces-for-import/36796281#36796281)
* [Exploring ES6: Modules](http://exploringjs.com/es6/ch_modules.html)
* [Understanding ES6: Modules](https://leanpub.com/understandinges6/read#leanpub-auto-encapsulating-code-with-modules)

## Code Splitting

Instead of downloading the entire app before users can use it, code splitting allows you to split your code into small chunks which you can then load on demand.

This project setup supports code splitting via [dynamic `import()`](http://2ality.com/2017/01/import-operator.html#loading-code-on-demand). Its [proposal](https://github.com/tc39/proposal-dynamic-import) is in stage 3. The `import()` function-like form takes the module name as an argument and returns a [`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) which always resolves to the namespace object of the module.

Here is an example:

### `moduleA.js`

```js
const moduleA = 'Hello';

export { moduleA };
```
### `App.js`

```js
import React, { Component } from 'react';

class App extends Component {
  handleClick = () => {
    import('./moduleA')
      .then(({ moduleA }) => {
        // Use moduleA
      })
      .catch(err => {
        // Handle failure
      });
  };

  render() {
    return (
      <div>
        <button onClick={this.handleClick}>Load</button>
      </div>
    );
  }
}

export default App;
```

This will make `moduleA.js` and all its unique dependencies as a separate chunk that only loads after the user clicks the 'Load' button.

You can also use it with `async` / `await` syntax if you prefer it.

### With React Router

If you are using React Router check out [this tutorial](http://serverless-stack.com/chapters/code-splitting-in-create-react-app.html) on how to use code splitting with it. You can find the companion GitHub repository [here](https://github.com/AnomalyInnovations/serverless-stack-demo-client/tree/code-splitting-in-create-react-app).

Also check out the [Code Splitting](https://reactjs.org/docs/code-splitting.html) section in React documentation.

## Adding a Stylesheet

This project setup uses [Webpack](https://webpack.js.org/) for handling all assets. Webpack offers a custom way of “extending” the concept of `import` beyond JavaScript. To express that a JavaScript file depends on a CSS file, you need to **import the CSS from the JavaScript file**:

### `Button.css`

```css
.Button {
  padding: 20px;
}
```

### `Button.js`

```js
import React, { Component } from 'react';
import './Button.css'; // Tell Webpack that Button.js uses these styles

class Button extends Component {
  render() {
    // You can use them as regular CSS styles
    return <div className="Button" />;
  }
}
```

**This is not required for React** but many people find this feature convenient. You can read about the benefits of this approach [here](https://medium.com/seek-blog/block-element-modifying-your-javascript-components-d7f99fcab52b). However you should be aware that this makes your code less portable to other build tools and environments than Webpack.

In development, expressing dependencies this way allows your styles to be reloaded on the fly as you edit them. In production, all CSS files will be concatenated into a single minified `.css` file in the build output.

If you are concerned about using Webpack-specific semantics, you can put all your CSS right into `src/index.css`. It would still be imported from `src/index.js`, but you could always remove that import if you later migrate to a different build tool.

## Post-Processing CSS

This project setup minifies your CSS and adds vendor prefixes to it automatically through [Autoprefixer](https://github.com/postcss/autoprefixer) so you don’t need to worry about it.

For example, this:

```css
.App {
  display: flex;
  flex-direction: row;
  align-items: center;
}
```

becomes this:

```css
.App {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
      -ms-flex-direction: row;
          flex-direction: row;
  -webkit-box-align: center;
      -ms-flex-align: center;
          align-items: center;
}
```

If you need to disable autoprefixing for some reason, [follow this section](https://github.com/postcss/autoprefixer#disabling).

## Adding a CSS Preprocessor (Sass, Less etc.)

Generally, we recommend that you don’t reuse the same CSS classes across different components. For example, instead of using a `.Button` CSS class in `<AcceptButton>` and `<RejectButton>` components, we recommend creating a `<Button>` component with its own `.Button` styles, that both `<AcceptButton>` and `<RejectButton>` can render (but [not inherit](https://facebook.github.io/react/docs/composition-vs-inheritance.html)).

Following this rule often makes CSS preprocessors less useful, as features like mixins and nesting are replaced by component composition. You can, however, integrate a CSS preprocessor if you find it valuable. In this walkthrough, we will be using Sass, but you can also use Less, or another alternative.

First, let’s install the command-line interface for Sass:

```sh
npm install --save node-sass-chokidar
```

Alternatively you may use `yarn`:

```sh
yarn add node-sass-chokidar
```

Then in `package.json`, add the following lines to `scripts`:

```diff
   "scripts": {
+    "build-css": "node-sass-chokidar src/ -o src/",
+    "watch-css": "npm run build-css && node-sass-chokidar src/ -o src/ --watch --recursive",
     "start": "react-scripts start",
     "build": "react-scripts build",
     "test": "react-scripts test --env=jsdom",
```

>Note: To use a different preprocessor, replace `build-css` and `watch-css` commands according to your preprocessor’s documentation.

Now you can rename `src/App.css` to `src/App.scss` and run `npm run watch-css`. The watcher will find every Sass file in `src` subdirectories, and create a corresponding CSS file next to it, in our case overwriting `src/App.css`. Since `src/App.js` still imports `src/App.css`, the styles become a part of your application. You can now edit `src/App.scss`, and `src/App.css` will be regenerated.

To share variables between Sass files, you can use Sass imports. For example, `src/App.scss` and other component style files could include `@import "./shared.scss";` with variable definitions.

To enable importing files without using relative paths, you can add the  `--include-path` option to the command in `package.json`.

```
"build-css": "node-sass-chokidar --include-path ./src --include-path ./node_modules src/ -o src/",
"watch-css": "npm run build-css && node-sass-chokidar --include-path ./src --include-path ./node_modules src/ -o src/ --watch --recursive",
```

This will allow you to do imports like

```scss
@import 'styles/_colors.scss'; // assuming a styles directory under src/
@import 'nprogress/nprogress'; // importing a css file from the nprogress node module
```

At this point you might want to remove all CSS files from the source control, and add `src/**/*.css` to your `.gitignore` file. It is generally a good practice to keep the build products outside of the source control.

As a final step, you may find it convenient to run `watch-css` automatically with `npm start`, and run `build-css` as a part of `npm run build`. You can use the `&&` operator to execute two scripts sequentially. However, there is no cross-platform way to run two scripts in parallel, so we will install a package for this:

```sh
npm install --save npm-run-all
```

Alternatively you may use `yarn`:

```sh
yarn add npm-run-all
```

Then we can change `start` and `build` scripts to include the CSS preprocessor commands:

```diff
   "scripts": {
     "build-css": "node-sass-chokidar src/ -o src/",
     "watch-css": "npm run build-css && node-sass-chokidar src/ -o src/ --watch --recursive",
-    "start": "react-scripts start",
-    "build": "react-scripts build",
+    "start-js": "react-scripts start",
+    "start": "npm-run-all -p watch-css start-js",
+    "build-js": "react-scripts build",
+    "build": "npm-run-all build-css build-js",
     "test": "react-scripts test --env=jsdom",
     "eject": "react-scripts eject"
   }
```

Now running `npm start` and `npm run build` also builds Sass files.

**Why `node-sass-chokidar`?**

`node-sass` has been reported as having the following issues:

- `node-sass --watch` has been reported to have *performance issues* in certain conditions when used in a virtual machine or with docker.

- Infinite styles compiling [#1939](https://github.com/facebookincubator/create-react-app/issues/1939)

- `node-sass` has been reported as having issues with detecting new files in a directory [#1891](https://github.com/sass/node-sass/issues/1891)

 `node-sass-chokidar` is used here as it addresses these issues.

## Adding Images, Fonts, and Files

With Webpack, using static assets like images and fonts works similarly to CSS.

You can **`import` a file right in a JavaScript module**. This tells Webpack to include that file in the bundle. Unlike CSS imports, importing a file gives you a string value. This value is the final path you can reference in your code, e.g. as the `src` attribute of an image or the `href` of a link to a PDF.

To reduce the number of requests to the server, importing images that are less than 10,000 bytes returns a [data URI](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URIs) instead of a path. This applies to the following file extensions: bmp, gif, jpg, jpeg, and png. SVG files are excluded due to [#1153](https://github.com/facebookincubator/create-react-app/issues/1153).

Here is an example:

```js
import React from 'react';
import logo from './logo.png'; // Tell Webpack this JS file uses this image

console.log(logo); // /logo.84287d09.png

function Header() {
  // Import result is the URL of your image
  return <img src={logo} alt="Logo" />;
}

export default Header;
```

This ensures that when the project is built, Webpack will correctly move the images into the build folder, and provide us with correct paths.

This works in CSS too:

```css
.Logo {
  background-image: url(./logo.png);
}
```

Webpack finds all relative module references in CSS (they start with `./`) and replaces them with the final paths from the compiled bundle. If you make a typo or accidentally delete an important file, you will see a compilation error, just like when you import a non-existent JavaScript module. The final filenames in the compiled bundle are generated by Webpack from content hashes. If the file content changes in the future, Webpack will give it a different name in production so you don’t need to worry about long-term caching of assets.

Please be advised that this is also a custom feature of Webpack.

**It is not required for React** but many people enjoy it (and React Native uses a similar mechanism for images).<br>
An alternative way of handling static assets is described in the next section.

## Using the `public` Folder

>Note: this feature is available with `react-scripts@0.5.0` and higher.

### Changing the HTML

The `public` folder contains the HTML file so you can tweak it, for example, to [set the page title](#changing-the-page-title).
The `<script>` tag with the compiled code will be added to it automatically during the build process.

### Adding Assets Outside of the Module System

You can also add other assets to the `public` folder.

Note that we normally encourage you to `import` assets in JavaScript files instead.
For example, see the sections on [adding a stylesheet](#adding-a-stylesheet) and [adding images and fonts](#adding-images-fonts-and-files).
This mechanism provides a number of benefits:

* Scripts and stylesheets get minified and bundled together to avoid extra network requests.
* Missing files cause compilation errors instead of 404 errors for your users.
* Result filenames include content hashes so you don’t need to worry about browsers caching their old versions.

However there is an **escape hatch** that you can use to add an asset outside of the module system.

If you put a file into the `public` folder, it will **not** be processed by Webpack. Instead it will be copied into the build folder untouched.   To reference assets in the `public` folder, you need to use a special variable called `PUBLIC_URL`.

Inside `index.html`, you can use it like this:

```html
<link rel="shortcut icon" href="%PUBLIC_URL%/favicon.ico">
```

Only files inside the `public` folder will be accessible by `%PUBLIC_URL%` prefix. If you need to use a file from `src` or `node_modules`, you’ll have to copy it there to explicitly specify your intention to make this file a part of the build.

When you run `npm run build`, Create React App will substitute `%PUBLIC_URL%` with a correct absolute path so your project works even if you use client-side routing or host it at a non-root URL.

In JavaScript code, you can use `process.env.PUBLIC_URL` for similar purposes:

```js
render() {
  // Note: this is an escape hatch and should be used sparingly!
  // Normally we recommend using `import` for getting asset URLs
  // as described in “Adding Images and Fonts” above this section.
  return <img src={process.env.PUBLIC_URL + '/img/logo.png'} />;
}
```

Keep in mind the downsides of this approach:

* None of the files in `public` folder get post-processed or minified.
* Missing files will not be called at compilation time, and will cause 404 errors for your users.
* Result filenames won’t include content hashes so you’ll need to add query arguments or rename them every time they change.

### When to Use the `public` Folder

Normally we recommend importing [stylesheets](#adding-a-stylesheet), [images, and fonts](#adding-images-fonts-and-files) from JavaScript.
The `public` folder is useful as a workaround for a number of less common cases:

* You need a file with a specific name in the build output, such as [`manifest.webmanifest`](https://developer.mozilla.org/en-US/docs/Web/Manifest).
* You have thousands of images and need to dynamically reference their paths.
* You want to include a small script like [`pace.js`](http://github.hubspot.com/pace/docs/welcome/) outside of the bundled code.
* Some library may be incompatible with Webpack and you have no other option but to include it as a `<script>` tag.

Note that if you add a `<script>` that declares global variables, you also need to read the next section on using them.

## Using Global Variables

When you include a script in the HTML file that defines global variables and try to use one of these variables in the code, the linter will complain because it cannot see the definition of the variable.

You can avoid this by reading the global variable explicitly from the `window` object, for example:

```js
const $ = window.$;
```

This makes it obvious you are using a global variable intentionally rather than because of a typo.

Alternatively, you can force the linter to ignore any line by adding `// eslint-disable-line` after it.

## Adding Bootstrap

You don’t have to use [React Bootstrap](https://react-bootstrap.github.io) together with React but it is a popular library for integrating Bootstrap with React apps. If you need it, you can integrate it with Create React App by following these steps:

Install React Bootstrap and Bootstrap from npm. React Bootstrap does not include Bootstrap CSS so this needs to be installed as well:

```sh
npm install --save react-bootstrap bootstrap@3
```

Alternatively you may use `yarn`:

```sh
yarn add react-bootstrap bootstrap@3
```

Import Bootstrap CSS and optionally Bootstrap theme CSS in the beginning of your ```src/index.js``` file:

```js
import 'bootstrap/dist/css/bootstrap.css';
import 'bootstrap/dist/css/bootstrap-theme.css';
// Put any other imports below so that CSS from your
// components takes precedence over default styles.
```

Import required React Bootstrap components within ```src/App.js``` file or your custom component files:

```js
import { Navbar, Jumbotron, Button } from 'react-bootstrap';
```

Now you are ready to use the imported React Bootstrap components within your component hierarchy defined in the render method. Here is an example [`App.js`](https://gist.githubusercontent.com/gaearon/85d8c067f6af1e56277c82d19fd4da7b/raw/6158dd991b67284e9fc8d70b9d973efe87659d72/App.js) redone using React Bootstrap.

### Using a Custom Theme

Sometimes you might need to tweak the visual styles of Bootstrap (or equivalent package).<br>
We suggest the following approach:

* Create a new package that depends on the package you wish to customize, e.g. Bootstrap.
* Add the necessary build steps to tweak the theme, and publish your package on npm.
* Install your own theme npm package as a dependency of your app.

Here is an example of adding a [customized Bootstrap](https://medium.com/@tacomanator/customizing-create-react-app-aa9ffb88165) that follows these steps.

## Adding Flow

Flow is a static type checker that helps you write code with fewer bugs. Check out this [introduction to using static types in JavaScript](https://medium.com/@preethikasireddy/why-use-static-types-in-javascript-part-1-8382da1e0adb) if you are new to this concept.

Recent versions of [Flow](http://flowtype.org/) work with Create React App projects out of the box.

To add Flow to a Create React App project, follow these steps:

1. Run `npm install --save flow-bin` (or `yarn add flow-bin`).
2. Add `"flow": "flow"` to the `scripts` section of your `package.json`.
3. Run `npm run flow init` (or `yarn flow init`) to create a [`.flowconfig` file](https://flowtype.org/docs/advanced-configuration.html) in the root directory.
4. Add `// @flow` to any files you want to type check (for example, to `src/App.js`).

Now you can run `npm run flow` (or `yarn flow`) to check the files for type errors.
You can optionally use an IDE like [Nuclide](https://nuclide.io/docs/languages/flow/) for a better integrated experience.
In the future we plan to integrate it into Create React App even more closely.

To learn more about Flow, check out [its documentation](https://flowtype.org/).

## Adding a Router

Create React App doesn't prescribe a specific routing solution, but [React Router](https://reacttraining.com/react-router/) is the most popular one.

To add it, run:

```sh
npm install --save react-router-dom
```

Alternatively you may use `yarn`:

```sh
yarn add react-router-dom
```

To try it, delete all the code in `src/App.js` and replace it with any of the examples on its website. The [Basic Example](https://reacttraining.com/react-router/web/example/basic) is a good place to get started.

Note that [you may need to configure your production server to support client-side routing](#serving-apps-with-client-side-routing) before deploying your app.

## Adding Custom Environment Variables

>Note: this feature is available with `react-scripts@0.2.3` and higher.

Your project can consume variables declared in your environment as if they were declared locally in your JS files. By
default you will have `NODE_ENV` defined for you, and any other environment variables starting with
`REACT_APP_`.

**The environment variables are embedded during the build time**. Since Create React App produces a static HTML/CSS/JS bundle, it can’t possibly read them at runtime. To read them at runtime, you would need to load HTML into memory on the server and replace placeholders in runtime, just like [described here](#injecting-data-from-the-server-into-the-page). Alternatively you can rebuild the app on the server anytime you change them.

>Note: You must create custom environment variables beginning with `REACT_APP_`. Any other variables except `NODE_ENV` will be ignored to avoid accidentally [exposing a private key on the machine that could have the same name](https://github.com/facebookincubator/create-react-app/issues/865#issuecomment-252199527). Changing any environment variables will require you to restart the development server if it is running.

These environment variables will be defined for you on `process.env`. For example, having an environment
variable named `REACT_APP_SECRET_CODE` will be exposed in your JS as `process.env.REACT_APP_SECRET_CODE`.

There is also a special built-in environment variable called `NODE_ENV`. You can read it from `process.env.NODE_ENV`. When you run `npm start`, it is always equal to `'development'`, when you run `npm test` it is always equal to `'test'`, and when you run `npm run build` to make a production bundle, it is always equal to `'production'`. **You cannot override `NODE_ENV` manually.** This prevents developers from accidentally deploying a slow development build to production.

These environment variables can be useful for displaying information conditionally based on where the project is
deployed or consuming sensitive data that lives outside of version control.

First, you need to have environment variables defined. For example, let’s say you wanted to consume a secret defined
in the environment inside a `<form>`:

```jsx
render() {
  return (
    <div>
      <small>You are running this application in <b>{process.env.NODE_ENV}</b> mode.</small>
      <form>
        <input type="hidden" defaultValue={process.env.REACT_APP_SECRET_CODE} />
      </form>
    </div>
  );
}
```

During the build, `process.env.REACT_APP_SECRET_CODE` will be replaced with the current value of the `REACT_APP_SECRET_CODE` environment variable. Remember that the `NODE_ENV` variable will be set for you automatically.

When you load the app in the browser and inspect the `<input>`, you will see its value set to `abcdef`, and the bold text will show the environment provided when using `npm start`:

```html
<div>
  <small>You are running this application in <b>development</b> mode.</small>
  <form>
    <input type="hidden" value="abcdef" />
  </form>
</div>
```

The above form is looking for a variable called `REACT_APP_SECRET_CODE` from the environment. In order to consume this
value, we need to have it defined in the environment. This can be done using two ways: either in your shell or in
a `.env` file. Both of these ways are described in the next few sections.

Having access to the `NODE_ENV` is also useful for performing actions conditionally:

```js
if (process.env.NODE_ENV !== 'production') {
  analytics.disable();
}
```

When you compile the app with `npm run build`, the minification step will strip out this condition, and the resulting bundle will be smaller.

### Referencing Environment Variables in the HTML

>Note: this feature is available with `react-scripts@0.9.0` and higher.

You can also access the environment variables starting with `REACT_APP_` in the `public/index.html`. For example:

```html
<title>%REACT_APP_WEBSITE_NAME%</title>
```

Note that the caveats from the above section apply:

* Apart from a few built-in variables (`NODE_ENV` and `PUBLIC_URL`), variable names must start with `REACT_APP_` to work.
* The environment variables are injected at build time. If you need to inject them at runtime, [follow this approach instead](#generating-dynamic-meta-tags-on-the-server).

### Adding Temporary Environment Variables In Your Shell

Defining environment variables can vary between OSes. It’s also important to know that this manner is temporary for the
life of the shell session.

#### Windows (cmd.exe)

```cmd
set "REACT_APP_SECRET_CODE=abcdef" && npm start
```

(Note: Quotes around the variable assignment are required to avoid a trailing whitespace.)

#### Windows (Powershell)

```Powershell
($env:REACT_APP_SECRET_CODE = "abcdef") -and (npm start)
```

#### Linux, macOS (Bash)

```bash
REACT_APP_SECRET_CODE=abcdef npm start
```

### Adding Development Environment Variables In `.env`

>Note: this feature is available with `react-scripts@0.5.0` and higher.

To define permanent environment variables, create a file called `.env` in the root of your project:

```
REACT_APP_SECRET_CODE=abcdef
```
>Note: You must create custom environment variables beginning with `REACT_APP_`. Any other variables except `NODE_ENV` will be ignored to avoid [accidentally exposing a private key on the machine that could have the same name](https://github.com/facebookincubator/create-react-app/issues/865#issuecomment-252199527). Changing any environment variables will require you to restart the development server if it is running.

`.env` files **should be** checked into source control (with the exclusion of `.env*.local`).

#### What other `.env` files can be used?

>Note: this feature is **available with `react-scripts@1.0.0` and higher**.

* `.env`: Default.
* `.env.local`: Local overrides. **This file is loaded for all environments except test.**
* `.env.development`, `.env.test`, `.env.production`: Environment-specific settings.
* `.env.development.local`, `.env.test.local`, `.env.production.local`: Local overrides of environment-specific settings.

Files on the left have more priority than files on the right:

* `npm start`: `.env.development.local`, `.env.development`, `.env.local`, `.env`
* `npm run build`: `.env.production.local`, `.env.production`, `.env.local`, `.env`
* `npm test`: `.env.test.local`, `.env.test`, `.env` (note `.env.local` is missing)

These variables will act as the defaults if the machine does not explicitly set them.<br>
Please refer to the [dotenv documentation](https://github.com/motdotla/dotenv) for more details.

>Note: If you are defining environment variables for development, your CI and/or hosting platform will most likely need
these defined as well. Consult their documentation how to do this. For example, see the documentation for [Travis CI](https://docs.travis-ci.com/user/environment-variables/) or [Heroku](https://devcenter.heroku.com/articles/config-vars).

#### Expanding Environment Variables In `.env`

>Note: this feature is available with `react-scripts@1.1.0` and higher.

Expand variables already on your machine for use in your `.env` file (using [dotenv-expand](https://github.com/motdotla/dotenv-expand)).

For example, to get the environment variable `npm_package_version`:

```
REACT_APP_VERSION=$npm_package_version
# also works:
# REACT_APP_VERSION=${npm_package_version}
```

Or expand variables local to the current `.env` file:

```
DOMAIN=www.example.com
REACT_APP_FOO=$DOMAIN/foo
REACT_APP_BAR=$DOMAIN/bar
```

## Can I Use Decorators?

Many popular libraries use [decorators](https://medium.com/google-developers/exploring-es7-decorators-76ecb65fb841) in their documentation.<br>
Create React App doesn’t support decorator syntax at the moment because:

* It is an experimental proposal and is subject to change.
* The current specification version is not officially supported by Babel.
* If the specification changes, we won’t be able to write a codemod because we don’t use them internally at Facebook.

However in many cases you can rewrite decorator-based code without decorators just as fine.<br>
Please refer to these two threads for reference:

* [#214](https://github.com/facebookincubator/create-react-app/issues/214)
* [#411](https://github.com/facebookincubator/create-react-app/issues/411)

Create React App will add decorator support when the specification advances to a stable stage.

## Fetching Data with AJAX Requests

React doesn't prescribe a specific approach to data fetching, but people commonly use either a library like [axios](https://github.com/axios/axios) or the [`fetch()` API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) provided by the browser. Conveniently, Create React App includes a polyfill for `fetch()` so you can use it without worrying about the browser support.

The global `fetch` function allows to easily makes AJAX requests. It takes in a URL as an input and returns a `Promise` that resolves to a `Response` object. You can find more information about `fetch` [here](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch).

This project also includes a [Promise polyfill](https://github.com/then/promise) which provides a full implementation of Promises/A+. A Promise represents the eventual result of an asynchronous operation, you can find more information about Promises [here](https://www.promisejs.org/) and [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise). Both axios and `fetch()` use Promises under the hood. You can also use the [`async / await`](https://davidwalsh.name/async-await) syntax to reduce the callback nesting.

You can learn more about making AJAX requests from React components in [the FAQ entry on the React website](https://reactjs.org/docs/faq-ajax.html).

## Integrating with an API Backend

These tutorials will help you to integrate your app with an API backend running on another port,
using `fetch()` to access it.

### Node
Check out [this tutorial](https://www.fullstackreact.com/articles/using-create-react-app-with-a-server/).
You can find the companion GitHub repository [here](https://github.com/fullstackreact/food-lookup-demo).

### Ruby on Rails

Check out [this tutorial](https://www.fullstackreact.com/articles/how-to-get-create-react-app-to-work-with-your-rails-api/).
You can find the companion GitHub repository [here](https://github.com/fullstackreact/food-lookup-demo-rails).

## Proxying API Requests in Development

>Note: this feature is available with `react-scripts@0.2.3` and higher.

People often serve the front-end React app from the same host and port as their backend implementation.<br>
For example, a production setup might look like this after the app is deployed:

```
/             - static server returns index.html with React app
/todos        - static server returns index.html with React app
/api/todos    - server handles any /api/* requests using the backend implementation
```

Such setup is **not** required. However, if you **do** have a setup like this, it is convenient to write requests like `fetch('/api/todos')` without worrying about redirecting them to another host or port during development.

To tell the development server to proxy any unknown requests to your API server in development, add a `proxy` field to your `package.json`, for example:

```js
  "proxy": "http://localhost:4000",
```

This way, when you `fetch('/api/todos')` in development, the development server will recognize that it’s not a static asset, and will proxy your request to `http://localhost:4000/api/todos` as a fallback. The development server will **only** attempt to send requests without `text/html` in its `Accept` header to the proxy.

Conveniently, this avoids [CORS issues](http://stackoverflow.com/questions/21854516/understanding-ajax-cors-and-security-considerations) and error messages like this in development:

```
Fetch API cannot load http://localhost:4000/api/todos. No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'http://localhost:3000' is therefore not allowed access. If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.
```

Keep in mind that `proxy` only has effect in development (with `npm start`), and it is up to you to ensure that URLs like `/api/todos` point to the right thing in production. You don’t have to use the `/api` prefix. Any unrecognized request without a `text/html` accept header will be redirected to the specified `proxy`.

The `proxy` option supports HTTP, HTTPS and WebSocket connections.<br>
If the `proxy` option is **not** flexible enough for you, alternatively you can:

* [Configure the proxy yourself](#configuring-the-proxy-manually)
* Enable CORS on your server ([here’s how to do it for Express](http://enable-cors.org/server_expressjs.html)).
* Use [environment variables](#adding-custom-environment-variables) to inject the right server host and port into your app.

### "Invalid Host Header" Errors After Configuring Proxy

When you enable the `proxy` option, you opt into a more strict set of host checks. This is necessary because leaving the backend open to remote hosts makes your computer vulnerable to DNS rebinding attacks. The issue is explained in [this article](https://medium.com/webpack/webpack-dev-server-middleware-security-issues-1489d950874a) and [this issue](https://github.com/webpack/webpack-dev-server/issues/887).

This shouldn’t affect you when developing on `localhost`, but if you develop remotely like [described here](https://github.com/facebookincubator/create-react-app/issues/2271), you will see this error in the browser after enabling the `proxy` option:

>Invalid Host header

To work around it, you can specify your public development host in a file called `.env.development` in the root of your project:

```
HOST=mypublicdevhost.com
```

If you restart the development server now and load the app from the specified host, it should work.

If you are still having issues or if you’re using a more exotic environment like a cloud editor, you can bypass the host check completely by adding a line to `.env.development.local`. **Note that this is dangerous and exposes your machine to remote code execution from malicious websites:**

```
# NOTE: THIS IS DANGEROUS!
# It exposes your machine to attacks from the websites you visit.
DANGEROUSLY_DISABLE_HOST_CHECK=true
```

We don’t recommend this approach.

### Configuring the Proxy Manually

>Note: this feature is available with `react-scripts@1.0.0` and higher.

If the `proxy` option is **not** flexible enough for you, you can specify an object in the following form (in `package.json`).<br>
You may also specify any configuration value [`http-proxy-middleware`](https://github.com/chimurai/http-proxy-middleware#options) or [`http-proxy`](https://github.com/nodejitsu/node-http-proxy#options) supports.
```js
{
  // ...
  "proxy": {
    "/api": {
      "target": "<url>",
      "ws": true
      // ...
    }
  }
  // ...
}
```

All requests matching this path will be proxies, no exceptions. This includes requests for `text/html`, which the standard `proxy` option does not proxy.

If you need to specify multiple proxies, you may do so by specifying additional entries.
Matches are regular expressions, so that you can use a regexp to match multiple paths.
```js
{
  // ...
  "proxy": {
    // Matches any request starting with /api
    "/api": {
      "target": "<url_1>",
      "ws": true
      // ...
    },
    // Matches any request starting with /foo
    "/foo": {
      "target": "<url_2>",
      "ssl": true,
      "pathRewrite": {
        "^/foo": "/foo/beta"
      }
      // ...
    },
    // Matches /bar/abc.html but not /bar/sub/def.html
    "/bar/[^/]*[.]html": {
      "target": "<url_3>",
      // ...
    },
    // Matches /baz/abc.html and /baz/sub/def.html
    "/baz/.*/.*[.]html": {
      "target": "<url_4>"
      // ...
    }
  }
  // ...
}
```

### Configuring a WebSocket Proxy

When setting up a WebSocket proxy, there are a some extra considerations to be aware of.

If you’re using a WebSocket engine like [Socket.io](https://socket.io/), you must have a Socket.io server running that you can use as the proxy target. Socket.io will not work with a standard WebSocket server. Specifically, don't expect Socket.io to work with [the websocket.org echo test](http://websocket.org/echo.html).

There’s some good documentation available for [setting up a Socket.io server](https://socket.io/docs/).

Standard WebSockets **will** work with a standard WebSocket server as well as the websocket.org echo test. You can use libraries like [ws](https://github.com/websockets/ws) for the server, with [native WebSockets in the browser](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket).

Either way, you can proxy WebSocket requests manually in `package.json`:

```js
{
  // ...
  "proxy": {
    "/socket": {
      // Your compatible WebSocket server
      "target": "ws://<socket_url>",
      // Tell http-proxy-middleware that this is a WebSocket proxy.
      // Also allows you to proxy WebSocket requests without an additional HTTP request
      // https://github.com/chimurai/http-proxy-middleware#external-websocket-upgrade
      "ws": true
      // ...
    }
  }
  // ...
}
```

## Using HTTPS in Development

>Note: this feature is available with `react-scripts@0.4.0` and higher.

You may require the dev server to serve pages over HTTPS. One particular case where this could be useful is when using [the "proxy" feature](#proxying-api-requests-in-development) to proxy requests to an API server when that API server is itself serving HTTPS.

To do this, set the `HTTPS` environment variable to `true`, then start the dev server as usual with `npm start`:

#### Windows (cmd.exe)

```cmd
set HTTPS=true&&npm start
```

#### Windows (Powershell)

```Powershell
($env:HTTPS = $true) -and (npm start)
```

(Note: the lack of whitespace is intentional.)

#### Linux, macOS (Bash)

```bash
HTTPS=true npm start
```

Note that the server will use a self-signed certificate, so your web browser will almost definitely display a warning upon accessing the page.

## Generating Dynamic `<meta>` Tags on the Server

Since Create React App doesn’t support server rendering, you might be wondering how to make `<meta>` tags dynamic and reflect the current URL. To solve this, we recommend to add placeholders into the HTML, like this:

```html
<!doctype html>
<html lang="en">
  <head>
    <meta property="og:title" content="__OG_TITLE__">
    <meta property="og:description" content="__OG_DESCRIPTION__">
```

Then, on the server, regardless of the backend you use, you can read `index.html` into memory and replace `__OG_TITLE__`, `__OG_DESCRIPTION__`, and any other placeholders with values depending on the current URL. Just make sure to sanitize and escape the interpolated values so that they are safe to embed into HTML!

If you use a Node server, you can even share the route matching logic between the client and the server. However duplicating it also works fine in simple cases.

## Pre-Rendering into Static HTML Files

If you’re hosting your `build` with a static hosting provider you can use [react-snapshot](https://www.npmjs.com/package/react-snapshot) or [react-snap](https://github.com/stereobooster/react-snap) to generate HTML pages for each route, or relative link, in your application. These pages will then seamlessly become active, or “hydrated”, when the JavaScript bundle has loaded.

There are also opportunities to use this outside of static hosting, to take the pressure off the server when generating and caching routes.

The primary benefit of pre-rendering is that you get the core content of each page _with_ the HTML payload—regardless of whether or not your JavaScript bundle successfully downloads. It also increases the likelihood that each route of your application will be picked up by search engines.

You can read more about [zero-configuration pre-rendering (also called snapshotting) here](https://medium.com/superhighfives/an-almost-static-stack-6df0a2791319).

## Injecting Data from the Server into the Page

Similarly to the previous section, you can leave some placeholders in the HTML that inject global variables, for example:

```js
<!doctype html>
<html lang="en">
  <head>
    <script>
      window.SERVER_DATA = __SERVER_DATA__;
    </script>
```

Then, on the server, you can replace `__SERVER_DATA__` with a JSON of real data right before sending the response. The client code can then read `window.SERVER_DATA` to use it. **Make sure to [sanitize the JSON before sending it to the client](https://medium.com/node-security/the-most-common-xss-vulnerability-in-react-js-applications-2bdffbcc1fa0) as it makes your app vulnerable to XSS attacks.**

## Running Tests

>Note: this feature is available with `react-scripts@0.3.0` and higher.<br>
>[Read the migration guide to learn how to enable it in older projects!](https://github.com/facebookincubator/create-react-app/blob/master/CHANGELOG.md#migrating-from-023-to-030)

Create React App uses [Jest](https://facebook.github.io/jest/) as its test runner. To prepare for this integration, we did a [major revamp](https://facebook.github.io/jest/blog/2016/09/01/jest-15.html) of Jest so if you heard bad things about it years ago, give it another try.

Jest is a Node-based runner. This means that the tests always run in a Node environment and not in a real browser. This lets us enable fast iteration speed and prevent flakiness.

While Jest provides browser globals such as `window` thanks to [jsdom](https://github.com/tmpvar/jsdom), they are only approximations of the real browser behavior. Jest is intended to be used for unit tests of your logic and your components rather than the DOM quirks.

We recommend that you use a separate tool for browser end-to-end tests if you need them. They are beyond the scope of Create React App.

### Filename Conventions

Jest will look for test files with any of the following popular naming conventions:

* Files with `.js` suffix in `__tests__` folders.
* Files with `.test.js` suffix.
* Files with `.spec.js` suffix.

The `.test.js` / `.spec.js` files (or the `__tests__` folders) can be located at any depth under the `src` top level folder.

We recommend to put the test files (or `__tests__` folders) next to the code they are testing so that relative imports appear shorter. For example, if `App.test.js` and `App.js` are in the same folder, the test just needs to `import App from './App'` instead of a long relative path. Colocation also helps find tests more quickly in larger projects.

### Command Line Interface

When you run `npm test`, Jest will launch in the watch mode. Every time you save a file, it will re-run the tests, just like `npm start` recompiles the code.

The watcher includes an interactive command-line interface with the ability to run all tests, or focus on a search pattern. It is designed this way so that you can keep it open and enjoy fast re-runs. You can learn the commands from the “Watch Usage” note that the watcher prints after every run:

![Jest watch mode](http://facebook.github.io/jest/img/blog/15-watch.gif)

### Version Control Integration

By default, when you run `npm test`, Jest will only run the tests related to files changed since the last commit. This is an optimization designed to make your tests run fast regardless of how many tests you have. However it assumes that you don’t often commit the code that doesn’t pass the tests.

Jest will always explicitly mention that it only ran tests related to the files changed since the last commit. You can also press `a` in the watch mode to force Jest to run all tests.

Jest will always run all tests on a [continuous integration](#continuous-integration) server or if the project is not inside a Git or Mercurial repository.

### Writing Tests

To create tests, add `it()` (or `test()`) blocks with the name of the test and its code. You may optionally wrap them in `describe()` blocks for logical grouping but this is neither required nor recommended.

Jest provides a built-in `expect()` global function for making assertions. A basic test could look like this:

```js
import sum from './sum';

it('sums numbers', () => {
  expect(sum(1, 2)).toEqual(3);
  expect(sum(2, 2)).toEqual(4);
});
```

All `expect()` matchers supported by Jest are [extensively documented here](https://facebook.github.io/jest/docs/en/expect.html#content).<br>
You can also use [`jest.fn()` and `expect(fn).toBeCalled()`](https://facebook.github.io/jest/docs/en/expect.html#tohavebeencalled) to create “spies” or mock functions.

### Testing Components

There is a broad spectrum of component testing techniques. They range from a “smoke test” verifying that a component renders without throwing, to shallow rendering and testing some of the output, to full rendering and testing component lifecycle and state changes.

Different projects choose different testing tradeoffs based on how often components change, and how much logic they contain. If you haven’t decided on a testing strategy yet, we recommend that you start with creating simple smoke tests for your components:

```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

it('renders without crashing', () => {
  const div = document.createElement('div');
  ReactDOM.render(<App />, div);
});
```

This test mounts a component and makes sure that it didn’t throw during rendering. Tests like this provide a lot of value with very little effort so they are great as a starting point, and this is the test you will find in `src/App.test.js`.

When you encounter bugs caused by changing components, you will gain a deeper insight into which parts of them are worth testing in your application. This might be a good time to introduce more specific tests asserting specific expected output or behavior.

If you’d like to test components in isolation from the child components they render, we recommend using [`shallow()` rendering API](http://airbnb.io/enzyme/docs/api/shallow.html) from [Enzyme](http://airbnb.io/enzyme/). To install it, run:

```sh
npm install --save enzyme enzyme-adapter-react-16 react-test-renderer
```

Alternatively you may use `yarn`:

```sh
yarn add enzyme enzyme-adapter-react-16 react-test-renderer
```

As of Enzyme 3, you will need to install Enzyme along with an Adapter corresponding to the version of React you are using. (The examples above use the adapter for React 16.)

The adapter will also need to be configured in your [global setup file](#initializing-test-environment):

#### `src/setupTests.js`
```js
import { configure } from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';

configure({ adapter: new Adapter() });
```

>Note: Keep in mind that if you decide to "eject" before creating `src/setupTests.js`, the resulting `package.json` file won't contain any reference to it. [Read here](#initializing-test-environment) to learn how to add this after ejecting.

Now you can write a smoke test with it:

```js
import React from 'react';
import { shallow } from 'enzyme';
import App from './App';

it('renders without crashing', () => {
  shallow(<App />);
});
```

Unlike the previous smoke test using `ReactDOM.render()`, this test only renders `<App>` and doesn’t go deeper. For example, even if `<App>` itself renders a `<Button>` that throws, this test will pass. Shallow rendering is great for isolated unit tests, but you may still want to create some full rendering tests to ensure the components integrate correctly. Enzyme supports [full rendering with `mount()`](http://airbnb.io/enzyme/docs/api/mount.html), and you can also use it for testing state changes and component lifecycle.

You can read the [Enzyme documentation](http://airbnb.io/enzyme/) for more testing techniques. Enzyme documentation uses Chai and Sinon for assertions but you don’t have to use them because Jest provides built-in `expect()` and `jest.fn()` for spies.

Here is an example from Enzyme documentation that asserts specific output, rewritten to use Jest matchers:

```js
import React from 'react';
import { shallow } from 'enzyme';
import App from './App';

it('renders welcome message', () => {
  const wrapper = shallow(<App />);
  const welcome = <h2>Welcome to React</h2>;
  // expect(wrapper.contains(welcome)).to.equal(true);
  expect(wrapper.contains(welcome)).toEqual(true);
});
```

All Jest matchers are [extensively documented here](http://facebook.github.io/jest/docs/en/expect.html).<br>
Nevertheless you can use a third-party assertion library like [Chai](http://chaijs.com/) if you want to, as described below.

Additionally, you might find [jest-enzyme](https://github.com/blainekasten/enzyme-matchers) helpful to simplify your tests with readable matchers. The above `contains` code can be written more simply with jest-enzyme.

```js
expect(wrapper).toContainReact(welcome)
```

To enable this, install `jest-enzyme`:

```sh
npm install --save jest-enzyme
```

Alternatively you may use `yarn`:

```sh
yarn add jest-enzyme
```

Import it in [`src/setupTests.js`](#initializing-test-environment) to make its matchers available in every test:

```js
import 'jest-enzyme';
```

#### Use `react-testing-library`

As an alternative or companion to `enzyme`, you may consider using `react-testing-library`. [`react-testing-library`](https://github.com/kentcdodds/react-testing-library) is a library for testing React components in a way that resembles the way the components are used by end users. It is well suited for unit, integration, and end-to-end testing of React components and applications. It works more directly with DOM nodes, and therefore it's recommended to use with [`jest-dom`](https://github.com/gnapse/jest-dom) for improved assertions.

To install `react-testing-library` and `jest-dom`, you can run:

```sh
npm install --save react-testing-library jest-dom
```

Alternatively you may use `yarn`:

```sh
yarn add react-testing-library jest-dom
```

Similar to `enzyme` you can create a `src/setupTests.js` file to avoid boilerplate in your test files:

```js
// react-testing-library renders your components to document.body,
// this will ensure they're removed after each test.
import 'react-testing-library/cleanup-after-each';

// this adds jest-dom's custom assertions
import 'jest-dom/extend-expect';
```

Here's an example of using `react-testing-library` and `jest-dom` for testing that the `<App />` component renders "Welcome to React".

```js
import React from 'react';
import { render } from 'react-testing-library';
import App from './App';

it('renders welcome message', () => {
  const { getByText } = render(<App />);
  expect(getByText('Welcome to React')).toBeInTheDOM();
});
```

Learn more about the utilities provided by `react-testing-library` to facilitate testing asynchronous interactions as well as selecting form elements from [the `react-testing-library` documentation](https://github.com/kentcdodds/react-testing-library) and [examples](https://codesandbox.io/s/github/kentcdodds/react-testing-library-examples).

### Using Third Party Assertion Libraries

We recommend that you use `expect()` for assertions and `jest.fn()` for spies. If you are having issues with them please [file those against Jest](https://github.com/facebook/jest/issues/new), and we’ll fix them. We intend to keep making them better for React, supporting, for example, [pretty-printing React elements as JSX](https://github.com/facebook/jest/pull/1566).

However, if you are used to other libraries, such as [Chai](http://chaijs.com/) and [Sinon](http://sinonjs.org/), or if you have existing code using them that you’d like to port over, you can import them normally like this:

```js
import sinon from 'sinon';
import { expect } from 'chai';
```

and then use them in your tests like you normally do.

### Initializing Test Environment

>Note: this feature is available with `react-scripts@0.4.0` and higher.

If your app uses a browser API that you need to mock in your tests or if you just need a global setup before running your tests, add a `src/setupTests.js` to your project. It will be automatically executed before running your tests.

For example:

#### `src/setupTests.js`
```js
const localStorageMock = {
  getItem: jest.fn(),
  setItem: jest.fn(),
  clear: jest.fn()
};
global.localStorage = localStorageMock
```

>Note: Keep in mind that if you decide to "eject" before creating `src/setupTests.js`, the resulting `package.json` file won't contain any reference to it, so you should manually create the property `setupTestFrameworkScriptFile` in the configuration for Jest, something like the following:

>```js
>"jest": {
>   // ...
>   "setupTestFrameworkScriptFile": "<rootDir>/src/setupTests.js"
>  }
>  ```

### Focusing and Excluding Tests

You can replace `it()` with `xit()` to temporarily exclude a test from being executed.<br>
Similarly, `fit()` lets you focus on a specific test without running any other tests.

### Coverage Reporting

Jest has an integrated coverage reporter that works well with ES6 and requires no configuration.<br>
Run `npm test -- --coverage` (note extra `--` in the middle) to include a coverage report like this:

![coverage report](http://i.imgur.com/5bFhnTS.png)

Note that tests run much slower with coverage so it is recommended to run it separately from your normal workflow.

#### Configuration

The default Jest coverage configuration can be overriden by adding any of the following supported keys to a Jest config in your package.json.

Supported overrides:
 - [`collectCoverageFrom`](https://facebook.github.io/jest/docs/en/configuration.html#collectcoveragefrom-array)
 - [`coverageReporters`](https://facebook.github.io/jest/docs/en/configuration.html#coveragereporters-array-string)
 - [`coverageThreshold`](https://facebook.github.io/jest/docs/en/configuration.html#coveragethreshold-object)
 - [`snapshotSerializers`](https://facebook.github.io/jest/docs/en/configuration.html#snapshotserializers-array-string)

Example package.json:

```json
{
  "name": "your-package",
  "jest": {
    "collectCoverageFrom" : [
      "src/**/*.{js,jsx}",
      "!<rootDir>/node_modules/",
      "!<rootDir>/path/to/dir/"
    ],
    "coverageThreshold": {
      "global": {
        "branches": 90,
        "functions": 90,
        "lines": 90,
        "statements": 90
      }
    },
    "coverageReporters": ["text"],
    "snapshotSerializers": ["my-serializer-module"]
  }
}
```

### Continuous Integration

By default `npm test` runs the watcher with interactive CLI. However, you can force it to run tests once and finish the process by setting an environment variable called `CI`.

When creating a build of your application with `npm run build` linter warnings are not checked by default. Like `npm test`, you can force the build to perform a linter warning check by setting the environment variable `CI`. If any warnings are encountered then the build fails.

Popular CI servers already set the environment variable `CI` by default but you can do this yourself too:

### On CI servers
#### Travis CI

1. Following the [Travis Getting started](https://docs.travis-ci.com/user/getting-started/) guide for syncing your GitHub repository with Travis.  You may need to initialize some settings manually in your [profile](https://travis-ci.org/profile) page.
1. Add a `.travis.yml` file to your git repository.
```
language: node_js
node_js:
  - 6
cache:
  directories:
    - node_modules
script:
  - npm run build
  - npm test
```
1. Trigger your first build with a git push.
1. [Customize your Travis CI Build](https://docs.travis-ci.com/user/customizing-the-build/) if needed.

#### CircleCI

Follow [this article](https://medium.com/@knowbody/circleci-and-zeits-now-sh-c9b7eebcd3c1) to set up CircleCI with a Create React App project.

### On your own environment
##### Windows (cmd.exe)

```cmd
set CI=true&&npm test
```

```cmd
set CI=true&&npm run build
```

(Note: the lack of whitespace is intentional.)

##### Windows (Powershell)

```Powershell
($env:CI = $true) -and (npm test)
```

```Powershell
($env:CI = $true) -and (npm run build)
```

##### Linux, macOS (Bash)

```bash
CI=true npm test
```

```bash
CI=true npm run build
```

The test command will force Jest to run tests once instead of launching the watcher.

>  If you find yourself doing this often in development, please [file an issue](https://github.com/facebookincubator/create-react-app/issues/new) to tell us about your use case because we want to make watcher the best experience and are open to changing how it works to accommodate more workflows.

The build command will check for linter warnings and fail if any are found.

### Disabling jsdom

By default, the `package.json` of the generated project looks like this:

```js
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom"
```

If you know that none of your tests depend on [jsdom](https://github.com/tmpvar/jsdom), you can safely remove `--env=jsdom`, and your tests will run faster:

```diff
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
-   "test": "react-scripts test --env=jsdom"
+   "test": "react-scripts test"
```

To help you make up your mind, here is a list of APIs that **need jsdom**:

* Any browser globals like `window` and `document`
* [`ReactDOM.render()`](https://facebook.github.io/react/docs/top-level-api.html#reactdom.render)
* [`TestUtils.renderIntoDocument()`](https://facebook.github.io/react/docs/test-utils.html#renderintodocument) ([a shortcut](https://github.com/facebook/react/blob/34761cf9a252964abfaab6faf74d473ad95d1f21/src/test/ReactTestUtils.js#L83-L91) for the above)
* [`mount()`](http://airbnb.io/enzyme/docs/api/mount.html) in [Enzyme](http://airbnb.io/enzyme/index.html)

In contrast, **jsdom is not needed** for the following APIs:

* [`TestUtils.createRenderer()`](https://facebook.github.io/react/docs/test-utils.html#shallow-rendering) (shallow rendering)
* [`shallow()`](http://airbnb.io/enzyme/docs/api/shallow.html) in [Enzyme](http://airbnb.io/enzyme/index.html)

Finally, jsdom is also not needed for [snapshot testing](http://facebook.github.io/jest/blog/2016/07/27/jest-14.html).

### Snapshot Testing

Snapshot testing is a feature of Jest that automatically generates text snapshots of your components and saves them on the disk so if the UI output changes, you get notified without manually writing any assertions on the component output. [Read more about snapshot testing.](http://facebook.github.io/jest/blog/2016/07/27/jest-14.html)

### Editor Integration

If you use [Visual Studio Code](https://code.visualstudio.com), there is a [Jest extension](https://github.com/orta/vscode-jest) which works with Create React App out of the box. This provides a lot of IDE-like features while using a text editor: showing the status of a test run with potential fail messages inline, starting and stopping the watcher automatically, and offering one-click snapshot updates.

![VS Code Jest Preview](https://cloud.githubusercontent.com/assets/49038/20795349/a032308a-b7c8-11e6-9b34-7eeac781003f.png)

## Debugging Tests

There are various ways to setup a debugger for your Jest tests. We cover debugging in Chrome and [Visual Studio Code](https://code.visualstudio.com/).

>Note: debugging tests requires Node 8 or higher.

### Debugging Tests in Chrome

Add the following to the `scripts` section in your project's `package.json`
```json
"scripts": {
    "test:debug": "react-scripts --inspect-brk test --runInBand --env=jsdom"
  }
```
Place `debugger;` statements in any test and run:
```bash
$ npm run test:debug
```

This will start running your Jest tests, but pause before executing to allow a debugger to attach to the process.

Open the following in Chrome
```
about:inspect
```

After opening that link, the Chrome Developer Tools will be displayed. Select `inspect` on your process and a breakpoint will be set at the first line of the react script (this is done simply to give you time to open the developer tools and to prevent Jest from executing before you have time to do so). Click the button that looks like a "play" button in the upper right hand side of the screen to continue execution. When Jest executes the test that contains the debugger statement, execution will pause and you can examine the current scope and call stack.

>Note: the --runInBand cli option makes sure Jest runs test in the same process rather than spawning processes for individual tests. Normally Jest parallelizes test runs across processes but it is hard to debug many processes at the same time.

### Debugging Tests in Visual Studio Code

Debugging Jest tests is supported out of the box for [Visual Studio Code](https://code.visualstudio.com).

Use the following [`launch.json`](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations) configuration file:
```
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Debug CRA Tests",
      "type": "node",
      "request": "launch",
      "runtimeExecutable": "${workspaceRoot}/node_modules/.bin/react-scripts",      
      "args": [
        "test",
        "--runInBand",
        "--no-cache",
        "--env=jsdom"
      ],
      "cwd": "${workspaceRoot}",
      "protocol": "inspector",
      "console": "integratedTerminal",
      "internalConsoleOptions": "neverOpen"
    }
  ]
}
```

## Developing Components in Isolation

Usually, in an app, you have a lot of UI components, and each of them has many different states.
For an example, a simple button component could have following states:

* In a regular state, with a text label.
* In the disabled mode.
* In a loading state.

Usually, it’s hard to see these states without running a sample app or some examples.

Create React App doesn’t include any tools for this by default, but you can easily add [Storybook for React](https://storybook.js.org) ([source](https://github.com/storybooks/storybook)) or [React Styleguidist](https://react-styleguidist.js.org/) ([source](https://github.com/styleguidist/react-styleguidist)) to your project. **These are third-party tools that let you develop components and see all their states in isolation from your app**.

![Storybook for React Demo](http://i.imgur.com/7CIAWpB.gif)

You can also deploy your Storybook or style guide as a static app. This way, everyone in your team can view and review different states of UI components without starting a backend server or creating an account in your app.

### Getting Started with Storybook

Storybook is a development environment for React UI components. It allows you to browse a component library, view the different states of each component, and interactively develop and test components.

First, install the following npm package globally:

```sh
npm install -g @storybook/cli
```

Then, run the following command inside your app’s directory:

```sh
getstorybook
```

After that, follow the instructions on the screen.

Learn more about React Storybook:

* Screencast: [Getting Started with React Storybook](https://egghead.io/lessons/react-getting-started-with-react-storybook)
* [GitHub Repo](https://github.com/storybooks/storybook)
* [Documentation](https://storybook.js.org/basics/introduction/)
* [Snapshot Testing UI](https://github.com/storybooks/storybook/tree/master/addons/storyshots) with Storybook + addon/storyshot

### Getting Started with Styleguidist

Styleguidist combines a style guide, where all your components are presented on a single page with their props documentation and usage examples, with an environment for developing components in isolation, similar to Storybook. In Styleguidist you write examples in Markdown, where each code snippet is rendered as a live editable playground.

First, install Styleguidist:

```sh
npm install --save react-styleguidist
```

Alternatively you may use `yarn`:

```sh
yarn add react-styleguidist
```

Then, add these scripts to your `package.json`:

```diff
   "scripts": {
+    "styleguide": "styleguidist server",
+    "styleguide:build": "styleguidist build",
     "start": "react-scripts start",
```

Then, run the following command inside your app’s directory:

```sh
npm run styleguide
```

After that, follow the instructions on the screen.

Learn more about React Styleguidist:

* [GitHub Repo](https://github.com/styleguidist/react-styleguidist)
* [Documentation](https://react-styleguidist.js.org/docs/getting-started.html)

## Publishing Components to npm

Create React App doesn't provide any built-in functionality to publish a component to npm. If you're ready to extract a component from your project so other people can use it, we recommend moving it to a separate directory outside of your project and then using a tool like [nwb](https://github.com/insin/nwb#react-components-and-libraries) to prepare it for publishing.

## Making a Progressive Web App

By default, the production build is a fully functional, offline-first
[Progressive Web App](https://developers.google.com/web/progressive-web-apps/).

Progressive Web Apps are faster and more reliable than traditional web pages, and provide an engaging mobile experience:

 * All static site assets are cached so that your page loads fast on subsequent visits, regardless of network connectivity (such as 2G or 3G). Updates are downloaded in the background.
 * Your app will work regardless of network state, even if offline. This means your users will be able to use your app at 10,000 feet and on the subway.
 * On mobile devices, your app can be added directly to the user's home screen, app icon and all. You can also re-engage users using web **push notifications**. This eliminates the need for the app store.

The [`sw-precache-webpack-plugin`](https://github.com/goldhand/sw-precache-webpack-plugin)
is integrated into production configuration,
and it will take care of generating a service worker file that will automatically
precache all of your local assets and keep them up to date as you deploy updates.
The service worker will use a [cache-first strategy](https://developers.google.com/web/fundamentals/instant-and-offline/offline-cookbook/#cache-falling-back-to-network)
for handling all requests for local assets, including the initial HTML, ensuring
that your web app is reliably fast, even on a slow or unreliable network.

### Opting Out of Caching

If you would prefer not to enable service workers prior to your initial
production deployment, then remove the call to `registerServiceWorker()`
from [`src/index.js`](src/index.js).

If you had previously enabled service workers in your production deployment and
have decided that you would like to disable them for all your existing users,
you can swap out the call to `registerServiceWorker()` in
[`src/index.js`](src/index.js) first by modifying the service worker import:
```javascript
import { unregister } from './registerServiceWorker';
```
and then call `unregister()` instead.
After the user visits a page that has `unregister()`,
the service worker will be uninstalled. Note that depending on how `/service-worker.js` is served,
it may take up to 24 hours for the cache to be invalidated.

### Offline-First Considerations

1. Service workers [require HTTPS](https://developers.google.com/web/fundamentals/getting-started/primers/service-workers#you_need_https),
although to facilitate local testing, that policy
[does not apply to `localhost`](http://stackoverflow.com/questions/34160509/options-for-testing-service-workers-via-http/34161385#34161385).
If your production web server does not support HTTPS, then the service worker
registration will fail, but the rest of your web app will remain functional.

1. Service workers are [not currently supported](https://jakearchibald.github.io/isserviceworkerready/)
in all web browsers. Service worker registration [won't be attempted](src/registerServiceWorker.js)
on browsers that lack support.

1. The service worker is only enabled in the [production environment](#deployment),
e.g. the output of `npm run build`. It's recommended that you do not enable an
offline-first service worker in a development environment, as it can lead to
frustration when previously cached assets are used and do not include the latest
changes you've made locally.

1. If you *need* to test your offline-first service worker locally, build
the application (using `npm run build`) and run a simple http server from your
build directory. After running the build script, `create-react-app` will give
instructions for one way to test your production build locally and the [deployment instructions](#deployment) have
instructions for using other methods. *Be sure to always use an
incognito window to avoid complications with your browser cache.*

1. If possible, configure your production environment to serve the generated
`service-worker.js` [with HTTP caching disabled](http://stackoverflow.com/questions/38843970/service-worker-javascript-update-frequency-every-24-hours).
If that's not possible—[GitHub Pages](#github-pages), for instance, does not
allow you to change the default 10 minute HTTP cache lifetime—then be aware
that if you visit your production site, and then revisit again before
`service-worker.js` has expired from your HTTP cache, you'll continue to get
the previously cached assets from the service worker. If you have an immediate
need to view your updated production deployment, performing a shift-refresh
will temporarily disable the service worker and retrieve all assets from the
network.

1. Users aren't always familiar with offline-first web apps. It can be useful to
[let the user know](https://developers.google.com/web/fundamentals/instant-and-offline/offline-ux#inform_the_user_when_the_app_is_ready_for_offline_consumption)
when the service worker has finished populating your caches (showing a "This web
app works offline!" message) and also let them know when the service worker has
fetched the latest updates that will be available the next time they load the
page (showing a "New content is available; please refresh." message). Showing
this messages is currently left as an exercise to the developer, but as a
starting point, you can make use of the logic included in [`src/registerServiceWorker.js`](src/registerServiceWorker.js), which
demonstrates which service worker lifecycle events to listen for to detect each
scenario, and which as a default, just logs appropriate messages to the
JavaScript console.

1. By default, the generated service worker file will not intercept or cache any
cross-origin traffic, like HTTP [API requests](#integrating-with-an-api-backend),
images, or embeds loaded from a different domain. If you would like to use a
runtime caching strategy for those requests, you can [`eject`](#npm-run-eject)
and then configure the
[`runtimeCaching`](https://github.com/GoogleChrome/sw-precache#runtimecaching-arrayobject)
option in the `SWPrecacheWebpackPlugin` section of
[`webpack.config.prod.js`](../config/webpack.config.prod.js).

### Progressive Web App Metadata

The default configuration includes a web app manifest located at
[`public/manifest.json`](public/manifest.json), that you can customize with
details specific to your web application.

When a user adds a web app to their homescreen using Chrome or Firefox on
Android, the metadata in [`manifest.json`](public/manifest.json) determines what
icons, names, and branding colors to use when the web app is displayed.
[The Web App Manifest guide](https://developers.google.com/web/fundamentals/engage-and-retain/web-app-manifest/)
provides more context about what each field means, and how your customizations
will affect your users' experience.

## Analyzing the Bundle Size

[Source map explorer](https://www.npmjs.com/package/source-map-explorer) analyzes
JavaScript bundles using the source maps. This helps you understand where code
bloat is coming from.

To add Source map explorer to a Create React App project, follow these steps:

```sh
npm install --save source-map-explorer
```

Alternatively you may use `yarn`:

```sh
yarn add source-map-explorer
```

Then in `package.json`, add the following line to `scripts`:

```diff
   "scripts": {
+    "analyze": "source-map-explorer build/static/js/main.*",
     "start": "react-scripts start",
     "build": "react-scripts build",
     "test": "react-scripts test --env=jsdom",
```

Then to analyze the bundle run the production build then run the analyze
script.

```
npm run build
npm run analyze
```

## Deployment

`npm run build` creates a `build` directory with a production build of your app. Set up your favorite HTTP server so that a visitor to your site is served `index.html`, and requests to static paths like `/static/js/main.<hash>.js` are served with the contents of the `/static/js/main.<hash>.js` file.

### Static Server

For environments using [Node](https://nodejs.org/), the easiest way to handle this would be to install [serve](https://github.com/zeit/serve) and let it handle the rest:

```sh
npm install -g serve
serve -s build
```

The last command shown above will serve your static site on the port **5000**. Like many of [serve](https://github.com/zeit/serve)’s internal settings, the port can be adjusted using the `-p` or `--port` flags.

Run this command to get a full list of the options available:

```sh
serve -h
```

### Other Solutions

You don’t necessarily need a static server in order to run a Create React App project in production. It works just as fine integrated into an existing dynamic one.

Here’s a programmatic example using [Node](https://nodejs.org/) and [Express](http://expressjs.com/):

```javascript
const express = require('express');
const path = require('path');
const app = express();

app.use(express.static(path.join(__dirname, 'build')));

app.get('/', function (req, res) {
  res.sendFile(path.join(__dirname, 'build', 'index.html'));
});

app.listen(9000);
```

The choice of your server software isn’t important either. Since Create React App is completely platform-agnostic, there’s no need to explicitly use Node.

The `build` folder with static assets is the only output produced by Create React App.

However this is not quite enough if you use client-side routing. Read the next section if you want to support URLs like `/todos/42` in your single-page app.

### Serving Apps with Client-Side Routing

If you use routers that use the HTML5 [`pushState` history API](https://developer.mozilla.org/en-US/docs/Web/API/History_API#Adding_and_modifying_history_entries) under the hood (for example, [React Router](https://github.com/ReactTraining/react-router) with `browserHistory`), many static file servers will fail. For example, if you used React Router with a route for `/todos/42`, the development server will respond to `localhost:3000/todos/42` properly, but an Express serving a production build as above will not.

This is because when there is a fresh page load for a `/todos/42`, the server looks for the file `build/todos/42` and does not find it. The server needs to be configured to respond to a request to `/todos/42` by serving `index.html`. For example, we can amend our Express example above to serve `index.html` for any unknown paths:

```diff
 app.use(express.static(path.join(__dirname, 'build')));

-app.get('/', function (req, res) {
+app.get('/*', function (req, res) {
   res.sendFile(path.join(__dirname, 'build', 'index.html'));
 });
```

If you’re using [Apache HTTP Server](https://httpd.apache.org/), you need to create a `.htaccess` file in the `public` folder that looks like this:

```
    Options -MultiViews
    RewriteEngine On
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^ index.html [QSA,L]
```

It will get copied to the `build` folder when you run `npm run build`. 

If you’re using [Apache Tomcat](http://tomcat.apache.org/), you need to follow [this Stack Overflow answer](https://stackoverflow.com/a/41249464/4878474).

Now requests to `/todos/42` will be handled correctly both in development and in production.

On a production build, and in a browser that supports [service workers](https://developers.google.com/web/fundamentals/getting-started/primers/service-workers),
the service worker will automatically handle all navigation requests, like for
`/todos/42`, by serving the cached copy of your `index.html`. This
service worker navigation routing can be configured or disabled by
[`eject`ing](#npm-run-eject) and then modifying the
[`navigateFallback`](https://github.com/GoogleChrome/sw-precache#navigatefallback-string)
and [`navigateFallbackWhitelist`](https://github.com/GoogleChrome/sw-precache#navigatefallbackwhitelist-arrayregexp)
options of the `SWPreachePlugin` [configuration](../config/webpack.config.prod.js).

When users install your app to the homescreen of their device the default configuration will make a shortcut to `/index.html`. This may not work for client-side routers which expect the app to be served from `/`. Edit the web app manifest at [`public/manifest.json`](public/manifest.json) and change `start_url` to match the required URL scheme, for example:

```js
  "start_url": ".",
```

### Building for Relative Paths

By default, Create React App produces a build assuming your app is hosted at the server root.<br>
To override this, specify the `homepage` in your `package.json`, for example:

```js
  "homepage": "http://mywebsite.com/relativepath",
```

This will let Create React App correctly infer the root path to use in the generated HTML file.

**Note**: If you are using `react-router@^4`, you can root `<Link>`s using the `basename` prop on any `<Router>`.<br>
More information [here](https://reacttraining.com/react-router/web/api/BrowserRouter/basename-string).<br>
<br>
For example:
```js
<BrowserRouter basename="/calendar"/>
<Link to="/today"/> // renders <a href="/calendar/today">
```

#### Serving the Same Build from Different Paths

>Note: this feature is available with `react-scripts@0.9.0` and higher.

If you are not using the HTML5 `pushState` history API or not using client-side routing at all, it is unnecessary to specify the URL from which your app will be served. Instead, you can put this in your `package.json`:

```js
  "homepage": ".",
```

This will make sure that all the asset paths are relative to `index.html`. You will then be able to move your app from `http://mywebsite.com` to `http://mywebsite.com/relativepath` or even `http://mywebsite.com/relative/path` without having to rebuild it.

### [Azure](https://azure.microsoft.com/)

See [this](https://medium.com/@to_pe/deploying-create-react-app-on-microsoft-azure-c0f6686a4321) blog post on how to deploy your React app to Microsoft Azure.

See [this](https://medium.com/@strid/host-create-react-app-on-azure-986bc40d5bf2#.pycfnafbg) blog post or [this](https://github.com/ulrikaugustsson/azure-appservice-static) repo for a way to use automatic deployment to Azure App Service.

### [Firebase](https://firebase.google.com/)

Install the Firebase CLI if you haven’t already by running `npm install -g firebase-tools`. Sign up for a [Firebase account](https://console.firebase.google.com/) and create a new project. Run `firebase login` and login with your previous created Firebase account.

Then run the `firebase init` command from your project’s root. You need to choose the **Hosting: Configure and deploy Firebase Hosting sites** and choose the Firebase project you created in the previous step. You will need to agree with `database.rules.json` being created, choose `build` as the public directory, and also agree to **Configure as a single-page app** by replying with `y`.

```sh
    === Project Setup

    First, let's associate this project directory with a Firebase project.
    You can create multiple project aliases by running firebase use --add,
    but for now we'll just set up a default project.

    ? What Firebase project do you want to associate as default? Example app (example-app-fd690)

    === Database Setup

    Firebase Realtime Database Rules allow you to define how your data should be
    structured and when your data can be read from and written to.

    ? What file should be used for Database Rules? database.rules.json
    ✔  Database Rules for example-app-fd690 have been downloaded to database.rules.json.
    Future modifications to database.rules.json will update Database Rules when you run
    firebase deploy.

    === Hosting Setup

    Your public directory is the folder (relative to your project directory) that
    will contain Hosting assets to uploaded with firebase deploy. If you
    have a build process for your assets, use your build's output directory.

    ? What do you want to use as your public directory? build
    ? Configure as a single-page app (rewrite all urls to /index.html)? Yes
    ✔  Wrote build/index.html

    i  Writing configuration info to firebase.json...
    i  Writing project information to .firebaserc...

    ✔  Firebase initialization complete!
```

IMPORTANT: you need to set proper HTTP caching headers for `service-worker.js` file in `firebase.json` file or you will not be able to see changes after first deployment ([issue #2440](https://github.com/facebookincubator/create-react-app/issues/2440)). It should be added inside `"hosting"` key like next:

```
{
  "hosting": {
    ...
    "headers": [
      {"source": "/service-worker.js", "headers": [{"key": "Cache-Control", "value": "no-cache"}]}
    ]
    ...
```

Now, after you create a production build with `npm run build`, you can deploy it by running `firebase deploy`.

```sh
    === Deploying to 'example-app-fd690'...

    i  deploying database, hosting
    ✔  database: rules ready to deploy.
    i  hosting: preparing build directory for upload...
    Uploading: [==============================          ] 75%✔  hosting: build folder uploaded successfully
    ✔  hosting: 8 files uploaded successfully
    i  starting release process (may take several minutes)...

    ✔  Deploy complete!

    Project Console: https://console.firebase.google.com/project/example-app-fd690/overview
    Hosting URL: https://example-app-fd690.firebaseapp.com
```

For more information see [Add Firebase to your JavaScript Project](https://firebase.google.com/docs/web/setup).

### [GitHub Pages](https://pages.github.com/)

>Note: this feature is available with `react-scripts@0.2.0` and higher.

#### Step 1: Add `homepage` to `package.json`

**The step below is important!**<br>
**If you skip it, your app will not deploy correctly.**

Open your `package.json` and add a `homepage` field for your project:

```json
  "homepage": "https://myusername.github.io/my-app",
```

or for a GitHub user page:

```json
  "homepage": "https://myusername.github.io",
```

Create React App uses the `homepage` field to determine the root URL in the built HTML file.

#### Step 2: Install `gh-pages` and add `deploy` to `scripts` in `package.json`

Now, whenever you run `npm run build`, you will see a cheat sheet with instructions on how to deploy to GitHub Pages.

To publish it at [https://myusername.github.io/my-app](https://myusername.github.io/my-app), run:

```sh
npm install --save gh-pages
```

Alternatively you may use `yarn`:

```sh
yarn add gh-pages
```

Add the following scripts in your `package.json`:

```diff
  "scripts": {
+   "predeploy": "npm run build",
+   "deploy": "gh-pages -d build",
    "start": "react-scripts start",
    "build": "react-scripts build",
```

The `predeploy` script will run automatically before `deploy` is run.

If you are deploying to a GitHub user page instead of a project page you'll need to make two
additional modifications:

1. First, change your repository's source branch to be any branch other than **master**.
1. Additionally, tweak your `package.json` scripts to push deployments to **master**:
```diff
  "scripts": {
    "predeploy": "npm run build",
-   "deploy": "gh-pages -d build",
+   "deploy": "gh-pages -b master -d build",
```

#### Step 3: Deploy the site by running `npm run deploy`

Then run:

```sh
npm run deploy
```

#### Step 4: Ensure your project’s settings use `gh-pages`

Finally, make sure **GitHub Pages** option in your GitHub project settings is set to use the `gh-pages` branch:

<img src="http://i.imgur.com/HUjEr9l.png" width="500" alt="gh-pages branch setting">

#### Step 5: Optionally, configure the domain

You can configure a custom domain with GitHub Pages by adding a `CNAME` file to the `public/` folder.

#### Notes on client-side routing

GitHub Pages doesn’t support routers that use the HTML5 `pushState` history API under the hood (for example, React Router using `browserHistory`). This is because when there is a fresh page load for a url like `http://user.github.io/todomvc/todos/42`, where `/todos/42` is a frontend route, the GitHub Pages server returns 404 because it knows nothing of `/todos/42`. If you want to add a router to a project hosted on GitHub Pages, here are a couple of solutions:

* You could switch from using HTML5 history API to routing with hashes. If you use React Router, you can switch to `hashHistory` for this effect, but the URL will be longer and more verbose (for example, `http://user.github.io/todomvc/#/todos/42?_k=yknaj`). [Read more](https://reacttraining.com/react-router/web/api/Router) about different history implementations in React Router.
* Alternatively, you can use a trick to teach GitHub Pages to handle 404 by redirecting to your `index.html` page with a special redirect parameter. You would need to add a `404.html` file with the redirection code to the `build` folder before deploying your project, and you’ll need to add code handling the redirect parameter to `index.html`. You can find a detailed explanation of this technique [in this guide](https://github.com/rafrex/spa-github-pages).

#### Troubleshooting

##### "/dev/tty: No such a device or address"

If, when deploying, you get `/dev/tty: No such a device or address` or a similar error, try the follwing:

1. Create a new [Personal Access Token](https://github.com/settings/tokens)
2. `git remote set-url origin https://<user>:<token>@github.com/<user>/<repo>` .
3. Try `npm run deploy again`

### [Heroku](https://www.heroku.com/)

Use the [Heroku Buildpack for Create React App](https://github.com/mars/create-react-app-buildpack).<br>
You can find instructions in [Deploying React with Zero Configuration](https://blog.heroku.com/deploying-react-with-zero-configuration).

#### Resolving Heroku Deployment Errors

Sometimes `npm run build` works locally but fails during deploy via Heroku. Following are the most common cases.

##### "Module not found: Error: Cannot resolve 'file' or 'directory'"

If you get something like this:

```
remote: Failed to create a production build. Reason:
remote: Module not found: Error: Cannot resolve 'file' or 'directory'
MyDirectory in /tmp/build_1234/src
```

It means you need to ensure that the lettercase of the file or directory you `import` matches the one you see on your filesystem or on GitHub.

This is important because Linux (the operating system used by Heroku) is case sensitive. So `MyDirectory` and `mydirectory` are two distinct directories and thus, even though the project builds locally, the difference in case breaks the `import` statements on Heroku remotes.

##### "Could not find a required file."

If you exclude or ignore necessary files from the package you will see a error similar this one:

```
remote: Could not find a required file.
remote:   Name: `index.html`
remote:   Searched in: /tmp/build_a2875fc163b209225122d68916f1d4df/public
remote:
remote: npm ERR! Linux 3.13.0-105-generic
remote: npm ERR! argv "/tmp/build_a2875fc163b209225122d68916f1d4df/.heroku/node/bin/node" "/tmp/build_a2875fc163b209225122d68916f1d4df/.heroku/node/bin/npm" "run" "build"
```

In this case, ensure that the file is there with the proper lettercase and that’s not ignored on your local `.gitignore` or `~/.gitignore_global`.

### [Netlify](https://www.netlify.com/)

**To do a manual deploy to Netlify’s CDN:**

```sh
npm install netlify-cli -g
netlify deploy
```

Choose `build` as the path to deploy.

**To setup continuous delivery:**

With this setup Netlify will build and deploy when you push to git or open a pull request:

1. [Start a new netlify project](https://app.netlify.com/signup)
2. Pick your Git hosting service and select your repository
3. Set `yarn build` as the build command and `build` as the publish directory
4. Click `Deploy site`

**Support for client-side routing:**

To support `pushState`, make sure to create a `public/_redirects` file with the following rewrite rules:

```
/*  /index.html  200
```

When you build the project, Create React App will place the `public` folder contents into the build output.

### [Now](https://zeit.co/now)

Now offers a zero-configuration single-command deployment. You can use `now` to deploy your app for free.

1. Install the `now` command-line tool either via the recommended [desktop tool](https://zeit.co/download) or via node with `npm install -g now`.

2. Build your app by running `npm run build`.

3. Move into the build directory by running `cd build`.

4. Run `now --name your-project-name` from within the build directory. You will see a **now.sh** URL in your output like this:

    ```
    > Ready! https://your-project-name-tpspyhtdtk.now.sh (copied to clipboard)
    ```

    Paste that URL into your browser when the build is complete, and you will see your deployed app.

Details are available in [this article.](https://zeit.co/blog/unlimited-static)

### [S3](https://aws.amazon.com/s3) and [CloudFront](https://aws.amazon.com/cloudfront/)

See this [blog post](https://medium.com/@omgwtfmarc/deploying-create-react-app-to-s3-or-cloudfront-48dae4ce0af) on how to deploy your React app to Amazon Web Services S3 and CloudFront.

### [Surge](https://surge.sh/)

Install the Surge CLI if you haven’t already by running `npm install -g surge`. Run the `surge` command and log in you or create a new account.

When asked about the project path, make sure to specify the `build` folder, for example:

```sh
       project path: /path/to/project/build
```

Note that in order to support routers that use HTML5 `pushState` API, you may want to rename the `index.html` in your build folder to `200.html` before deploying to Surge. This [ensures that every URL falls back to that file](https://surge.sh/help/adding-a-200-page-for-client-side-routing).

## Advanced Configuration

You can adjust various development and production settings by setting environment variables in your shell or with [.env](#adding-development-environment-variables-in-env).

Variable | Development | Production | Usage
:--- | :---: | :---: | :---
BROWSER | :white_check_mark: | :x: | By default, Create React App will open the default system browser, favoring Chrome on macOS. Specify a [browser](https://github.com/sindresorhus/opn#app) to override this behavior, or set it to `none` to disable it completely. If you need to customize the way the browser is launched, you can specify a node script instead. Any arguments passed to `npm start` will also be passed to this script, and the url where your app is served will be the last argument. Your script's file name must have the `.js` extension.
HOST | :white_check_mark: | :x: | By default, the development web server binds to `localhost`. You may use this variable to specify a different host.
PORT | :white_check_mark: | :x: | By default, the development web server will attempt to listen on port 3000 or prompt you to attempt the next available port. You may use this variable to specify a different port.
HTTPS | :white_check_mark: | :x: | When set to `true`, Create React App will run the development server in `https` mode.
PUBLIC_URL | :x: | :white_check_mark: | Create React App assumes your application is hosted at the serving web server's root or a subpath as specified in [`package.json` (`homepage`)](#building-for-relative-paths). Normally, Create React App ignores the hostname. You may use this variable to force assets to be referenced verbatim to the url you provide (hostname included). This may be particularly useful when using a CDN to host your application.
CI | :large_orange_diamond: | :white_check_mark: | When set to `true`, Create React App treats warnings as failures in the build. It also makes the test runner non-watching. Most CIs set this flag by default.
REACT_EDITOR | :white_check_mark: | :x: | When an app crashes in development, you will see an error overlay with clickable stack trace. When you click on it, Create React App will try to determine the editor you are using based on currently running processes, and open the relevant source file. You can [send a pull request to detect your editor of choice](https://github.com/facebookincubator/create-react-app/issues/2636). Setting this environment variable overrides the automatic detection. If you do it, make sure your systems [PATH](https://en.wikipedia.org/wiki/PATH_(variable)) environment variable points to your editor’s bin folder. You can also set it to `none` to disable it completely.
CHOKIDAR_USEPOLLING | :white_check_mark: | :x: | When set to `true`, the watcher runs in polling mode, as necessary inside a VM. Use this option if `npm start` isn't detecting changes.
GENERATE_SOURCEMAP | :x: | :white_check_mark: | When set to `false`, source maps are not generated for a production build. This solves OOM issues on some smaller machines.
NODE_PATH | :white_check_mark: |  :white_check_mark: | Same as [`NODE_PATH` in Node.js](https://nodejs.org/api/modules.html#modules_loading_from_the_global_folders), but only relative folders are allowed. Can be handy for emulating a monorepo setup by setting `NODE_PATH=src`.

## Troubleshooting

### `npm start` doesn’t detect changes

When you save a file while `npm start` is running, the browser should refresh with the updated code.<br>
If this doesn’t happen, try one of the following workarounds:

* If your project is in a Dropbox folder, try moving it out.
* If the watcher doesn’t see a file called `index.js` and you’re referencing it by the folder name, you [need to restart the watcher](https://github.com/facebookincubator/create-react-app/issues/1164) due to a Webpack bug.
* Some editors like Vim and IntelliJ have a “safe write” feature that currently breaks the watcher. You will need to disable it. Follow the instructions in [“Adjusting Your Text Editor”](https://webpack.js.org/guides/development/#adjusting-your-text-editor).
* If your project path contains parentheses, try moving the project to a path without them. This is caused by a [Webpack watcher bug](https://github.com/webpack/watchpack/issues/42).
* On Linux and macOS, you might need to [tweak system settings](https://github.com/webpack/docs/wiki/troubleshooting#not-enough-watchers) to allow more watchers.
* If the project runs inside a virtual machine such as (a Vagrant provisioned) VirtualBox, create an `.env` file in your project directory if it doesn’t exist, and add `CHOKIDAR_USEPOLLING=true` to it. This ensures that the next time you run `npm start`, the watcher uses the polling mode, as necessary inside a VM.

If none of these solutions help please leave a comment [in this thread](https://github.com/facebookincubator/create-react-app/issues/659).

### `npm test` hangs on macOS Sierra

If you run `npm test` and the console gets stuck after printing `react-scripts test --env=jsdom` to the console there might be a problem with your [Watchman](https://facebook.github.io/watchman/) installation as described in [facebookincubator/create-react-app#713](https://github.com/facebookincubator/create-react-app/issues/713).

We recommend deleting `node_modules` in your project and running `npm install` (or `yarn` if you use it) first. If it doesn't help, you can try one of the numerous workarounds mentioned in these issues:

* [facebook/jest#1767](https://github.com/facebook/jest/issues/1767)
* [facebook/watchman#358](https://github.com/facebook/watchman/issues/358)
* [ember-cli/ember-cli#6259](https://github.com/ember-cli/ember-cli/issues/6259)

It is reported that installing Watchman 4.7.0 or newer fixes the issue. If you use [Homebrew](http://brew.sh/), you can run these commands to update it:

```
watchman shutdown-server
brew update
brew reinstall watchman
```

You can find [other installation methods](https://facebook.github.io/watchman/docs/install.html#build-install) on the Watchman documentation page.

If this still doesn’t help, try running `launchctl unload -F ~/Library/LaunchAgents/com.github.facebook.watchman.plist`.

There are also reports that *uninstalling* Watchman fixes the issue. So if nothing else helps, remove it from your system and try again.

### `npm run build` exits too early

It is reported that `npm run build` can fail on machines with limited memory and no swap space, which is common in cloud environments. Even with small projects this command can increase RAM usage in your system by hundreds of megabytes, so if you have less than 1 GB of available memory your build is likely to fail with the following message:

>  The build failed because the process exited too early. This probably means the system ran out of memory or someone called `kill -9` on the process.

If you are completely sure that you didn't terminate the process, consider [adding some swap space](https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-ubuntu-14-04) to the machine you’re building on, or build the project locally.

### `npm run build` fails on Heroku

This may be a problem with case sensitive filenames.
Please refer to [this section](#resolving-heroku-deployment-errors).

### Moment.js locales are missing

If you use a [Moment.js](https://momentjs.com/), you might notice that only the English locale is available by default. This is because the locale files are large, and you probably only need a subset of [all the locales provided by Moment.js](https://momentjs.com/#multiple-locale-support).

To add a specific Moment.js locale to your bundle, you need to import it explicitly.<br>
For example:

```js
import moment from 'moment';
import 'moment/locale/fr';
```

If import multiple locales this way, you can later switch between them by calling `moment.locale()` with the locale name:

```js
import moment from 'moment';
import 'moment/locale/fr';
import 'moment/locale/es';

// ...

moment.locale('fr');
```

This will only work for locales that have been explicitly imported before.

### `npm run build` fails to minify

Some third-party packages don't compile their code to ES5 before publishing to npm. This often causes problems in the ecosystem because neither browsers (except for most modern versions) nor some tools currently support all ES6 features. We recommend to publish code on npm as ES5 at least for a few more years.

<br>
To resolve this:

1. Open an issue on the dependency's issue tracker and ask that the package be published pre-compiled.
  * Note: Create React App can consume both CommonJS and ES modules. For Node.js compatibility, it is recommended that the main entry point is CommonJS. However, they can optionally provide an ES module entry point with the `module` field in `package.json`. Note that **even if a library provides an ES Modules version, it should still precompile other ES6 features to ES5 if it intends to support older browsers**.

2. Fork the package and publish a corrected version yourself. 

3. If the dependency is small enough, copy it to your `src/` folder and treat it as application code.

In the future, we might start automatically compiling incompatible third-party modules, but it is not currently supported. This approach would also slow down the production builds.

## Alternatives to Ejecting

[Ejecting](#npm-run-eject) lets you customize anything, but from that point on you have to maintain the configuration and scripts yourself. This can be daunting if you have many similar projects. In such cases instead of ejecting we recommend to *fork* `react-scripts` and any other packages you need. [This article](https://auth0.com/blog/how-to-configure-create-react-app/) dives into how to do it in depth. You can find more discussion in [this issue](https://github.com/facebookincubator/create-react-app/issues/682).

## Something Missing?

If you have ideas for more “How To” recipes that should be on this page, [let us know](https://github.com/facebookincubator/create-react-app/issues) or [contribute some!](https://github.com/facebookincubator/create-react-app/edit/master/packages/react-scripts/template/README.md)
