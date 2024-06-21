# TYPESCRIPT
<img src="./images/Typescript_logo_2020.svg.png" alt="TypeScript Logo" width="200">

## JavaScript

JavaScript tiene carencias:
- Antiguamente no era OOP.
- No es tipado (tiene tipado dinámico).
- Autocompletado deficiente.
- Muchos errores de ejecución.

## TypeScript
- Busca misma experiencia de desarrollo que en lenguajes como Java o C#.
- Es un JS mejorado, un superset que añade nuevas características.
- No añade una capa de dificultad relevante en comparación con todo lo que aporta.
- TS no puede ser ejecutado el navegador. Lo transpilaremos para que sea ejecutable (transpilar es lo mismo que compilar pero cuando lo hacemos de lenguaje de alto nivel a lenguaje de alto nivel)
- Es el lenguaje obligado de Angular. Podemos usarlo también en React y Node, y en proyectos Vanilla.


## Tipos de datos: primitivos
Hasta donde sea posible, TypeScript va a tratar de adjudicar un tipo a cada variable o constante que creemos. A esto se le llama inferir el tipo.
Los tipos de datos primitivos son los básicos, que son:
- string: para almacenar textos.
- number: para almacenar cualquier dato de tipo numérico.
- boolean: true/false.
- any: admite cualquier tipo de valor.
- arrays: al igual que en JS, los usamos para almacenar colecciones de datos.

## Inferir el tipo (Tipos de datos)
Cuando no indicamos el tipo explicitamente, se asigna a la variable el tipo de dato del valor.
```ts
let message = "Hello, world!";
```

En este caso, el compilador de TypeScript infiere que message es de tipo string porque se le asigna un valor de cadena. A partir de ese punto, el compilador tratará a message como una cadena, proporcionando las comprobaciones de tipo y las capacidades de autocompletado correspondientes.

## Type Assertion ó AS (Tipos de datos)

En TypeScript, la keyword `as` se utiliza para realizar una **conversión de tipo** o **type assertion** (afirmación de tipo). Esto le indica al compilador que trate una expresión como si fuera de un tipo específico. Es importante notar que `as` no realiza ninguna conversión de tipo en tiempo de ejecución; simplemente le dice al compilador cómo debe tratar la variable en tiempo de compilación.

```typescript
let variable = valor as Tipo;
```

## Tipos de datos: tuplas
Una tupla en typescript es un array de elementos que están tipados.
Podríamos decir que se trata de un array de propiedades.
TypeScript tiene un análisis especial en torno a los arreglos que contengan múltiples tipos, y donde es importante el orden en que se indexan.

Estos se llaman tuplas (en inglés **tuples**). Piensa en ellas como una forma de conectar algunos datos pero con menos sintaxis que un objeto ordenado por llaves.

Puedes crear una tupla utilizando la sintaxis de arreglos en JavaScript, pero declarando su tipo como una tupla:
```ts
type Usuario = [string, number, string];

let usuario1: Usuario = ["Juan", 30, "juan@example.com"];

console.log(usuario1[0]); // Imprime "Juan"
console.log(usuario1[1]); // Imprime 30
console.log(usuario1[2]); // Imprime "juan@example.com"

```

## Tipos de datos: enumeraciones
Es una clase especial que representa un grupo de constantes. Ayuda a trabajar con datos que tienen un fuerte sentido semántico y cuya información esté circunscrita a unos determinados valores.
Permiten, por ejemplo, asociar unos valores semánticos a otros numéricos.

```ts
enum CardinalDirections {
  North,
  East,
  South,
  West
}
let currentDirection = CardinalDirections.North;
```

```ts
enum Estado {
  "Pendiente",
  "En curso",
  "Completado"
}
```

## Tipos de datos: void
Es un tipo propio de las funciones. Cuando una función tiene una salida (retorna un valor), ese valor será de un tipo, y ese tipo se le asignará a la función. Cuando la función no devuelva nada, su tipo será **void**.
```ts
function saludar(): void {
  console.log("¡Hola!");
}

saludar(); // Imprime "¡Hola!"

```

## Programación Orientada a Objetos
La Programación Orientada a Objetos (OOP, por sus siglas en inglés) es un paradigma de programación que organiza el software en "objetos", que pueden contener datos y código. Los principios fundamentales de OOP son:

