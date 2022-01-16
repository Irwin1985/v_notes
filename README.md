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
