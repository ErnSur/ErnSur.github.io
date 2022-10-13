## Scriptable Object Messages

### Awake
- only fired once when the object is instanced
- Thus In Editor it wont be fired on entering playmode

Someone of the forum said:

> There are 3 different cases that will cause a ScriptableObject to call Awake()

1 - When it's created. (Editor Only)
2 - When a scene which contains an object that references the asset is loaded.
3 - When the asset is first selected in the project window IF Awake() hasn't already been called. (Editor Only)
>

this is because SO's are lazyli created in the editor. If you open the project and nothing in your editor layout is referencing this object it will not be created.

### OnEnable
- called when so is loaded

it will happen on
- Entering Playmode in Editor
- Recompile

Difference between loaded/created
- will awake be called after loading so after Resources.unloadUnusedassets?


 if you have a private field in a ScriptableObject, it WILL NOT RESET each run. I.e., if you make a bool Initialized;, then once true it will stay true until you leave the editor. Huge potential source of confusion.

 to solve this you can use [NonSerialized] attribute



 ### Resources
 - https://forum.unity.com/threads/scriptableobject-behaviour-discussion-how-scriptable-objects-work.541212/
 - https://forum.unity.com/threads/solved-but-unhappy-scriptableobject-awake-never-execute.488468/