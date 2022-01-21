# Apuntes del lenguaje Vlang
Hago estas anotaciones para recordar algunas tecnicas y conceptos importantes del lenguaje V.

## Miscelaneas

- `typeof(var_name).name` devuelve el tipo de dato de la variable `var_name`

## Propiedades y métodos del String

- `str1.len` es una propiedad que devuelve el tamaño de la cadena.
- `str1.count(str2)` cuenta las veces que se repite `str2` en `str1`
- `r'Hola \como estas\'` la 'r' delante indica un string crudo.
- `'pepe'.substr(0,3)` devuelve un extracto de la cadena.
- `'pera, manzana, uva'.split(',')` devuelve un array con las coincidencias.
- `'😀 😃 😄 😁'.runes()` convierte el string a un array de runes.
- `str1.contains(str2)` devuelve `true` si `str1` contiene `str2`.
- `str1.starts_with(str2)` devuelve `true` si `str1` comienza con `str2`.

## Propiedades y métodos del Array
- `array.cap` propiedad que imprime la capacidad del array.
- `array.len` propiedad que imprime la longitud del array.
- `mut i := []int{len:3}` crea un array de `int` con longitud 3.
- `mut j := []int{len: 3, init: 1}` crea un array de `int` de longitud 3 e inicializado con 1.
-  `array.delete(2)` elimina el índice 2 del array.
-  `5 in [1, 5, 7]` busca 5 en el array.
-  `mut i := [4]int{}` crea un array de `int` con longitud 4.
-  `array2 := array1.clone()` clona `array1` en `array2`.
-  `[3, 2, 8, 1].sort()` ordena el array de forma ascendente.
-  `[3, 2, 8, 1].sort(a > b)` ordena el array de forma descendente.
-  `[1, 3, 5, 7].filter(it < 5)` crea un array con los elementos que cumplan con el filtro.
-  `['Juan', 'Alberto', 'Carlos'].map('Señor $it')` mapea el array anteponiendo `Señor` en cada elemento.
-  El siguiente ejemplo muestra el método `map()` que recibe una función anónima:
```v
names := ['Juan', 'Alberto', 'Carlos']
result := names.map(fn(s string) {
  if s.contains('o') {
    return 1
  } else {
    return 0
  }
})
``` 

## Propiedades y métodos de los Diccionarios
- `mut books := map[string]int{}` crea un diccionario explícito de tipo `string:int` vacío.
- `mut books := {'book1': 10, 'book2': 20}` versión resumida para crear un diccionario `string:int`.
- `books.len` devuelve la cantidad de elementos presentes en el diccionario.
- `books['no existe']` las claves inexistentes devuelven cero `0`.
- `books.delete('book1')` borra el elemento del diccionario dada su clave.

## Pattern matching con `match`

```v
age := 18
match age {
  1...18 { 'Eres un chaval!' }
  19...35 { 'Eres un adulto joven' }
  36...65 { 'Eres un adulto' } 
  66...95 { 'Eres un anciano' }
  else { 'Eres un alienigena' }
}
println(age)
```

## Ejemplos con `for loop`

```v
frutas := ['manzanas', 'mangos', 'peras', 'piñas']
// loop con índice y valor
for i, v in frutas {
  println('frutas[$i] = $v')
}

// loop solo con valor
for v in frutas {
  println('me gusta comer: $v')
}

// loop al estilo C
for i := 0; i < frutas.len; i++ {
  println('no me gusta comer: $frutas[i]')
}

// loop en un rango
for i in 10..20 {
  println(i)
}

// loop infinito
for {
  println('me imprimo para siempre...!!!')
}
```
## Funciones de alto orden
Son las funciones que se reciben como parámetro y se retornan desde otras funciones.

```v
fn temprano() string {
  return 'Buenos días'
}

fn tarde() string {
  return 'Buenas tardes'
}

fn noche() string {
  return 'Buenas noches'
}

fn saludar(func fn() string, name string) string {
  return '$func() $name'
}

fn main() {
  println(saludar(temprano, 'Irwin'))
  println(saludar(tarde, 'Irwin'))
  println(saludar(noche, 'Irwin'))
}
```

