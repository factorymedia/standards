# Factory Media PHP Unit Testing Guide

## Table of Contents

  1. [Tools](#tools)
  2. [Guidelines](#guidelines)
  3. [Sample unit tests](#examples)
  4. [Resources](#resources)

## Tools 
The suggested tool is PHPUnit, since it's the [offcial framework used by Wordpress](https://make.wordpress.org/core/handbook/testing/automated-testing/phpunit/). It provides a test runner, assertions, bootstraps the Wordpress environment to make it easy to test plugins, helpers, etc. It's also quite fast!

### Install PHPUnit
Please follow these steps to install phpunit globally.

```
wget https://phar.phpunit.de/phpunit.phar

chmod +x phpunit.phar

sudo mv phpunit.phar /usr/local/bin/phpunit
```
PHPUnit uses an .xml file, `phpunit.xml`, that needs to go in the root coresites directory. Here are the suggested contents:

```xml
<phpunit
    bootstrap="./tests/includes/bootstrap.php"
    backupGlobals="false"
    colors="true"
    convertErrorsToExceptions="true"
    convertNoticesToExceptions="true"
    convertWarningsToExceptions="true">
    <testsuites>
        <testsuite>
            <directory prefix="test-" suffix=".php">./tests/</directory>
        </testsuite>
    </testsuites>
</phpunit>
```

Finally, PHPUnit uses a separate Wordpress configuration file, `wp-tests-config.php`, also provided on the repo. 

### Create test database
PHPUnit requires a empty Wordpress database with some additional test tables. To create it download and extract the `test_db.zip` and import into your MySQL instance with:

```
unzip fm_wordpress_tests.zip
mysql --user=root --default-character-set=utf8 --comments < fm_wordpress_tests.sql
```

Suggested test directory structure

```
/coresites
|   phpunit.xml
+-- tests
|   +-- includes <- Wodpress files required for bootstrapping.
|   +-- helpers
|     test-thumbor_helper.php
|     test-author_helper.php
|     ...
|   +-- plugins
|     ...
|   wp-tests-config.php

```

In order to run all tests all that's needed is ```phpunit```. This will bootstrap Wordpress and run all test-*.php files in the default folder. You can also run a specific test, like so `phpunit tests/test-author_helper.php`.

## Guidelines
Please read [https://phpunit.de/manual/current/en/writing-tests-for-phpunit.html](https://phpunit.de/manual/current/en/writing-tests-for-phpunit.html).


## Examples
The following is a very basic sample unit test for our AuthorHelper class.

```php
<?php

class AuthorHelperTest extends PHPUnit_Framework_TestCase{

  public function setUp() {
    $this->author = AuthorHelper::get_author_data(1);
  }

  public function testGetDisplayName() {
    $this->assertEquals($this->author->data->display_name, 'admin');
  }

  public function testEmail() {
    $this->assertEquals($this->author->data->user_email, 'admin@example.org');
  }

  public function testBadAvatar() {
    $this->assertFalse($this->author->avatar_url);
  }
}

?>
```


## Resources
* [PHPUnit is the official testing framework chosen by the core team to test our PHP code.](https://make.wordpress.org/core/handbook/testing/automated-testing/phpunit/)
* [PHPUnit Manual](https://phpunit.de/manual/5.0/en/index.html)
* [Adding Database Tests to Existing PHPUnit Test Cases](http://digitalsandwich.com/adding-database-tests-to-existing-phpunit-test-cases/)
* [Unit Testing Tutorial Part I: Introduction to PHPUnit](https://jtreminio.com/2013/03/unit-testing-tutorial-introduction-to-phpunit/)
