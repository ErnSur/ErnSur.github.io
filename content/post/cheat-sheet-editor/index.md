---
title: "Unity Editor Scripting Cheat Sheet"
date: 2022-07-16T20:47:30+02:00
image: header.png
categories:
  - editor
  - unity
draft: false
---

## Asset Management
> **Asset**: An object imported from project file. One project file can be imported into many assets, like a png tileset image can be imported into many sprite assets.

- {{<doc AssetDatabase>}} CRUD for project assets
  - Operates on project relative paths and asset GUIDs
- {{<doc Resources>}} Allows you to find and access Objects including assets
  - Can load assets without project relative paths
- {{<doc AssetPostprocessor>}} & {{<doc AssetModificationProcessor>}} Take action after asset import
- {{<doc PrefabUtility>}} Inspect / Modify Prefabs
    - Instantiate prefabs in the scene as "prefab instances" and not regular game objects
    - Create prefab variants
    - Add components
    - Apply overrides
    - Supports {{<doc Undo>}} out of the box
- `UnityEditorInternal.InternalEditorUtility.LoadSerializedFileAndForget` Loads assets from outside of the project folder
- {{<manual `Special Folder Names` SpecialFolders>}}


### Entry points
- {{<doc InitializeOnLoadAttribute>}} & {{<doc InitializeOnLoadMethodAttribute>}} Allows you to initialize an Editor class when Unity loads, and when your scripts are recompiled
- {{<doc EditorApplication-delayCall>}} Delegate which is called once after all inspectors update.
- {{<doc EditorApplication-update>}} Similar to `Monobehaviour.Update` but for editor only use
## UI

### Entry points

#### Menu Items
- {{<doc MenuItem>}} Create context menu items
- {{<doc ContextMenu>}} Create object specific context menu items
- {{<doc ContextMenuItemAttribute>}} Create property specific context menu items
- {{<doc IHasCustomMenu>}} Defines a method to add custom menu items to an Editor Window
- {{<doc EditorApplication-contextualPropertyMenu>}} Callback raised whenever the user contex-clicks on a property in an Inspector
- `UnityEditor.SceneManagement.SceneHierarchyHooks` Add menu items to Hierarchy window
  - `addItemsToCreateMenu`
  - `addItemsToGameObjectContextMenu`
  - `addItemsToSceneHeaderContextMenu`
  - `addItemsToSubSceneHeaderContextMenu`

#### Editors
- {{<doc Editor>}} Create a custom "Inspector view" for the specific component or scriptable object
- {{<doc AssetImporters.AssetImporterEditor>}} Create a custom editor for the {{<doc AssetImporters.ScriptedImporter>}}
- {{<doc ObjectPreview>}} Create a custom preview for specific component or scriptable object
- {{<doc PropertyDrawer>}} Customize input field for your serializable c# type

#### Windows
- {{<doc EditorWindow>}} Create custom windows
- {{<doc PopupWindow>}} & {{<doc PopupWindowContent>}} Create a popup window
- {{<doc ScriptableWizard>}} Editor window designed for quick object creation tools
- {{<doc SettingsProvider>}} Create custom view in Preferences or ProjectSettings windows

#### Addons
- {{<doc Overlays.Overlay>}} Customizable panels and toolbars
- {{<doc EditorTools.EditorTool>}} Custom tools are like the built-in Move, Rotate, Scale tools or component specific tools like colider bounds editor
- {{<doc EditorApplication-projectWindowItemOnGUI>}} Customize project window items
- {{<doc EditorApplication-hierarchyWindowItemOnGUI>}} Customize hierarchy window items
- {{<doc Editor-finishedDefaultHeaderGUI>}} Add UI to the default header drawer in Inspector Window

### UI Systems
- {{<manual `UI Toolkit` UIElements>}}
  - Namespaces `UnityEngine.UIElements` and `UnityEditor.UIElements`
- {{<manual `IMGUI (Legacy)` GUIScriptingGuide>}}
  - Static classes: {{<doc GUI>}}, {{<doc GUILayout>}}, {{<doc EditorGUI>}}, {{<doc EditorGUILayout>}}

### Scene View UI

- {{<doc Gizmos>}} Draw things like tool handles or bounding boxes
- {{<doc DrawGizmo>}} Supply a gizmo renderer for any component
- {{<doc Handles>}} Custom 3D GUI controls and drawing in the Scene view
- {{<doc HandleUtility>}} Helper functions for Scene View style 3D GUI
- {{<doc Overlays.Overlay>}} Customizable panels and toolbars
  - {{<doc Overlays.ToolbarOverlay>}} 
  - {{<doc Overlays.ITransientOverlay>}}
### Utility
- {{<doc EditorGUIUtility-isProSkin>}} is user using a dark or light editor theme



## Serialization

