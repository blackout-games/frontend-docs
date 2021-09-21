# Troubleshooting

Troubleshooting code is the number one skill you should focus on. Its not the most fun but it is highly valued and you should be very well equip with all concepts of Profiling and Debugging using breakpoints in the IDE. Below I will give an overview of how I approach these things but I strongly urge you do you own self study.

If there are performance issues these are simple steps to take

## Profiling

Enable deep profile and play the troublesome section of the game. You should see a spike when the issue happens. Click on that which will pause the game. You can then breakdown in the profiles hierarchy what method is heavy. You can then inspect that method to find out what is causing the spike. Common issues generally are things like:  
- Instantiation - Too many gameObjects instantiating/destroying in a single frame
- Using `FindObjectOfType<T>()` or `GetComponent<T>()` too many times
- Garbage Collection - methods like LINQ
- Methods being called multiple times on single frame
- Badly done loops, e.g. looping over a list multiple times etc.

## Breakpoints

Breakpoints may not help with performance troubleshooting as much as the above approaches but it can help tackle bugs within the code where variables are being assigned wrong values or code is not executing when it should be. You can create a breakpoint which will help visualise the code as you step through it.

*Note: Please note that using this method will only allow you to look at the code within the IDE, Unity will be unuseable once execution is paused. To step through using the Unity editor use [Debug.Break()](https://docs.unity3d.com/ScriptReference/Debug.Break.html)*

If you are unfamilar with how to debug code I suggest you read [Unity's documentation](https://docs.unity3d.com/Manual/ManagedCodeDebugging.html) and watch [Jason Weimann's video](https://youtu.be/uFFexymk3FY)

## Stopwatch

I've made a helper class `BStopwatch` which uses `System.Diagnostics.Stopwatch` under the hood. It is another way to find troublesome code and help narrow down the cause in conjunction with the Profile.

A simple way to do this is to instantiate an instance of the stopwatch and log the elapsed time after each logic block within a method.

Example:
```csharp
private void HeavyMethod()
{
    var sw = new BStopwatch(true); // true will start the stopwatch straight away

    foreach(var item in someList)
    {
        // thicc boy loop logic
    }

    B.Log($"Loop time: {sw.ElapsedMilliseconds()}"); // the stopwatch will by default restart

    Method();

    B.Log($"Method time: {sw.ElapsedMilliseconds()}");

    AnotherMethod();

    B.Log($"AnotherMethod time: {sw.ElapsedMilliseconds()}");
}
```

Doing this way you can see how long each logic block takes which narrows your search

## Debug.Break()

[API Docs](https://docs.unity3d.com/ScriptReference/Debug.Break.html)

Similar to breakpoints within the IDE Unity also provide a way to pause execution. Unlike breakpoints this will allow you to use the Unity editor and step through frame by frame within the game. Breakpoints only allow you to look at the code.
