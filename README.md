# ConditionalHideAttribute

Author's article: https://www.brechtos.com/hiding-or-disabling-inspector-properties-using-propertydrawers-within-unity-5/

In this example most of the properties under the Auto Aim category will only be relevant when EnableAutoAim is set to true.
Say that, to avoid confusion and to keep our inspector nice and clean, we want to hide all the properties that are directly related to the EnableAutoAim if this property is set to false. Or instead of hiding them, we want to at least disable them, preventing user input.

![Example](https://github.com/user-attachments/assets/733f675b-854e-47c5-baee-1d6e6d719c01)


## How to use

As mentioned we are going to need 2 scripts: ConditionalHideAttribute and ConditionalHidePropertyDrawer.
Important to note is that the ConditionalHidePropertyDrawer needs to be placed inside an Editor folder within the project and the ConditionalHideAttribute scripts needs to be located outside this folder. Otherwise you will not be able to find and use the attribute.

![image](https://github.com/user-attachments/assets/13d863cc-503b-4832-b84f-9fe1ed668152)


Using our ConditionalHideAttribute is fairly straightforward and works much like the HeaderAttribute.

```csharp
[Header("Auto Aim")]
public bool EnableAutoAim = false;
 
[ConditionalHide("EnableAutoAim", true)]
public float Range = 0.0f;
[ConditionalHide("EnableAutoAim", true)]
public bool AimAtClosestTarget = true; 
[ConditionalHide("EnableAutoAim", true)]
public DemoClass ExampleDataClass = new DemoClass();
[ConditionalHide("EnableAutoAim", true)]
public AnimationCurve AimOffsetCurve = new AnimationCurve();
```

Leaving out the second parameter or setting it to false draws the property using the enable/disable method.

```csharp
[Header("Resources")]
public bool ConsumeResources = true;
 
[ConditionalHide("ConsumeResources")]
public bool DestroyOnResourcesDepleted = true;
[ConditionalHide("ConsumeResources")]
public float ResourceAmount = 100.0f;
[ConditionalHide("ConsumeResources")]
public DemoClass AnotherExampleDataClass = new DemoClass();
```


## Some Caveats

This being Unity after all there are a few situations where this custom attribute will fail.
The first situation being Lists and arrays.
In those cases the property drawer will be applied to the content of the list but not on the list itself… .
You will still be able to adjust the number of items as well.
However when the List is part of a Seriazable class everything will behave as expected.

Why is this you ask? Because Unity, that’s why.

![image](https://github.com/user-attachments/assets/d6b536e7-d12e-40c2-b9ab-a614c0678e30)

![image](https://github.com/user-attachments/assets/a3c61c49-3c69-4bc5-a9c4-d9109210fa35)


Another situation where the use of this attribute will fail is when being combined with different property drawers that change the way the property itself is being drawn such as the TextAreaAttribute.
This makes sense as this attribute performs his own OnGUI drawing and GetPropertyHeight calculation of the same property. The attribute that is called last will be the attribute that determines what is being drawn in this case.

