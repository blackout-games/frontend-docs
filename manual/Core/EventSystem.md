# EventBus

The EventBus system is a elegant way to decouple some parts of code. 

Do not use this every frame or in high usage situations as there is a bit of overhead due to dynamic invoke of delegates and class instantiation and garbage collection. In cases you need to use the message a lot it is suggested that you use normal C# System.Action or event delegates. 

[API Docs](https://blackout-games.github.io/frontend-docs/api/Core/Blackout.Core.EventSystem.EventBus.EventBus.html)

## Creating a new event message

All events must extend the `EventMessage` class.

```csharp

public class MyMessage : EventMessage
{
    public float MyValue {get;}

    public MyMessage(float value)
    {
        MyValue = value;
    }
}

```

## Publishing 

```csharp

EventBus.Publish(new MyMessage(0.69f)); // nice
EventBus.Publish(new MyMessage { MyValue = 0.69f }); // also nice

```

## Subscribing

```csharp

private void Start()
{
    EventBus
        .Subscribe<MyMessage>(MyEventCallback)
        .BindTo(this); // removes need to unsubscribe from OnDestroyed()

    // you can also use LINQ to filer messages with .Where()
    EventBus
        .Where<MyMessage>(x => x.MyValue > 0.69f) // will only invoke callback if this is true
        .Subscribe<MyMessage>(MyEventCallback)
        .BindTo(this); // removes need to unsubscribe from OnDestroyed()
}

private void MyEventCallback(MyMessage message)
{
    // handle the data
    B.Log(message.MyValue);
}

```

## Unsubscribing

Its generally best to use the `.Bind()` extension but if you need to you can unsubscribe from the event as well

```csharp

EventBus.Unsubscribe<MyMessage>(MyEventCallback);

```