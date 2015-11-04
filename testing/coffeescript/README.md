# Factory Media CoffeeScript Unit Testing Guide

## Table of Contents

  1. [Tools](#tools)
  2. [Guidelines](#guidelines)
  3. [Sample unit tests](#examples)
  4. [Resources](#resources)

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

### Suggested test directory structure

```
.
+-- test
|   +-- helpers
|     spec_helper.coffee
|   +-- advertising
|     polar_spec.coffee
|   mocha.opts

```
Create a ```helpers``` folder to add the spec_helper.coffee and any other helpers your tests might need.

Add a folder per "module", for example ```advertising``` on this case. Place inside this folder all the tests for advertising components.

The ```mocha.opts``` file needs to be on the root of the test folder in order to be detected.

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

Please make sure you are running node 4.x or upwards. To easily run multiple node versions you can use [n](https://github.com/tj/n):
```
npm install -g n
n latest
```

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


## Examples
The following example tests a 'sharing buttons' component built with React. It makes sure that the control provides the minimal UI components for a user to embed a video and also makes sure the component behaves correctly based on a video's embedabble property, i.e. don't allow private videos to be embedded.

This example uses a ```before``` hook that runs once andcreates an instance of our sharing_buttons component. Then it uses a ```beforeEach``` that runs before each test and makes sure a fresh copy of the component is rendered.

```
describe 'Sharing buttons', ->

  before ->
    @SharingButtons = require '../app/components/sharing_buttons.cjsx'

  beforeEach ->
    @sharingButtonsInstace = utils.renderIntoDocument  <@SharingButtons data={videos.videos[0].videos[0]}/>

  it 'Provides an input to change the iframe width', ->
    expect(document.getElementById 'iframe-width').not.to.equal null

  it 'Provides an input to change the iframe height', ->
    expect(document.getElementById 'iframe-height').not.to.equal null

  it 'Provides an input to change the iframe width', ->
    expect(document.getElementById 'iframe-width').not.to.equal null

  it 'Shows resulting iframe html code', ->
    expect(document.getElementById 'video-embed-iframe').not.to.equal null

  it 'Shows resulting embed url', ->
    expect(document.getElementById 'video-embed-url').not.to.equal null

  it 'Shows resulting iframe html code', ->
    expect(document.getElementById 'video-embed-iframe').not.to.equal null

  it 'Shows a button to copy the iframe html code', ->
    expect(React.findDOMNode @sharingButtonsInstace.refs.copyIframe).not.to.equal null

  it 'Shows a button to copy the embed url', ->
    expect(React.findDOMNode @sharingButtonsInstace.refs.copyUrl).not.to.equal null

  it 'Shows embed button if video is embedable', ->
    video = videos.videos[0].videos[0]
    video.embeddable = true
    @sharingButtonsInstace = utils.renderIntoDocument  <@SharingButtons data={video}/>
    expect(React.findDOMNode @sharingButtonsInstace.refs.embedButton).not.to.equal null

  it 'Hides embed button if video is not embedable', ->
    video = videos.videos[0].videos[0]
    video.embeddable = false
    @sharingButtonsInstace = utils.renderIntoDocument  <@SharingButtons data={video}/>
    expect(React.findDOMNode @sharingButtonsInstace.refs.embedButton).to.equal null
```

This other example uses a spy (provided by sinon.js) to make sure a function is called based on an user action.

````
describe 'VideoForm', ->

  beforeEach ->
    VideoForm = require '../app/components/video_form.cjsx'
    @dummyHandler =  @sandbox.spy()
    @videoForm = utils.renderIntoDocument  <VideoForm handleSubmit={@dummyHandler} a={1}/>

  (...skipping tests to get to the cool example...)

  it 'POSTS the video to the Videos API on submit', ->
    @videoForm.basicInfo = {category_slug:'BMX'}
    button = TestUtils.findRenderedDOMComponentWithTag @videoForm, 'button'
    TestUtils.Simulate.click button
    expect(@requestStub).to.have.been.called
````

## Resources
* [Setting up Unit Testing with Mocha and Chai](https://egghead.io/lessons/javascript-how-to-write-a-javascript-library-setting-up-unit-testing-with-mocha-and-chai)
* [Unit Testing with Mocha and Chai](https://egghead.io/lessons/javascript-how-to-write-a-javascript-library-unit-testing-with-mocha-and-chai)
* [How to Write Your First Test With Mocha](http://webapplog.com/mocha-test/)
* [How To Use Mocha With Node.js For Test-Driven Development](http://webapplog.com/tdd/)
* [Unit Test like a Secret Agent with Sinon.js](http://elijahmanor.com/unit-test-like-a-secret-agent-with-sinon-js/)
* [Unit test your client-side JavaScript (uses jsdom)](http://krasimirtsonev.com/blog/article/unit-test-your-client-side-javascript-jsdom-nodejs)
* [Writing Testable JavaScript](http://alistapart.com/article/writing-testable-javascript)
