# Modals

There are 3 types of modals which we use and have defined as:
    
- **Prompt Modal**: This is the simplest modal type.
    - Its height can be predetermined and wont be changed after showing. i.e it doesn't have tabs which can alter its height.
    - Will always be anchored centre screen with a centre pivot point.
- **Rich modal:**
    - Height can be changed after showing. Maybe be because of tabs or dynamically added list items etc.
    - Anchored towards top of screen with pivot centre top. This allows the content to dynamically change height without the top bar of the modal changing position on screen. 
- **Overlay Modal:**


## Modal Manager

So basically everything in a rich modal needs to be contained within the Modal prefab as pictured.
Each panel that is dynamically toggled needs to use a ModalContentPanel script. Which You don't need to do anything after applying it. I could add extra features in future.
These panels communicate directly with Modal.cs which figures out what size it needs to tween to.
Each ModelContentPanel needs to follow one rule and that is LayoutGroup Child Force Expand Height needs to be false. It could be true, and it does work sometimes, but generally I've found having that off just eliminates a lot of height issues.
There is one optional setting for Layout Element Flexible Height set to 0 helps eliminate height issues as well.
The height issues I've run into are:
panel height grows infinitely (Child Force Expand FALSE solves that)
children expand unwantedly (Flexible Height 0 solves that)
So far that's it. The rest takes care of its self. If you do work out why those fixes need to be that way it would be great otherwise I don't see it being too cumbersome to have them as strict rules.