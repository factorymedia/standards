# Factory Media CoffeeScript unit testing Guide

## Table of Contents

  1. [Tools](#tools)
  2. [Guidelines](#guidelines)
  3. [Resources](#resources)
  
## Tools (This is a suggested setup, loads of room to improve!)
The suggested setup uses independent libraries that work great with each other and allows for the freedom to change components if better ones come along later on.

- [1.1](#1.1) Test Framework: [mocha](http://mochajs.org/)
Mocha provides a flexible framework allows for testing javascript both for client and server, and can be run from the command line or within a browser. 
It also can be complemented easily with other libraries for greater flexibility.

### Instalation and setup
It makes sense to install mocha globally:
```
npm install -g mocha
```
Create a ```test``` directory and place a ```mocha.opts``` file in there with the following content:

```
--reporter spec
--debug
--compilers coffee:coffee-script/register
--recursive
--colors
```

In order to run all tests all that's needed is ```mocha```. This should look for all tests in the default folder and it's subdirectories, and load the default options file.


- [1.2](#1.2) Assertions: [chai](http://chaijs.com/)

Chai provides server assertions in several both BDD and TDD styles. 

Sample chai assertions:
```
expect(media).not.to.be.undefined
expect(template.label).to.exist
expect(username.innerHTML).to.equal @video.user.username
expect(views.innerHTML).to.contain @video.views
```

Install chai as a project development dependency with:
```
npm install -D chai
```

- [1.3](#1.3) Spies, Mocks and Stubs: [synon](http://sinonjs.org/)

From their website:

"Standalone test spies, stubs and mocks for JavaScript.
No dependencies, works with any unit testing framework."

Sample synon uses:
```

```

Install synon as a project development dependency with:
```
npm install -D synon
```

- [1.4](#1.4) DOM implementation: [jsdom](https://github.com/tmpvar/jsdom)

When running the tests from the command line without a browser, jsdom provides an easy way to create an environment where DOM elements or load third party libraries to satisfy our test requirments. This would provide a ```document```d and ```window``` objects you can refer to.

Install jsdom as a project development dependency with:
```
npm install -D jsdom
```

- [1.5](#1.5) Bring it all together on a spec_helper file

Mocha will automatically load the spec_helper file, there's no need to require it in every test. 

Sample spec_helper.coffee
```
chai = require 'chai'
sinon = require 'sinon'
jsdom = require 'jsdom'

# Setup DOM
document = jsdom.jsdom '<html><body></body></html>', features:
    FetchExternalResources: ['script']
    ProcessExternalResources: ['script']
global.document = document
global.window = document.defaultView

# Setup globals
global.expect = chai.expect
global.sinon = sinon
```

## Guidelines
Please read [https://github.com/mawrkus/js-unit-testing-guide#unit-tests](https://github.com/mawrkus/js-unit-testing-guide#unit-tests). 
:)



## Resources
* [Setting up Unit Testing with Mocha and Chai](https://egghead.io/lessons/javascript-how-to-write-a-javascript-library-setting-up-unit-testing-with-mocha-and-chai)
* [Unit Testing with Mocha and Chai](https://egghead.io/lessons/javascript-how-to-write-a-javascript-library-unit-testing-with-mocha-and-chai)
* [How to Write Your First Test With Mocha](http://webapplog.com/mocha-test/)
* [How To Use Mocha With Node.js For Test-Driven Development](http://webapplog.com/tdd/)
* [Unit Test like a Secret Agent with Sinon.js](http://elijahmanor.com/unit-test-like-a-secret-agent-with-sinon-js/)
* [Unit test your client-side JavaScript (uses jsdom)](http://krasimirtsonev.com/blog/article/unit-test-your-client-side-javascript-jsdom-nodejs)
* [Writing Testable JavaScript](http://alistapart.com/article/writing-testable-javascript)