## Funciones anónimas
Son funciones que no tienen un identificador y se pueden usar en cualquier contexto. Ejemplo:

```v
println(saludar(fn() string {
  return 'Buenas anonimas, '
  }, 'Irwin'))
```
## Funciones que retornan múltiples valores
```v
fn mayusculas(str string) (string, int) {
  return str.to_upper(), str.len
}

fn main() {
  str_may, str_size := mayusculas('irwin')
  println("'$str_may' tiene $str_size de longitud.")
}
```

## Permitir variables globales
En `v` no se permiten las variables globales pero si quieres hacerlo entonces tienes que compilar o ejecutar el script de la siguiente manera: `v -enable-globals run main.v`

## Funciones que retornan un option
Esto es basicamente que una función puede retornar un valor o `none` que es un tipo especial que indica `ausencia de valor`. El llamador de este tipo de funciones siempre debe evaluar el resultado porque puede o no haber valor.
```v
fn adolescente(edad string) ?string {
  if edad <= 0 {
    return none
  } else if edad >= 13 && edad <= 19 {
    return 'es adolescente'
  } else {
    return 'no es adolescente'
  }
}

fn main() {
  result := adolescente(-3) or { 'edad inválida' }
  println(result)
}
```

También se puede retornar un `error` en lugar de `none`:
```v
fn adolescente(edad string) ?string {
  if edad <= 0 {
    return error('edad inválida')
  } else if edad >= 13 && edad <= 19 {
    return 'es adolescente'
  } else {
    return 'no es adolescente'
  }
}

fn main() {
  result := adolescente(-3) or { err.msg } // err es una variable especial en caso de error.
  println(result)
}
```

## Ejecución diferida dentro de una función
Una función puede ejecutar código diferido, esto quiere decir que cuando el cuerpo de la funció se haya ejecutado, procederá a ejecutarse el bloque diferido.

```v
fn defer_test() {
  println('Hola')
  defer {
    println('Hola desde el bloque diferido :)')
  }
  println('Como estas?')
  println('bueno... chao!')
}

fn main() {
  defer_test()
}
```

## Funciones como elementos de array o diccionario
La condición es que las funciones que formarán parte de los elementos del array o mapa, tienen que coincidir con la misma firma.

```v
module main

fn add(x int, y int) int {
  return x + y
}

fn sub(x int, y int) int {
  return x - y
}

fn mul(x int, y int) int {
  return x * y
}

fn div(x int, y int) int {
  return x / y
}

fn main() {
  math := [add, sub, mul, div]
  x, y := 10, 2
  for f in math {
    println(f(x, y))
  }
}
```

El mismo ejemplo con un diccionario:

```v
module main

fn add(x int, y int) int {
  return x + y
}

fn sub(x int, y int) int {
  return x - y
}

fn mul(x int, y int) int {
  return x * y
}

fn div(x int, y int) int {
  return x / y
}

fn main() {
  math := {
    'suma': add
    'resta': sub
    'multiplicacion': mul
    'division': div
  }
  x, y := 10, 2
  for k, v in math {
    println("la $k de ($x, $y) es: ${v(x, y)}")
  }
}
```

## Crear un proyecto en V por consola
```v
v new my_project
Input your project description: Descripción de tu proyecto. Lo demás lo puedes dejar por defecto si quieres.
Input your project version: (0.0.0) 
Input your project license: (MIT) 
Initialising ...
Complete!
```
Lo anterior crea una carpeta llamada `my_project` con los siguientes ficheros:
1. **.gitignore:**
2. **my_project.v:** el punto de entrada para la aplicación que vamos a crear. El módulo tiene que ser `main` y lleva la función `main`.
3. **v.mod:** contiene la información del modulo creado.

## Concurrencia y Paralelismo
- **Concurrencia:** una o más tareas ejecutandose al mismo tiempo.
- **Paralelismo:** varias tareas ejecutandose para resolver un problema en común.

