
# Leafdoc


Leafdoc (or 🍂📄 for short) is a NaturalDocs- and JSdoc-like documentation generator.

Leafdoc's goals are to help produce documentation which is:
* **Concise**: If you need half a page to describe what a function does, then Leafdoc is probably not for you.
* **Non-intrusive**: Allow devs to write the minimum possible amount of documentation lines. A two-line function shouldn't need 15 lines of docs.
* **Narrative**: Forget about exhaustive code introspection, and focus into providing human-readable explanations and per-class code examples using Markdown to do the heavy lifting.
* **Language-independent**: Designed with Javascript in mind (and its dozens of ways of doing class inheritance or defining methods), Leafdoc doesn't parse your source code, just the comments.

## Try


See [Leafdoc's documentation as generated by Leafdoc](http://ivansanchez.github.io/Leafdoc/doc.html), or a test of [partial Leaflet documentation as generated by Leafdoc](http://ivansanchez.github.io/Leafdoc/Leaflet-docs.html).


### Try it yourself

```
git clone [...]
npm install
js test.js > leafdoc.html
```


## Write

The syntax is pretty much the tried-and-true directives-in-comment-blocks from JSdoc, NaturalDocs and a gazillion others. But instead of an `@` symbol, Leafdoc uses a leaf:

```
/*
 * 🍂method addDir: this
 * 🍂param dirname: String
 * 🍂param extension: String
 *
 * Recursively scans a directory, and parses any files that match the given `extension`
 */
```

Leafdoc was designed with compactness in mind, so it accepts comment blocks consisting of adjacent `//` comment lines, and a shorthand syntax for function/method parameters. This is equivalent to the example just above:

```
// 🍂method addDir (dirname: String, extension: String): this
// Recursively scans a directory, and parses any files that match the given `extension`
```


### Valid directives

* `🍂class` and `🍂namespace` should be at the top of your files. They define the context of the rest of the directives. A namespace can be used in more than one file (for example, when plugging more functionality to an existing class).
* `🍂example` lets some space to demonstrate how the class / namespace is meant to be used.
* `🍂section` allows you to group several functions, events, methods or options together, thematically.
* Methods, functions, options, etc are a generic thing internally named "documentable":
	* `🍂method`
	* `🍂function`
	* `🍂factory`
	* `🍂option`
	* `🍂event`
* Functions/methods/factories can have parameters, specified with `🍂param (name)`, `🍂param (name): (type)` or inline (see below).
* Classes, namespaces and documentables can have `🍂aka (alternative name)`, (short for Also Known As). This allows to create links to the same thing using different names.
* Anything can have several `🍂comment`s, explaining the thing. `🍂comment` directives can be implicit, because any (non-empty) line without an explicit directive equals to a comment.
* If a function (or any other documentable) has several alternative uses, use the `🍂alternative` directive after to re-defining the documentable, e.g.:
  ```
  /*
  🍂method on(type: String, fn: Function): this
  Adds the function `fn` as an event handler for the `type` event.

  🍂alternative
  🍂method on(types: String, fn: Function): this
  Given an array of event types (strings), attaches `fn` as an event handler to each of them.
  */
  ```
  In this example, the two alternatives are `on(type, fn)` and `on(fnMap)`. They will be shown as two different documentables.

* `🍂inherits (parent)` means that a class or namespace inherits all documentables from another class or namespace.

### Shorthand syntax

Any documentable has the same syntax: name, optional parameters, optional type / return type/value, default value.

```
name [ ( params ) ] [: type] [= default]
```
where `params` is
```
name[?] [ : type ]  [ , name[?] [ : type ]  [ , name[?] [ : type ] ...  ]   ]
```

Which means the following are possible:

```
An option usually has a type and a default value:
🍂option enabled: Boolean = true

But we can omit the default value:
🍂option enabled: Boolean

Or omit just the data type:
🍂option enabled = true

Or even just say there is an option:
🍂option enabled

A function with a return type:
🍂function get: String

A function with one parameter:
🍂function get(id: Int): String

Or two:
A function with one parameter:
🍂function sum(a: Int, b: Int): Int

The last function can also be written without the parameters shorthand:
🍂function sum: Int
🍂param a: Int
🍂param b: Int

Use an interrogation sign to specify a parameter as optional:
🍂function update(force? : Boolean): null
```

You can specify everything (name, params, type, default), but no documentable uses them all in the templates. The usual schema is:

|            | Params | Type | Default |
| ---------- | ------ | ---- | ------- |
| example    |        |      |         |
| option     |        |  X   |   X     |
| property   |        |  X   |   X     |
| event      |        |  X   |         |
| method     |   X    |  X   |         |
| function   |   X    |  X   |         |
| factory    |   X    |      |         |


### Customization

The output relies on a set of `handlebars` templates. By default, the ones in `templates/basic` will be used. In order to use another set of templates, pass the `templateDir` option to the Leafdoc constructor, like so:

```js
var l = new Leafdoc({templateDir: 'leaflet'});
```

I will write no detailed docs on how to modify the templates. If you know some HTML and `handlebars`, just copy them and hack things away :-)

## Known gotchas

* Leafdoc relies on some ugly per-module globals, so having more than one instance in your code (in the same process) might break things up. The use case for Leafdoc is to have just one instance when building docs for your code.


## Troubleshooting

### I'm using Debian and I cannot see the leaf character!

Run `apt-get install ttf-ancient-fonts` and don't ask why the fallback file for emojis is packaged as "ancient fonts".

### I cannot type 🍂 in my keyboard!

(Linux only) Write this into a plain text file:

```
keycode  46 = l L l L U1F342 Lstroke lstroke
```

Then run `xmodmap name-of-the-file`. Now 🍂 is mapped to `AltGr+l`.
