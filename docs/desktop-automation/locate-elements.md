---
layout: default
title:  "Locate Elements"
excerpt: "Learn how to locate desktop elements with BELLATRIX desktop module."
date:   2021-06-22 06:50:17 +0200
parent: desktop-automation
permalink: /desktop-automation/locate-elements/
anchors:
  example: Example
  explanations: Explanations
  available-create-methods: Available Create Methods
  find-multiple-elements: Find Multiple Elements
  available-createall-methods: Available CreateAll Methods
  find-nested-elements: Find Nested Elements
  available-create-methods-for-finding-nested-elements: Available Nested Create Methods
  available-createall-methods-for-finding-nested-elements: Available Nested CreateAll Methods
---
Example
-------
```csharp
[Test]
public void MessageChanged_When_ButtonHovered_Wpf()
{
    var button = App.Components.CreateByName<Button>("E Button");

    button.Hover();

    Console.WriteLine(button.By.Value);

    Console.WriteLine(button.WrappedComponent.Coordinates);
}
```

Explanations
-------
```csharp
var button = App.Components.CreateByName<Button>("E Button");
```
There are different ways to locate elements in the app. To do it you use the element create service. You need to know that BELLATRIX has a built-in complex mechanism for waiting for elements, so you do not need to worry about this anymore. Keep in mind that when you use the Create methods, the element is not searched. All elements use lazy loading.Which means that they are searched once you perform an action or assertion on them. By default on each new action, the element is searched again and be refreshed.
```csharp
Console.WriteLine(button.By.Value);
```
Because of the proxy element mechanism (we have a separate type of element instead of single WebDriver IWebElement interface) we have several benefits. Each control (element type- ComboBox, TextField and so on) contains only the actions you can do with it, and the methods are named properly. In vanilla WebDriver to type the text you call **SendKeys** method. Also, we have some additional properties in the proxy web control such as- By. Now you can get the locator with which you element was found.
```csharp
Console.WriteLine(button.WrappedComponent.Coordinates);
```
You can access the WebDriver wrapped element through WrappedElement and the current WebDriver instance through- WrappedDriver

Available Create Methods
------------------------
BELLATRIX extends the vanilla WebDriver selectors and give you additional ones.
### CreateByTag ###
```csharp
App.Components.CreateByTag<Button>("button");
```
Searches the element by its tag.
### CreateById ###
```csharp
App.Components.CreateById<Button>("myId");
```
Searches the element by its ID.
### CreateByXpath ###
```csharp
App.Components.CreateByXpath<Button>("//*[@title='Add to cart']");
```
Searches the element by XPath locator.
### CreateByClass ###
```csharp
App.Components.CreateByClassContaining<Button>("ul.products");
```
Searches the element by its CSS classes.
### CreateByName ###
```csharp
App.Components.CreateByName<Button>("products");
```
Searches the element by its name.
### CreateByAccessibilityId ###
```csharp
App.Components.CreateByAccessibilityId<Button>("myCustomButton");
```
Searches the element by its accessibility ID.
### CreateByAutomationId ###
```csharp
App.Components.CreateByAutomationId<Search>("search");
```
Searches the element by its automation ID.  

Find Multiple Elements
----------------------
Sometimes we need to find more than one component. For example, in this test we want to locate all Add to Cart buttons.
To do it you can use the element create service CreateAll method.

```csharp
[Test]
public void CheckAllAddToCartButtons()
{
    App.Navigation.Navigate("http://demos.bellatrix.solutions/");

    var blogLink = App.Components.CreateAllByXpath<Anchor>("//*[@title='Add to cart']");
}
```

Available CreateAll Methods
------------------------
### CreateAllByTag ###
```csharp
App.Components.CreateAllByTag<Button>("button");
```
Searches the elements by its tag.
### CreateAllById ###
```csharp
App.Components.CreateAllById<Button>("myId");
```
Searches the elements by its ID.
### CreateAllByXpath ###
```csharp
App.Components.CreateAllByXpath<Button>("//*[@title='Add to cart']");
```
Searches the elements by XPath locator.
### CreateAllByClass ###
```csharp
App.Components.CreateAllByClassContaining<Button>("ul.products");
```
Searches the elements by its CSS classes.
### CreateAllByName ###
```csharp
App.Components.CreateAllByName<Button>("products");
```
Searches the elements by its name.
### CreateAllByAccessibilityId ###
```csharp
App.Components.CreateAllByAccessibilityId<Button>("myCustomButton");
```
Searches the elements by its accessibility ID.
### CreateAllByAutomationId ###
```csharp
App.Components.CreateAllByAutomationId<Search>("search");
```
Searches the elements by its automation ID.

Find Nested Elements
----------------------
Sometimes it is easier to locate one element and then find the next one that you need, inside it.
For example in this test the list box is located and then the button inside it.

```csharp
public void ReturnNestedElement_When_ElementContainsOneChildElement_Wpf()
{
    var comboBox = App.Components.CreateByAutomationId<ComboBox>("listBoxEnabled");
    var comboBoxItem = comboBox.CreateByAutomationId<Button>("lb2");

    comboBoxItem.Hover();
}
```

**Note**: *It is entirely legal to create a Button instead of **TextField**. BELLATRIX library does not care about the real type of the elements. The proxy types are convenience wrappers so to say. Meaning they give you a better interface of predefined properties and methods to make your tests more readable.*

Available Create Methods for Finding Nested Elements
----------------------------------------------------
### CreateByTag ###
```csharp
component.CreateByTag<Button>("button");
```
Searches the element by its tag.
### CreateById ###
```csharp
component.CreateById<Button>("myId");
```
Searches the element by its ID.
### CreateByXpath ###
```csharp
component.CreateByXpath<Button>("//*[@title='Add to cart']");
```
Searches the element by XPath locator.
### CreateByClass ###
```csharp
component.CreateByClassContaining<Button>("ul.products");
```
Searches the element by its CSS classes.
### CreateByName ###
```csharp
component.CreateByName<Button>("products");
```
Searches the element by its name.
### CreateByAccessibilityId ###
```csharp
component.CreateByAccessibilityId<Button>("myCustomButton");
```
Searches the element by its accessibility ID.
### CreateByAutomationId ###
```csharp
component.CreateByAutomationId<Search>("search");
```
Searches the element by its automation ID.
### CreateAllByTag ###
```csharp
component.CreateAllByTag<Button>("button");
```
Searches the elements by its tag.
### CreateAllById ###
```csharp
component.CreateAllById<Button>("myId");
```
Searches the elements by its ID.
### CreateAllByXpath ###
```csharp
component.CreateAllByXpath<Button>("//*[@title='Add to cart']");
```
Searches the elements by XPath locator.
### CreateAllByClass ###
```csharp
component.CreateAllByClassContaining<Button>("ul.products");
```
Searches the elements by their classes.
### CreateAllByName ###
```csharp
component.CreateAllByName<Button>("products");
```
Searches the elements by its name.
### CreateAllByAccessibilityId ###
```csharp
component.CreateAllByAccessibilityId<Button>("myCustomButton");
```
Searches the elements by its accessibility ID.
### CreateAllByAutomationId ###
```csharp
component.CreateAllByAutomationId<Search>("search");
```
Searches the elements by its automation ID.