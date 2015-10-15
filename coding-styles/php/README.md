# Factorymedia PHP Style Guide


## Table of Contents

  1. [Single and Double Quotes](#single-and-double-quotes)
  1. [Indentation](#indentation)
  1. [Brace Style](#brace-style)
  1. [Regular Expressions](#regular-expressions)  
  1. [No Shorthand PHP Tags](#no-shorthand-php-tags)
  1. [Remove Trailing Spaces](#remove-trailing-spaces)
  1. [Space Usage](#space-usage)
  1. [Formatting SQL statements](#formatting-sql-statements)
  1. [Naming Conventions](#naming-conventions)
  1. [Self-Explanatory Flag Values for Function Arguments](#self-explanatory-flag-values-for-function-arguments)
  1. [Ternary Operator](#ternary-operator)
  1. [Yoda Conditions](#yoda-conditions)
  1. [Clever Code](#clever-code)
  1. [Error Control Operator](#error-control-operator)
  1. [Don’t extract](#dont-extract)


## Single and Double Quotes

Use single and double quotes if appropriate. However, use single quotes as much as possible, unless you’re evaluating anything in the string. You should almost never have to escape quotes in a string, because you can just alternate your quoting style, like so:

```
echo '<a href="/static/link" title="Yeah yeah!">Link name</a>';
echo "<a href='$link' title='$linktitle'>$linkname</a>";
```
Text that goes into attributes should be run through `esc_attr()` so that single or double quotes do not end the attribute value and invalidate the HTML and cause a security issue. See Data Validation in the Codex for further details.

**[⬆ back to top](#factorymedia-php-style-guide)**

## Indentation

Your indentation should always reflect logical structure. Use real tabs and not spaces, as this allows the most flexibility across clients.

Exception: if you have a block of code that would be more readable if things are aligned, use spaces:
```
[tab]$foo   = 'somevalue';
[tab]$foo2  = 'somevalue2';
[tab]$foo34 = 'somevalue3';
[tab]$foo5  = 'somevalue4';
```

For associative arrays, values should start on a new line. Note the comma after the last array item: this is recommended because it makes it easier to change the order of the array, and makes for cleaner diffs when new items are added.
```
$my_array = array(
[tab]'foo'   => 'somevalue',
[tab]'foo2'  => 'somevalue2',
[tab]'foo3'  => 'somevalue3',
[tab]'foo34' => 'somevalue3',
);
```
Rule of thumb: Tabs should be used at the beginning of the line for indentation, while spaces can be used mid-line for alignment.

**[⬆ back to top](#factorymedia-php-style-guide)**

## Brace Style

Braces shall be used for all blocks in the style shown here:

```
if ( condition ) {
    action1();
    action2();
} elseif ( condition2 && condition3 ) {
    action3();
    action4();
} else {
    defaultaction();
}
```

Furthermore, if you have a really long block, consider whether it can be broken into two or more shorter blocks or functions. If you consider such a long block unavoidable, please put a short comment at the end so people can tell at glance what that ending brace ends – typically this is appropriate for a logic block, longer than about 35 rows, but any code that’s not intuitively obvious can be commented.

Braces should always be used, even when they are not required:

```
if ( condition ) {
    action0();
}

if ( condition ) {
    action1();
} elseif ( condition2 ) {
    action2a();
    action2b();
}

foreach ( $items as $item ) {
    process_item( $item );
}
```
Note that requiring the use of braces just means that single-statement inline control structures are prohibited. You are free to use the alternative syntax for control structures (e.g. `if`/`endif`, `while`/`endwhile`)—especially in your templates where PHP code is embedded within HTML, for instance:

```
<?php if ( have_posts() ) : ?>
    <div class='hfeed'>
        <?php while ( have_posts() ) : the_post(); ?>
            <article id="post-<?php the_ID() ?>" class="<?php post_class() ?>">
                <!-- ... -->
            </article>
        <?php endwhile; ?>
    </div>
<?php endif; ?>
```

**[⬆ back to top](#factorymedia-php-style-guide)**

## Regular Expressions

Perl compatible regular expressions (PCRE, `preg_` functions) should be used in preference to their POSIX counterparts. Never use the `/e` switch, use `preg_replace_callback` instead.

It’s most convenient to use single-quoted strings for regular expressions since, contrary to double-quoted strings, they have only two metasequences: `\'` and `\\`.

**[⬆ back to top](#factorymedia-php-style-guide)**

## No Shorthand PHP Tags

Important: Never use shorthand PHP start tags. Always use full PHP tags.

Correct:

```
<?php ... ?>
<?php echo $var; ?>
```

Incorrect:

```
<? ... ?>
<?= $var ?>
```

**[⬆ back to top](#factorymedia-php-style-guide)**

## Remove Trailing Spaces

Remove trailing whitespace at the end of each line of code. Omitting the closing PHP tag at the end of a file is preferred. If you use the tag, make sure you remove trailing whitespace.

**[⬆ back to top](#factorymedia-php-style-guide)**

## Space Usage

Always put spaces after commas, and on both sides of logical, comparison, string and assignment operators.

```
x == 23
foo && bar
! foo
array( 1, 2, 3 )
$baz . '-5'
$term .= 'X'
```
Put spaces on both sides of the opening and closing parenthesis of `if`, `elseif`, `foreach`, `for`, and `switch` blocks.

```
foreach ( $foo as $bar ) { ...
```

When defining a function, do it like so:

```
function my_function( $param1 = 'foo', $param2 = 'bar' ) { ...
```

When calling a function, do it like so:

```
my_function( $param1, func_param( $param2 ) );
```

When performing logical comparisons, do it like so:

```
if ( ! $foo ) { ...
```

When type casting, do it like so:

```
foreach ( (array) $foo as $bar ) { ...

$foo = (boolean) $bar;
```

When referring to array items, only include a space around the index if it is a variable, for example:

```
$x = $foo['bar']; // correct
$x = $foo[ 'bar' ]; // incorrect

$x = $foo[0]; // correct
$x = $foo[ 0 ]; // incorrect

$x = $foo[ $bar ]; // correct
$x = $foo[$bar]; // incorrect
```

**[⬆ back to top](#factorymedia-php-style-guide)**

## Formatting SQL statements

When formatting SQL statements you may break it into several lines and indent if it is sufficiently complex to warrant it. Most statements work well as one line though. Always capitalize the SQL parts of the statement like `UPDATE` or `WHERE`.

Functions that update the database should expect their parameters to lack SQL slash escaping when passed. Escaping should be done as close to the time of the query as possible, preferably by using `$wpdb->prepare()`

`$wpdb->prepare()` is a method that handles escaping, quoting, and int-casting for SQL queries. It uses a subset of the `sprintf()` style of formatting. Example :

```
$var = "dangerous'"; // raw data that may or may not need to be escaped
$id = some_foo_number(); // data we expect to be an integer, but we're not certain

$wpdb->query( $wpdb->prepare( "UPDATE $wpdb->posts SET post_title = %s WHERE ID = %d", $var, $id ) );
```

`%s` is used for string placeholders and %d is used for integer placeholders. Note that they are not ‘quoted’! `$wpdb->prepare()` will take care of escaping and quoting for us. The benefit of this is that we don’t have to remember to manually use `esc_sql()`, and also that it is easy to see at a glance whether something has been escaped or not, because it happens right when the query happens.

**[⬆ back to top](#factorymedia-php-style-guide)**

## Naming Conventions

Use lowercase letters in variable, action, and function names (never `camelCase`). Separate words via underscores. Don’t abbreviate variable names un-necessarily; let the code be unambiguous and self-documenting.

```
function some_name( $some_variable ) { [...] }
```

Class names should use capitalized words separated by underscores. Any acronyms should be all upper case.

```
class Walker_Category extends Walker { [...] }
class WP_HTTP { [...] }
```

Constants should be in all upper-case with underscores separating words:

```
define( 'DOING_AJAX', true );
```

Files should be named descriptively using lowercase letters. Hyphens should separate words.

```
my-plugin-name.php
```

Class file names should be based on the class name with `class-` prepended and the underscores in the class name replaced with hyphens, for example `WP_Error` becomes:

```
class-wp-error.php
```

This file-naming standard is for all current and new files with classes. There is one exception for three files that contain code that got ported into BackPress: class.wp-dependencies.php, class.wp-scripts.php, class.wp-styles.php. Those files are prepended with class., a dot after the word class instead of a hyphen.

Files containing template tags in `wp-includes` should have `-template` appended to the end of the name so that they are obvious.

```
general-template.php
```

**[⬆ back to top](#factorymedia-php-style-guide)**

## Self-Explanatory Flag Values for Function Arguments

Prefer string values to just 'true' and 'false' when calling functions.

```
// Incorrect
function eat( $what, $slowly = true ) {
...
}
eat( 'mushrooms' );
eat( 'mushrooms', true ); // what does true mean?
eat( 'dogfood', false ); // what does false mean? The opposite of true?
```

Since PHP doesn’t support named arguments, the values of the flags are meaningless, and each time we come across a function call like the examples above, we have to search for the function definition. The code can be made more readable by using descriptive string values, instead of booleans.

```
// Correct
function eat( $what, $speed = 'slowly' ) {
...
}
eat( 'mushrooms' );
eat( 'mushrooms', 'slowly' );
eat( 'dogfood', 'quickly' );
```

When more words are needed to describe the function parameters, an '$args' array may be a better pattern.

```
// Even Better
function eat( $what, $args ) {
...
}
eat ( 'noodles', array( 'speed' => 'moderate' ) );
```

**[⬆ back to top](#factorymedia-php-style-guide)**

## Ternary Operator

Ternary operators are fine, but always have them test if the statement is true, not false. Otherwise, it just gets confusing. (An exception would be using '! empty()', as testing for false here is generally more intuitive.)

For example:

```
// (if statement is true) ? (do this) : (else, do this);
$musictype = ( 'jazz' == $music ) ? 'cool' : 'blah';
// (if field is not empty ) ? (do this) : (else, do this);
```

**[⬆ back to top](#factorymedia-php-style-guide)**

## Yoda Conditions

```
if ( true == $the_force ) {
    $victorious = you_will( $be );
}
```
When doing logical comparisons, always put the variable on the right side, constants or literals on the left.

In the above example, if you omit an equals sign (admit it, it happens even to the most seasoned of us), you’ll get a parse error, because you can’t assign to a constant like 'true'. If the statement were the other way around '( $the_force = true )', the assignment would be perfectly valid, returning '1', causing the if statement to evaluate to 'true', and you could be chasing that bug for a while.

A little bizarre, it is, to read. Get used to it, you will.

This applies to ==, !=, ===, and !==. Yoda conditions for <, >, <= or >= are significantly more difficult to read and are best avoided.

**[⬆ back to top](#factorymedia-php-style-guide)**

## Clever Code

In general, readability is more important than cleverness or brevity.
```
isset( $var ) || $var = some_function();
```

Although the above line is clever, it takes a while to grok if you’re not familiar with it. So, just write it like this:

```
if ( ! isset( $var ) ) {
    $var = some_function();
}
```

**[⬆ back to top](#factorymedia-php-style-guide)**

## Error Control Operator

As noted in the [PHP docs](http://www.php.net//manual/en/language.operators.errorcontrol.php):
```
PHP supports one error control operator: the at sign (@). When prepended to an expression in PHP, any error messages that might be generated by that expression will be ignored.
```

While this operator does exist in Core, it is often used lazily instead of doing proper error checking. Its use is highly discouraged, as even the PHP docs also state:
```
Warning: Currently the “@” error-control operator prefix will even disable error reporting for critical errors that will terminate script execution. Among other things, this means that if you use “@” to suppress errors from a certain function and either it isn’t available or has been mistyped, the script will die right there with no indication as to why.
```

**[⬆ back to top](#factorymedia-php-style-guide)**

## Don’t extract()

Per [#22400](https://core.trac.wordpress.org/ticket/22400):
```
'extract()' is a terrible function that makes code harder to debug and harder to understand. We should discourage it’s use and remove all of our uses of it.

Joseph Scott has [a good write-up of why it’s bad](https://josephscott.org/archives/2009/02/i-dont-like-phps-extract-function/).
```

**[⬆ back to top](#factorymedia-php-style-guide)**

## PHP Coding Standards

- [Reference](https://make.wordpress.org/core/handbook/best-practices/coding-standards/php/)

## License

(The MIT License)

Copyright (c) 2015 Factorymedia Ltd

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

**[⬆ back to top](#factorymedia-php-style-guide)**