1. **Clases y Objetos**: 
   - **Clases**: Plantillas para crear objetos. Definen las propiedades (atributos) y comportamientos (métodos) que los objetos creados a partir de la clase pueden tener.
   - **Objetos**: Instancias de clases que representan entidades concretas con características y comportamientos específicos.

2. **Encapsulamiento**: 
   - Consiste en ocultar los detalles internos de los objetos y exponer solo lo necesario a través de métodos públicos. Esto protege los datos y asegura la integridad del objeto.

3. **Herencia**: 
   - Permite que una clase (subclase) herede atributos y métodos de otra clase (superclase). Facilita la reutilización del código y la creación de jerarquías de clases.

4. **Polimorfismo**: 
   - Permite que diferentes clases respondan a la misma interfaz o método de diferentes maneras. Mejora la flexibilidad y la escalabilidad del código.

5. **Abstracción**: 
   - Consiste en simplificar la complejidad del sistema mediante la definición de clases abstractas que representan conceptos generales, dejando los detalles específicos a las clases derivadas.

Estos principios ayudan a estructurar el código de manera modular, facilitando su mantenimiento, reutilización y escalabilidad.

## Desarrollo con TypeScript
### Requisitos:
NodeJS tiene un gestor de paquetes (un instalador) que es NPM.

#### Instalación/comprobación de NODE JS
Comprobar versión de Node
```bash
node -v
```

Comprobar versión de npm
```bash
npm -v
```

Instalar TypeScript en local (en toda la máquina)
```bash
npm install -g typescript
```

Instalar TypeScript sólo en ese proyecto (se instala en la carpeta node_modules). Nos ubicamos primero en la carpeta del proyecto.
```bash
npm install typescript
```

Comprobar versión (de esta forma comprobamos si está instalado)
```bash
tsc -v
```

#### Inicializar un proyecto de TypeScript:
Creamos la carpeta del proyecto. Desde terminal, en ese directorio, ejecutamos el siguiente comando:

```bash
tsc --init
```
Esto generará en la carpeta del proyecto un fichero llamado 'tsconfig.json'. Este fichero contiene la configuración del proyecto en cuanto al funcionamiento de TS se refiere. Sus partes más relevantes son:
- **target**: version de JS del archivo final.
- **removeComments**: quita nuestros comentarios en el momento de transpilar, para que no aparezcan en el archivo JS final.
- **outDir**: directorio de destino tras transpilar. Por defecto, serña el mismo directorio del archivo a transpilar. Es recomendable cambiarlo y poner un directorio específico, para no mezclar código fuente y código transpilado.
- **sourceMap**: hace un cruce entre el código fuente y el transpilado e indica la línea en la que se da el error en el código fuente. De esa forma, se puede hacer trazabilidad de los ficheros TS desde el inspector del navegador. Para ello crea un segundo archivo con extensión `.js.map`.
- **noImplicitAny**: genera error si el tipo ANY es utilizado en el código fuente.

#### Transpilar de TS a JS
Transpilar un fichero concreto de TS: esto generará un archivo JS. (no respeta outDir y otras propiedades del json)
```bash
tsc 01-HolaMundo.ts 
```

Transpilar el proyecto (todos los TS a JS):
```bash
tsc
```

Transpilar constantemente (facilita el desarrollo).
```
tsc -w
```

## Desarrollo con VITE
Vite es un servidor de desarrollo para aplicaciones web front-end que se ha vuelto muy popular en los últimos años. Está creado por Evan You, el mismo creador de Vue.js, y se caracteriza por su increíble velocidad y su experiencia de desarrollo fluida.

¿Qué hace que Vite sea tan rápido? Vite aprovecha varias técnicas innovadoras para lograr una velocidad sin precedentes.

### Instalar Vite
Comando para instalr Vite en mi máquina, de forma global (g):
```bash
npm install -g vite
```

### Crear proyecto con Vite
Comando crear proyecto:
```bash
npm create vite@latest
```

Comando para live preview:
```bash
npm run dev
```

Comando para live preview accesible desde otros equipos de mi red LAN:
```bash
npm run dev -- --host
```