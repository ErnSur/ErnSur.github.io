If youre creating a function that returns a boolean you can name this function in two different ways:

1. Describe why you are checking certain behaviour (bad)

example: ShouldExitApplication, CanOpenFile, 

2. describe conditions you are checking (good)

example: QuitButtonClicked, IsFileBeingUsed, 

second one is better because the name will stay the same regardless of the context in which you are using the function.

```
// its ok
if(ShouldExitApplication())
    Exit()

// this is less
if(ShouldExitApplication())
    ShowPopup()
    
// now it makes more sense
if(QuitButtonClicked())
    ShowPopup()
```
