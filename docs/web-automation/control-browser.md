---
layout: default
title:  "Control Browser"
excerpt: "Learn how to control browsers with BELLATRIX web module."
date:   2021-02-12 06:50:17 +0200
parent: web-automation
permalink: /web-automation/control-browser/
anchors:
  overview: Overview
  explanations: Explanations
  drivers-browsers-processes-cleanup: Drivers Browsers Processes Cleanup
  configuration: Configuration
  playwright: Playwright
---
Overview
--------

This is how one BELLATRIX test class looks like.
```csharp
[TestFixture]
[Browser(BrowserType.Firefox, Lifecycle.ReuseIfStarted)]
public class BellatrixBrowserLifecycleTests : WebTest
{
    [Test]
    public void PromotionsPageOpened_When_PromotionsButtonClicked()
    {
        App.Navigation.Navigate("http://demos.bellatrix.solutions/");

        var promotionsLink = App.Components.CreateByLinkText<Anchor>("Promotions");

        promotionsLink.Click();
    }

    [Test]
    [Browser(BrowserType.Chrome, Lifecycle.RestartOnFail)]
    public void BlogPageOpened_When_PromotionsButtonClicked()
    {
        App.Navigation.Navigate("http://demos.bellatrix.solutions/");

        var blogLink = App.Components.CreateByLinkText<Anchor>("Blog");

        blogLink.Click();
    }
}
```

Explanations
------------
```csharp
[TestFixture]
```
This is the main attribute that you need to mark each class that contains MSTest tests.
```csharp
[Browser(BrowserType.Firefox, Lifecycle.ReuseIfStarted)]
```
This is the attribute for automatic start/control of WebDriver browsers by BELLATRIX. If you have to do it manually properly, you will need thousands of lines of code. 
**BrowserType** controls which browser is used. Available options for Bellatrix.Web are:
- Chrome
- Firefox
- Edge
- Chrome in headless mode
- Firefox in headless mode.

**Note**: *Headless mode = executed in the browser but the browser's UI is not rendered, in theory, should be faster. In practice the time gain is little.*

The **Lifecycle** enum controls when the browser is started and stopped. This can drastically increase or decrease the tests execution time, depending on your needs. However you need to be careful because in case of tests failures the browser may need to be restarted.

Available options:
- **RestartEveryTime**- for each test, a separate WebDriver instance is created and the previous browser is closed. The new browser comes with new cookies and cache.
- **RestartOnFail**- the browser is only restarted if the previous test failed or if the previous test's browser was different.
- **ReuseIfStarted**- the browser is restarted only if the previous test's browser was different. 

**Note**: *Use last option with caution since if a test failure occurs all subsequent tests may also fail.*

```csharp
public class BellatrixBrowserLifecycleTests : WebTest
```
All web BELLATRIX test classes should inherit from the WebTest base class. This way you can use all built-in BELLATRIX tools and functionalities.
```csharp
[Browser(BrowserType.Firefox, Lifecycle.ReuseIfStarted)]
public class BellatrixBrowserLifecycleTests : WebTest
```
If you place the attribute over the class all tests inherit the Lifecycle. It is possible to place it over a given test to override the class Lifecycle only for that particular test.
```csharp
[Test]
public void PromotionsPageOpened_When_PromotionsButtonClicked()
```
All MSTest tests should be marked with the **TestMethod** attribute.
```csharp
App.Navigation.Navigate("http://demos.bellatrix.solutions/");
```
There is more about the App class in the next sections. It is the primary point for accessing BELLATRIX services. It comes from the **WebTest** class as a property. Here we use the BELLATRIX navigation service to navigate to the demo page.
```csharp
var promotionsLink = App.Components.CreateByLinkText<Anchor>("Promotions");
```
Use the element creation service to create an instance of the anchor. There are much more details about this process in the next sections.
```csharp
[Test]
[Browser(BrowserType.Chrome, Lifecycle.RestartOnFail)]
public void BlogPageOpened_When_PromotionsButtonClicked()
{
    App.Navigation.Navigate("http://demos.bellatrix.solutions/");

    var blogLink = App.Components.CreateByLinkText<Anchor>("Blog");

    blogLink.Click();
}
```
As mentioned above you can override the browser Lifecycle for a particular test. The global Lifecycle for all tests in the class is to reuse an instance of Edge browser. Only for this particular test, BELLATRIX opens Chrome and restarts it only on fail.

Drivers Browsers Processes Cleanup
------------
By default BELLATRIX includes internal module for handling residual browsers and drivers that for some reason weren't closed or killed. You can call the service to handle other processes too. Just call the static class **ProcessCleanupService**.
It is a bit tricky to handle such processes when parallel execution is enabled. We use a different logic in this case. You need to let BELLATRIX know if you will execute the tests in parallel. You need to set the isParallelExecutionEnabled to true in the testFrameworkSettings.json configuration file.
```
"processCleanupSettings": {
  "isParallelExecutionEnabled": "false"
},
```

Configuration
------------
If you don't use the attribute, the default information from the configuration will be used placed under the executionSettings section. Also, you can add additional driver arguments under the arguments section array in the configuration file.
```json
"executionSettings": {
  "executionType": "regular",
  "defaultBrowser": "chrome",
  "defaultLifeCycle": "restart every time",
  "resolution": "1920x1080",
  "browserVersion": "91",
  "url": "http://127.0.0.1:4444/wd/hub",
  "arguments": [
    {
      "name": "{runName}"
    }
  ]
}
```

Playwright
------------
Available options for **BrowserTypes** (equivalent to **BrowserType** in selenium module) are:
- Chromium
- Chrome
- Edge
- Firefox
- WebKit
- Chromium in headless mode
- Chrome in headless mode
- Edge in headless mode
- Firefox in headless mode
- WebKit in headless mode

Available options for **Lifecycle**:
- **RestartEveryTime**- for each test, a separate **Playwright**, **Browser**, **BrowserContext**, and **Page** instance are created and the previous ones are closed.
- **RestartOnFail**- the browser is only restarted if the previous test failed or if the previous test's browser was different.
- **ReuseIfStarted**- the **BrowserContext** and **Page** are closed and new ones are opened. The whole browser is restarted only if the previous test's browser was different.

Be mindful that **BrowserContext** holds information like cookies and session. To restart the **BrowserContext** is equivalent to clear the cookies in Selenium and much more.