## El Prototipo

JavaScript no tiene un modelo de herencia clásico, en vez usa uno basado en 
*prototipos*. 

Aunque muchos consideran que esta es una de las debilidades de JavaScript, en 
realidad el modelo de herencia basada en prototipos es mas poderoso que el modelo
clásico. Por ejemplo, es bastante trivial construir el modelo clásico encima del
modelo basado en *prototipos*, pero el contrario es una tarea mucho mas difícil.

Debido al hecho que JavaScript es básicamente el único lenguaje de amplio uso
que tiene este modelo de herencia, toma tiempo acostumbrarse a la diferencia
entre ambos modelos.

La primera diferencia grande es que la herencia en JavaScript se hace utilizando
 *cadenas de prototipos* o *prototype chains*.

> **Nota:** Simplemente usando `Bar.prototype = Foo.prototype` dará como resultado que ambos objetos 
> compartan el **mismo** prototipo. Por ende, cambios al prototipo de cualquiera de los dos objetos
> afectaran el prototipo del otro objeto, lo cual en la mayoría de los casos no es el efecto deseado.

    function Foo() {
        this.value = 42;
    }
    Foo.prototype = {
        method: function() {}
    };

    function Bar() {}

    // Asigna el prototipo de Bar a una nueva instancia de Foo
    Bar.prototype = new Foo();
    Bar.prototype.foo = 'Hello World';

    // Debes asegurarte que el constructor sea Bar
    Bar.prototype.constructor = Bar;

    var test = new Bar() // create a new bar instance

    // La cadena de prototipos
    test [instance of Bar]
        Bar.prototype [instance of Foo] 
            { foo: 'Hello World' }
            Foo.prototype
                { method: ... }
                Object.prototype
                    { toString: ... /* etc. */ }

En el código anterior, el objeto `test` heredara de `Bar.prototype` y
`Foo.prototype`; por lo tanto, tendrá acceso a la función `method` que fue 
definida en `Foo`. Adicionalmente podrá acceder a la propiedad `value` de
la **única** instancia de `Foo` que compone su prototipo. Es importante notar
que `new Bar()` **no** crea una nueva instancia de `Foo`, pero reusa la que
fue asignada a su prototipo; por ende, todas las instancias de `Bar` tendrán
la **misma** propiedad llamada `value`.

> **Nota:** **No** debe usar `Bar.prototype = Foo`, ya que no apuntara al prototipo de  
> `Foo`, sino al objeto de la función `Foo`. Así que la cadena de prototipos saltara 
> a `Function.prototype` pero no a `Foo.prototype`;
> por ende, `method` no estará en la cadena de prototipos.

### Property Lookup

When accessing the properties of an object, JavaScript will traverse the
prototype chain **upwards** until it finds a property with the requested name.

When it reaches the top of the chain - namely `Object.prototype` - and still
hasn't found the specified property, it will return the value
[undefined](#core.undefined) instead.

### The Prototype Property

While the prototype property is used by the language to build the prototype
chains, it is still possible to assign **any** given value to it. Although 
primitives will simply get ignored when assigned as a prototype.

    function Foo() {}
    Foo.prototype = 1; // no effect

Assigning objects, as shown in the example above, will work, and allows for dynamic
creation of prototype chains.

### Performance

The lookup time for properties that are high up on the prototype chain can have a
negative impact on performance critical sections of code. Additionally, trying to 
access non-existent properties will always traverse the full prototype chain. 

Also, when [iterating](#object.forinloop) over the properties of an object 
**every** property that is on the prototype chain will get enumerated.

### Extension of Native Prototypes

One mis-feature that is often used is to extend `Object.prototype` or one of the
other built in prototypes.

This technique is called [monkey patching][1] and breaks *encapsulation*. While 
used by widely spread frameworks such as [Prototype][2], there is still no good 
reason for cluttering built-in types with additional *non-standard* functionality.

The **only** good reason for extending a built-in prototype is to backport 
the features of newer JavaScript engines; for example, 
[`Array.forEach`][3].

### In Conclusion

It is a **must** to understand the prototypal inheritance model completely 
before writing complex code which makes use of it. Also, watch the length of 
the prototype chains and break them up if necessary to avoid possible 
performance issues. Further, the native prototypes should **never** be extended 
unless it is for the sake of compatibility with newer JavaScript features.

[1]: http://en.wikipedia.org/wiki/Monkey_patch
[2]: http://prototypejs.org/
[3]: https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/Array/forEach

