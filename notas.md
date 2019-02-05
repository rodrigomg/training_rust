### Rust training
### Rust examples by chapter, book rust programming language

## Pertenencia

Se refiere al manejo de memoria.
Regla que debe cumplir el compilador, regla de memoria, es similiar a c y c++, interoperabilidad.
No dejas la resposabilidad completa al programador o al sistema.
Manejo de memoria
Stack, valores fijos
Heap, valores variables
3 conceptos de pertenencia
  * El valor pertenece
  * Solamente a un dueño
  * Fuera de alcance se desecha
Estas reglas a variables de tamaño no fijo

## Train the trainers

Consultar ayuda de comandos, ejemplo:

cargo help new

Desde la versión 1.25 el --bin no es necesario.

cargo new hello_cargo

cargo new hello_cargo --lib, genera una biblioteca

Cargo.lock maneja las versiones finales del proyecto.

Rust usa la inferencia de tipos, lenguaje tipado, todas la variables tienen tipo.

Match es similar a switch o case.

Usa patern matching y destructuración.

En rust todas las variables son no opcionales. Si se necesita opcional se lo tienes que indicar de manera explicita.

Struct, type, and enum, nuevos tipos de dato en rust.

Trait, interfaz mas clase abstracta.

## Manejo de memoria:

# Data:
Código compilado y variables estáticas.

Stack (Pila) Estática:
Orden en que se van llamando las funciones. Esto se usa para guardas las variables locales. Etiqueta y apuntadores.
Se guardan datos que ya se sabe el tamaño que tienen.
La memoria se libera sola.

Heap Variable:
Aquí se almacenan los valores de las variables.
El tamaño de lo que se guarda es variable.
La memoria no se libera sola.

Ejemplo de genericos en suma:

use std::ops::Add;

fn add<T:Add<Output=T>>(first: T, second: T) -> T {
    first + second
}

fn main() {
    println!("{}",add(98.87,0.9813));
}

Genericos:
Option para el manejo de null
Result para el manejo de excepciones.

Ejemplo de borrowing
```
#[derive(Debug)]
struct PhoneNumber {
    value: i32,
}
fn call_me_maybe(maybe_number: &Option<PhoneNumber>) {
    match *maybe_number {
        Some(ref s) => println!("calling {:?}", s),
        None => println!("no number, not calling")
    }
}

fn main() {
    let number = Some(PhoneNumber {value: 491728});
    call_me_maybe(&number);
}
```

# Test:

Unidad

```
enum Direction { North, South, East, West }

fn is_north(dir: Direction) -> bool {
    match dir {
        Direction::North => true,
        _ => false,
    }
}

#[test]
fn is_north_works() {
    assert!(is_north(Direction::North) == true);
    assert!(is_north(Direction::South) == false);
}
```

# Submodule
```
enum Direction { North, South, East, West }

fn is_north(dir: Direction) -> bool {
    match dir {
        Direction::North => true,
        _ => false,
    }
}

#[cfg(test)]
mod tests {
    use super::{is_north, Direction};

    #[test]
    fn is_north_works() {
        assert!(is_north(Direction::North) == true);
        assert!(is_north(Direction::South) == false);
    }
}
```

# Uso de self y Self

```
struct Point {
    x: i32,
    y: i32
}

impl Point {
    fn distance(&self, other: &Self) -> f64 {
    fn distance(&self, other: &Point) -> f64 {
    fn distance(self: &Self, other: &Point) -> f64 {
    fn distance(self: &Point, other: &Point) -> f64 {
}
```

# lifetimes

```
#[derive(Debug)]
struct Cmd<'a> {
    binary: &'a str,
    args: Vec<&'a str>,
}

fn read_line() -> String{
    String::from("hi")
}

fn parse(_s: &str) -> (&str, Vec<&str>){
    unimplemented!();
}

fn main() {
    let s = read_line();
    let (binary, args) = parse(&s);

    let cmd = Cmd {
        binary: binary,
        args: args,
    };

    println!("{:?}", cmd)
}
```

## Tipos de datos scalar:
Enteros, flotantes, boleanos y caracteres.

# Enteros:

Signed -> Con signo, números negativos y positivos.
i8 -> −(2 n-1) to (2 n-1 − 1) n es el número de bits (8,16,32,64).

Unsigned -> Sin signo, números positivos.
u8 -> 0 a 2n − 1,  puede almacenar numeros de 0 a 2 a la 8 - 1, de 0 a 255.

# Flotantes:

f32 (bits) mayor precisión, precisión simple.

f64 (bits) mayor velocidad, rust lo toma como default, doble precisión.


# Tipos compuestos:

Rust permite agrupar multiples valores en un solo tipo.

Existen dos tipo primitivos compuestos en rust: tuplas y arreglos

Tulplas, coleción de objetos de difernete tipo.

Arreglos, colección de obajetos del mismo tipo.

En rust a diferencia de otros lenguajes de bajo nivel cuando se intenta acceder a un índice inválido dentro de un arreglo te proteje y el programa entra en "pánico" y su ejecución termina.

# Reglas de propiedad:

* Cada valor en Rust tiene una variable que se llama su propietario.
* Solo puede haber un dueño a la vez.
* Cuando el propietario queda fuera del alcance, el valor se eliminará.
* La memoria debe solicitarse al sistema operativo en tiempo de ejecución.
* Necesitamos una forma de devolver esta memoria al sistema operativo cuando
hemos terminado con nuestra cadena.

Rust hace uso del patrón RAII (Resource Acquisition Is Initialization, c++), patrón de desasignación de recursos al final de la vida útil de un elemento.

Cuando un variable está fuera de su alcance (scope) rust hace el llamado de la función drop de manera automática para liberar la memoria (es la razón por la que el lenguaje no tiene en garbage collector), este manejo de recursos de la memoria esta referido a el heap de la misma.

# Almacenamiento en memoria de un String

Stack:
* nombre
* puntero
* longuitud
* capacidad

Heap:
* Contenido del String
* Varibles que no se conoce su tamaño en tiempo de compilación o que su tamaño puede cambiar en tiempo de ejecución.

Shallow copy, en rust es conocido como move.
Deep copy, en rust es conocido como clone.

Rust nunca crea copias de los datos por default.

Cuando el tamaño de la variable es conocida en tiempo de compilación esta variable es alamcenada en el Stack de la memoria por lo que puede ser copiada (swallow copy) entre variables, sin la necesidad de clonarla.


