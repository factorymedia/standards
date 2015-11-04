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
  1. [Classes & Modules](#classes--modules)
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

  - [1.2](#1.2) <a name='1.2'></a> Indent `when` as deep as `case`. This is the style established in both "The Ruby Programming Language" and "Programming Ruby".

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

    > The only exception, regarding operators, is the exponent operator

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

  - [1.8](#1.8) <a name='1.8'></a> Use space after `{` or before `}`.

    ```ruby
    # bad
    {one: 1, two: 2}

    # good - space after { and before }
    { one: 1, two: 2 }

    # good
    sum = 1 + 2
    a, b = 1, 2
    [1, 2, 3].each { |e| puts e }
    ```  

  - [1.9](#1.9) <a name='1.9'></a> No space after `!`.

    ```ruby
    # bad
    ! something

    # good
    !something
    ```

  - [1.10](#1.10) <a name='1.10'></a> No space inside range literals.

    ```ruby
    # bad
    1 .. 3
    'a' ... 'z'

    # good
    1..3
    'a'...'z'
    ```

  - [1.11](#1.11) <a name='1.11'></a> Use spaces around the = operator when assigning default values to method parameters

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

  - [1.12](#1.12) <a name='1.12'></a> Align the elements of array literals spanning multiple lines.

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

  - [1.13](#1.13) <a name='1.13'></a> Add a new line after if conditions span multiple lines to help differentiate between the conditions and the body.

    ```ruby
    if @reservation_alteration.checkin == @reservation.start_date &&
      @reservation_alteration.checkout == (@reservation.start_date + @reservation.nights)

      redirect_to_alteration @reservation_alteration
    end
    ```

  - [1.14](#1.14) <a name='1.14'></a> Add a new line after conditionals, blocks, case statements, etc.

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

## Classes & Modules

  - [9.1](#9.1) <a name='9.1'> Use a consistent structure in your class definitions.

    ```ruby
    class Person
      # extend and include go first
      extend SomeModule
      include AnotherModule

      # inner classes
      CustomErrorKlass = Class.new(StandardError)

      # constants are next
      SOME_CONSTANT = 20

      # afterwards we have attribute macros
      attr_reader :name

      # followed by other macros (if any)
      validates :name

      # public class methods are next in line
      def self.some_method
      end

      # initialization goes between class methods and other instance methods
      def initialize
      end

      # followed by other public instance methods
      def some_method
      end

      # protected and private methods are grouped near the end
      protected

      def some_protected_method
      end

      private

      def some_private_method
      end
    end
    ```

  - [9.2](#9.2) <a name='9.2'> Don't nest multi line classes within classes. Try to have such nested classes each in their own file in a folder named like the containing class.

    ```ruby
    # bad

    # foo.rb
    class Foo
      class Bar
        # 30 methods inside
      end

      class Car
        # 20 methods inside
      end

      # 30 methods inside
    end

    # good

    # foo.rb
    class Foo
      # 30 methods inside
    end

    # foo/bar.rb
    class Foo
      class Bar
        # 30 methods inside
      end
    end

    # foo/car.rb
    class Foo
      class Car
        # 20 methods inside
      end
    end
    ```

  - [9.3](#9.3) <a name='9.3'> Prefer modules to classes with only class methods. Classes should be used only when it makes sense to create instances out of them.

    ```ruby
    # bad
    class SomeClass
      def self.some_method
        ...
      end

      def self.some_other_method
        ...
      end
    end

    # good
    module SomeModule
      extend self

      def some_method
        ...
      end

      def some_other_method
        ...
      end
    end
    ```

  - [9.4](#9.4) <a name='9.4'> Always supply a proper `to_s` method for classes that represent domain objects.

    ```ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def to_s
        "#{@first_name} #{@last_name}"
      end
    end
    ```

  - [9.5](#9.5) <a name='9.5'> Use the `attr` family of functions to define trivial accessors or mutators.

    ```ruby
    # bad
    class Person
      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def first_name
        @first_name
      end

      def last_name
        @last_name
      end
    end

    # good
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end
    ```

  - [9.6](#9.6) <a name='9.6'> Avoid the use of `attr`. Use `attr_reader` and `attr_accessor` instead.

    ```ruby
    # bad - creates a single attribute accessor (deprecated in 1.9)
    attr :something, true
    attr :one, :two, :three # behaves as attr_reader

    # good
    attr_accessor :something
    attr_reader :one, :two, :three
    ```

  - [9.7](#9.7) <a name='9.7'> Consider using `Struct.new`, which defines the trivial accessors, constructor and comparison operators for you.

    ```ruby
    # good
    class Person
      attr_accessor :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end

    # better
    Person = Struct.new(:first_name, :last_name) do
    end
    ```

  - [9.8](#9.8) <a name='9.8'> Don't extend an instance initialized by `Struct.new`. Extending it introduces a superfluous class level and may also introduce weird errors if the file is required multiple times.

    ```ruby
    # bad
    class Person < Struct.new(:first_name, :last_name)
    end

    # good
    Person = Struct.new(:first_name, :last_name)
    ```

  - [9.9](#9.9) <a name='9.9'> Consider adding factory methods to provide additional sensible ways to create instances of a particular class.

    ```ruby
    class Person
      def self.create(options_hash)
        ...
      end
    end
    ```

  - [9.10](#9.10) <a name='9.10'> Avoid the usage of class (`@@`) variables due to their "nasty" behavior in inheritance.

    ```ruby
    class Parent
      @@class_var = 'parent'

      def self.print_class_var
        puts @@class_var
      end
    end

    class Child < Parent
      @@class_var = 'child'
    end

    Parent.print_class_var # => will print "child"
    ```

  - [9.11](#9.11) <a name='9.11'> Indent the `public`, `protected`, and `private` methods as much as the method definitions they apply to. Leave one blank line above the visibility modifier and one blank line below.

    ```ruby
    class SomeClass
      def public_method
        # ...
      end

      private

      def private_method
        # ...
      end

      def another_private_method
        # ...
      end
    end
    ```

  - [9.12](#9.12) <a name='9.12'> Use `def self.method` to define singleton methods. This makes the methods more resistant to refactoring changes.

    ```ruby
    class TestClass
      # bad
      def TestClass.some_method
        ...
      end

      # good
      def self.some_other_method
        ...
      end
    end
    ```

  - [9.13](#9.13) <a name='9.13'> Avoid `class << self` except when necessary, e.g. single accessors and aliased attributes.

    ```ruby
    class TestClass
      # bad
      class << self
        def first_method
          ...
        end

        def second_method_etc
          ...
        end
      end

      # good
      class << self
        attr_accessor :per_page
        alias_method :nwo, :find_by_name_with_owner
      end

      def self.first_method
        ...
      end

      def self.second_method_etc
        ...
      end
    end
    ```

  - [9.14](#9.14) <a name='9.14'> Prefer `alias` when aliasing methods in lexical class scope as the resolution of `self` in this context is also lexical, and it communicates clearly to the user that the indirection of your alias will not be altered at runtime or by any subclass unless made explicit.

    ```ruby
    class Westerner
      def first_name
        @names.first
      end

      alias given_name first_name
    end
    ```

  - [9.15](#9.15) <a name='9.15'> Always use `alias_method` when aliasing methods of modules, classes, or singleton classes at runtime, as the lexical scope of `alias` leads to unpredictability in these cases.

    ```ruby
    module Mononymous
      def self.included(other)
        other.class_eval { alias_method :full_name, :given_name }
      end
    end

    class Sting < Westerner
      include Mononymous
    end
    ```

**[⬆ back to top](#table-of-contents)**

## Exceptions

  - [10.1](#10.1) <a name='10.1'> Use `raise` instead of `fail`

    ```ruby
    # bad
    fail('message')

    # good - signals a RuntimeError by default
    raise('message')
    ```

  - [10.2](#10.2) <a name='10.2'> Don't specify `RuntimeError` explicitly in the two argument version of `raise`.

    ```ruby
    # bad
    raise(RuntimeError, 'message')

    # good - signals a RuntimeError by default
    raise('message')
    ```

  - [10.3](#10.3) <a name='10.3'> Prefer supplying an exception class and a message as two separate arguments to `raise`, instead of an exception instance.

    ```ruby
    # bad
    raise SomeException.new('message')

    # good
    raise(SomeException, 'message')
    ```

  - [10.4](#10.4) <a name='10.4'> Do not return from an `ensure` block. If you explicitly return from a method inside an `ensure` block, the return will take precedence over any exception being raised, and the method will return as if no exception had been raised at all. In effect, the exception will be silently thrown away.

    ```ruby
    def foo
      raise
    ensure
      return 'very bad idea'
    end
    ```

  - [10.5](#10.5) <a name='10.5'> Use implicit begin blocks where possible.

    ```ruby
    # bad
    def foo
      begin
        # main logic goes here
      rescue
        # failure handling goes here
      end
    end

    # good
    def foo
      # main logic goes here
    rescue
      # failure handling goes here
    end
    ```

  - [10.6](#10.6) <a name='10.6'> Don't suppress exceptions.

    ```ruby
    # bad
    begin
      # an exception occurs here
    rescue SomeError
      # the rescue clause does absolutely nothing
    end

    # bad
    do_something rescue nil
    ```

  - [10.7](#10.7) <a name='10.7'> Avoid using `rescue` in its modifier form.

    ```ruby
    # bad - this catches exceptions of StandardError class and its descendant classes
    read_file rescue handle_error($!)

    # good - this catches only the exceptions of Errno::ENOENT class and its descendant classes
    def foo
      read_file
    rescue Errno::ENOENT => ex
      handle_error(ex)
    end
    ```

  - [10.8](#10.8) <a name='10.8'> Don't use exceptions for flow of control.

    ```ruby
    # bad
    begin
      n / d
    rescue ZeroDivisionError
      puts 'Cannot divide by 0!'
    end

    # good
    if d.zero?
      puts 'Cannot divide by 0!'
    else
      n / d
    end
    ```

  - [10.9](#10.9) <a name='10.9'> Avoid rescuing the Exception class.

    ```ruby
    # bad
    begin
      # calls to exit and kill signals will be caught (except kill -9)
      exit
    rescue Exception
      puts "you didn't really want to exit, right?"
      # exception handling
    end

    # good
    begin
      # a blind rescue rescues from StandardError, not Exception as many
      # programmers assume.
    rescue => e
      # exception handling
    end

    # also good
    begin
      # an exception occurs here
    rescue StandardError => e
      # exception handling
    end
    ```

  - [10.10](#10.10) <a name='10.10'> Put more specific exceptions higher up the rescue chain, otherwise they'll never be rescued from.

    ```ruby
    # bad
    begin
      # some code
    rescue StandardError => e
      # some handling
    rescue IOError => e
      # some handling that will never be executed
    end

    # good
    begin
      # some code
    rescue IOError => e
      # some handling
    rescue StandardError => e
      # some handling
    end
    ```

  - [10.11](#10.11) <a name='10.11'> Release external resources obtained by your program in an `ensure` block.

    ```ruby
    f = File.open('testfile')
    begin
      # .. process
    rescue
      # .. handle error
    ensure
      f.close if f
    end
    ```

  - [10.12](#10.12) <a name='10.12'> Use versions of resource obtaining methods that do automatic resource cleanup when possible.

    ```ruby
    # bad - you need to close the file descriptor explicitly
    f = File.open('testfile')
      # ...
    f.close

    # good - the file descriptor is closed automatically
    File.open('testfile') do |f|
      # ...
    end
    ```

**[⬆ back to top](#table-of-contents)**

## Collections

  - [11.1](#11.1) <a name='11.1'> Use `Set` instead of `Array` when dealing with unique elements. `Set` implements a collection of unordered values with no duplicates. This is a hybrid of `Array`'s intuitive inter-operation facilities and `Hash`'s fast lookup.

    ```ruby
    require 'set'
    s = Set.new
    ```

  - [11.2](#11.2) <a name='11.2'> Prefer literal array and hash creation notation.

    ```ruby
    # bad
    arr = Array.new
    hash = Hash.new

    # good
    arr = []
    hash = {}
    ```

  - [11.3](#11.3) <a name='11.3'> Prefer `%w` to the literal array syntax when you need an array of words (non-empty strings without spaces and special characters in them). Apply this rule only to arrays with two or more elements.

    ```ruby
    # bad
    STATES = ['draft', 'open', 'closed']

    # good
    STATES = %w(draft open closed)
    ```

  - [11.4](#11.4) <a name='11.4'> Prefer `%i` to the literal array syntax when you need an array of symbols. Apply this rule only to arrays with two or more elements.

    ```ruby
    # bad
    STATES = [:draft, :open, :closed]

    # good
    STATES = %i(draft open closed)
    ```

  - [11.5](#11.5) <a name='11.5'> When accessing the first or last element from an array, prefer `first` or `last` over `[0]` or `[-1]`

    ```ruby
    array = [1, 2, 3]
    # bad
    a[0]
    a[-1]

    #good
    a.first
    a.last
    ```

  - [11.6](#11.6) <a name='11.6'> Prefer symbols instead of strings as hash keys.

    ```ruby
    # bad
    hash = { 'one' => 1, 'two' => 2, 'three' => 3 }

    # good
    hash = { one: 1, two: 2, three: 3 }
    ```

  - [11.7](#11.7) <a name='11.7'> Use the Ruby 1.9 hash literal syntax when your hash keys are symbols.

    ```ruby
    # bad
    hash = { :one => 1, :two => 2, :three => 3 }

    # good
    hash = { one: 1, two: 2, three: 3 }
    ```

  - [11.8](#11.8) <a name='11.8'> Use hash rockets when only argument is hash, use hash literal for keyword arguments.

    ```ruby
    def hash_arg(hash = {})
    end

    # bad
    hash_arg(arg1: 1, arg2: 2)

    # good
    hash_arg(:arg1 => 1, :arg2 => 2)

    def keyword_arg(arg1:, arg2)
    end

    # good
    keyword_arg(arg1: 1, arg2: 2)
    ```

  - [11.9](#11.9) <a name='11.9'> Use `Hash#key?` instead of `Hash#has_key?` and `Hash#value?` instead of `Hash#has_value?`.

    ```ruby
    # bad
    hash.has_key?(:test)
    hash.has_value?(value)

    # good
    hash.key?(:test)
    hash.value?(value)
    ```

  - [11.10](#11.10) <a name='11.10'> Use `Hash#fetch` when dealing with hash keys that should be present.

    ```ruby
    heroes = { batman: 'Bruce Wayne', superman: 'Clark Kent' }
    # bad - if we make a mistake we might not spot it right away
    heroes[:batman] # => "Bruce Wayne"
    heroes[:supermann] # => nil

    # good - fetch raises a KeyError making the problem obvious
    heroes.fetch(:supermann)
    ```

  - [11.11](#11.11) <a name='11.11'> Introduce default values for hash keys via `Hash#fetch` as opposed to using custom logic.

    ```ruby
    batman = { name: 'Bruce Wayne', is_evil: false }

    # bad - if we just use || operator with falsy value we won't get the expected result
    batman[:is_evil] || true # => true

    # good - fetch work correctly with falsy values
    batman.fetch(:is_evil, true) # => false
    ```

  - [11.12](#11.12) <a name='11.12'> Prefer the use of the block instead of the default value in `Hash#fetch` if the code that has to be evaluated may have side effects or be expensive.

    ```ruby
    batman = { name: 'Bruce Wayne' }

    # bad - if we use the default value, we eager evaluate it
    # so it can slow the program down if done multiple times
    batman.fetch(:powers, obtain_batman_powers) # obtain_batman_powers is an expensive call

    # good - blocks are lazy evaluated, so only triggered in case of KeyError exception
    batman.fetch(:powers) { obtain_batman_powers }
    ```

  - [11.13](#11.13) <a name='11.13'> Use `Hash#values_at` when you need to retrieve several values consecutively from a hash.

    ```ruby
    # bad
    email = data['email']
    username = data['nickname']

    # good
    email, username = data.values_at('email', 'nickname')
    ```

  - [11.14](#11.14) <a name='11.14'> When accessing elements of a collection, avoid direct access via `[n]` by using an alternate form of the reader method if it is supplied. This guards you from calling [] on nil.

    ```ruby
    # bad
    Regexp.last_match[1]

    # good
    Regexp.last_match(1)
    ```

  - [11.15](#11.15) <a name='11.15'> Favor the use of `Array#join` over the fairly cryptic `Array#`* with a string argument.

    ```ruby
    # bad
    %w(one two three) * ', '
    # => 'one, two, three'

    # good
    %w(one two three).join(', ')
    # => 'one, two, three'
    ```

  - [11.16](#11.16) <a name='11.16'> Use `[*var]` or `Array()` instead of explicit `Array` check, when dealing with a variable you want to treat as an Array, but you're not certain it's an array.

    ```ruby
    # bad
    paths = [paths] unless paths.is_a? Array
    paths.each { |path| do_something(path) }

    # good
    [*paths].each { |path| do_something(path) }

    # good (and a bit more readable)
    Array(paths).each { |path| do_something(path) }
    ```

**[⬆ back to top](#table-of-contents)**

## Strings

  - [12.1](#12.1) <a name='12.1'> Prefer string interpolation instead of string concatenation

    ```ruby
    # bad
    email_with_name = user.name + ' <' + user.email + '>'

    # good
    email_with_name = "#{user.name} <#{user.email}>"

    # good
    email_with_name = format('%s <%s>', user.name, user.email)
    ```

  - [12.2](#12.2) <a name='12.2'> With interpolated expressions, there should be no padded-spacing inside the braces

    ```ruby
    # bad
    "From: #{ user.first_name }, #{ user.last_name }"

    # good
    "From: #{user.first_name}, #{user.last_name}"
    ```

  - [12.3](#12.3) <a name='12.3'> Prefer single-quoted strings when you don't need string interpolation or special symbols such as `\t`, `\n`, `'`, etc.

    ```ruby
    # bad
    name = "Bozhidar"

    # good
    name = 'Bozhidar'
    ```

  - [12.4](#12.4) <a name='12.4'> Don't use the character literal syntax `?x`. Since Ruby 1.9 it's basically redundant - `?x` would interpreted as 'x' (a string with a single character in it).

    ```ruby
    # bad
    char = ?c

    # good
    char = 'c'
    ```

  - [12.5](#12.5) <a name='12.5'> Don't leave out `{}` around instance and global variables being interpolated into a string.

    ```ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      # bad - valid, but awkward
      def to_s
        "#@first_name #@last_name"
      end

      # good
      def to_s
        "#{@first_name} #{@last_name}"
      end
    end
    ```

  - [12.6](#12.6) <a name='12.6'> Don't use `Object#to_s` on interpolated objects. It's invoked on them automatically.

    ```ruby
    # bad
    message = "This is the #{result.to_s}."

    # good
    message = "This is the #{result}."
    ```

  - [12.7](#12.7) <a name='12.7'> Avoid using `String#+` when you need to construct large data chunks. Instead, use `String#<<`. Concatenation mutates the string instance in-place and is always faster than `String#+`, which creates a bunch of new string objects.

    ```ruby
    # bad
    html = ''
    html += '<h1>Page title</h1>'

    paragraphs.each do |paragraph|
      html += "<p>#{paragraph}</p>"
    end

    # good and also fast
    html = ''
    html << '<h1>Page title</h1>'

    paragraphs.each do |paragraph|
      html << "<p>#{paragraph}</p>"
    end
    ```

  - [12.8](#12.8) <a name='12.8'> Don't use `String#gsub` in scenarios in which you can use a faster more specialized alternative.

    ```ruby
    url = 'http://example.com'
    str = 'lisp-case-rules'

    # bad
    url.gsub("http://", "https://")
    str.gsub("-", "_")

    # good
    url.sub("http://", "https://")
    str.tr("-", "_")
    ```

  - [12.9](#12.9) <a name='12.9'> When using heredocs for multi-line strings keep in mind the fact that they preserve leading whitespace. It's a good practice to employ some margin based on which to trim the excessive whitespace.

    ```ruby
    code = <<-END.gsub(/^\s+\|/, '')
      |def test
      |  some_method
      |  other_method
      |end
    END
    # => "def test\n  some_method\n  other_method\nend\n"
    ```

  - [12.10](#12.10) <a name='12.10'> Use `%()` for single-line strings which require both interpolation and embedded double-quotes. For multi-line strings, prefer heredocs.

    ```ruby
    # bad (no interpolation needed)
    %(<div class="text">Some text</div>)
    # should be '<div class="text">Some text</div>'

    # bad (no double-quotes)
    %(This is #{quality} style)
    # should be "This is #{quality} style"

    # bad (multiple lines)
    %(<div>\n<span class="big">#{exclamation}</span>\n</div>)
    # should be a heredoc.

    # good (requires interpolation, has quotes, single line)
    %(<tr><td class="name">#{name}</td>)
    ```

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

- [13.3](#13.3) <a name='13.3'></a> Use OptionParser for parsing complex command line options and ruby -s for trivial command line options.

- [13.4](#13.4) <a name='13.4'></a> Prefer Time.now over Time.new when retrieving the current system time.

- [13.5](#13.5) <a name='13.5'></a> Avoid more than three levels of block nesting.

- [13.6](#13.6) <a name='13.6'></a> Avoid methods longer than 10 LOC (lines of code). Ideally, most methods will be shorter than 5 LOC. Empty lines do not contribute to the relevant LOC.

- [13.7](#13.7) <a name='13.7'></a> Avoid parameter lists longer than three or four parameters.

- [13.8](#13.8) <a name='13.8'></a> Use module instance variables instead of global variables.

    ```ruby
    # bad
    $foo_bar = 1

    # good
    module Foo
      class << self
        attr_accessor :bar
      end
    end

    Foo.bar = 1
    ```

**[⬆ back to top](#table-of-contents)**

## Be Consistent

> If you're editing code, take a few minutes to look at the code around you and
> determine its style. If they use spaces around all their arithmetic
> operators, you should too. If their comments have little boxes of hash marks
> around them, make your comments have little boxes of hash marks around them
> too.

> The point of having style guidelines is to have a common vocabulary of coding
> so people can concentrate on what you're saying rather than on how you're
> saying it. We present global style rules here so people know the vocabulary,
> but local style is also important. If code you add to a file looks
> drastically different from the existing code around it, it throws readers out
> of their rhythm when they go to read it. Avoid this.

&mdash;[Google C++ Style Guide][google-c++]

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
