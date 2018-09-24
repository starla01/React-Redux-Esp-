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
