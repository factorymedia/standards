# Factory Media CoffeeScript Style Guide

It was inspired by [polarmobile's CoffeeScript Style Guide](https://github.com/polarmobile/coffeescript-style-guide)

## Table of Contents

  1. [Whitespace](#types)
  1. [Maximum Line Length](#references)
  1. [Blank lines](#objects)
  1. [Remove Trailing Spaces](#arrays)
  1. [Optional Commas](#destructuring)
  1. [Encoding](#strings)
  1. [Module Imports](#functions)
  1. [Comments](#arrow-functions)
  1. [Naming Conventions](#constructors)
  1. [Functions](#modules)
  1. [Strings](#iterators-and-generators)
  1. [Conditionals](#properties)
  1. [Looping and Comprehensions](#variables)
  1. [Extending Native Objects](#hoisting)
  1. [Exceptions](#comparison-operators--equality)
  1. [Annotations](#blocks)
  1. [Miscellaneous](#comments)

## Whitespace

  - [1.1](#1.1)<a name='1.1'></a> Use soft tabs set to 2 spaces. Never mix tabs and spaces
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

  - [1.2](#1.2)<a name='1.2'></a> Avoid extraneous whitespace in the following situations:
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
    console.log x, y

    # bad
    console.log x , y
    ```
  - [1.3](#1.3)<a name='1.3'></a> Always surround these binary operators with a `single space` on either side:

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

  - [2.1](#2.1)<a name='2.1'></a> Limit all lines to a maximum of 80 characters

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

  - [7.1](#7.1)<a name='7.1'></a> All require statements should be placed on separate lines in the following order:
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

**[⬆ back to top](#table-of-contents)**

## Functions

**[⬆ back to top](#table-of-contents)**

## Strings

**[⬆ back to top](#table-of-contents)**

## Conditionals

**[⬆ back to top](#table-of-contents)**

## Looping and Comprehensions

**[⬆ back to top](#table-of-contents)**

## Extending Native Objects

**[⬆ back to top](#table-of-contents)**

## Exceptions

**[⬆ back to top](#table-of-contents)**

## Annotations

**[⬆ back to top](#table-of-contents)**

## Miscellaneous

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
