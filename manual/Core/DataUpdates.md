# Data Updates

The `DataUpdates` class is a wrapper for handling the `dataUpdates` socket events. It is a set and forget what you can apply updates to WebDataModels without the need for fetching to update the data thus limiting bandwidth and data usage. The `DataUpdates` class will also update the cache with the current data so there is no dirty data in cache.

For example, if a user has the game open on their phone and on desktop and makes a change to their lineup on the phone it will automatically update on the desktop version as well.

## Examples

This simple example shows how to register to the dataUpdates events and handle the incoming data.

*TODO: this could be done better using the EventBus system using generics. Rather than doing than doing type checks at the callback method level. Seems a bit cumbersome to me. I don't know why I did it that way.*

```csharp
private void Start()
{
    DataUpdates.DataUpdateEvent += OnDataUpdated;
}

private void OnDestroyed()
{
    DataUpdates.DataUpdateEvent -= OnDataUpdated;
}

private void OnDataUpdated(SocketOperation operation, object data)
{
    // early return if the data expecting is not the data received.
    if (!(data is LineupModel lineup))
        return;

    switch (operation)
    {
        case SocketOperation.Create:
            // handle the creation of a new lineup
            break;

        case SocketOperation.Delete:
            // handle the deletion of a lineup
            break;
        
        case SocketOperation.Patch:
            // handle line data being changed
            break;

        default:
            throw new ArgumentOutOfRangeException();
    }
}
```