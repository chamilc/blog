---
title: Running Protractor tests on Visual Studio
author: Menaka
linkedin: https://lk.linkedin.com/in/menaka-de-silva-496b7618
---

What would you do if you already have a test framework written on pure
selenium webdriver, and then the system under test is upgraded to an
angular UI? Well, you will have to rewrite your tests entirely, all over
again... Not really! You can still use your existing test framework to
work on the new Angular UI with minimal changes. If your tests are
written on .NET, fortunately there’s a .NET port of Protractor built on
top of Selenium WebDriver.

Let’s look at how we can integrate Protractor in our selenium test
framework written on C\# .NET.

1.  Open up your test automation solution on visual studio.

2.  Browse for the Protractor package against your test solution. Select
    the projects you want to install Protractor to, and then
    click ‘install’.

<img src="/img/ProtractorVS1.png" width="665" height="452" />

**Figure 1**

1.  You will see your installed Protractor version in packages.config.

<img src="/img/ProtractorVS2.png" width="640" height="111" />

**Figure 2**

1.  Navigate to where you have initialized the WebDriver object. In our
    example project, WebDriver is initialized in the Global.cs class
    under the Example.TestAutomation.Scripts project.

<img src="/img/ProtractorVS3.png" width="701" height="551" />

**Figure 3**

Notice how the Protractor namespace is used and an object of
NgWebDriver(Protractor Driver) is created. In addition, there is an
IWebDriver object defined as well.

1.  In the same class, we will have a \[AssemblyInitialize\] type of a
    method, which will allow creating a new NgWebDriver instance by
    wrapping the IWebDriver instance we created as shown in the
    following code:

<img src="/img/ProtractorVS4.png" width="487" height="341" />

The \[AssemblyInitialize\] method will run before running all the tests
in the assembly. Therefore, the driver initialization happens in the
beginning before any test is run. Later on, any test that uses the
Driver object of IWebDriver type is actually using the NgWebDriver
object. Whenever a test uses:

Driver.FindElement(By.XPath("Xpath"));

It is actually referring to the NgWebDriver. This allow all tests to use
Protractor in their element identification with only few modifications
to the Global.cs class.

1.  Remove all the Driver.Wait() or Thread.Sleep() you have used in your
    test methods, because now we don’t need to wait until the UI
    elements are ready. Protractor driver (NgWebDriver) understands the
    time to wait for Angular UIs.

2.  Save your work, and run your test methods, which now will run
    lightning fast!


