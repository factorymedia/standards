# Factorymedia PHP Style Guide


## Single and Double Quotes

Use always single over double quotes if possible. If you’re not evaluating anything in the string, use single quotes. You should almost never have to escape quotes in a string, because you can just alternate your quoting style, like so:

```
echo '<a href="/static/link" title="Yeah yeah!">Link name</a>';
echo "<a href='$link' title='$linktitle'>$linkname</a>";
```
Text that goes into attributes should be run through `esc_attr()` so that single or double quotes do not end the attribute value and invalidate the HTML and cause a security issue. See Data Validation in the Codex for further details.

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

## Regular Expressions

Perl compatible regular expressions (PCRE, `preg_` functions) should be used in preference to their POSIX counterparts. Never use the `/e` switch, use `preg_replace_callback` instead.

It’s most convenient to use single-quoted strings for regular expressions since, contrary to double-quoted strings, they have only two metasequences: `\'` and `\\`.

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

## Remove Trailing Spaces

Remove trailing whitespace at the end of each line of code. Omitting the closing PHP tag at the end of a file is preferred. If you use the tag, make sure you remove trailing whitespace.

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

## Database Queries

## Naming Conventions

## Self-Explanatory Flag Values for Function Arguments

## Ternary Operator

## Yoda Conditions

## Clever Code

## Error Control Operator

## Don’t extract()

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