- {{<doc JsonUtility>}} JSON serializer
- {{<doc EditorJsonUtility>}} JSON serializer that will include editor only values of the engine objects (`MonoBehaviour`, `ScriptableObject`, etc.)
- {{<doc SerializedObject>}} & {{<doc SerializedProperty>}} When you want to modify a component/asset avoid modifying it directly, instead use this API to properly save assets, scenes, support {{<doc Undo>}} etc.
- {{<doc Undo>}} When modifying scene objects or assets use this class to support undo feature
- {{<doc ISerializationCallbackReceiver>}} Add logic to serialization events 
- {{<doc MonoBehaviour.OnValidate>}} & {{<doc ScriptableObject.OnValidate>}} calls when a value changes in the Inspector
- {{<doc EditorPrefs>}} Save and load data. **Warning**: Editor prefs are shared between all unity projects on user mashine. For project/tool/sdk editor only settings use {{<doc ScriptableSingleton_1>}} or {{<doc ScriptableObject>}} instead.
- {{<doc ScriptableSingleton_1>}} In classes that derive from ScriptableSingleton, serializable data you add survives assembly reloading in the Editor. Also, if the class uses the FilePathAttribute, the serializable data persists between sessions of Unity.
## Native Popups

- {{<doc EditorUtility.DisplayDialog>}} Get confirmation from the user
- {{<doc EditorUtility.DisplayPopupMenu>}} Open context menu
- Get the user select path
  - {{<doc EditorUtility.OpenFilePanel>}}
  - {{<doc EditorUtility.OpenFilePanelWithFilters>}}
  - {{<doc EditorUtility.OpenFolderPanel>}}
  - {{<doc EditorUtility.SaveFilePanel>}}
  - {{<doc EditorUtility.SaveFilePanelInProject>}}
  - {{<doc EditorUtility.SaveFolderPanel>}}
- Progress bar
  - {{<doc EditorUtility.DisplayProgressBar>}}
  - {{<doc EditorUtility.ClearProgressBar>}}
- {{<doc EditorGUIUtility.GetMainWindowPosition>}} Gets editor window rect

## Icons

> Functions that load or set editor icons will find the appropriate texture for the active editor theme if certain conditions are met  
> - Icon textures for light and dark themes are in the same directory
> - Icons for the dark theme are indicated by the prefix: "d_"
> - The icon name or path passed to the function argument was pointing to the light version of the texture
>   
> **Example**:  
> Given that these files exist:  
> - _Icons/ScriptIcon.png_
> - _Icons/d_ScriptIcon.png_
> 
> `monoImporter.SetIcon(AssetDatabase.LoadAssetAtPath<Texture2D>("Icons/ScriptIcon.png"));`  
> In the above example editor shows different mono importer icons depending on the active editor theme.

- [Iconography](https://www.foundations.unity.com/fundamentals/iconography)
- {{<doc EditorGUIUtility.IconContent>}} Load icon texture from editor resources. Find icons in [Editor Icon Browser](https://github.com/ErnSur/unity-editor-icons)
- {{<doc MonoImporter.SetIcon>}} Sets a custom icon to associate with the imported MonoScript.
- {{<doc PluginImporter.SetIcon>}} Sets the custom icon to associate with a MonoScript imported by a managed plugin (like .dll).
- {{<doc EditorGUIUtility.SetIconForObject>}} Set icon texture for GameObject or MonoScript.
- {{<doc EditorGUIUtility.SetIconSize>}} Change IMGUI size of the icon.


## Advanced / Tricks
- {{<doc CustomEditorForRenderPipelineAttribute>}} Tells an Editor class which run-time type it's an editor for when the given RenderPipeline is activated
- {{<doc IMGUI.Controls.TreeView>}} IMGUI TreeView
- {{<doc IMGUI.Controls.AdvancedDropdown>}} Dropdown with pages as in "Add Component" button.
- Open _About Unity_ menu and type "internal" to toggle editor developer mode.
  - Reload and inspect the editor window from its tab context menu
  - Enable "Debug-Internal" inspector mode from the tab context menu
  - Bunch of other internal tools, you can look for those in [Unity C#Reference repo](https://github.com/Unity-Technologies/UnityCsReference)


## Misc
- {{<doc EditorGUIUtility.hierarchyMode>}} Reason why `Foldout` can behave differently in inspector view. [Unity Answers](https://answers.unity.com/questions/1320999/editorguifoldout-docs-wrong.html)
- `EditorInternalUtility.IsValidFileName`


## Resources

- [Unity Editor Foundations](https://www.foundations.unity.com/)
- [Unity C#Reference repo](https://github.com/Unity-Technologies/UnityCsReference)
- [My GitHub projects](https://github.com/ernsur "https://github.com/ernsur") & [Stars](https://github.com/ErnSur?tab=stars "https://github.com/ErnSur?tab=stars")
- [Unity Forums](https://forum.unity.com/forums/ui-toolkit.178/)

## Useful Internals

Those are internal classes/methods that I found and thought might be useful. As with any internal stuff your need to keep in mind that this functionality is subject to change.

- `ScriptAttributeUtility.GetFieldInfoFromProperty`
- `ScriptAttributeUtility.GetDrawerTypeForType`
- You can create an assembly with a name like "Unity.InternalAPIEditorBridge.<index>" that has access to the editor's internal API. To find possible indexes search [editor source code](https://github.com/Unity-Technologies/UnityCsReference/blob/master/Editor/Mono/AssemblyInfo/AssemblyInfo.cs)

## Obscure
- {{<doc IApplyRevertPropertyContextMenuItemProvider>}}