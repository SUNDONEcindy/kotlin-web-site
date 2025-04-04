[//]: # (title: Type-safe builders)

By using well-named functions as builders in combination with [function literals with receiver](lambdas.md#function-literals-with-receiver) 
it is possible to create type-safe, statically-typed builders in Kotlin.

Type-safe builders allow creating Kotlin-based domain-specific languages (DSLs) suitable for building complex hierarchical 
data structures in a semi-declarative way. Sample use cases for the builders are:

* Generating markup with Kotlin code, such as [HTML](https://github.com/Kotlin/kotlinx.html) or XML
* Configuring routes for a web server: [Ktor](https://ktor.io/docs/routing.html)

Consider the following code:

```kotlin
import com.example.html.* // see declarations below

fun result() =
    html {
        head {
            title {+"XML encoding with Kotlin"}
        }
        body {
            h1 {+"XML encoding with Kotlin"}
            p  {+"this format can be used as an alternative markup to XML"}

            // an element with attributes and text content
            a(href = "https://kotlinlang.org") {+"Kotlin"}

            // mixed content
            p {
                +"This is some"
                b {+"mixed"}
                +"text. For more see the"
                a(href = "https://kotlinlang.org") {+"Kotlin"}
                +"project"
            }
            p {+"some text"}

            // content generated by
            p {
                for (arg in args)
                    +arg
            }
        }
    }
```

This is completely legitimate Kotlin code.
You can [play with this code online (modify it and run in the browser) here](https://play.kotlinlang.org/byExample/09_Kotlin_JS/06_HtmlBuilder).

## How it works

Assume that you need to implement a type-safe builder in Kotlin.
First of all, define the model you want to build. In this case you need to model HTML tags.
It is easily done with a bunch of classes.
For example, `HTML` is a class that describes the `<html>` tag defining children like `<head>` and `<body>`.
(See its declaration [below](#full-definition-of-the-com-example-html-package).)

Now, let's recall why you can say something like this in the code:

```kotlin
html {
 // ...
}
```

`html` is actually a function call that takes a [lambda expression](lambdas.md) as an argument.
This function is defined as follows:

```kotlin
fun html(init: HTML.() -> Unit): HTML {
    val html = HTML()
    html.init()
    return html
}
```

This function takes one parameter named `init`, which is itself a function.
The type of the function is `HTML.() -> Unit`, which is a *function type with receiver*.
This means that you need to pass an instance of type `HTML` (a *receiver*) to the function,
and you can call members of that instance inside the function.

The receiver can be accessed through the `this` keyword:

```kotlin
html {
    this.head { ... }
    this.body { ... }
}
```

(`head` and `body` are member functions of `HTML`.)

Now, `this` can be omitted, as usual, and you get something that looks very much like a builder already:

```kotlin
html {
    head { ... }
    body { ... }
}
```

So, what does this call do? Let's look at the body of `html` function as defined above.
It creates a new instance of `HTML`, then it initializes it by calling the function that is passed as an argument
(in this example this boils down to calling `head` and `body` on the `HTML` instance), and then it returns this instance. 
This is exactly what a builder should do.

The `head` and `body` functions in the `HTML` class are defined similarly to `html`. 
The only difference is that they add the built instances to the `children` collection of the enclosing `HTML` instance:

```kotlin
fun head(init: Head.() -> Unit): Head {
    val head = Head()
    head.init()
    children.add(head)
    return head
}

fun body(init: Body.() -> Unit): Body {
    val body = Body()
    body.init()
    children.add(body)
    return body
}
```

Actually these two functions do just the same thing, so you can have a generic version, `initTag`:

```kotlin
protected fun <T : Element> initTag(tag: T, init: T.() -> Unit): T {
    tag.init()
    children.add(tag)
    return tag
}
```

So, now your functions are very simple:

```kotlin
fun head(init: Head.() -> Unit) = initTag(Head(), init)

fun body(init: Body.() -> Unit) = initTag(Body(), init)
```

And you can use them to build `<head>` and `<body>` tags. 

One other thing to be discussed here is how you add text to tag bodies. In the example above you say something like:

```kotlin
html {
    head {
        title {+"XML encoding with Kotlin"}
    }
    // ...
}
```

So basically, you just put a string inside a tag body, but there is this little `+` in front of it,
so it is a function call that invokes a prefix `unaryPlus()` operation.
That operation is actually defined by an extension function `unaryPlus()` that is a member of the `TagWithText` abstract 
class (a parent of `Title`):

```kotlin
operator fun String.unaryPlus() {
    children.add(TextElement(this))
}
```

So, what the prefix `+` does here is wrapping a string into an instance of `TextElement` and adding it to the `children` collection,
so that it becomes a proper part of the tag tree.

All this is defined in a package `com.example.html` that is imported at the top of the builder example above.
In the last section you can read through the full definition of this package.

## Scope control: @DslMarker

When using DSLs, one might have come across the problem that too many functions can be called in the context. 
You can call methods of every available implicit receiver inside a lambda and therefore get an inconsistent result, 
like the tag `head` inside another `head`: 

```kotlin
html {
    head {
        head {} // should be forbidden
    }
    // ...
}
```

In this example only members of the nearest implicit receiver `this@head` must be available; `head()` is a member of the 
outer receiver `this@html`, so it must be illegal to call it.

To address this problem, there is a special mechanism to control receiver scope.

To make the compiler start controlling scopes you only have to annotate the types of all receivers used in the DSL with 
the same marker annotation.
For instance, for HTML Builders you declare an annotation `@HTMLTagMarker`:

```kotlin
@DslMarker
annotation class HtmlTagMarker
```

An annotation class is called a DSL marker if it is annotated with the `@DslMarker` annotation.

In our DSL all the tag classes extend the same superclass `Tag`.
It's enough to annotate only the superclass with `@HtmlTagMarker` and after that the Kotlin compiler will treat all the 
inherited classes as annotated:

```kotlin
@HtmlTagMarker
abstract class Tag(val name: String) { ... }
```

You don't have to annotate the `HTML` or `Head` classes with `@HtmlTagMarker` because their superclass is already annotated:

```kotlin
class HTML() : Tag("html") { ... }

class Head() : Tag("head") { ... }
```

After you've added this annotation, the Kotlin compiler knows which implicit receivers are part of the same DSL and allows to call members of the nearest receivers only: 

```kotlin
html {
    head {
        head { } // error: a member of outer receiver
    }
    // ...
}
```

Note that it's still possible to call the members of the outer receiver, but to do that you have to specify this receiver explicitly:

```kotlin
html {
    head {
        this@html.head { } // possible
    }
    // ...
}
```

You can also apply the `@DslMarker` annotation directly to [function types](lambdas.md#function-types).
Simply annotate the `@DslMarker` annotation with `@Target(AnnotationTarget.TYPE)`:

```kotlin
@Target(AnnotationTarget.TYPE)
@DslMarker
annotation class HtmlTagMarker
```

As a result, the `@DslMarker` annotation can be applied to function types, most commonly to lambdas with receivers. For example:

```kotlin
fun html(init: @HtmlTagMarker HTML.() -> Unit): HTML { ... }

fun HTML.head(init: @HtmlTagMarker Head.() -> Unit): Head { ... }

fun Head.title(init: @HtmlTagMarker Title.() -> Unit): Title { ... }
```

When you call these functions, the `@DslMarker` annotation restricts access to outer receivers in the body of a lambda marked with it unless you specify them explicitly:

```kotlin
html {
    head {
        title {
            // Access to title, head or other functions of outer receivers is restricted here.
        }
    }
}
```

Only the nearest receiver's members and extensions are accessible within a lambda, preventing unintended interactions between nested scopes.

### Full definition of the com.example.html package

This is how the package `com.example.html` is defined (only the elements used in the example above).
It builds an HTML tree. It makes heavy use of [extension functions](extensions.md) and
[lambdas with receiver](lambdas.md#function-literals-with-receiver).

```kotlin
package com.example.html

interface Element {
    fun render(builder: StringBuilder, indent: String)
}

class TextElement(val text: String) : Element {
    override fun render(builder: StringBuilder, indent: String) {
        builder.append("$indent$text\n")
    }
}

@DslMarker
annotation class HtmlTagMarker

@HtmlTagMarker
abstract class Tag(val name: String) : Element {
    val children = arrayListOf<Element>()
    val attributes = hashMapOf<String, String>()

    protected fun <T : Element> initTag(tag: T, init: T.() -> Unit): T {
        tag.init()
        children.add(tag)
        return tag
    }

    override fun render(builder: StringBuilder, indent: String) {
        builder.append("$indent<$name${renderAttributes()}>\n")
        for (c in children) {
            c.render(builder, indent + "  ")
        }
        builder.append("$indent</$name>\n")
    }

    private fun renderAttributes(): String {
        val builder = StringBuilder()
        for ((attr, value) in attributes) {
            builder.append(" $attr=\"$value\"")
        }
        return builder.toString()
    }

    override fun toString(): String {
        val builder = StringBuilder()
        render(builder, "")
        return builder.toString()
    }
}

abstract class TagWithText(name: String) : Tag(name) {
    operator fun String.unaryPlus() {
        children.add(TextElement(this))
    }
}

class HTML : TagWithText("html") {
    fun head(init: Head.() -> Unit) = initTag(Head(), init)

    fun body(init: Body.() -> Unit) = initTag(Body(), init)
}

class Head : TagWithText("head") {
    fun title(init: Title.() -> Unit) = initTag(Title(), init)
}

class Title : TagWithText("title")

abstract class BodyTag(name: String) : TagWithText(name) {
    fun b(init: B.() -> Unit) = initTag(B(), init)
    fun p(init: P.() -> Unit) = initTag(P(), init)
    fun h1(init: H1.() -> Unit) = initTag(H1(), init)
    fun a(href: String, init: A.() -> Unit) {
        val a = initTag(A(), init)
        a.href = href
    }
}

class Body : BodyTag("body")
class B : BodyTag("b")
class P : BodyTag("p")
class H1 : BodyTag("h1")

class A : BodyTag("a") {
    var href: String
        get() = attributes["href"]!!
        set(value) {
            attributes["href"] = value
        }
}

fun html(init: HTML.() -> Unit): HTML {
    val html = HTML()
    html.init()
    return html
}
```
