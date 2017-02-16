Repositorio original: [ryanmcdermott/clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript)

# clean-code-javascript 

## Contenido
  1. [Introducción](#introducción)
  2. [Variables](#variables)
  3. [Funciones](#funciones)
  4. [Objetos y estructuras de data](#objectos-y-estructuras-de-data)
  5. [Clases](#clases)
  6. [SOLID](#solid)
  7. [Pruebas](#pruebas)
  8. [Concurrencia](#concucrencia)
  9. [Resolver los errores](#resolver-los-errores)
  10. [Formatear](#formatear)
  11. [Comentarios](#commentarios)

## Introducción

![Imagen gracioso de la estimacion de la calidad de software como una cifra 
de cuantos expletivos que uno puede gritar al leer programas](http://www.osnews.com/images/comics/wtfm.jpg)

Los principios de la ingeniería de software, del libro de Robert C. Martin [*Clean Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882), adaptado para JavaScript. Esta no es una guía de estilo, sino, es una guia para crear software que sea reutilizable, comprensible y que se pueda mejorar con el tiempo.

No hay que seguir tan estrictamente todos los principios en este libro, y vale la pena mencionar que hacia muchos habrá controversia en cuanto al consentimiento. Estas son estimaciones y nada mas, pero son estimaciones hechas después de muchos años de experiencia colectiva de los autores de *Clean Code*.

Nuestra obra de ingeniería de software lleva poco más que 50 años como negocio, y aún estamos aprendiendo. Cuando la arquitectura de software llegue a ser tan vieja como la arquitectura si misma, quizás tendremos reglas más estrictas a las que tenemos que adherir. Hasta entonces, dejemos que estas estimaciones sirvan como ejemplo para medir la calidad de nuestro código en JavaScript que tu y tu equipo producen.

Una cosa más: el saber de que este libro no te hará inmediatamente un ingeniero mucho más mejor es importante, y tampoco lo de trabajar con estas herramientas durante muchos años no garantiza que usted no se equivoque nunca. Cada parte del código empieza como primer draft, como se moldea el clay hasta llegar a la forma final. Por fin, chisel amos las imperfecciones cuando lo repasamos con nuestros compañeros de trabajo. No seas tan duro contigo mismo para las etapas iniciales que necesitan mejoramiento. Trabaja más duro a mejorar el programa!

## **Variables**
### Utiliza nombres significativos y pronunciables para los variables

**Mal hecho:**
```javascript
const yyyymmdstr = moment().format('YYYY/MM/DD');
```

**Bien hecho:**
```javascript
const fechaActual = moment().format('YYYY/MM/DD');
```
**[⬆ vuelve hasta arriba](#contenido)**

### Utiliza el vocabulario igual para los variables del mismo tipo 

**Mal hecho:**
```javascript
conseguirInfoUsuario();
conseguirDataDelCliente();
conseguirRecordDelCliente();
```

**Bien hecho:**
```javascript
conseguirUsuario();
```
**[⬆ vuelve hasta arriba](#contenido)**

### Utiliza nombres buscables 

Nosotros leemos mucho mas codigo que jamás escribiremos. Es importante que el código que escribimos sea legible y buscable. Cuando faltamos nombrar a los variables de manera buscable y legible, acabamos confundiendo a nuestros lectores. Hecha un vistazo a las herramientas para ayudarte: [buddy.js](https://github.com/danielstjules/buddy.js) and
[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md)

**Mal hecho:**
```javascript
// Para que rayos sirve 86400000? 
setTimeout(hastaLaInfinidadYMasAlla, 86400000);

```

**Bien hecho:**
```javascript
// Declaralos como variables globales de 'const'.
const MILISEGUNDOS_EN_UN_DIA = 8640000;

setTimeout(hastaLaInfinidadYMasAlla, MILISEGUNDOS_EN_UN_DIA);

```
**[⬆ vuelve hasta arriba](#contenido)**

### Utiliza variables explicativos
**Mal hecho:**
```javascript
const address = 'One Infinite Loop, Cupertino 95014';
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(address.match(cityZipCodeRegex)[1], address.match(cityZipCodeRegex)[2]);
```

**Bien hecho:**
```javascript
const address = 'One Infinite Loop, Cupertino 95014';
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```
**[⬆ vuelve hasta arriba](#contenido)**

### Evitar el mapeo mental 
El Explicito es mejor que el implicito.

**Mal hecho:**
```javascript
const ubicaciones = ['Austin', 'New York', 'San Francisco'];
ubicaciones.forEach((u) => {
  hazUnaCosa();
  hasMasCosas()
  // ...
  // ...
  // ...
  // Espera, para que existe la 'u'?
  ejecuta(u);
});
```

**Bien hecho:**
```javascript
const ubicaciones = ['Austin', 'New York', 'San Francisco'];
ubicaciones.forEach((ubicacion) => {
  hazUnaCosa();
  hasMasCosas()
  // ...
  // ...
  // ...
  ejecuta(ubicacion);
});
```
**[⬆ vuelve hasta arriba](#contenido)**

### No incluyas contexto innecesario  
Si tu clase / nombre tu objeto te dice algo, no lo repitas 
en el nombre de variable tambien. 

**Mal hecho:**
```javascript
const Car = {
  carMake: 'Honda',
  carModel: 'Accord',
  carColor: 'Blue'
};

function paintCar(car) {
  car.carColor = 'Red';
}
```

**Bien hecho:**
```javascript
const Car = {
  make: 'Honda',
  model: 'Accord',
  color: 'Blue'
};

function paintCar(car) {
  car.color = 'Red';
}
```
**[⬆ vuelve hasta arriba](#contenido)**

### Utiliza argumentos originales en vez utilizar condicionales 
Los argumentos defaults muchas veces son mas organizados que utilizar los condicionales.
Se conciente que si tu los usas, tu funcion solo tendra valores para los argumentos de 'undefined'.
Los demas valores de 'falso' como `''`, `""`, `false`, `null`, `0`, y
`NaN`, no se replaceran con un valor default.

**Mal hecho:**
```javascript
function createMicrobrewery(name) {
  const breweryName = name || 'Hipster Brew Co.';
  // ...
}

```

**Bien hecho:**
```javascript
function createMicrobrewery(breweryName = 'Hipster Brew Co.') {
  // ...
}

```
**[⬆ vuelve hasta arriba](#contenido)**

## **Funciones**
### Argumentos de funciones (2 o menos idealmente)

Limitar la cantidad de parámetros de tus funciones es increíblemente importante ya que hace que tus pruebas del código sean más fáciles. Al pasar los 3 argumentos, llegarás a un escenario de una explosión combinatoria en que hay que comprobar con pruebas muchos casos únicos con un argumento separado.

Uno o dos argumentos es la situación ideal, y más que eso uno debe evitar si es posible. Todo lo que se puede consolidar se debe consolidar. Normalmente, si tienes más que dos argumentos, tu función sirve para hacer demasiado. En otros casos, es mejor refactorizar y hacerlo un objeto para encapsular las funciones extras.

Ya que JavaScript te deja crear objetos cuando quieras sin incorporar la arquitectura de 'clases', se puede usar un objeto si necesitas muchos argumentos.

Para hacerlo más obvio cuáles argumentos espera la función, se puede usar el syntaxe de ES2015/ES6: 'destructurar'. Este syntaxe tiene varias ventajas:

1. Cuando alguien se fija en el firme de la función, es inmediatamente claro cuáles argumentos se usan.
2. Destructurar también copia los valores específicos y primitivos del objeto argumento que se le pasa a la función. Esto puede evitar los efectos extras. Ojo: objetos y arrays que se destructuran del objeto argumento NO se copian.
3. Los 'linters' te pueden avisar cuales argumentos / propiedades no se usan, lo cual sería imposible sin deestructurar.


**Mal hecho:**
```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}
```

**Bien hecho:**
```javascript
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```
**[⬆ vuelve hasta arriba](#contenido)**

### Las funciones deben existir para hacer una sola cosa 
Esta regla por mucho es la más importante en la ingeniería de software. Cuando las funciones sirven para hacer más que una sola cosa, se dificultan las pruebas, la composición y el entender. Cuando puedes isolar una función hasta tener solo una acción, se pueden mejorar más fácil y tu codigo llegue a ser mucho más limpio. Si solamente entiendes una cosa de esta guia, entiende esta reglas y estarás adelantado de muchos desarrolladores.

**Mal hecho:**
```javascript
function emailClients(clients) {
  clients.forEach((client) => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**Bien hecho:**
```javascript
function emailClients(clients) {
  clients
    .filter(isClientActive)
    .forEach(email);
}

function isClientActive(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```
**[⬆ vuelve hasta arriba](#contenido)**

### Los nombres de las funciones deben explicar lo que hacen

**Mal hecho:**
```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();
// Es dificil entender del nombre lo que hace la funcion 
addToDate(date, 1);
```

**Bien hecho:**
```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```
**[⬆ vuelve hasta arriba](#contenido)**

### Las funciones deben tener solo un nivel de abstraccion 
Cuando tienes mas que un nivel de abstraccion tu funcion suele servir 
para hacer demasiado. Crear varias funciones mas pequenas se debe a mejor reutilizacion
y comprobacion mas facil. 

**Mal hecho:**
```javascript
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(' ');
  const tokens = [];
  REGEXES.forEach((REGEX) => {
    statements.forEach((statement) => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach((token) => {
    // lex...
  });

  ast.forEach((node) => {
    // parse...
  });
}
```

**Bien hecho:**
```javascript
function tokenize(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(' ');
  const tokens = [];
  REGEXES.forEach((REGEX) => {
    statements.forEach((statement) => {
      tokens.push( /* ... */ );
    });
  });

  return tokens;
}

function lexer(tokens) {
  const ast = [];
  tokens.forEach((token) => {
    ast.push( /* ... */ );
  });

  return ast;
}

function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const ast = lexer(tokens);
  ast.forEach((node) => {
    // parse...
  });
}
```
**[⬆ vuelve hasta arriba](#contenido)**

### Eliminar codigo duplicado 
Haz tanto como puedes para evitar codigo duplicado. El codigo duplicado es malo 
ya que significa que hay varios lugares donde hay que actualizar algo si un cambio 
es necesario en tu logico.

Imaginate que estas un restaurante y necesitas organizar tu inventorio: todos 
tus tomates, cebolla, pimientos y tal. Si tienes varias listas donde organizas 
el inventorio, cada lista se tendra que actualizar en cuanto se baja tu inventorio. 
En cambio, si logras tener una sola lista, solo se actualizara en un lugar a la hora 
de apuntar el inventorio. 

Muchas veces tienes codigo duplicado debido al tener dos o mas cosas semejantes.
Estos archivos comparten varias cosas, pero su diferencia te obliga separarlos para 
tener dos o mas funciones que hacen cosas muy similares. Remover el codigo duplicado 
significa que se puede hacer la misma cosa que un solo funcion/modulo/clase.

Obtener la abstraccion correcta es critica y por eso debes de adherir a los principios 
de SOLID que se explican en las seccion de *Clases*. Las abstraciones males pueden ser 
aun peores que el codigo duplicado, asi que ten cuidado! Es decir, si puedes hacer una 
buena abstraccion, hazla! No te repitas, si no te daras cuenta de que andas actualizando 
mucho codigo en varios lugares a la hora de implementar un cambio.

**Mal hecho:**
```javascript
function showDeveloperList(developers) {
  developers.forEach((developer) => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();
    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers) {
  managers.forEach((manager) => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**Bien hecho:**
```javascript
function showEmployeeList(employees) {
  employees.forEach((employee) => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    let portfolio = employee.getGithubLink();

    if (employee.type === 'manager') {
      portfolio = employee.getMBAProjects();
    }

    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```
**[⬆ vuelve hasta arriba](#contenido)**

### Crear objectos defaults con Object.assign

**Mal hecho:**
```javascript
const menuConfig = {
  title: null,
  body: 'Bar',
  buttonText: null,
  cancellable: true
};

function createMenu(config) {
  config.title = config.title || 'Foo';
  config.body = config.body || 'Bar';
  config.buttonText = config.buttonText || 'Baz';
  config.cancellable = config.cancellable === undefined ? config.cancellable : true;
}

createMenu(menuConfig);
```

**Bien hecho:**
```javascript
const menuConfig = {
  title: 'Order',
  // El usuario no tenia la clave 'body'
  buttonText: 'Send',
  cancellable: true
};

function createMenu(config) {
  config = Object.assign({
    title: 'Foo',
    body: 'Bar',
    buttonText: 'Baz',
    cancellable: true
  }, config);
  // el variable 'config' ahora iguala: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```
**[⬆ vuelve hasta arriba](#contenido)**

### No utilices 'marcadores' como parametros de las funciones
Los marcadores existen para decirle a tu usuario que esta function hace mas que una sola cosa. 
Como se ha mencionado antes las funciones deben hacer una sola cosa. 
Divide tus funciones en varias funciones mas pequenas si se adhieren a distintos metodos basados en un booleado. 

**Mal hecho:**
```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Bien hecho:**
```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```
**[⬆ vuelve hasta arriba](#contenido)**

### Evitar que las funciones produzcan efectos extras (parte 1)
Una funcion produce un efecto extra si hace cualquier cosa mas que solo tomar 
un valor y volver(lo/los). Un efecto extra podria ser escribir a un archivo, 
modificar un variable global, o accidentalmente enviar todo tu dinero a un 
desconfiado. 

Bueno, las funciones necesitan tener efectos extras a menudo. Como el ejemplo anterior, 
puede que sea necesario escribir hasta un archivo. En ese caso, hay que centralizar en el 'por que' 
de lo que estas haciendo. No tengas varias funciones y clases que escriben hasta un archivo particular. 
En cambio, crea un 'servicio' que se dedica a eso: uno y solo un servicio.

El punto clave aqui es evitar las equivocaciones comunes como compartir 'estado' entre 
objeto sin ninguna estructura, utilizar tipos de data mutables que se pueden escribir hasta 
lo que sea, y no centralizar donde se ocurren los efectos extras. Si puedes conseguir esto, 
seras mas feliz que la mayoria de los demas programadores.

**Mal hecho:**
```javascript
// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
let name = 'Ryan McDermott';

function splitIntoFirstAndLastName() {
  name = name.split(' ');
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**Bien hecho:**
```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(' ');
}

const name = 'Ryan McDermott';
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```
**[⬆ vuelve hasta arriba](#contenido)**

### Evitar los efectos extra(parte 2)
En JavaScript, los primitivos se pasan por valores y los objetos/arrays se
pasan por referencia. En el caso de los objetos y los array, si tu funcion 
hace un cambio en la shopping cart array, por ejemplo, con agregar una cosa a 
la hora de comprar, resulta que todas las demas funciones que utilizan este array 
estaran afectadas. Esto puede ser bueno o malo. Imaginemos una situacion mala:

El usuario le da clik a "Comprarar", un boton que invoca la funcion de "comprar".
Esta funcion hace una solicitud del red y envia el array de 'cart' hasta el servidor.
Debido a la conexion mala del red, la funcion sigue intentando invocarse para mandar 
la solicitud. Ahora, que pasa mientras tanto cuando el usuario le da click otra vez 
al boton en una cosa que no querian antes de que empezase la solicitud del red? 
Bueno, si pasa eso y comienza la solicitud del red, la funcion de 'comprar' 
mandara sin querer la cosa que estaba agregada accidentalmente ya que tiene una 
referencia al array de 'shopping cart' que la funcion 'addItemToCart' modifico 
con agregar una cosa no deseada.

Una buena solucion seria que la funcion 'addItemToCart' siempre copiara la 'carta',
editarla, y devolversela a la copia. Esto asegura que ninguna otra funcion relacionada 
se afectara por estos cambios.

Dos cosas para mencionar con esta solucion:
  1. Puede que existan escenarios donde de verdad quieres modificar el objeto de input, 
  pero cuando adoptas esta practicar de programar, te daras cuentas de que estos casos son 
  bastante unicos.
  2. Copiar objetos grandes pueden ser muy caros en cuanto a la velocidad y calidad de tu programa.
  Fortunadamente, no hay mucho problema con esto ya que existen muchas [recursos](https://facebook.github.io/immutable-js/)
  que nos dejan lograr el copiar de objetos y arrays sin perder actuacion.

**Mal hecho:**
```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**Bien hecho:**
```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date : Date.now() }];
};
```

**[⬆ vuelve hasta arriba](#contenido)**

### No escribas hasta las funciones globales
Polluting globals is a bad practice in JavaScript because you could clash with another
library and the user of your API would be none-the-wiser until they get an
exception in production. Let's think about an example: what if you wanted to
extend JavaScript's native Array method to have a `diff` method that could
show the difference between two arrays? You could write your new function
to the `Array.prototype`, but it could clash with another library that tried
to do the same thing. What if that other library was just using `diff` to find
the difference between the first and last elements of an array? This is why it
would be much better to just use ES2015/ES6 classes and simply extend the `Array` global.



**Mal hecho:**
```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**Bien hecho:**
```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```
**[⬆ vuelve hasta arriba](#contenido)**

### Favorece a la programacion funcional en vez de la programaccion imperativa
JavaScript no es un idioma funcional tal como es Haskell, pero tiene su propio 
sabor funcional. Los idiomas funcionales son mas limipos y faciles de comprobar.
Favorece este estilo de programar cuando puedes.

**Mal hecho:**
```javascript
const programmerOutput = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  }, {
    name: 'Suzie Q',
    linesOfCode: 1500
  }, {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  }, {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode;
}
```

**Bien hecho:**
```javascript
const programmerOutput = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  }, {
    name: 'Suzie Q',
    linesOfCode: 1500
  }, {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  }, {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

const INITIAL_VALUE = 0;

const totalOutput = programmerOutput
  .map((programmer) => programmer.linesOfCode)
  .reduce((acc, linesOfCode) => acc + linesOfCode, INITIAL_VALUE);
```
**[⬆ vuelve hasta arriba](#contenido)**

### Encapsular condicionales

**Mal hecho:**
```javascript
if (fsm.state === 'fetching' && isEmpty(listNode)) {
  // ...
}
```

**Bien hecho:**
```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === 'fetching' && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```
**[⬆ vuelve hasta arriba](#contenido)**

### Evitar condicionales negativas

**Mal hecho:**
```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**Bien hecho:**
```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```
**[⬆ vuelve hasta arriba](#contenido)**

### Evitar condicionales
This seems like an impossible task. Upon first hearing this, most people say,
"how am I supposed to do anything without an `if` statement?" The answer is that
you can use polymorphism to achieve the same task in many cases. The second
question is usually, "well that's great but why would I want to do that?" The
answer is a previous clean code concept we learned: a function should only do
one thing. When you have classes and functions that have `if` statements, you
are telling your user that your function does more than one thing. Remember,
just do one thing.

Este parece ser un reto imposible. Al escuchar esto por primera vez, 
la mayoria de la gente dira: "como se supone que programo sin un 'if'?"
Bueno, la respuesta es que puedes utilizar polymorfismo para lograr los 
mismos retos en muchos escenarios. La segunda pregunta 

**Mal hecho:**
```javascript
class Airplane {
  // ...
  getCruisingAltitude() {
    switch (this.type) {
      case '777':
        return this.getMaxAltitude() - this.getPassengerCount();
      case 'Air Force One':
        return this.getMaxAltitude();
      case 'Cessna':
        return this.getMaxAltitude() - this.getFuelExpenditure();
    }
  }
}
```

**Bien hecho:**
```javascript
class Airplane {
  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```
**[⬆ vuelve hasta arriba](#contenido)**

### Evitar la comprobacion de tipos (parte 1)
JavaScript is untyped, which means your functions can take any type of argument.
Sometimes you are bitten by this freedom and it becomes tempting to do
type-checking in your functions. There are many ways to avoid having to do this.
The first thing to consider is consistent APIs.

**Mal hecho:**
```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location('texas'));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location('texas'));
  }
}
```

**Bien hecho:**
```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location('texas'));
}
```
**[⬆ vuelve hasta arriba](#contenido)**

### Evitar la comprobacion de tipos (parte 2)
If you are working with basic primitive values like strings, integers, and arrays,
and you can't use polymorphism but you still feel the need to type-check,
you should consider using TypeScript. It is an excellent alternative to normal
JavaScript, as it provides you with static typing on top of standard JavaScript
syntax. The problem with manually type-checking normal JavaScript is that
doing it well requires so much extra verbiage that the faux "type-safety" you get
doesn't make up for the lost readability. Keep your JavaScript clean, write
good tests, and have good code reviews. Otherwise, do all of that but with
TypeScript (which, like I said, is a great alternative!).

**Mal hecho:**
```javascript
function combine(val1, val2) {
  if (typeof val1 === 'number' && typeof val2 === 'number' ||
      typeof val1 === 'string' && typeof val2 === 'string') {
    return val1 + val2;
  }

  throw new Error('Must be of type String or Number');
}
```

**Bien hecho:**
```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```
**[⬆ vuelve hasta arriba](#contenido)**

### Don't over-optimize
Modern browsers do a lot of optimization under-the-hood at runtime. A lot of
times, if you are optimizing then you are just wasting your time. [There are good
resources](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)
for seeing where optimization is lacking. Target those in the meantime, until
they are fixed if they can be.

**Mal hecho:**
```javascript

// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Bien hecho:**
```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```
**[⬆ vuelve hasta arriba](#contenido)**

### Eliminar el codigo muerto
El codigo muerto es tan maligante como el codigo duplicado. No hay razon 
para guardarlo. Si no se usa, eliminalo! Aun estara en tu historia del control version
si de verdad lo necesitas.

**Mal hecho:**
```javascript
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');

```

**Bien hecho:**
```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```
**[⬆ vuelve hasta arriba](#contenido)**

## **Objetos y estructuras de data**
### Use getters and setters
Using getters and setters to access data on objects could be better than simply
looking for a property on an object. "Why?" you might ask. Well, here's an
unorganized list of reasons why:

* When you want to do more beyond getting an object property, you don't have
to look up and change every accessor in your codebase.
* Makes adding validation simple when doing a `set`.
* Encapsulates the internal representation.
* Easy to add logging and error handling when getting and setting.
* You can lazy load your object's properties, let's say getting it from a
server.


**Mal hecho:**
```javascript
function makeBankAccount() {
  // ...

  return {
    balance: 0,
    // ...
  };
}

const account = makeBankAccount();
account.balance = 100;
```

**Bien hecho:**
```javascript
function makeBankAccount() {
  // this one is private
  let balance = 0;

  // a "getter", made public via the returned object below
  function getBalance() {
    return balance;
  }

  // a "setter", made public via the returned object below
  function setBalance(amount) {
    // ... validate before updating the balance
    balance = amount;
  }

  return {
    // ...
    getBalance,
    setBalance,
  };
}

const account = makeBankAccount();
account.setBalance(100);
```
**[⬆ vuelve hasta arriba](#contenido)**


### Make objects have private members
This can be accomplished through closures (for ES5 and below).

**Mal hecho:**
```javascript

const Employee = function(name) {
  this.name = name;
};

Employee.prototype.getName = function getName() {
  return this.name;
};

const employee = new Employee('John Doe');
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: undefined
```

**Bien hecho:**
```javascript
function makeEmployee(name) {
  return {
    getName() {
      return name;
    },
  };
}

const employee = makeEmployee('John Doe');
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
```
**[⬆ vuelve hasta arriba](#contenido)**


## **Clases**
### Prefiere ES2015/ES6 clases en vez de funciones normales de ES5
It's very difficult to get readable class inheritance, construction, and method
definitions for classical ES5 classes. If you need inheritance (and be aware
that you might not), then prefer ES2015/ES6 classes. However, prefer small functions over
classes until you find yourself needing larger and more complex objects.

**Mal hecho:**
```javascript
const Animal = function(age) {
  if (!(this instanceof Animal)) {
    throw new Error('Instantiate Animal with `new`');
  }

  this.age = age;
};

Animal.prototype.move = function move() {};

const Mammal = function(age, furColor) {
  if (!(this instanceof Mammal)) {
    throw new Error('Instantiate Mammal with `new`');
  }

  Animal.call(this, age);
  this.furColor = furColor;
};

Mammal.prototype = Object.create(Animal.prototype);
Mammal.prototype.constructor = Mammal;
Mammal.prototype.liveBirth = function liveBirth() {};

const Human = function(age, furColor, languageSpoken) {
  if (!(this instanceof Human)) {
    throw new Error('Instantiate Human with `new`');
  }

  Mammal.call(this, age, furColor);
  this.languageSpoken = languageSpoken;
};

Human.prototype = Object.create(Mammal.prototype);
Human.prototype.constructor = Human;
Human.prototype.speak = function speak() {};
```

**Bien hecho:**
```javascript
class Animal {
  constructor(age) {
    this.age = age;
  }

  move() { /* ... */ }
}

class Mammal extends Animal {
  constructor(age, furColor) {
    super(age);
    this.furColor = furColor;
  }

  liveBirth() { /* ... */ }
}

class Human extends Mammal {
  constructor(age, furColor, languageSpoken) {
    super(age, furColor);
    this.languageSpoken = languageSpoken;
  }

  speak() { /* ... */ }
}
```
**[⬆ vuelve hasta arriba](#contenido)**


### Utiliza la agregaccion de metodos
This pattern is very useful in JavaScript and you see it in many libraries such
as jQuery and Lodash. It allows your code to be expressive, and less verbose.
For that reason, I say, use method chaining and take a look at how clean your code
will be. In your class functions, simply return `this` at the end of every function,
and you can chain further class methods onto it.

**Mal hecho:**
```javascript
class Car {
  constructor() {
    this.make = 'Honda';
    this.model = 'Accord';
    this.color = 'white';
  }

  setMake(make) {
    this.make = make;
  }

  setModel(model) {
    this.model = model;
  }

  setColor(color) {
    this.color = color;
  }

  save() {
    console.log(this.make, this.model, this.color);
  }
}

const car = new Car();
car.setColor('pink');
car.setMake('Ford');
car.setModel('F-150');
car.save();
```

**Bien hecho:**
```javascript
class Car {
  constructor() {
    this.make = 'Honda';
    this.model = 'Accord';
    this.color = 'white';
  }

  setMake(make) {
    this.make = make;
    // NOTE: Returning this for chaining
    return this;
  }

  setModel(model) {
    this.model = model;
    // NOTE: Returning this for chaining
    return this;
  }

  setColor(color) {
    this.color = color;
    // NOTE: Returning this for chaining
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // NOTE: Returning this for chaining
    return this;
  }
}

const car = new Car()
  .setColor('pink')
  .setMake('Ford')
  .setModel('F-150')
  .save();
```
**[⬆ vuelve hasta arriba](#contenido)**

### Prefer composition over inheritance
As stated famously in [*Design Patterns*](https://en.wikipedia.org/wiki/Design_Patterns) by the Gang of Four,
you should prefer composition over inheritance where you can. There are lots of
good reasons to use inheritance and lots of good reasons to use composition.
The main point for this maxim is that if your mind instinctively goes for
inheritance, try to think if composition could model your problem better. In some
cases it can.

You might be wondering then, "when should I use inheritance?" It
depends on your problem at hand, but this is a decent list of when inheritance
makes more sense than composition:

1. Your inheritance represents an "is-a" relationship and not a "has-a"
relationship (Human->Animal vs. User->UserDetails).
2. You can reuse code from the base classes (Humans can move like all animals).
3. You want to make global changes to derived classes by changing a base class.
(Change the caloric expenditure of all animals when they move).

**Mal hecho:**
```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// Bad because Employees "have" tax data. EmployeeTaxData is not a type of Employee
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**Bien hecho:**
```javascript
class EmployeeTaxData {
  constructor(ssn, salary) {
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}

class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  setTaxData(ssn, salary) {
    this.taxData = new EmployeeTaxData(ssn, salary);
  }
  // ...
}
```
**[⬆ vuelve hasta arriba](#contenido)**

## **SOLID**
### El principio unico de responsabilidad (SRP)
Como se menciona en Clean Code, "Nunca debe exister mas que una sola razon para cambiar 
una clase". Vale la pena decir que es normal querer llenar una 'clase' con muchas funciones, 
igual que cuando solo te permiten llevar una maleta en el vuelo. El problema existe en que 
tu 'clase' no estara cohesivo conceptualmente y le dara muchas razones para cambiarse.
Minimizar la cantidad de veces que necesitas cambiar una clase es importante. Es importante 
ya que con demasiada funcionalidad viene dificultad de modificarlo y entender como afectara 
a otros modulos dependientes en tu programa.

**Mal hecho:**
```javascript
class UserSettings {
  constructor(user) {
    this.user = user;
  }

  changeSettings(settings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

**Bien hecho:**
```javascript
class UserAuth {
  constructor(user) {
    this.user = user;
  }

  verifyCredentials() {
    // ...
  }
}


class UserSettings {
  constructor(user) {
    this.user = user;
    this.auth = new UserAuth(user);
  }

  changeSettings(settings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```
**[⬆ vuelve hasta arriba](#contenido)**

### Principio de abierto/cerrado (OCP)
Como dijo Bertrand Meyer, "las entidades de software (clases, modulos, funciones, etc.)
deben abrirse para extension, pero cerrarse para modificacion. Que significa eso? 
Bueno, este principio basicamente nos dice que debes permitir que tus usarios 
introduzcan nuevas funcionalidades sin cambiar el codigo existente.

**Mal hecho:**
```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = 'ajaxAdapter';
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = 'nodeAdapter';
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    if (this.adapter.name === 'ajaxAdapter') {
      return makeAjaxCall(url).then((response) => {
        // transform response and return
      });
    } else if (this.adapter.name === 'httpNodeAdapter') {
      return makeHttpCall(url).then((response) => {
        // transform response and return
      });
    }
  }
}

function makeAjaxCall(url) {
  // request and return promise
}

function makeHttpCall(url) {
  // request and return promise
}
```

**Bien hecho:**
```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = 'ajaxAdapter';
  }

  request(url) {
    // request and return promise
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = 'nodeAdapter';
  }

  request(url) {
    // request and return promise
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    return this.adapter.request(url).then((response) => {
      // transform response and return
    });
  }
}
```
**[⬆ vuelve hasta arriba](#contenido)**

### Liskov Substitution Principle (LSP)
This is a scary term for a very simple concept. It's formally defined as "If S
is a subtype of T, then objects of type T may be replaced with objects of type S
(i.e., objects of type S may substitute objects of type T) without altering any
of the desirable properties of that program (correctness, task performed,
etc.)." That's an even scarier definition.

The best explanation for this is if you have a parent class and a child class,
then the base class and child class can be used interchangeably without getting
incorrect results. This might still be confusing, so let's take a look at the
classic Square-Rectangle example. Mathematically, a square is a rectangle, but
if you model it using the "is-a" relationship via inheritance, you quickly
get into trouble.

**Mal hecho:**
```javascript
class Rectangle {
  constructor() {
    this.width = 0;
    this.height = 0;
  }

  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }

  setWidth(width) {
    this.width = width;
  }

  setHeight(height) {
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width) {
    this.width = width;
    this.height = width;
  }

  setHeight(height) {
    this.width = height;
    this.height = height;
  }
}

function renderLargeRectangles(rectangles) {
  rectangles.forEach((rectangle) => {
    rectangle.setWidth(4);
    rectangle.setHeight(5);
    const area = rectangle.getArea(); // BAD: Returns 25 for Square. Should be 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Bien hecho:**
```javascript
class Shape {
  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(length) {
    super();
    this.length = length;
  }

  getArea() {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes) {
  shapes.forEach((shape) => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```
**[⬆ vuelve hasta arriba](#contenido)**

### Interface Segregation Principle (ISP)
JavaScript doesn't have interfaces so this principle doesn't apply as strictly
as others. However, it's important and relevant even with JavaScript's lack of
type system.

ISP states that "Clients should not be forced to depend upon interfaces that
they do not use." Interfaces are implicit contracts in JavaScript because of
duck typing.

A good example to look at that demonstrates this principle in JavaScript is for
classes that require large settings objects. Not requiring clients to setup
huge amounts of options is beneficial, because most of the time they won't need
all of the settings. Making them optional helps prevent having a
"fat interface".

**Mal hecho:**
```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.animationModule.setup();
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName('body'),
  animationModule() {} // Most of the time, we won't need to animate when traversing.
  // ...
});

```

**Bien hecho:**
```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.options = settings.options;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.setupOptions();
  }

  setupOptions() {
    if (this.options.animationModule) {
      // ...
    }
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName('body'),
  options: {
    animationModule() {}
  }
});
```
**[⬆ vuelve hasta arriba](#contenido)**

### Dependency Inversion Principle (DIP)
This principle states two essential things:
1. High-level modules should not depend on low-level modules. Both should
depend on abstractions.
2. Abstractions should not depend upon details. Details should depend on
abstractions.

This can be hard to understand at first, but if you've worked with AngularJS,
you've seen an implementation of this principle in the form of Dependency
Injection (DI). While they are not identical concepts, DIP keeps high-level
modules from knowing the details of its low-level modules and setting them up.
It can accomplish this through DI. A huge benefit of this is that it reduces
the coupling between modules. Coupling is a very bad development pattern because
it makes your code hard to refactor.

As stated previously, JavaScript doesn't have interfaces so the abstractions
that are depended upon are implicit contracts. That is to say, the methods
and properties that an object/class exposes to another object/class. In the
example below, the implicit contract is that any Request module for an
`InventoryTracker` will have a `requestItems` method.

**Mal hecho:**
```javascript
class InventoryRequester {
  constructor() {
    this.REQ_METHODS = ['HTTP'];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryTracker {
  constructor(items) {
    this.items = items;

    // BAD: We have created a dependency on a specific request implementation.
    // We should just have requestItems depend on a request method: `request`
    this.requester = new InventoryRequester();
  }

  requestItems() {
    this.items.forEach((item) => {
      this.requester.requestItem(item);
    });
  }
}

const inventoryTracker = new InventoryTracker(['apples', 'bananas']);
inventoryTracker.requestItems();
```

**Bien hecho:**
```javascript
class InventoryTracker {
  constructor(items, requester) {
    this.items = items;
    this.requester = requester;
  }

  requestItems() {
    this.items.forEach((item) => {
      this.requester.requestItem(item);
    });
  }
}

class InventoryRequesterV1 {
  constructor() {
    this.REQ_METHODS = ['HTTP'];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryRequesterV2 {
  constructor() {
    this.REQ_METHODS = ['WS'];
  }

  requestItem(item) {
    // ...
  }
}

// By constructing our dependencies externally and injecting them, we can easily
// substitute our request module for a fancy new one that uses WebSockets.
const inventoryTracker = new InventoryTracker(['apples', 'bananas'], new InventoryRequesterV2());
inventoryTracker.requestItems();
```
**[⬆ vuelve hasta arriba](#contenido)**

## **Testing**
Testing is more important than shipping. If you have no tests or an
inadequate amount, then every time you ship code you won't be sure that you
didn't break anything. Deciding on what constitutes an adequate amount is up
to your team, but having 100% coverage (all statements and branches) is how
you achieve very high confidence and developer peace of mind. This means that
in addition to having a great testing framework, you also need to use a
[good coverage tool](http://gotwarlost.github.io/istanbul/).

There's no excuse to not write tests. There's [plenty of good JS test frameworks]
(http://jstherightway.org/#testing-tools), so find one that your team prefers.
When you find one that works for your team, then aim to always write tests
for every new feature/module you introduce. If your preferred method is
Test Driven Development (TDD), that is great, but the main point is to just
make sure you are reaching your coverage goals before launching any feature,
or refactoring an existing one.

### Single concept per test

**Mal hecho:**
```javascript
import assert from 'assert';

describe('MakeMomentJSGreatAgain', () => {
  it('handles date boundaries', () => {
    let date;

    date = new MakeMomentJSGreatAgain('1/1/2015');
    date.addDays(30);
    assert.equal('1/31/2015', date);

    date = new MakeMomentJSGreatAgain('2/1/2016');
    date.addDays(28);
    assert.equal('02/29/2016', date);

    date = new MakeMomentJSGreatAgain('2/1/2015');
    date.addDays(28);
    assert.equal('03/01/2015', date);
  });
});
```

**Bien hecho:**
```javascript
import assert from 'assert';

describe('MakeMomentJSGreatAgain', () => {
  it('handles 30-day months', () => {
    const date = new MakeMomentJSGreatAgain('1/1/2015');
    date.addDays(30);
    assert.equal('1/31/2015', date);
  });

  it('handles leap year', () => {
    const date = new MakeMomentJSGreatAgain('2/1/2016');
    date.addDays(28);
    assert.equal('02/29/2016', date);
  });

  it('handles non-leap year', () => {
    const date = new MakeMomentJSGreatAgain('2/1/2015');
    date.addDays(28);
    assert.equal('03/01/2015', date);
  });
});
```
**[⬆ vuelve hasta arriba](#contenido)**

## **Concurrency**
### Use Promises, not callbacks
Callbacks aren't clean, and they cause excessive amounts of nesting. With ES2015/ES6,
Promises are a built-in global type. Use them!

**Mal hecho:**
```javascript
import { get } from 'request';
import { writeFile } from 'fs';

get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', (requestErr, response) => {
  if (requestErr) {
    console.error(requestErr);
  } else {
    writeFile('article.html', response.body, (writeErr) => {
      if (writeErr) {
        console.error(writeErr);
      } else {
        console.log('File written');
      }
    });
  }
});

```

**Bien hecho:**
```javascript
import { get } from 'request';
import { writeFile } from 'fs';

get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
  .then((response) => {
    return writeFile('article.html', response);
  })
  .then(() => {
    console.log('File written');
  })
  .catch((err) => {
    console.error(err);
  });

```
**[⬆ vuelve hasta arriba](#contenido)**

### Async/Await are even cleaner than Promises
Promises are a very clean alternative to callbacks, but ES2017/ES8 brings async and await
which offer an even cleaner solution. All you need is a function that is prefixed
in an `async` keyword, and then you can write your logic imperatively without
a `then` chain of functions. Use this if you can take advantage of ES2017/ES8 features
today!

**Mal hecho:**
```javascript
import { get } from 'request-promise';
import { writeFile } from 'fs-promise';

get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
  .then((response) => {
    return writeFile('article.html', response);
  })
  .then(() => {
    console.log('File written');
  })
  .catch((err) => {
    console.error(err);
  });

```

**Bien hecho:**
```javascript
import { get } from 'request-promise';
import { writeFile } from 'fs-promise';

async function getCleanCodeArticle() {
  try {
    const response = await get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin');
    await writeFile('article.html', response);
    console.log('File written');
  } catch(err) {
    console.error(err);
  }
}
```
**[⬆ vuelve hasta arriba](#contenido)**


## **Resolver los errores**
Los errores emergidos son buenos! Significan que tu ejecucion ha tenido 
exito a la hora de idetnificar un error en tu programa y te avisa con 
detener la ejeucion del 'stack' actual, matando el proceso (en Node),
y notificarte en el 'console' con un reporte de 'stack trace'

### No les ignores a los errores pillados
Doing nothing with a caught error doesn't give you the ability to ever fix
or react to said error. Logging the error to the console (`console.log`)
isn't much better as often times it can get lost in a sea of things printed
to the console. If you wrap any bit of code in a `try/catch` it means you
think an error may occur there and therefore you should have a plan,
or create a code path, for when it occurs.

**Mal hecho:**
```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**Bien hecho:**
```javascript
try {
  functionThatMightThrow();
} catch (error) {
  // One option (more noisy than console.log):
  console.error(error);
  // Another option:
  notifyUserOfError(error);
  // Another option:
  reportErrorToService(error);
  // OR do all three!
}
```

### No le ignores a las promesas rechazadas 
Igual que no debes ignorar a los errores no pillados 
de un 'try/catch'

**Mal hecho:**
```javascript
getdata()
  .then((data) => {
    functionThatMightThrow(data);
  })
  .catch((error) => {
    console.log(error);
  });
```

**Bien hecho:**
```javascript
getdata()
  .then((data) => {
    functionThatMightThrow(data);
  })
  .catch((error) => {
    // One option (more noisy than console.log):
    console.error(error);
    // Another option:
    notifyUserOfError(error);
    // Another option:
    reportErrorToService(error);
    // OR do all three!
  });
```

**[⬆ vuelve hasta arriba](#contenido)**

## **Formatear**
Formatear es subjetivo. Como muchas reglas en esta guia, no hay que seguirlas
100%. El punto clave es: NO DISCUTAS sobre formatear. 
Hay muchas [herramientas](http://standardjs.com/rules.html) para facilitar esto.
Utiliza una! Te desperdicias de tu propio tiempo y el tiempo de los demas cuando 
discutes sobre formatear.

Para las cosas que no tienen relevancia al formateo automatico (indentacion, tabulos vs espacios,
quotaciones de doble vs single, etc.), busca aqui para aconsejarte.

### Use consistent capitalization
JavaScript is untyped, so capitalization tells you a lot about your variables,
functions, etc. These rules are subjective, so your team can choose whatever
they want. The point is, no matter what you all choose, just be consistent.

**Mal hecho:**
```javascript
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const Artists = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restore_database() {}

class animal {}
class Alpaca {}
```

**Bien hecho:**
```javascript
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const songs = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const artists = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restoreDatabase() {}

class Animal {}
class Alpaca {}
```
**[⬆ vuelve hasta arriba](#contenido)**


### Los llamadores y llamantes de las funciones deben ser cercas 
Si una funcion le llama a otra, mantiene esa funcionas verticalmente cerca en 
su archivo de fuente. Idealmente, mantiente el llamador justo encima del llamante.
Solemos leer codigo arriba-abajo, como un periodico. Debido a esto, haz que tus programas
sean legibles asi.

**Mal hecho:**
```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  lookupPeers() {
    return db.lookup(this.employee, 'peers');
  }

  lookupManager() {
    return db.lookup(this.employee, 'manager');
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(user);
review.perfReview();
```

**Bien hecho:**
```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  lookupPeers() {
    return db.lookup(this.employee, 'peers');
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  lookupManager() {
    return db.lookup(this.employee, 'manager');
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**[⬆ vuelve hasta arriba](#contenido)**

## **Commentarios**
### Solo comentar las cosas que tienen logico complexo.
Los comentarios existen para pedir perdon, pero no son un requisito. 
El codigo bueno *mas que nada* se documenta a si mismo.

**Mal hecho:**
```javascript
function hashIt(data) {
  // The hash
  let hash = 0;

  // Length of string
  const length = data.length;

  // Loop through every character in data
  for (let i = 0; i < length; i++) {
    // Get character code.
    const char = data.charCodeAt(i);
    // Make the hash
    hash = ((hash << 5) - hash) + char;
    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

**Bien hecho:**
```javascript

function hashIt(data) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = ((hash << 5) - hash) + char;
    // Convertir a un integero 32-bit
    hash &= hash;
  }
}

```
**[⬆ vuelve hasta arriba](#contenido)**

### No dejes codigo inutilizado en tus archivos 
El control de version existe para una razon. Deja el codigo viejo 
en tu historia (git).

**Mal hecho:**
```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Bien hecho:**
```javascript
doStuff();
```
**[⬆ vuelve hasta arriba](#contenido)**

### No escribas comentarios de jornada 
Ojo: utiliza el control de version (git)! No hay necesidad para el codigo no utilizado, 
comentado, y especialmente comentarios de jornada. En cambio, utiliza 'git log' para 
recuperar una historia de lo que has hecho.

**Mal hecho:**
```javascript
/**
 * 2016-12-20: Remover monads, no los entendia bien (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Removed type-checking (LI)
 * 2015-03-14: Added combine with type-checking (JR)
 */
function combine(a, b) {
  return a + b;
}
```

**Bien hecho:**
```javascript
function combine(a, b) {
  return a + b;
}
```
**[⬆ vuelve hasta arriba](#contenido)**

### Evitar los marcadores posicionales
Los marcadores posicionales suelen dificultar las cosas. Deja que las funciones,
los nombres de tus variables, la indentacion adecuada y el formatear cree una estructura 
visual a tu codigo.

**Mal hecho:**
```javascript
////////////////////////////////////////////////////////////////////////////////
// Instanciacion del modelo de Scope
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
  menu: 'foo',
  nav: 'bar'
};

////////////////////////////////////////////////////////////////////////////////
// Iniciar de acciones 
////////////////////////////////////////////////////////////////////////////////
const actions = function() {
  // ...
};
```

**Bien hecho:**
```javascript
$scope.model = {
  menu: 'foo',
  nav: 'bar'
};

const actions = function() {
  // ...
};
```
**[⬆ vuelve hasta arriba](#contenido)**