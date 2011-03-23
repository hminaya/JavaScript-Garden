## Uso de los Objetos y sus Propiedades

Todo en JavaScript actúa como un objeto, con las únicas dos excepciones siendo 
[`null`](#core.undefined) y [`undefined`](#core.undefined).

    false.toString() // 'false'
    [1, 2, 3].toString(); // '1,2,3'
    
    function Foo(){}
    Foo.bar = 1;
    Foo.bar; // 1

Una percepción errónea muy común es que los literales numéricos no pueden ser usados como objetos. 
Esto es debido a una falla en el parser de JavaScript que trata de convertir la 
*notación del punto* en una literal de punto flotante.

    2.toString(); // provoca un SyntaxError

Existen un par de soluciones que pueden ser utilizadas para hacer que los literales numéricos
actúen como objetos.

    2..toString(); // el segundo punto es reconocido correctamente
    2 .toString(); // fijense en el espacio a la izquierda del punto
    (2).toString(); // el 2 se evalúa primero

### Objetos como un Tipo de Dato

Los Objetos en JavaScript también pueden ser utilizados como un [*Hashmap*][1], consisten
mayormente un mapeo de propiedades a valores.

Usando el  object literal - `{}` notation - es posible crear un objeto simple. Este nuevo
objeto [hereda](#object.prototype) de `Object.prototype` y no tiene [propiedades propias](#object.hasownproperty) definidas.

    var foo = {}; // un nuevo objeto vacio.

    // un nuevo objeto con una propiedad llamada 'test' con el valor 12
    var bar = {test: 12}; 

### Accediendo a Propiedades

Las propiedades de un objeto pueden se pueden acceder de dos maneras, mediante
la notación del punto, o la notación de corchetes.
    
    var foo = {name: 'Kitten'}
    foo.name; // kitten
    foo['name']; // kitten
    
    var get = 'name';
    foo[get]; // kitten
    
    foo.1234; // SyntaxError
    foo['1234']; // funciona

Ambas notaciones funcionan de la misma manera, la única diferencia viene siendo
que la notación de corchetes permite la asignación dinámica de propiedades, así 
como también el uso de nombres para propiedades que de otra manera dieran un 
error de syntax.

### Borrando Propiedades

En realidad la una manera de remover una propiedad de un objeto es utilizando el
operador `delete`; asignarle `undefined` o `null` a una propiedad solamente remueve el 
*valor* que esta asociado con la propiedad, pero no la *llave*.

    var obj = {
        bar: 1,
        foo: 2,
        baz: 3
    };
    obj.bar = undefined;
    obj.foo = null;
    delete obj.baz;

    for(var i in obj) {
        if (obj.hasOwnProperty(i)) {
            console.log(i, '' + obj[i]);
        }
    }

El código anterior imprime `bar undefined` y `foo null` - solamente `baz` fue removido
y por ende no se imprime.

### Notación de Llaves

    var test = {
        'case': 'Soy una palabra clave, debo ser escrita como un string',
        delete: 'Yo también lo soy, así que' // produzco un SyntaxError
    };

Las propiedades de los Objetos pueden ser escritas como caracteres simples y como strings. Debido a otro
error en el diseño del parser de JavaScript el código anterior debe producir un `SyntaxError` antes
de ECMAScript 5.

Este error proviene del hecho que `delete` es una *palabra reservada*; por ende, debe ser escrita
como un *string literal* para asegurarse que sea interpretada correctamente por los motores de 
JavaScript viejos.

[1]: http://en.wikipedia.org/wiki/Hashmap

