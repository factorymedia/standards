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

  - [6.1](#6.1) <a name='6.1'> Do not use `then` for multi-line `if/unless`

    ```ruby
    # bad
    if some_condition then
      ...
    end

    # good
    if some_condition
      ...
    end
    ```

  - [6.2](#6.2) <a name='6.2'> Always put the condition on the same line as the `if/unless` in a multi-line conditional.

    ```ruby
    # bad
    if
      some_condition
      do_something
      do_something_else
    end

    # good
    if some_condition
      do_something
      do_something_else
    end
    ```

  - [6.3](#6.3) <a name='6.3'> The `and`, `or`, and `not` keywords are banned. It's just not worth it. Always use `&&`, `||`, and `!` instead.

    ```ruby
    # bad
    # boolean expression
    if some_condition and some_other_condition
      do_something
    end

    # control flow
    document.saved? or document.save!

    # braces are required because of op precedence
    x = (not something)

    # good
    # boolean expression
    if some_condition && some_other_condition
      do_something
    end

    # control flow
    document.saved? || document.save!

    # good
    x = !something
    ```

  - [6.4](#6.4) <a name='6.4'> Avoid the use of `!!`

    ```ruby
    # bad
    x = 'test'
    # obscure nil check
    if !!x
      # body omitted
    end

    x = false
    # double negation is useless on booleans
    !!x # => false

    # good
    x = 'test'
    unless x.nil?
      # body omitted
    end
    ```

  - [6.5](#6.5) <a name='6.5'> Favor modifier `if/unless` usage when you have a single-line body. Another good alternative is the usage of control flow `&&/||`.

    ```ruby
    # bad
    if some_condition
      do_something
    end

    # good
    do_something if some_condition

    # another good option
    some_condition && do_something
    ```

  - [6.6](#6.6) <a name='6.6'> Favor `unless` over if for negative conditions (or control flow `||`).

    ```ruby
    # bad
    do_something if !some_condition

    # bad
    do_something if not some_condition

    # good
    do_something unless some_condition

    # another good option
    some_condition || do_something
    ```

  - [6.7](#6.7) <a name='6.7'> Never use `unless` with `else`. Rewrite these with the positive case first.  

    ```ruby
    # bad
    unless success?
      puts 'failure'
    else
      puts 'success'
    end

    # good
    if success?
      puts 'success'
    else
      puts 'failure'
    end
    ```

  - [6.8](#6.8) <a name='6.8'> Avoid unless with multiple conditions.

    ```ruby
    # bad
    unless foo? && bar?
      ...
    end

    # okay
    if !(foo? && bar?)
      ...
    end
    ```

  - [6.9](#6.9) <a name='6.9'> Don't use parentheses around the condition of an `if/unless/while`.

    ```ruby
    # bad
    if (x > 10)
      ...
    end

    # good
    if x > 10
      ...
    end
    ```

  - [6.10](#6.10) <a name='6.10'> Avoid modifier `if/unless` usage at the end of a non-trivial multi-line block.

    ```ruby
    # bad
    10.times do
      # multi-line body omitted
    end if some_condition

    # good
    if some_condition
      10.times do
        # multi-line body omitted
      end
    end
    ```

  - [6.11](#6.11) <a name='6.11'> Avoid nested modifier `if/unless/while/until` usage. Favor `&&/||` if appropriate.

    ```ruby
    # bad
    do_something if other_condition if some_condition

    # good
    do_something if some_condition && other_condition
    ```

  - [6.12](#6.12) <a name='6.12'> Favor the ternary operator(`?:`) over `if/then/else/end` constructs. It's more common and obviously more concise.

    ```ruby
    # bad
    result = if some_condition then something else something_else end

    # good
    result = some_condition ? something : something_else
    ```

  - [6.13](#6.13) <a name='6.13'> Use one expression per branch in a ternary operator. This also means that ternary operators must not be nested. Prefer `if/else` constructs in these cases.

    ```ruby
    # bad
    some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

    # good
    if some_condition
      nested_condition ? nested_something : nested_something_else
    else
      something_else
    end
    ```

  - [6.14](#6.14) <a name='6.14'> Do not use `if x; ....` Use the ternary operator instead

    ```ruby
    # bad
    result = if some_condition; something else something_else end

    # good
    result = some_condition ? something : something_else
    ```

  - [6.15](#6.15) <a name='6.15'> Avoid multi-line `?:` (the ternary operator); use `if/unless` instead.

**[⬆ back to top](#table-of-contents)**

## Syntax

  - [7.1](#7.1) <a name='7.1'> Do not use `for`, unless you know exactly why. Most of the time iterators should be used instead. `for` is implemented in terms of `each` (so you're adding a level of indirection), but with a twist - `for` doesn't introduce a new scope (unlike `each`) and variables defined in its block will be visible outside it.

    ```ruby
    arr = [1, 2, 3]

    # bad
    for elem in arr do
      puts elem
    end

    # note that elem is accessible outside of the for loop
    elem # => 3

    # good
    arr.each { |elem| puts elem }

    # elem is not accessible outside each's block
    elem # => NameError: undefined local variable or method `elem'
    ```

  - [7.2](#7.2) <a name='7.2'> Prefer `{...}` over `do...end` for single-line blocks. Avoid using `{...}` for multi-line blocks (multiline chaining is always ugly). Always use `do...end` for "control flow" and "method definitions" (e.g. in Rakefiles and certain DSLs). Avoid `do...end` when chaining.

    ```ruby
    names = %w(Bozhidar Steve Sarah)

    # bad
    names.each do |name|
      puts name
    end

    # good
    names.each { |name| puts name }

    # bad
    names.select do |name|
      name.start_with?('S')
    end.map { |name| name.upcase }

    # good
    names.select { |name| name.start_with?('S') }.map(&:upcase)
    ```

  - [7.3](#7.3) <a name='7.3'> Consider using explicit block argument to avoid writing block literal that just passes its arguments to another block. Beware of the performance impact, though, as the block gets converted to a Proc.

    ```ruby
    require 'tempfile'

    # bad
    def with_tmp_dir
      Dir.mktmpdir do |tmp_dir|
        Dir.chdir(tmp_dir) { |dir| yield dir }  # block just passes arguments
      end
    end

    # good
    def with_tmp_dir(&block)
      Dir.mktmpdir do |tmp_dir|
        Dir.chdir(tmp_dir, &block)
      end
    end

    with_tmp_dir do |dir|
      puts "dir is accessible as a parameter and pwd is set: #{dir}"
    end
    ```

  - [7.4](#7.4) <a name='7.4'> Avoid `return` where not required for flow of control.

    ```ruby
    # bad
    def some_method(some_arr)
      return some_arr.size
    end

    # good
    def some_method(some_arr)
      some_arr.size
    end
    ```

  - [7.5](#7.5) <a name='7.5'> Avoid `self` where not required. (It is only required when calling a self write accessor.)

    ```ruby
    # bad
    def ready?
      if self.last_reviewed_at > self.last_updated_at
        self.worker.update(self.content, self.options)
        self.status = :in_progress
      end
      self.status == :verified
    end

    # good
    def ready?
      if last_reviewed_at > last_updated_at
        worker.update(content, options)
        self.status = :in_progress
      end
      status == :verified
    end
    ```

  - [7.6](#7.6) <a name='7.6'> Don't use the return value of `=` (an assignment) in conditional expressions unless the assignment is wrapped in parentheses. This is a fairly popular idiom among Rubyists that's sometimes referred to as safe assignment in condition.

    ```ruby
    # bad (+ a warning)
    if v = array.grep(/foo/)
      do_something(v)
      ...
    end

    # good (MRI would still complain, but RuboCop won't)
    if (v = array.grep(/foo/))
      do_something(v)
      ...
    end

    # good
    v = array.grep(/foo/)
    if v
      do_something(v)
      ...
    end
    ```

  - [7.7](#7.7) <a name='7.7'> Use shorthand self assignment operators whenever applicable.

    ```ruby
    # bad
    x = x + y
    x = x * y
    x = x**y
    x = x / y
    x = x || y
    x = x && y

    # good
    x += y
    x *= y
    x **= y
    x /= y
    x ||= y
    x &&= y
    ```

  - [7.8](#7.8) <a name='7.8'> Use `||=` to initialize variables only if they're not already initialized.

    ```ruby
    # bad
    name = name ? name : 'Bozhidar'

    # bad
    name = 'Bozhidar' unless name

    # good - set name to Bozhidar, only if it's nil or false
    name ||= 'Bozhidar'
    ```

  - [7.9](#7.9) <a name='7.9'> Don't use `||=` to initialize boolean variables. (Consider what would happen if the current value happened to be `false`.)

    ```ruby
    # bad - would set enabled to true even if it was false
    enabled ||= true

    # good
    enabled = true if enabled.nil?
    ```

  - [7.10](#7.10) <a name='7.10'> Avoid using Perl-style special variables (like `$:`, `$;`, etc. ).

    ```ruby
    # bad
    $:.unshift File.dirname(__FILE__)

    # good
    require 'English'
    $LOAD_PATH.unshift File.dirname(__FILE__)
    ```

  - [7.11](#7.11) <a name='7.11'> Use the new lambda literal syntax for single line body blocks. Use the `lambda` method for multi-line blocks.

    ```ruby
    # bad
    l = lambda { |a, b| a + b }
    l.call(1, 2)

    # correct, but looks extremely awkward
    l = ->(a, b) do
      tmp = a * 7
      tmp * b / 50
    end

    # good
    l = ->(a, b) { a + b }
    l.call(1, 2)

    l = lambda do |a, b|
      tmp = a * 7
      tmp * b / 50
    end
    ```

  - [7.12](#7.12) <a name='7.12'> Don't omit the parameter parentheses when defining a stabby lambda with parameters.

    ```ruby
    # bad
    l = ->x, y { something(x, y) }

    # good
    l = ->(x, y) { something(x, y) }
    ```

  - [7.13](#7.13) <a name='7.13'> Omit the parameter parentheses when defining a stabby lambda with no parameters.

    ```ruby
    # bad
    l = ->() { something }

    # good
    l = -> { something }
    ```

  - [7.14](#7.14) <a name='7.14'> Prefer `proc` over `Proc.new`.

    ```ruby
    # bad
    p = Proc.new { |n| puts n }

    # good
    p = proc { |n| puts n }
    ```

  - [7.15](#7.15) <a name='7.15'> Prefer `proc.call()` over `proc[]` or `proc.()` for both lambdas and procs.

    ```ruby
    # bad - looks similar to Enumeration access
    l = ->(v) { puts v }
    l[1]

    # also bad - uncommon syntax
    l = ->(v) { puts v }
    l.(1)

    # good
    l = ->(v) { puts v }
    l.call(1)
    ```

  - [7.16](#7.16) <a name='7.16'> Prefix with `_` unused block parameters and local variables.

    ```ruby
    # bad
    result = hash.map { |k, v| v + 1 }

    def something(x)
      unused_var, used_var = something_else(x)
      # ...
    end

    # good
    result = hash.map { |_k, v| v + 1 }

    def something(x)
      _unused_var, used_var = something_else(x)
      # ...
    end

    # good
    result = hash.map { |_, v| v + 1 }

    def something(x)
      _, used_var = something_else(x)
      # ...
    end
    ```

  - [7.17](#7.17) <a name='7.17'> Use ranges or `Comparable#between?` instead of complex comparison logic when possible.

    ```ruby
    # bad
    do_something if x >= 1000 && x <= 2000

    # good
    do_something if (1000..2000).include?(x)

    # good
    do_something if x.between?(1000, 2000)
    ```

  - [7.18](#7.18) <a name='7.18'> Avoid the use of `BEGIN` blocks.

    ```ruby
    # bad
    begin
      @splines.reticulate
    end if false

    # good
    @splines.reticulate if false
    ```

  - [7.19](#7.19) <a name='7.19'> Do not use `END` blocks. Use `Kernel#at_exit` instead.

    ```ruby
    # bad
    END { puts 'Goodbye!' }

    # good
    at_exit { puts 'Goodbye!' }

    ```

  - [7.20](#7.20) <a name='7.20'> Prefer a guard clause when you can assert invalid data. A guard clause is a conditional statement at the top of a function that bails out as soon as it can.

    ```ruby
    # bad
    def compute_thing(thing)
      if thing[:foo]
        update_with_bar(thing)
        if thing[:foo][:bar]
          partial_compute(thing)
        else
          re_compute(thing)
        end
      end
    end

    # good
    def compute_thing(thing)
      return unless thing[:foo]
      update_with_bar(thing[:foo])
      return re_compute(thing) unless thing[:foo][:bar]
      partial_compute(thing)
    end
    ```

  - [7.21](#7.21) <a name='7.21'> Prefer `next` in loops instead of conditional blocks.

    ```ruby
    # bad
    [0, 1, 2, 3].each do |item|
      if item > 1
        puts item
      end
    end

    # good
    [0, 1, 2, 3].each do |item|
      next unless item > 1
      puts item
    end
    ```

  - [7.22](#7.22) <a name='7.22'> Prefer `map` over `collect`, `find` over `detect`, `select` over `find_all`, `reduce` over `inject` and `size` over `length`. This is not a hard requirement

  - [7.23](#7.23) <a name='7.23'> Don't use `count` as a substitute for `size`. For Enumerable objects other than `Array` it will iterate the entire collection in order to determine its size.

    ```ruby
    # bad
    some_hash.count

    # good
    some_hash.size
    ```

  - [7.24](#7.24) <a name='7.24'> Prefer `reverse_each` to `reverse.each` because some classes that `include Enumerable` will provide an efficient implementation. Even in the worst case where a class does not provide a specialized implementation, the general implementation inherited from `Enumerable` will be at least as efficient as using `reverse.each`.

    ```ruby
    # bad
    array.reverse.each { ... }

    # good
    array.reverse_each { ... }
    ```

  - [7.25](#7.25) <a name='7.25'> When a method block takes only one argument, and the body consists solely of reading an attribute or calling one method with no arguments, use the &: shorthand.

    ```ruby
    # bad
    bluths.map { |bluth| bluth.occupation }
    bluths.select { |bluth| bluth.blue_self? }

    # good
    bluths.map(&:occupation)
    bluths.select(&:blue_self?)
    ```

**[⬆ back to top](#table-of-contents)**

## Naming

  - [8.1](#8.1) <a name='8.1'> Name identifiers in English.

    ```ruby
    # bad - identifier using non-ascii characters
    заплата = 1_000

    # bad - identifier is a Bulgarian word, written with Latin letters (instead of Cyrillic)
    zaplata = 1_000

    # good
    salary = 1_000
    ```

  - [8.2](#8.2) <a name='8.2'> Use `snake_case` for symbols, methods and variables.

    ```ruby
    # bad
    :'some symbol'
    :SomeSymbol
    :someSymbol

    someVar = 5

    def someMethod
      ...
    end

    def SomeMethod
      ...
    end

    # good
    :some_symbol

    def some_method
      ...
    end
    ```

  - [8.3](#8.3) <a name='8.3'> Use `CamelCase` for classes and modules. (Keep acronyms like HTTP, RFC, XML uppercase.)

    ```ruby
    # bad
    class Someclass
      ...
    end

    class Some_Class
      ...
    end

    class SomeXml
      ...
    end

    class XmlSomething
      ...
    end

    # good
    class SomeClass
      ...
    end

    class SomeXML
      ...
    end

    class XMLSomething
      ...
    end
    ```

  - [8.4](#8.4) <a name='8.4'> Use `snake_case` for naming files, e.g. `hello_world.rb`.

  - [8.5](#8.5) <a name='8.5'> Use `snake_case` for naming directories, e.g. `lib/hello_world/hello_world.rb`.

  - [8.6](#8.6) <a name='8.6'> Aim to have just a single class/module per source file. Name the file name as the class/module, but replacing CamelCase with snake_case.

  - [8.7](#8.7) <a name='8.7'> Use `SCREAMING_SNAKE_CASE` for other constants.

    ```ruby
    # bad
    SomeConst = 5

    # good
    SOME_CONST = 5
    ```

  - [8.8](#8.8) <a name='8.8'> The names of predicate methods (methods that return a boolean value) should end in a question mark. (i.e. `Array#empty?`). Methods that don't return a boolean, shouldn't end in a question mark.

    ```ruby
    # bad
    def size?
      ...
      1
    end

    # good
    def empty?
      ...
      true
    end
    ```

  - [8.9](#8.9) <a name='8.9'> The names of potentially dangerous methods (i.e. methods that modify `self` or the arguments, `exit!` (doesn't run the finalizers like `exit` does), etc.) should end with an exclamation mark if there exists a safe version of that dangerous method.

    ```ruby
    # bad - there is no matching 'safe' method
    class Person
      def update!
      end
    end

    # good
    class Person
      def update
      end
    end

    # good
    class Person
      def update!
      end

      def update
      end
    end
    ```

  - [8.10](#8.10) <a name='8.10'> Define the non-bang (safe) method in terms of the bang (dangerous) one if possible.

    ```ruby
    class Array
      def flatten_once!
        res = []

        each do |e|
          [*e].each { |f| res << f }
        end

        replace(res)
      end

      def flatten_once
        dup.flatten_once!
      end
    end
    ```

  - [8.11](#8.11) <a name='8.11'> When defining binary operators, name the parameter `other`(`<<` and `[]` are exceptions to the rule, since their semantics are different).

    ```ruby
    def +(other)
      # body omitted
    end
    ```

  - [8.12](#8.12) <a name='8.12'> Name throwaway variables `_`.

    ```ruby
    payment, _ = Payment.complete_paypal_payment!(params[:token],
                                                  native_currency,
                                                  created_at)
    ```

**[⬆ back to top](#table-of-contents)**

## Classes

**[⬆ back to top](#table-of-contents)**

## Exceptions

**[⬆ back to top](#table-of-contents)**

## Collections

  Favor the use of Array#join over the fairly cryptic Array#* with [link] a string argument.

  Use `[*var]` or `Array()` instead of explicit Array check, when dealing with a variable you want to treat as an Array, but you're not certain it's an array.


**[⬆ back to top](#table-of-contents)**

## Strings

**[⬆ back to top](#table-of-contents)**

## Misc

- [13.1](#13.1) <a name='13.1'></a> Avoid line continuation `\` where not required. In practice, avoid using line continuations for anything but string concatenation

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

- [13.2](#13.2) <a name='13.2'></a> Add underscores to large numeric literals to improve their readability.

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
