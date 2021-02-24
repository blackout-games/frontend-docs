# Overview
Unity currently doesn't have any events to subscribe to for device orientation changes, so this is our solution. 


# Orientation

Basic run down:
- Any custom script you wish to be manipulated by orientation changes i.e TraningScreen, it should implement `IOrientationChangable` this only has an `OrienationChanged(bool isPortrait)` method. You don't *need* to, but it will life so much easier if we need to debug as we can just do a scene search for that interface. 
NOTE: You will still need to add these scripts to a parent `OrienationListener` as they are not self subscribing.
- If you want to create a self subscribing scrip like `MoveUI.cs` then you will need to implement the `IOrienationListener` interface. You can then use `Orienation.Sub()` in `OnEnabled()/OnDisabled()` methods to add/remove the listener. `Orientation.Sub()` can also be assed to a constructor which is good for subscripting a gameObject that insn't enabled in the inspector yet. This is used in `MoveUI.cs`
NOTE: `Orientation.cs` will remove any redundant listeners but it is still good practice to manually use `Orientation.UnSub()`.


# MoveUI

`MoveUI.cs` implements the `IOrientationListener` interface and registers itself to orientation changes
- No need to add it to a parent OrienationListener :champagne: 
- This will still work in PlayMode, EditorMode, and PrefabEditingMode


# Orientation Window

`OrientationWindow.cs` in `Blackout > Window > Orientation Window` shows you to view all the registered `IOrientationListener`'s in all currently loaded scenes. It will also scan the PrefabStage while in editing mode (there was an edge case I had to work around)

![](/img/orientation-window.gif)

# Orientation Listener

`OrienationListener.cs` is a `MonoBehaviour` and is good for doing generic orientation tasks like enable/disable gameObjects or changing sprites or whatever a MoveUI doesn't do

On the `OrienationListener` script there is a "Scan" button (pictured) which will help you find any children objects which implement the IOrienationChangable interface, which is just a OnOrientationChanged(bool) function. It will exclude any instances where its already been added to the inspector. Its just a helper so you don't miss any.

![](/img/orientation-script.png)

