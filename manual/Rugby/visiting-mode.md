# Visiting Mode
<H2>Scenes and screens that support visiting mode:</H2>

- Competitions
  + leagues
- Squad
  + player-info/table view
- Grounds
- Club
  + club-info

<H2>How to set up a Scene for visiting mode</H2>
 
 Screens that allow Visiting mode should use ClubModel.Active instead of ClubModel.Current
 
 BlackoutScene.cs has two lists:

- DisabledWhileVisiting
- UninteractableWhileVisiting 

Within the Scene drag gameobjects that are to be disabled into `DisabledWhileVisiting` (including Screens), and drag Selectable components that should not be interactable in Visiting mode into UninteractableWhileVisiting.

`BlackoutScene.cs` has a listener for the `SceneChangedMessage` that runs a virtual method called `OnSceneChange`.
Within Scenes inheriting `BlackoutSene.cs` that are supporting visiting mode you will need to override this method and call the base, like the following:
```
protected override void OnSceneChange(SceneChangedMessage msg)
{
  base.OnSceneChange(msg);
  if (IsVisiting)
  {
    ReloadScreenSwipe(screenSwipe);
    return;
  }
}
```

You will need to pass the Scenes screenswipe into the `ReloadScreenSwipe` method, this will reload the screen swipe pages and also refresh the Topbar Pagination so that screens that don't allow visiting cannot be accessed.

Scenes where visiting mode isn't allowed but are still wanting to access the `SceneChangedMessage` should override the method but **NOT** call `base.OnSceneChange(msg)`

<H2>How Visiting Mode Works</H2>

`BlackoutScene.ActiveClubId` is changed to the club id that is being visited

`_appUrlWhenVisitingStarted` is set to the navigaton url for the scene/screen where visiting mode was started from, this is later used to return to that scene/screen when visiting mode is exited

Once `ActiveClubId` is set, anywhere accessing `ClubModel.Active` or `BlackoutScene.ActiveClubId` will get the visited clubs ClubModel/Id. This is then used to load that clubs data instead of the currently logged in club.

`LocalSettings.currentClubId` stores your currently logged in club Id, this is accessed through `ClubModel.Current` and used in Scenes and Screens that do not allow visiting.