## Terminología
- **Programa:** Un programa es un conjunto de instrucciones en forma de funciones y sentencias que nos ayudan a lograr un trabajo en particular.
- **Proceso:** Un programa con una o más funciones y sentencias, cuando comienza a ejecutarse, se asocia con el proceso. Un proceso puede tener uno o más
subprocesos, cada uno de ellos ejecutándose en un subproceso diferente.
- **Hilo:** un hilo permite que una o más tareas se ejecuten en orden secuencial.
- **Tarea:** una tarea es una unidad de trabajo que se ejecuta en un subproceso. Se puede representar como una función en V.

## El módulo time para pausar el proceso actual
```v
import time

fn main() {
  println("Antes...")
  time.sleep(i64(500) * time.millisecond)
  println("Después...")
}
```

## El módulo time para mostrar el tiempo transcurrido en un proceso
```v
import time

fn main() {
  sw := time.new_stopwatch()
  for i in 1..101 {
    println('$i')
  }
  println('Tiempo transcurrido: ${sw.elapsed().seconds()}')
}
```

## Ejecutando tarea asíncrona
```v
fn contar_hasta(tope int) {
  for i in 1..tope {
    // nada, solo contar...
  }
}

fn main() {
  println('Antes...')
  thread := go contar_hasta(1000000000)
  thread.wait() // espera a que termine
  println('Después...')
}
```

## Array de hilos (thread)
```v
import time
fn retirar_dinero() {
  println('retirando dinero...')
  time.sleep(i64(5) * time.second)
  println('dinero retirado...')
}

fn tomar_metro() {
  println('tomando el metro...')
  time.sleep(i64(3) * time.second)
  println('montado en el metro..')
}

fn salir_del_trabajo() {
   println('saliendo del trabajo')
   time.sleep(i64(4) * time.second)
   println('fuera del trabajo...')
}

fn main() {
  sw := time.new_stopwatch() // para medir el tiempo total
  
  mut t := []thread{}
  t << go salir_del_trabajo()
  t << go retirar_dinero()
  t << go tomar_metro()
  t.wait() // esperar que la tarea más larga termine
  println('he salido del trabajo y vuelto a casa es ${sw.elapsed().seconds()} segundos')
}
```

## Array de thread con funciones que retornan un valor

El resultado es un array del tipo en común que retornan las funciones.

## Array de hilos (thread)
```v
import time
fn retirar_dinero() string {
  println('retirando dinero...')
  time.sleep(i64(5) * time.second)
  println('dinero retirado...')
  return 'Dinero retirado'
}

fn tomar_metro() string {
  println('tomando el metro...')
  time.sleep(i64(3) * time.second)
  println('montado en el metro..')
  return 'Subido en el metro'
}

fn salir_del_trabajo() string {
   println('saliendo del trabajo')
   time.sleep(i64(4) * time.second)
   println('fuera del trabajo...')
   return 'fuera del trabajo'
}

fn main() {
  sw := time.new_stopwatch() // para medir el tiempo total
  
  mut t := []thread string{}
  t << go salir_del_trabajo()
  t << go retirar_dinero()
  t << go tomar_metro()
  
  result := t.wait() // esperar que la tarea más larga termine
  
  // result es de tipo []string
  println('he salido del trabajo y vuelto a casa es ${sw.elapsed().seconds()} segundos')
  
  for r in result {
    println(r) // es un string por cada función
  }
}
```

## Concurrencia con funciones anónimas

La gramática es 'go' function_declaration '()': es decir, primero colocamos la palabra reservada `go` seguido de la definición de la función anónima y finalmente los paréntesis para invocar a la función debido a `go`.

```v
fn main() {
  t := go fn() string {
         return 'Hola anónimo!'
       }()
  res := t.wait()
  println(res)
}
```

## Test unitarios
Las pruebas rápidas se pueden hacer con `assert <Expr>` pero una suite completa sería:

```v
fn testsuite_begin() {
  println('antes de ejecutar los tests!')
}

fn test_something() {
  assert 2 == 2
}

fn testsuite_end() {
  println('después de ejecutar los tests!')
}
```
