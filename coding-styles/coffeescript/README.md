# Factory Media CoffeeScript Style Guide

It was inspired by [polarmobile's CoffeeScript Style Guide](https://github.com/polarmobile/coffeescript-style-guide)

## Table of Contents

  1. [Whitespace](#whitespace)
  1. [Maximum Line Length](#maximum-line-length)
  1. [Blank lines](#blank-lines)
  1. [Remove Trailing Spaces](#remove-trailing-spaces)
  1. [Optional Commas](#optional-commas)
  1. [Encoding](#encoding)
  1. [Module Imports](#module-imports)
  1. [Comments](#comments)
  1. [Naming Conventions](#naming-conventions)
  1. [Functions](#functions)
  1. [Strings](#strings)
  1. [Conditionals](#conditionals)
  1. [Looping and Comprehensions](#looping-and-comprehensions)
  1. [Extending Native Objects](#extending-native-objects)
  1. [Exceptions](#exceptions)
  1. [Annotations](#annotations)
  1. [Miscellaneous](#miscellaneous)

## Whitespace

  - [1.1](#1.1)<a name='1.1'></a> Use soft tabs set to 2 spaces. Never mix tabs and spaces.
    ```coffeescript
    # bad
    if yes
    ∙∙∙∙...

    # bad
    if yes
    ∙∙∙∙...
    ∙∙...
    }

    # good
    if yes
    ∙∙...
    }
    ```

  - [1.2](#1.2)<a name='1.2'></a> Avoid extraneous whitespace in the following situations.
  > Immediately inside parentheses, brackets or braces

    ```coffeescript
    # good
    ($ 'body')

    # bad
    ( $ 'body' )
    ```

  > Immediately before a comma

    ```coffeescript
    # good
    console.log(x, y)

    # bad
    console.log(x , y)
    ```
  - [1.3](#1.3)<a name='1.3'></a> Always surround these binary operators with a `single space` on either side.

  > assignment: =

  > Note that this also applies when indicating default parameter value(s) in a function declaration

    ```coffeescript
    # good
    test: (param = null) ->

    # bad
    test: (param=null) ->
    ```
  > augmented assignment: +=, -=, etc.

    ```coffeescript
    # good
    num += 1 ->

    # good
    num -= 1 ->

    # bad
    num+=1 ->
    ```

  > comparisons: ==, <, >, <=, >=, unless, etc.

    ```coffeescript
    # good
    if (day == bingoDay) {

    # good
    if (day >= bingoDay) {

    # bad
    if (day>=bingoDay) {
    ```

  > arithmetic operators: +, -, *, /, etc.

    ```coffeescript
    # good
    num + 1

    # bad
    num+1
    ```

  > (Do not use more than one space around these operators)

    ```coffeescript
    # good
    x = 1
    y = 1
    fooBar = 3

    # bad
    x      = 1
    y      = 1
    fooBar = 3
    ```

**[⬆ back to top](#table-of-contents)**

## Maximum Line Length

  - [2.1](#2.1)<a name='2.1'></a> Limit all lines to a maximum of 80 characters.

**[⬆ back to top](#table-of-contents)**

## Blank lines

  - [3.1](#3.1)<a name='3.1'></a> Separate top-level function and class definitions with a single blank line.
    ```coffeescript
    # bad
    class Animal
      move: (meters) ->
        .........

    # good
    class Animal

      move: (meters) ->
        .........
    ```

  - [3.2](#3.2)<a name='3.2'></a> Separate method definitions inside of a class with a single blank line.
    ```coffeescript
    # bad
    class Animal

      move: (meters) ->
        .........
      alert: (message) ->
        .........

    # good
    class Animal

      move: (meters) ->
        .........

      alert: (message) ->
        .........
    ```

**[⬆ back to top](#table-of-contents)**

## Remove Trailing Spaces

  - [4.1](#4.1)<a name='4.1'></a> Remove trailing whitespace at the end of each line of code.

**[⬆ back to top](#table-of-contents)**

## Optional Commas

  - [5.1](#5.1)<a name='5.1'></a> Avoid the use of commas before newlines when properties or elements of an Object or Array are listed on separate lines.
    ```coffeescript
    # bad
    foo = [
      'something',
      'string',
      'value'
    ]
    bar:
      label: 'test',
      key: value

    # good
    foo = [
      'something'
      'string'
      'value'
    ]
    bar:
      label: 'test'
      key: value
    ```

**[⬆ back to top](#table-of-contents)**

## Encoding

  - [6.1](#6.1)<a name='6.1'></a> UTF-8 is the preferred source file encoding.

**[⬆ back to top](#table-of-contents)**

## Module Imports

  - [7.1](#7.1)<a name='7.1'></a> All require statements should be placed on separate lines in the following order.
  > - Standard library imports (if a standard library exists)
  > - Third party library imports
  > - Local imports (imports specific to this application or library)

    ```coffeescript
    # bad
    require './my_local_class'
    require 'backbone'
    require 'lib/setup'

    # good
    require 'lib/setup'
    require 'backbone'
    require './my_local_class'
    ```

**[⬆ back to top](#table-of-contents)**

## Comments

  - [8.1](#8.1)<a name='8.1'></a> If modifying code that is described by an existing comment, update the comment such that it accurately reflects the new code. (Ideally, improve the code to obviate the need for the comment, and delete the comment entirely.)

  > The first word of the comment should be capitalized, unless the first word is an identifier that begins with a lower-case letter.

    ```coffeescript
    # bad
    # this is a comment.

    # good
    # This is a comment.

    abc_def = 1
    # good
    # abc_def should be removed later.
    ```

  > If a comment is short, the period at the end can be omitted.

    ```coffeescript    
    # good
    # Good
    # Bad    
    ```

  - [8.2](#8.2)<a name='8.2'></a> Block comments apply to the block of code that follows them.

  > Each line of a block comment starts with a # and a single space, and should be indented at the same level of the code that it describes.

    ```coffeescript
    # good
    # This is a block comment. Note that if this were a real block
    # comment, we would actually be describing the proceeding code.
    #
    init()
    ```

  > Paragraphs inside of block comments are separated by a line containing a single #.

    ```coffeescript
    # good
    # This is a block comment. Note that if this were a real block
    # comment, we would actually be describing the proceeding code.
    #
    # This is the second paragraph of the same block comment. Note
    ```

  - [8.3](#8.3)<a name='8.3'></a> Inline comments are placed on the line immediately above the statement that they are describing. If the inline comment is sufficiently short, it can be placed on the same line as the statement (separated by a single space from the end of the statement).

  > All inline comments should start with a # and a single space.

    ```coffeescript
    # good
    x = x + magic_number # Use magic number here

    # bad
    x = x + magic_number #Use magic number here
    ```

  > The use of inline comments should be limited, because their existence is typically a sign of a code smell.

    ```coffeescript
    # good
    x = x * magic_number # Use magic number here

    # bad
    x = x + magic_number # Use magic number here because x is blah blah blah blah, remove it later this line.
    ```

  > Do not use inline comments when they state the obvious

    ```coffeescript
    # good
    x = x + 1

    # bad
    x = x + 1 # Increment x
    ```

  > However, inline comments can be useful in certain scenarios

    ```coffeescript
    # good
    x = x + 1 # Compensate for border
    ```

**[⬆ back to top](#table-of-contents)**

## Naming Conventions

  - [9.1](#9.1)<a name='9.1'></a> Use camelCase (with a leading lowercase character) to name all variables, methods, and object properties.

    ```coffeescript
    # bad
    Foo = 0

    # bad
    Bar: (BeerBottle) ->

    # bad
    Object:
      Key: 'value'

    # good
    foo = 0

    # good
    bar: (beerBottle) ->

    #good
    object:
      key: 'value'
    ```

  - [9.2](#9.2)<a name='9.2'></a> Use CamelCase (with a leading uppercase character) to name all classes. (This style is also commonly referred to as PascalCase, CamelCaps, or CapWords, among [other alternatives](https://en.wikipedia.org/wiki/CamelCase#Variations_and_synonyms).)

    ```coffeescript
    # bad
    class animal

    # good
    class Animal
    ```

  > The official CoffeeScript convention is camelcase, because this simplifies interoperability with JavaScript. For more on this decision, see [here](https://github.com/jashkenas/coffeescript/issues/425).)

  - [9.3](#9.3)<a name='9.3'></a> For constants, use all uppercase with underscores.

    ```coffeescript
    # bad
    constant_like_this

    # bad
    CONTANTLIKETHIS

    # worst :(
    Constant_Like_This

    # good
    CONSTANT_LIKE_THIS
    ```

  - [9.4](#9.4)<a name='9.4'></a> Methods and variables that are intended to be "private" should begin with a leading underscore.

    ```coffeescript
    # bad
    privateMethod: ->

    # good
    _privateMethod: ->
    ```

**[⬆ back to top](#table-of-contents)**

## Functions

> These guidelines also apply to the methods of a class

  - [10.1](#10.1)<a name='10.1'></a> When declaring a function that takes arguments, always use a single space after the closing parenthesis of the arguments list.

    ```coffeescript
    # bad
    foo = (arg1, arg2)->

    # good
    foo = (arg1, arg2) ->
    ```

  - [10.2](#10.2)<a name='10.2'></a> Do not use parentheses when declaring functions that take no arguments.

    ```coffeescript
    # bad
    bar = () ->

    # good
    bar = ->
    ```

  - [10.3](#10.3)<a name='10.3'></a> In cases where method calls are being chained and the code does not fit on a single line, each call should be placed on a separate line and indented by one level (i.e., two spaces), with a leading `.`.

    ```coffeescript
    # bad
    [1..3].map((x) -> x * x).concat([10..12])
      .filter((x) -> x < 11)
      .reduce((x, y) -> x + y)

    # bad
    [1..3].map((x) -> x * x)
        .filter((x) -> x < 11)
        .reduce((x, y) -> x + y)

    # bad
    [1..3].
      map((x) -> x * x).
      filter((x) -> x < 11).
      reduce((x, y) -> x + y)

    # good
    [1..3]
      .map((x) -> x * x)
      .concat([10..12])
      .filter((x) -> x < 11)
      .reduce((x, y) -> x + y)
    ```

  - [10.4](#10.4)<a name='10.4'></a> When calling functions, always include parentheses in such a way that optimizes for readability.

    ```coffeescript
    # bad
    baz 12

    brush.ellipse x: 10, y: 20

    print inspect value

    # good
    baz(12)

    brush.ellipse(x: 10, y: 20)

    print(inspect(value))

    foo(4).bar(8)

    obj.value(10, 20) / obj.value(20, 10)

    new Tag(new Value(a, b), new Arg(c))
    ```

  - [10.5](#10.5)<a name='10.5'></a> You will sometimes see parentheses used to group functions (instead of being used to group function parameters). Examples of using this style (hereafter referred to as the "function grouping style").

    ```coffeescript
    # bad
    ($ '#selektor').addClass('klass')

    # bad
    (foo 4).bar(8)

    # good
    $('#selektor').addClass('klass')

    # good
    foo(4).bar(8)
    ```

**[⬆ back to top](#table-of-contents)**

## Strings

  - [11.1](#11.1)<a name='11.1'></a> Use string interpolation instead of string concatenation.

    ```coffeescript    
    # bad
    "this is an " + adjective + " string"

    # good
    "this is an #{adjective} string"
    ```

  - [11.2](#11.2)<a name='11.2'></a> Prefer single quoted strings ('') instead of double quoted ("") strings, unless features like string interpolation are being used for the given string.

    ```coffeescript
    # bad
    "this is a string"

    # good
    'this is a string'

    # good
    "this is an #{adjective} string"
    ```

**[⬆ back to top](#table-of-contents)**

## Conditionals

  - [12.1](#12.1)<a name='12.1'></a> Use unless over if for negative conditions.

    ```coffeescript    
    # bad
    if false

    # good
    unless true
    ```

  - [12.2](#12.2)<a name='12.2'></a> Instead of using unless...else, use if...else.

    ```coffeescript    
    # bad
    unless true
      ...
    else
      ...

    # good
    if false
      ...
    else
      ...
    ```

  - [12.3](#12.3)<a name='12.3'></a> Multi-line if/else clauses should use indentation.

    ```coffeescript    
    # bad
    if true then ...      
    else ...

    # good
    if true
      ...
    else
      ...
    ```

**[⬆ back to top](#table-of-contents)**

## Looping and Comprehensions

  - [13.1](#13.1)<a name='13.1'></a> Take advantage of comprehensions whenever possible.

    ```coffeescript    
    # bad
    results = []
    for item in array
      results.push item.name

    # good
    result = (item.name for item in array)
    ```

  - [13.2](#13.2)<a name='13.2'></a> To filter

    ```coffeescript    
    # good
    result = (item for item in array when item.name is "test")
    ```

  - [13.3](#13.3)<a name='13.3'></a> To iterate over the keys and values of objects.

    ```coffeescript        
    # good
    object = (one: 1, two: 2)
    alert("#{key} = #{value}") for key, value of object
    ```

**[⬆ back to top](#table-of-contents)**

## Extending Native Objects

  - [14.1](#14.1)<a name='14.1'></a> Do not modify native objects.

**[⬆ back to top](#table-of-contents)**

## Exceptions

  - [15.1](#15.1)<a name='15.1'></a> Do not suppress exceptions.

**[⬆ back to top](#table-of-contents)**

## Annotations

  - [16.1](#16.1)<a name='16.1'></a> Use annotations when necessary to describe a specific action that must be taken against the indicated block of code.
  > Write the annotation on the line immediately above the code that the annotation is describing.

  > The annotation keyword should be followed by a colon and a space, and a descriptive note.

    ```coffeescript
    # FIXME: The client's current state should *not* affect payload processing.
    resetClientState()
    processPayload()
    ```

  > If multiple lines are required by the description, indent subsequent lines with two spaces:

    ```coffeescript
    # TODO: Ensure that the value returned by this call falls within a certain
    #   range, or throw an exception.
    analyze()  
    ```

  - [16.2](#16.2)<a name='16.2'></a> Annotation types
    - `TODO`: describe missing functionality that should be added at a later date
    - `FIXME`: describe broken code that must be fixed
    - `OPTIMIZE`: describe code that is inefficient and may become a bottleneck
    - `HACK`: describe the use of a questionable (or ingenious) coding practice
    - `REVIEW`: describe code that should be reviewed to confirm implementation

  - [16.3](#16.3)<a name='16.3'></a> If a custom annotation is required, the annotation should be documented in the project's README.

**[⬆ back to top](#table-of-contents)**

## Miscellaneous

  - [17.1](#17.1)<a name='17.1'></a> and is preferred over &&.

    ```coffeescript
    # bad
    if a && b

    # good
    if a and b
    ```

  - [17.2](#17.2)<a name='17.2'></a> or is preferred over ||.

    ```coffeescript
    # bad
    if a && b

    # good
    if a and b
    ```

  - [17.3](#17.3)<a name='17.3'></a> is is preferred over ==.

    ```coffeescript
    # bad
    if a == b

    # good
    if a is b
    ```

  - [17.4](#17.4)<a name='17.4'></a> isnt is preferred over !=.

    ```coffeescript
    # bad
    if a != b

    # good
    if a isnt b
    ```

  - [17.5](#17.5)<a name='17.5'></a> not is preferred over !.

    ```coffeescript
    # bad
    if not b

    # good
    if !b
    ```

  - [17.6](#17.6)<a name='17.6'></a> or= should be used when possible.

    ```coffeescript
    # bad
    temp = temp || {}

    # good
    temp or= {}
    ```

  - [17.7](#17.7)<a name='17.7'></a> Prefer shorthand notation (::) for accessing an object's prototype.

    ```coffeescript
    # bad
    Array.prototype.slice

    # good
    Array::slice
    ```

  - [17.8](#17.8)<a name='17.8'></a> Prefer @property over this.property.

    ```coffeescript
    # bad
    return this.property

    # good
    return @property
    ```

  - [17.9](#17.9)<a name='17.9'></a> However, avoid the use of standalone @.

    ```coffeescript
    # bad
    return @

    # good
    return this
    ```

  - [17.10](#17.10)<a name='17.10'></a> Avoid return where not required, unless the explicit return increases clarity.

  - [17.11](#17.11)<a name='17.11'></a> Use splats (...) when working with functions that accept variable numbers of arguments.

    ```coffeescript
    # good
    (a, b, c, rest...)

    # good
    console.log(args...)
    ```

**[⬆ back to top](#table-of-contents)**

## License

(The MIT License)

Copyright (c) 2015 Factory Media Ltd

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ back to top](#table-of-contents)**
