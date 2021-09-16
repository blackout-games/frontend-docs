# API 

See https://rugbyapi.docs.apiary.io/ for more in depth API documentation

This guide will only show you how to do a simple API request on the front end.

For all of these requests its important to use async/await over promises as its more performant as well as being easier to read. Promises should be phased out over time.

[API Docs](https://blackout-games.github.io/frontend-docs/api/Core/Blackout.Core.Web.API.html)

## GET

This example shows you can get all players within a club

```csharp

// get data from server
var res = await API.GetAsync("players", $"?filter[club]={clubId}");

// log any errors
if (res.IsError())
{
    B.LogError(res.Exception);
    return;
}

// deserialise data
var players = res.Data.ToModelArray<PlayerModel>();

// adds players to cache
players.AppendManyToCache();

// you can now do what you will
foreach(var player in players)
{
    B.Log($"Player Name: {player.Name}");
}

```

## POST

This example shows how to create a new club

```csharp

var body = WebDataModel.CreateRequestBody("clubs",
                ("country", "nz"),
                ("name", "A silly club name"),
                ("sourceClub", currentClubId));

// send data to server
var res = await API.PostAsync("clubs", body);

// log any errors
if (res.IsError())
{
    B.LogError(res.Exception);
    return;
}

// deserialise data
var club = res.Data.ToModel<ClubModel>();

// adds players to cache
club.AppendToCache();

// you can now do what you will
B.Log($"New club created! {club.Name}");

```


## PATCH

This example shows how to change the username for the manager

```csharp

var body = WebDataModel.CreateRequestBody("managers", managerId, ("username", "A silly name"));

// update data on server
var res = await API.PatchAsync("managers", body);

// log any errors
if (res.IsError())
{
    B.LogError(res.Exception);
    return;
}

// deserialise data
var manager = res.Data.ToModel<ManagerModel>();

// adds players to cache
manager.AppendToCache();

// you can now do what you will
B.Log($"New manager name: {manager.Name}");

```

## DELETE

This example shows how to delete notifications

```csharp

// delete all notifications
var res = await API.DeleteAsync("notifications/all");

// log any errors
if (res.IsError())
{
    B.LogError(res.Exception);
    return;
}

// remove from cache
Cache.Clear("notifications");

// done
B.Log("Notifications has been deleted");

```