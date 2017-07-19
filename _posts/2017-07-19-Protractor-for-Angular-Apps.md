---
title: Protractor for Angular Apps
author: Menaka De Silva
linkedin: https://lk.linkedin.com/in/menaka-de-silva-496b7618
---

**Background**

Most modern web apps use Angular for a rich and efficient front-end user
experience. Angular supports multiple platforms and increases app
performance and productivity. Web UI test automation frameworks such as
Selenium WebDriver have limitations when interacting with Angular apps.
The locators in these tools do not include Angular specific html
attributes. Also these tools aren’t aware of the page load time of an
Angular web page, which results in using extensive Sleep() and Wait()
method calls in the code. Most of you might have done this in your
selenium tests, which causes our tests to fail because the UI elements
are not ready even after so much of waiting.

**Introducing Protractor**

The Angular team has come up with a solution for this limitation,
Protractor – a test tool written on top of WebDriverJS. It is a Node.js
program and runs tests written in JavaScript. Protractor actually uses
WebDriver to control browsers and simulate user actions. This results in
performing actions on UI elements as a real user would. It is written as
a wrapper for the JavaScript Selenium WebDriver. It includes the
capabilities of WebDriver and additional accessors to Angular models and
bindings.

Protractor supports JavaScript and Node.js based frameworks such as
Jasmine and Mocha. However, Jasmine is the default framework that comes
with Protractor, so we will be using Jasmine syntax in our examples.

Let’s look at how we can install and start using Protractor to automate
some simple scenarios.

1.  Download and install Node.js. Check the installed Node.js version by
    running node –version in command prompt.
    <pre>
         node –version
    </pre>

2.  Install JDK to run the standalone Selenium server. Check the
    installed java version by running java –version in command prompt.
    <pre>
        java –version
    </pre>

3.  Install Protractor by running
        <pre> npm install -g protractor </pre>

4.  Start up a Selenium server instance. Check the installed Protractor
    version by running
    <pre>
      protractor --version
     </pre>

5.  Protractor needs two files to run: The Conf.js and Spec.js.

6.  Create Spec.js file.

<img src="/img/Protractor1.png" width="319" height="14" /><img src="/img/Protractor2.png" width="346" height="16" /><img src="/img/Protractor3.png" width="559" height="212" />

> This is a simple test method to launch a URL and login using valid
> credentials. Then asserting using ‘expect’ if a particular title
> appears on the UI. The ‘describe’ and ‘it’ syntax comes from the
> Jasmine framework. ‘browser’ and ‘element’ are global variables in
> Protractor.

1.  Create Conf.js file.

<img src="/img/Protractor4.png" width="329" height="93" />

> Conf.js file has the configuration details Protractor needs. It
> specifies what spec files should be run, what is the framework to use
> (Jasmine), name of the selenium server etc. The defaults are used for
> other configurations. (E.g., default browser: Chrome)

1.  Set up a selenium server instance: Open up command prompt and run:

    <pre> webdriver-manager start </pre>

2.  Run protractor: Open up command prompt and run protractor conf.js

You will see a chrome browser window open up and navigate to the
specified URL, then perform the login action. Once the assertion is
passed using the ‘expect’, then a test execution log will be displayed
on the command prompt.

Multiple test scenarios:

If you want to add multiple scenarios in one spec.js file then it would
look something like this:

<img src="/img/Protractor5.png" width="509" height="344" />

The ‘beforeEach’ and multiple ‘describe’ ‘it’ keywords will support
having multiple scenarios on the same spec.js file. Congratulations! You
just ran your first Protractor test!

**How does Protractor work so well with Angular UIs?**

1.  **ElementFinder vs WebElements**

As we discussed earlier, WebDriver fails to work on Angular UIs because
of the limitations in WebDriver locators and failure to predict the page
load time. When a driver.findElement() is called and a WebElement object
is returned, the WebDriver immediately sends a command to the browser
asking it to locate the element.

> IWebElement button = driver.findElement (locator);

If we want to perform a button.click(), then this action fails because
the page is not loaded yet for the browser to locate the element.
Angular performs a life cycle operation in the backend before the UI is
fully loaded, if the UI elements are not loaded instantly it will fail
to perform any action on it. This will result in our entire Selenium
test method to fail.

In Protractor, the element() global variable will locate the element,
but will not send a command to the browser until an action (E.g.
button.click()) is performed.

> var button = element(locator);

This will store the locator information until an action is performed.
The button.click() will tell the browser to find the element and click
it. This will allow the action to be performed only when the UI elements
are loaded, therefore actions on Angular UI elements is more reliable in
Protractor.

2.  **Asynchronous scripting provides a synchronous control flow**

Protractor methods are asynchronous and return promises. Let us look at
an example to understand this.

<img src="/img/Protractor3.png" width="559" height="212" />

In the above example test script, we can write the commands in an
asynchronous style. However, in Protractor, WedDriverJS actually
maintains a queue of pending promises: this is called the control flow.
Therefore, Method 2 in this test script will run only after Method 1 is
resolved. This makes Protractor tests much more reliable because the
Method 2 sendkeys() only happens when the UI element is ready for the
action. These features make Protractor a powerful tool to use against
Angular UIs.

References: <br/>

[http://](http://www.protractortest.org/)[www.protractortest.org](http://www.protractortest.org/)

[http://](http://testautomation.applitools.com/)[testautomation.applitools.com](http://testautomation.applitools.com/)

[https://](https://docs.angularjs.org/guide/e2e-testing)[docs.angularjs.org/guide/e2e-testing](https://docs.angularjs.org/guide/e2e-testing)

[https://evanhahn.com/how-do-i-jasmine](https://evanhahn.com/how-do-i-jasmine/)[/](https://evanhahn.com/how-do-i-jasmine/)

[https://](https://github.com/angular/protractor/blob/master/docs)[github.com/angular/protractor/blob/master/docs](https://github.com/angular/protractor/blob/master/docs)
