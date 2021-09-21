# Cache

The cache is basically a dictionary jacked up on steroids and horse tranquilizers. 
Its where all the WebDataModels are stored, however it is generic and flexible enough to store any type of data its just that we generally only use for web models.

[API docs](https://blackout-games.github.io/frontend-docs/api/Core/Blackout.Core.Web.CacheSystem.Cache.html)

---

For all these methods use the namespace `Blackout.Core.Web.CacheSystem`

```csharp
using Blackout.Core.Web.CacheSystem;
```

Sometimes there is ambiguity with other classes in the solution called `Cache` in these cases use a direct using statement e.g.

```csharp
using Cache = Blackout.Core.Web.CacheSystem.Cache;
```

Rider will do both of these automatically for you so you don't need to worry about it but just in case you are crazy enough not to use Rider or just wanted the knowledge.

---

# Adding

## Add Single Item

```csharp
var customClass = new MyClass();
Cache.Add("item-key", customClass);
```

## Add Multiple Items 

```csharp
var customClasses = new List<MyClass>();
Cache.AddMany("item-key", customClasses);
```

## Add While Fetching

You can automatically store the fetched data from backend to the cache. Currently this is primarily done via the RSG Promise library, however, this should be phased out and UniTask's should be used in stead.

*Note: This will always override the currently stored value.*

### Promise

```csharp
Cache.Add("managers", () => {
    API.Get("managers/me")
        .Then(res => res["data"].ToModel<ManagerModel>())
});
```

### UniTask

```csharp
var task = API.AddAsync("countries/nz");
var nz = await Cache.GetAsync<CountryModel>("countries", task);
```

# Getting

```csharp
var customClass = Cache.Get<MyClass>("item-key");
```

## Get While Fetching

*Note: The fetch will only happen if the key doesn't exist. If the key does exist the cached value will be used. If you want to override the current value you need to use `Cache.Add()`*


### Promise

```csharp
Cache.Get("managers", () => {
    API.Get("managers/me")
        .Then(res => res["data"].ToModel<ManagerModel>())
});
```

### UniTask

```csharp
var task = API.AddAsync("countries/nz");
var nz = await Cache.GetAsync<CountryModel>("countries", task);
```

# Appending

Appending is used for a stored collection of items. Unlike `Cache.Add()` which will override the stored value each time `Cache.Append()` will add the new item to the collection.

```csharp
// appends the item to the collected. This wont override the item, i.e. there can be duplicate models stored
Cache.Append("countries", nz);

// appends item as unique instance. i.e. replaces model within collection (most likely used) 
Cache.AppendUniq("countries", nz, c => c.Id == nz.Id);
// or the extension method since its used so often
nz.AppendToCacheUniq();

// can append many models at once too
new List<CountryModel>().AppendManyToCacheUniq();
```

# Helpers

## Waiting

I've added some helper methods for some situations where you know else where the cache is going to be populated with the data you need so there is no need for a fetch action to be made. Instead you can just use a waiting coroutine for that data to be populated.

```csharp
// wait for an entire collection
var wait = new Cache.WaitForCollection<CountryModel>("counties");
yield return wait;
var result = wait.Result; // returns IEnumerable<T>

// wait for a single item
var wait = new Cache.WaitForItem<CountryModel>("counties");
yield return wait;
var result = wait.Result; // returns T

// wait for an item within a collection
var wait = new  Cache.WaitForItemInCollection<CountryModel>("counties", x => x.Id == "nz");
yield return wait;
var result = wait.Result; // returns T
```


## Refreshing

You can also refresh a single item with in a collection

```csharp
var clubModel = await Cache.RefreshModelInCollectionAsync<ClubModel>(WebConfig.Clubs.Type, LocalSettings.currentClubId);
```