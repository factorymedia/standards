# Factory Media Ruby Style Guide

Inspired by [Bozhidar Batsov Guide](https://github.com/bbatsov/ruby-style-guide), [Github's Guide](https://github.com/styleguide/ruby) and [Airbnb Ruby Style Guide](https://github.com/airbnb/ruby)

## Table of Contents

  1. [Whitespace](#whitespace)
  1. [Line Length](#line-length)
  1. [Encoding](#encoding)
  1. [Comments](#comments)
  1. [Methods](#methods)
  1. [Conditional Expressions](#conditional-expressions)
  1. [Syntax](#syntax)
  1. [Naming](#naming)
  1. [Classes](#classes)
  1. [Exceptions](#exceptions)
  1. [Collections](#collections)
  1. [Strings](#strings)
  1. [Misc](#misc)

## Whitespace

  - [1.1](#1.1) <a name='1.1'></a> Use two spaces per indentation level (aka soft tabs). No hard tabs.

    ```ruby
    # bad - four spaces
    def some_method
      do_something
    end

    # good
    def some_method
      do_something
    end
    ```

  - [1.2](#1.2) <a name='1.2'></a> Indent when as deep as case. This is the style established in both "The Ruby Programming Language" and "Programming Ruby".

    ```ruby
    # bad
    case
      when song.name == 'Misty'
        puts 'Not again!'
      when song.duration > 120
        puts 'Too long!'
      when Time.now.hour > 21
        puts "It's too late"
      else
        song.play
    end

    # good
    case
    when song.name == 'Misty'
      puts 'Not again!'
    when song.duration > 120
      puts 'Too long!'
    when Time.now.hour > 21
      puts "It's too late"
    else
      song.play
    end
    ```

  - [1.3](#1.3) <a name='1.3'></a> When assigning the result of a conditional expression to a variable, preserve the usual alignment of its branches.

    ```ruby
    # bad - pretty convoluted
    kind = case year
    when 1850..1889 then 'Blues'
    when 1890..1909 then 'Ragtime'
    when 1910..1929 then 'New Orleans Jazz'
    when 1930..1939 then 'Swing'
    when 1940..1950 then 'Bebop'
    else 'Jazz'
    end

    result = if some_cond
      calc_something
    else
      calc_something_else
    end

    # good - it's apparent what's going on
    kind = case year
           when 1850..1889 then 'Blues'
           when 1890..1909 then 'Ragtime'
           when 1910..1929 then 'New Orleans Jazz'
           when 1930..1939 then 'Swing'
           when 1940..1950 then 'Bebop'
           else 'Jazz'
           end

    result = if some_cond
               calc_something
             else
               calc_something_else
             end

    # good (and a bit more width efficient)
    kind =
      case year
      when 1850..1889 then 'Blues'
      when 1890..1909 then 'Ragtime'
      when 1910..1929 then 'New Orleans Jazz'
      when 1930..1939 then 'Swing'
      when 1940..1950 then 'Bebop'
      else 'Jazz'
      end

    result =
      if some_cond
        calc_something
      else
        calc_something_else
      end
  ```

  - [1.4](#1.4) <a name='1.4'></a> Indent succeeding lines in multi-line boolean expressions.

    ```ruby
    # good
    def is_eligible?(user)
      Trebuchet.current.launch?(ProgramEligibilityHelper::PROGRAM_TREBUCHET_FLAG) &&
        is_in_program?(user) &&
        program_not_expired
    end

    # bad
    def is_eligible?(user)
      Trebuchet.current.launch?(ProgramEligibilityHelper::PROGRAM_TREBUCHET_FLAG) &&
      is_in_program?(user) &&
      program_not_expired
    end
    ```

  - [1.5](#1.5) <a name='1.5'></a> Do not align function parameters one per line.
    ```ruby
    # bad
    def self.create_translation(phrase_id,
                                phrase_key,
                                target_locale,
                                value,
                                user_id,
                                do_xss_check,
                                allow_verification)
      ...
    end

    # good
    def self.create_translation(phrase_id, phrase_key, target_locale,
                                value, user_id, do_xss_check, allow_verification)
      ...
    end
    ```

  - [1.6](#1.6) <a name='1.6'></a> Never leave trailing whitespace.

  - [1.7](#1.7) <a name='1.7'></a> Use spaces around operators, after commas, colons and semicolons, around `{` and before `}`.

    ```ruby
    sum = 1 + 2
    a, b = 1, 2
    1 > 2 ? true : false; puts 'Hi'
    [1, 2, 3].each { |e| puts e }
    ```

    >> The only exception, regarding operators, is the exponent operator

    ```ruby
    # bad
    e = M * c ** 2

    # good
    e = M * c**2
    ```

  - [1.7](#1.7) <a name='1.7'></a> No spaces after `(`, `[` or before `]`, `)`.

    ```ruby
    # bad
    some( arg ).other
    [ 1, 2, 3 ].size

    # good
    some(arg).other
    [1, 2, 3].size
    ```

  - [1.8](#1.8) <a name='1.8'></a> No space after `!`.

    ```ruby
    # bad
    ! something

    # good
    !something
    ```

  - [1.9](#1.9) <a name='1.9'></a> No space inside range literals.

    ```ruby
    # bad
    1 .. 3
    'a' ... 'z'

    # good
    1..3
    'a'...'z'
    ```

  - [1.10](#1.10) <a name='1.10'></a> Use spaces around the = operator when assigning default values to method parameters

    ```ruby
    # bad
    def some_method(arg1=:default, arg2=nil, arg3=[])
      # do something...
    end

    # good
    def some_method(arg1 = :default, arg2 = nil, arg3 = [])
      # do something...
    end
    ```

  - [1.11](#1.11) <a name='1.11'></a> Align the elements of array literals spanning multiple lines.

    ```ruby
    # bad - single indent
    menu_item = ['Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam',
      'Baked beans', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam']

    # good
    menu_item = [
      'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam',
      'Baked beans', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam'
    ]

    # good
    menu_item =
      ['Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam',
       'Baked beans', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam']
    ```

  - [1.12](#1.12) <a name='1.12'></a> Add a new line after if conditions span multiple lines to help differentiate between the conditions and the body.

    ```ruby
    if @reservation_alteration.checkin == @reservation.start_date &&
      @reservation_alteration.checkout == (@reservation.start_date + @reservation.nights)

      redirect_to_alteration @reservation_alteration
    end
    ```

  - [1.13](#1.13) <a name='1.13'></a> Add a new line after conditionals, blocks, case statements, etc.

    ```ruby
    if robot.is_awesome?
      send_robot_present
    end

    robot.add_trait(:human_like_intelligence)
    ```

**[⬆ back to top](#table-of-contents)**

## Line Length

  - [2.1](#2.1) <a name='2.1'></a> Keep each line of code to a readable length. Unless you have a reason to, keep lines to fewer than 80 characters.

  - [2.2](#2.2) <a name='2.2'> Adopt a consistent multi-line method chaining style. When continuing a chained method invocation on another line keep the `.` on the second line.

    ```ruby
    # bad - need to consult first line to understand second line
    one.two.three.
     four

    # good - it's immediately clear what's going on the second line
    one.two.three
     .four
    ```  

  - [2.3](#2.3) <a name='2.3'> Liberal use of linebreaks inside unclosed `(` `{` `[`

    ```ruby
    scope = Translation::Phrase.includes(:phrase_translations).
    joins(:phrase_screenshots).
    where(:phrase_screenshots => {
      :controller => controller_name,
      :action => JAROMIR_JAGR_SALUTE,
    })
    ```

  - [2.4](#2.4) <a name='2.4'> Composing long strings by putting strings next to each other, separated by a backslash-then-newline.

    ```ruby
    translation = FactoryGirl.create(
      :phrase_translation,
      :locale => :is,
      :phrase => phrase,
      :key => 'phone_number_not_revealed_time_zone',
      :value => 'Símanúmerið þitt verður ekki birt. Það er aðeins hægt að hringja á '\
                'milli 9:00 og 21:00 %{time_zone}.'
    )
    ```

  - [2.5](#2.5) <a name='2.5'> Breaking long logical statements with linebreaks after operators like `&&` and `||`.

    ```ruby
    if @reservation_alteration.checkin == @reservation.start_date &&
      @reservation_alteration.checkout == (@reservation.start_date + @reservation.nights)

      redirect_to_alteration @reservation_alteration
    end
    ```

**[⬆ back to top](#table-of-contents)**

## Encoding

  - [3.1](#3.1) <a name='3.1'> Use UTF-8 as the source file encoding

**[⬆ back to top](#table-of-contents)**

## Comments

> Though a pain to write, comments are absolutely vital to keeping our code
> readable. The following rules describe what you should comment and where. But
> remember: while comments are very important, the best code is
> self-documenting. Giving sensible names to types and variables is much better
> than using obscure names that you must then explain through comments.

> When writing your comments, write for your audience: the next contributor who
> will need to understand your code. Be generous — the next one may be you!

&mdash;[Google C++ Style Guide](http://google-styleguide.googlecode.com/svn/trunk/cppguide.xml)

Portions of this section borrow heavily from the Google
[C++](https://google.github.io/styleguide/cppguide.html) and [Python](https://google.github.io/styleguide/pyguide.html) style guides.

  - [4.1](#4.1) <a name='4.1'> File/class-level comments

    Every class definition should have an accompanying comment that describes what
    it is for and how it should be used.

    A file that contains zero classes or more than one class should have a comment
    at the top describing its contents.

    ```ruby
    # Automatic conversion of one locale to another where it is possible, like
    # American to British English.
    module Translation
      # Class for converting between text between similar locales.
      # Right now only conversion between American English -> British, Canadian,
      # Australian, New Zealand variations is provided.
      class PrimAndProper
        def initialize
          @converters = { :en => { :"en-AU" => AmericanToAustralian.new,
                                   :"en-CA" => AmericanToCanadian.new,
                                   :"en-GB" => AmericanToBritish.new,
                                   :"en-NZ" => AmericanToKiwi.new,
                                 } }
        end

      ...

      # Applies transforms to American English that are common to
      # variants of all other English colonies.
      class AmericanToColonial
        ...
      end

      # Converts American to British English.
      # In addition to general Colonial English variations, changes "apartment"
      # to "flat".
      class AmericanToBritish < AmericanToColonial
        ...
      end
    ```

    All files, including data and config files, should have file-level comments. From     ```translation/config/colonial_spelling_variants.yml```:

    ```ruby
    # List of American-to-British spelling variants.
    #
    # This list is made with
    # lib/tasks/list_american_to_british_spelling_variants.rake.
    #
    # It contains words with general spelling variation patterns:
    #   [trave]led/lled, [real]ize/ise, [flav]or/our, [cent]er/re, plus
    # and these extras:
    #   learned/learnt, practices/practises, airplane/aeroplane, ...

    sectarianizes: sectarianises
    neutralization: neutralisation
    ...
    ```

  - [4.2](#4.2) <a name='4.2'> Function comments

    Every function declaration should have comments immediately preceding it that
    describe what the function does and how to use it. These comments should be
    descriptive ("Opens the file") rather than imperative ("Open the file"); the
    comment describes the function, it does not tell the function what to do. In
    general, these comments do not describe how the function performs its task.
    Instead, that should be left to comments interspersed in the function's code.

    Every function should mention what the inputs and outputs are, unless it meets
    all of the following criteria:

    * not externally visible
    * very short
    * obvious

    You may use whatever format you wish. In Ruby, two popular function
    documentation schemes are [TomDoc](http://tomdoc.org/) and
    [YARD](http://rubydoc.info/docs/yard/file/docs/GettingStarted.md). You can also
    just write things out concisely:

    ```ruby
    # Returns the fallback locales for the_locale.
    # If opts[:exclude_default] is set, the default locale, which is otherwise
    # always the last one in the returned list, will be excluded.
    #
    # For example:
    #   fallbacks_for(:"pt-BR")
    #     => [:"pt-BR", :pt, :en]
    #   fallbacks_for(:"pt-BR", :exclude_default => true)
    #     => [:"pt-BR", :pt]
    def fallbacks_for(the_locale, opts = {})
      ...
    end
    ```

  - [4.3](#4.3) <a name='4.3'> Block and inline comments

    The final place to have comments is in tricky parts of the code. If you're
    going to have to explain it at the next code review, you should comment it now.
    Complicated operations get a few lines of comments before the operations
    commence. Non-obvious ones get comments at the end of the line.

    ```ruby
    def fallbacks_for(the_locale, opts = {})
      # dup() to produce an array that we can mutate.
      ret = @fallbacks[the_locale].dup

      # We make two assumptions here:
      # 1) There is only one default locale (that is, it has no less-specific
      #    children).
      # 1) The default locale is just a language. (Like :en, and not :"en-US".)
      if opts[:exclude_default] &&
          ret.last == default_locale &&
          ret.last != language_from_locale(the_locale)
        ret.pop
      end

      ret
    end
    ```

    On the other hand, never describe the code. Assume the person reading the code
    knows the language (though not what you're trying to do) better than you do.

  - [4.4](#4.4) <a name='4.4'> `TODO` comments

    Use `TODO` comments for code that is temporary, a short-term solution, or
    good-enough but not perfect.

    `TODO`s should include the string `TODO` in all caps, followed by the full name
    of the person who can best provide context about the problem referenced by the
    `TODO`, in parentheses. A colon is optional. A comment explaining what there is
    to do is required. The main purpose is to have a consistent `TODO` format that
    can be searched to find the person who can provide more details upon request.
    A `TODO` is not a commitment that the person referenced will fix the problem.
    Thus when you create a TODO, it is almost always your name that is given.

    ```ruby
      # bad
      # TODO(RS): Use proper namespacing for this constant.

      # bad
      # TODO(drumm3rz4lyfe): Use proper namespacing for this constant.

      # good
      # TODO(Ringo Starr): Use proper namespacing for this constant.
    ```

  - [4.5](#4.5) <a name='4.5'> Commented-out code

    Never leave commented-out code in our codebase.

**[⬆ back to top](#table-of-contents)**

## Methods

  - [5.1](#5.1) <a name='5.1'> Use `def` with parentheses when there are parameters. Omit the parentheses when the method doesn't accept any parameters.

    ```ruby
    # bad
    def some_method()
      ...
    end

    # good
    def some_method
      ...
    end

    # bad
    def some_method_with_parameters param1, param2
      ...
    end

    # good
    def some_method_with_parameters(param1, param2)
      ...
    end
    ```

  - [5.2](#5.2) <a name='5.2'> Define optional arguments at the end of the list of arguments. Ruby has some unexpected results when calling methods that have optional arguments at the front of the list.

    ```ruby
    # bad
    def some_method(a = 1, b = 2, c, d)
      puts "#{a}, #{b}, #{c}, #{d}"
    end

    some_method('w', 'x') # => '1, 2, w, x'
    some_method('w', 'x', 'y') # => 'w, 2, x, y'
    some_method('w', 'x', 'y', 'z') # => 'w, x, y, z'

    # good
    def some_method(c, d, a = 1, b = 2)
      puts "#{a}, #{b}, #{c}, #{d}"
    end

    some_method('w', 'x') # => 'w, x, 1, 2'
    some_method('w', 'x', 'y') # => 'w, x, y, 2'
    some_method('w', 'x', 'y', 'z') # => 'w, x, y, z'
    ```

  - [5.3](#5.3) <a name='5.3'> Use `keyword arguments` if possible instead of `hash` (ruby >= 2.1)

    ```ruby
    # bad
    def obvious_total(options = {})
      options[:subtotal] + options[:tax] - options[:discount]        
      ...
    end

    # good
    def obvious_total(subtotal:, tax:, discount:)
      subtotal + tax - discount
    end
    ```

  - [5.4](#5.4) <a name='5.4'> Always use parentheses for a method call except for a method call if the method accepts no arguments.

    ``` ruby
    # bad
    @current_user = User.find_by_id 1964192

    # bad
    put! (x + y) % len, value

    # bad
    nil?()

    # good
    @current_user = User.find_by_id(1964192)

    # good
    put!((x + y) % len, value)

    # good
    nil?
    ```

  - [5.5](#5.5) <a name='5.5'> Never put a space between a method name and the opening parenthesis.

    ```ruby
    # bad
    f (3 + 2) + 1

    # good
    f(3 + 2) + 1
    ```

  - [5.6](#5.6) <a name='5.6'> If a method accepts an options hash as the last argument, do not use { } during invocation

    ```ruby
    # bad
    get '/v1/reservations', { :id => 54875 }

    # good
    get('/v1/reservations', :id => 54875)
    ```

  - [5.7](#5.7) <a name='5.7'> Align the parameters of a method call if they span more than one line. When aligning parameters is not appropriate due to line-length constraints, single indent for the lines after the first is also acceptable.

    ```ruby
    # bad (double indent)
    def send_mail(source)
     Mailer.deliver(
         to: 'bob@example.com',
         from: 'us@example.com',
         subject: 'Important message',
         body: source.text)
    end

    # good
    def send_mail(source)
     Mailer.deliver(to: 'bob@example.com',
                    from: 'us@example.com',
                    subject: 'Important message',
                    body: source.text)
    end

    # good (normal indent)
    def send_mail(source)
     Mailer.deliver(
       to: 'bob@example.com',
       from: 'us@example.com',
       subject: 'Important message',
       body: source.text
     )
    end
    ```

**[⬆ back to top](#table-of-contents)**

## Conditional Expressions

**[⬆ back to top](#table-of-contents)**

## Syntax

**[⬆ back to top](#table-of-contents)**

## Naming

**[⬆ back to top](#table-of-contents)**

## Classes

**[⬆ back to top](#table-of-contents)**

## Syntax

**[⬆ back to top](#table-of-contents)**

## Exceptions

**[⬆ back to top](#table-of-contents)**

## Collections

**[⬆ back to top](#table-of-contents)**

## Strings

**[⬆ back to top](#table-of-contents)**

## Misc

- [14.1](#14.1) <a name='14.1'></a> Avoid line continuation `\` where not required. In practice, avoid using line continuations for anything but string concatenation

  ```ruby
  # bad
  result = 1 - \
           2

  # good (but still ugly as hell)
  result = 1 \
           - 2

  long_string = 'First part of the long string' \
                ' and second part of the long string'
  ```

- [14.2](#14.2) <a name='14.2'></a> Add underscores to large numeric literals to improve their readability.

  ```ruby
  # bad - how many 0s are there?
  num = 1000000

  # good - much easier to parse for the human brain
  num = 1_000_000
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
