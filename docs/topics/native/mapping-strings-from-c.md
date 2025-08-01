[//]: # (title: Mapping strings from C – tutorial)

<tldr>
    <p>This is the final part of the <strong>Mapping Kotlin and C</strong> tutorial series. Before proceeding, make sure you've completed the previous steps.</p>
    <p><img src="icon-1-done.svg" width="20" alt="First step"/> <a href="mapping-primitive-data-types-from-c.md">Mapping primitive data types from C</a><br/>
        <img src="icon-2-done.svg" width="20" alt="Second step"/> <a href="mapping-struct-union-types-from-c.md">Mapping struct and union types from C</a><br/>
      <img src="icon-3-done.svg" width="20" alt="Third step"/> <a href="mapping-function-pointers-from-c.md">Mapping function pointers</a><br/>
      <img src="icon-4.svg" width="20" alt="Fourth step"/> <strong>Mapping strings from C</strong><br/>
    </p>
</tldr>

> The C libraries import is in [Beta](native-c-interop-stability.md). All Kotlin declarations
> generated by the cinterop tool from C libraries should have the `@ExperimentalForeignApi` annotation.
>
> Native platform libraries shipped with Kotlin/Native (like Foundation, UIKit, and POSIX)
> require opt-in only for some APIs.
>
{style="note"}
 
In the final part of the series, let's see how to deal with C strings in Kotlin/Native.

In this tutorial, you'll learn how to:

* [Pass a Kotlin string to C](#pass-kotlin-strings-to-c)
* [Read a C string in Kotlin](#read-c-strings-in-kotlin)
* [Receive C string bytes into a Kotlin string](#receive-c-string-bytes-from-kotlin)

## Working with C strings

C doesn't have a dedicated string type. Method signatures or documentation can help you identify 
whether a given `char *` represents a C string in a particular context.

Strings in the C language are null-terminated, so a trailing zero character `\0` is added to the end of a byte sequence
to mark the end of a string. Usually, [UTF-8 encoded strings](https://en.wikipedia.org/wiki/UTF-8) are used.
The UTF-8 encoding uses variable-width characters and is backward-compatible with [ASCII](https://en.wikipedia.org/wiki/ASCII).
Kotlin/Native uses UTF-8 character encoding by default.

To understand how strings are mapped between Kotlin and C, first create the library headers.
In the [first part of the series](mapping-primitive-data-types-from-c.md), you've already created a C library with the
necessary files. For this step:

1. Update your `lib.h` file with the following function declarations that work with C strings:

   ```c
   #ifndef LIB2_H_INCLUDED
   #define LIB2_H_INCLUDED
   
   void pass_string(char* str);
   char* return_string();
   int copy_string(char* str, int size);
   
   #endif
   ```

   This example shows common ways to pass or receive a string in the C language. Handle the return value of the
   `return_string()` function carefully. Ensure you use the correct `free()` function to release the returned `char*`.

2. Update the declarations in the `interop.def` file after the `---` separator:

   ```c
   ---
   
   void pass_string(char* str) {
   }
   
   char* return_string() {
     return "C string";
   }
   
   int copy_string(char* str, int size) {
       *str++ = 'C';
       *str++ = ' ';
       *str++ = 'K';
       *str++ = '/';
       *str++ = 'N';
       *str++ = 0;
       return 0;
   }
   ```

The `interop.def` file provides everything necessary to compile, run, or open the application in an IDE.

## Inspect generated Kotlin APIs for a C library

Let's see how C string declarations are mapped into Kotlin/Native:

1. In `src/nativeMain/kotlin`, update your `hello.kt` file from the [previous tutorial](mapping-function-pointers-from-c.md)
   with the following content:

   ```kotlin
   import interop.*
   import kotlinx.cinterop.ExperimentalForeignApi
  
   @OptIn(ExperimentalForeignApi::class)
   fun main() {
       println("Hello Kotlin/Native!")

       pass_string(/*fix me*/)
       val useMe = return_string()
       val useMe2 = copy_string(/*fix me*/)
   }
   ```

2. Use IntelliJ IDEA's [Go to declaration](https://www.jetbrains.com/help/rider/Navigation_and_Search__Go_to_Declaration.html)
   command (<shortcut>Cmd + B</shortcut>/<shortcut>Ctrl + B</shortcut>) to navigate to the following generated API for C
   functions:

   ```kotlin
   fun pass_string(str: kotlinx.cinterop.CValuesRef<kotlinx.cinterop.ByteVarOf<kotlin.Byte> /* from: kotlinx.cinterop.ByteVar */>?)
   fun return_string(): kotlinx.cinterop.CPointer<kotlinx.cinterop.ByteVarOf<kotlin.Byte> /* from: kotlinx.cinterop.ByteVar */>?
   fun copy_string(str: kotlinx.cinterop.CValuesRef<kotlinx.cinterop.ByteVarOf<kotlin.Byte> /* from: kotlinx.cinterop.ByteVar */>?, size: kotlin.Int): kotlin.Int
   ```

These declarations are straightforward. In Kotlin, C `char *` pointers are mapped into `str: CValuesRef<ByteVarOf>?` for
parameters and into `CPointer<ByteVarOf>?` for return types. Kotlin represents the `char` type as `kotlin.Byte`,
as it's usually an 8-bit signed value.

In the generated Kotlin declarations, `str` is defined as `CValuesRef<ByteVarOf<Byte>>?`.
Since this type is nullable, you can pass `null` as the argument value. 

## Pass Kotlin strings to C

Let's try to use the API from Kotlin. Call the `pass_string()` function first:

```kotlin
import interop.*
import kotlinx.cinterop.ExperimentalForeignApi
import kotlinx.cinterop.cstr

@OptIn(ExperimentalForeignApi::class)
fun passStringToC() {
    val str = "This is a Kotlin string"
    pass_string(str.cstr)
}
```

Passing a Kotlin string to C is straightforward, thanks to the `String.cstr` [extension property](extensions.md#extension-properties).
There is also the `String.wcstr` property for cases that involve UTF-16 characters.

## Read C strings in Kotlin

Now take a returned `char *` from the `return_string()` function and turn it into a Kotlin string:

```kotlin
import interop.*
import kotlinx.cinterop.ExperimentalForeignApi
import kotlinx.cinterop.toKString

@OptIn(ExperimentalForeignApi::class)
fun passStringToC() {
    val stringFromC = return_string()?.toKString()

    println("Returned from C: $stringFromC")
}
```

Here, the [`.toKString()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlinx.cinterop/to-k-string.html) extension
function converts a C string returned from the `return_string()` function into a Kotlin string.

Kotlin provides several extension functions for converting C `char *` strings into Kotlin strings,
depending on the encoding:

```kotlin
fun CPointer<ByteVarOf<Byte>>.toKString(): String // Standard function for UTF-8 strings
fun CPointer<ByteVarOf<Byte>>.toKStringFromUtf8(): String // Explicitly converts UTF-8 strings
fun CPointer<ShortVarOf<Short>>.toKStringFromUtf16(): String // Converts UTF-16 encoded strings
fun CPointer<IntVarOf<Int>>.toKStringFromUtf32(): String // Converts UTF-32 encoded strings
```

## Receive C string bytes from Kotlin

This time, use the `copy_string()` C function to write a C string to a given buffer. It takes two arguments:
a pointer to the memory location where the string should be written and the allowed buffer size.

The function should also return something to indicate if it has succeeded or failed. Let's assume `0` means it succeeded,
and the supplied buffer was big enough:

```kotlin
import interop.*
import kotlinx.cinterop.ExperimentalForeignApi
import kotlinx.cinterop.addressOf
import kotlinx.cinterop.usePinned

@OptIn(ExperimentalForeignApi::class)
fun sendString() {
    val buf = ByteArray(255)
    buf.usePinned { pinned ->
        if (copy_string(pinned.addressOf(0), buf.size - 1) != 0) {
            throw Error("Failed to read string from C")
        }
    }

    val copiedStringFromC = buf.decodeToString()
    println("Message from C: $copiedStringFromC")
}
```

Here, a native pointer is passed to the C function first. The [`.usePinned()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlinx.cinterop/use-pinned.html)
extension function temporarily pins the native memory address of the byte array. The C function fills in the byte array with data.
Another extension function, `ByteArray.decodeToString()`, turns the byte array into a Kotlin string, assuming UTF-8 encoding. 

## Update Kotlin code

Now that you've learned how to use C declarations in Kotlin code, try to use them in your project.
The code in the final `hello.kt` file may look like this:
 
```kotlin
import interop.*
import kotlinx.cinterop.*

@OptIn(ExperimentalForeignApi::class)
fun main() {
    println("Hello Kotlin/Native!")

    val str = "This is a Kotlin string"
    pass_string(str.cstr)

    val useMe = return_string()?.toKString() ?: error("null pointer returned")
    println(useMe)

    val copyFromC = ByteArray(255).usePinned { pinned ->
        val useMe2 = copy_string(pinned.addressOf(0), pinned.get().size - 1)
        if (useMe2 != 0) throw Error("Failed to read a string from C")
        pinned.get().decodeToString()
    }

    println(copyFromC)
}
```

To verify that everything works as expected, run the `runDebugExecutableNative` Gradle task [in your IDE](native-get-started.md)
or use the following command to run the code:

```bash
./gradlew runDebugExecutableNative
```

## What's next

Learn more in the [Interoperability with C](native-c-interop.md) documentation that covers more advanced scenarios.