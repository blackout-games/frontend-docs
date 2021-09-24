# Tutorial

### How it works in a nutshell

Each `TutorialStep` there will be an action that the user must take. Nine times out of ten this is a click action on some UI. The highlighted UI component is controlled using `Canvas.sortingOrder` and `CanvasGroup.interactable`. 

For example, if a button is needing to be pressed the tutorial manager will access that button via the `TutorialHandler` script and move its `Canvas.sortingOrder` up above the tutorial blocker. It will then loop through all root canvases in the scene and recursively disable all children object's `CanvasGroup.interactable` property. This will ensure that only the highlighted object is able to be interacted with. Once the action has been taken or the condition is met within the step the tutorial will progress to the next step.

## Tutorial Manager

`TutorialManager.cs` is the brains of the whole thing. It also controls when the arrow pointer, button glow, and secretary are displaying. Once displaying the arrow pointer and secretary have their own internal logic. 

## Steps

Desktop and Mobile each have their own set of steps as mobile generally requires extra tabs to be opened and has slightly different text to display. Other than that the steps work the same way for all situations.

`TutorialStep.cs` is a `ScriptableObject` and contains the instantiation of `TutorialCallbacks` and `TutorialConditions`. 

## Handlers

Tutorial handlers are what progress the tutorial steps. Each handler needs a condition to be met.

All you need to do is supply a step for the handler to listen out for so it can register itself as the current handler. 

The handler then called `TutorialManager.Instance.SetCurrentTutorialHandler(this);` internally which the Tutorial manager will then active that handler as the current one. No other action needs to be taken.

### Click Handler

Progresses the tutorial once an item is clicked. There is also and option for clicking (or pressing) anyway on screen `clickAnywhere`. 

## Callbacks

`TutorialCallback` is used for handling any Tutorial actions from `TutorialStep` or anywhere else. It's derived from `MonoBehaviour` because you can set callbacks dynamically from scene as well from editor (just create empty GO - set script - drag it inside TutorialStep or TutorialHandler to set up callback.

You can extend this class to perform very specific tasks and before performing the current step. 

## Conditions

TutorialCondition is a `ScriptableObject` that can be used for making conditions before a step is performed. Its basically like a `Func<bool>` really at the end of the day. 

YOu can extend this class to have more specific conditions.

Its not used very option within Rugby and is probably something that could be phased out in favour of putting it within the callbacks or handler classes. However when I was refactoring the tutorial system I just left it as it wasn't doing any harm. 

## Edge Cases

`TutorialCanvasGroupEdgeCase` is a script I created to handle random edge case situations where the `TutorialManager` would turn off all `CanvasGroup` interactivity but you still need to be able to do things like use the `ScrollRect` or something similar.

The way you can get around that is apply this script to a gameObject. In the inspector assign the Step you need this functionality for and apply the CanvasGroups you need to be overridden. 
