---
layout: default
title:  "KeyboardService"
excerpt: "Learn how to use BELLATRIX iOS KeyboardService."
date:   2021-11-22 06:50:17 +0200
parent: ios-automation
permalink: /ios-automation/keyboard-service/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```csharp
[TestFixture]
[IOS(Constants.IOSNativeAppPath,
    Constants.IOSDefaultVersion,
    Constants.IOSDefaultDeviceName,
    Lifecycle.RestartEveryTime)]
public class KeyboardServiceTests : IOSTest
{
    [Test]
    public void TestHideKeyBoard()
    {
        var textField = App.Components.CreateById<TextField>("IntegerA");
        textField.SetText(string.Empty);

        App.Keyboard.HideKeyboard();
    }
}
```

Explanations
------------
BELLATRIX gives you an interface for easier work with device's keyboard through **KeyboardService** class.
```csharp
App.Keyboard.HideKeyboard();
```
Hides the keyboard.