# Destructuring for dummies!

## Para que se utiliza?

Destructuring nos da una manera de extraer datos "individuales" de objetos o arrays y asignarlos a variables.
En ES6 puede aplicarse sólo a objetos (Object) y Arrays.
Ejemplos segun su tipo:

---

### Arrays

#### Ejemplo 1 (Array)

```javascript
const array = ["pato", "gallina", "pollo"];

// Con ES5
var pato = array[0];
var gallina = array[1];
var pollo = array[2];

// Con ES6 (Destructuring)
const [pato, gallina, pollo] = array;

// Consola:
// -» pato
// "pato"
// -» gallina
// "gallina"
// -» pollo
// "pollo"
```

#### Ejemplo 2 (Ignorando un elemento "gallina")

```javascript
const array = ["pato", "gallina", "pollo"];

// Con ES5
var pato = array[0];
var pollo = array[2];

// Con ES6 (Destructuring)
const [pato, , pollo] = array;

// Consola:
// -» pato
// "pato"
// -» pollo
// "pollo"
```

#### Ejemplo 3 (Intercambiar valores )

```javascript
let gallina = "gallina";
let pollo = "pollo";

// Con ES6 (Destructuring)
[gallina, pollo] = [pollo, gallina];

// Consola:
// -» pollo
// "gallina"
// -» gallina
// "pollo"
```

#### Ejemplo 4 (Destructuring múltiple arrays)

```javascript
const [pollo, [[gallina], pato]] = ["pollo", [["gallina"], "pato"]];

// Consola:
// -» pollo
// "pollo"
// -» gallina
// "gallina"
// -» pato
// "pato"
```

> Si lo analizamos el elemento "pollo" ha sido destructurado de una manera simple. En cambio "gallina" está dentro de un array que a la vez está dentro de un array y hemos podido hacer el destructuring múltiple sin ningún problema. "Pato" finalmente es un elemento dentro de un array por lo que su nivel de destructuración ha sido de 2.

---

### Objetos

#### Ejemplo 1 (Objecto)

```javascript
const objeto = {
  gallina: "gallina",
  pollo: "pollo",
};

// Con ES5
var gallina = objeto.gallina;
var pollo = objeto.pollo;

// Con ES6 (Destructuring)
const { gallina, pollo } = objeto;

// Consola:
// -» gallina
// "gallina"
// -» pollo
// "pollo"
```

#### Ejemplo 2 (Objecto cambiando el nombre de las variables)

```javascript
const objeto = {
  gallina: "gallina",
  pollo: "pollo",
};

// Con ES6 (Destructuring)
const { gallina: gallinita, pollo: pollito } = objeto;

// Consola:
// -» gallinita
// "gallina"
// -» pollito
// "pollo"
```

#### Ejemplo 3 (Objecto con Array Object anidado v1)

```javascript
var object = {
  a: "vaca",
  b: [
    {
      c: "pollo",
      d: "gallina",
    },
  ],
  e: ["pato", "cerdo"],
};

// Nos queremos quedar con el pollo cambiando el nombre a pollito.
const {
  b: [{ c: pollito }],
} = object;

// Consola:
// -» pollito
// "pollo"
```

#### Ejemplo 4 (Objecto con Array Object anidado v2)

```javascript
var object = {
  a: "vaca",
  b: [
    {
      c: "pollo",
      d: "gallina",
    },
  ],
  e: ["pato", "cerdo"],
};

// Nos queremos quedar con la gallina y guardarlo como gallina,
// y con el array de pato y cerdo, y guardarlo como animales.

const {
  b: [{ d: gallina }],
  e: animales,
} = object;

// Consola:
// -» galllina
// "gallina"
// -» animales
// ["pato", "cerdo"]
```

---

#### Ejemplo 5 (Objecto con Array Object anidado v3)

```javascript
var object = {
  a: "vaca",
  b: [
    {
      c: "pollo",
      d: "gallina",
    },
    [1, 2, 3, 4],
  ],
  e: ["pato", "cerdo"],
};

// Nos queremos quedar con el pollo y guardarlo como pollo,
// y con el array de numeros (1,2,3,4), y guardarlo como numeros.

const {
  b: [{ c: pollo }],
  b: [, numeros],
} = object;

// Consola:
// -» pollo
// "pollo"
// -» numeros
// (4) [1, 2, 3, 4]
```

---

### Funciones (Valores por defecto)

```javascript
// Con ES5
function hablar(props) {
  var animal = props.animal === undefined ? "vaca" : props.animal;
  var lugar = props.lugar === undefined ? "granja" : props.lugar;

  console.log("Hola soy " + animal + " y vivo en " + lugar);
}

// Con ES6 (Destructuring)
function hablar(props) {
  const { animal = "vaca", lugar = "granja" } = props;
  console.log(`Hola soy ${animal} y vivo en ${lugar}`);
}

// Consola:
// -» hablar({})
// "Hola soy vaca y vivo en granja"
```

---

## Uso Práctico

Después de haber visto ejemplos d destructuring seguro que os queda la duda de:

> ¿dónde podría aplicar yo esto para que mi código fuese más limpio y legible?

- Retornos de funciones
- Parámetros en las funciones
- Funciones de trabajo con arrays
- Destructuring múltiple
- Importación de objetos
- Destructuring en React

### Retornos de funciones

```javascript
function getPerson() {
  return {
    name: "Pepe",
    age: 26,
    hobbies: ["chess", "running", "basket"],
  };
}

const { name } = getPerson();

// Consola:
// -» name
// "Pepe"
```

### Parámetros en las funciones

```javascript
const people = [
  {
    name: "Pepe",
    age: 26,
    hobbies: ["chess", "running", "basket"],
  },
  {
    name: "Juan",
    age: 32,
    hobbies: ["basket"],
  },
  {
    name: "Paco",
    age: 45,
    hobbies: ["running"],
  },
];

// Con ES5
function getPeople(filter) {
  return people.filter(
    (person) =>
      (!filter.nameFilter || person.name === filter.nameFilter) &&
      (!filter.minAge || person.age > filter.minAge)
  );
}

// Con ES6 (Destructuring)
function getPeople({ nameFilter, minAge = 40 }) {
  return people.filter(
    (person) =>
      (!nameFilter || person.name === nameFilter) &&
      (!minAge || person.age > minAge)
  );
}

// Consola:
// -» const peopleBiggerThan40 = getPeople({ minAge: 40 });
// -» peopleBiggerThan40)
// 0: {name: "Paco", age: 45, hobbies: Array(1)}

// tambien podriamos llamar a la funcion sin pasarle el parámetro
// porque al hacer el destructuring le pasamos el valor 40 por defecto
// -» const peopleBiggerThan40 = getPeople({});
// -» peopleBiggerThan40)
// 0: {name: "Paco", age: 45, hobbies: Array(1)}
```

### Funciones de trabajo con arrays

Por ejemplo la función anterior podría quedar reducida a:

```javascript
function getPeople({ nameFilter, minAge }) {
  return people.filter(
    ({ name, age }) =>
      (!nameFilter || name === nameFilter) && (!minAge || age > minAge)
  );
}

// Hemos destructurado el objeto person dentro de la arrow function del filte
```

### Destructuring múltiple

Podemos hacer destructuring dentro de nuestro propio destructuring hecho,
mientras que no lo mezclemos con el destructuring de arrays.

```javascript
function getPeople({ nameFilter, minAge }) {
    return people.filter({ names: { name }, age } => (!nameFilter || name === nameFilter) && (!minAge || personAge > minAge));
}

```

### Importación de objetos

```javascript

```

### Destructuring en React

```javascript

```
