# Factory Media Ruby Style Guide

Inspired by [Bozhidar Batsov Guide](https://github.com/bbatsov/ruby-style-guide), [Github's Guide](https://github.com/styleguide/ruby) and [Airbnb Ruby Style Guide](https://github.com/airbnb/ruby)

## Table of Contents

  1. [Whitespace](#whitespace)
  1. [Line Length](#line-length)
  1. [Encoding](#encoding)
  1. [Comments](#comments)
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
