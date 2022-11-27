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
- {{<doc AssetDatabase>}} CRUD of Unity Editor
- {{<doc AssetPostprocessor>}} & {{<doc AssetModificationProcessor>}} Take action after asset import
- {{<doc PrefabUtility>}} Inspect / Modify Prefabs
    - Instantiate prefabs in the scene as "prefab instances" and not regular game objects
    - Create prefab variants
    - Add components
    - Apply overrides
    - Supports {{<doc Undo>}} out of the box
- `UnityEditorInternal.InternalEditorUtility.LoadSerializedFileAndForget` Loads assets from outside of the project folder

## UI

- Entry points:
  - {{<doc MenuItem>}} Create context menu items
  - {{<doc Editor>}} Create a custom "Inspector view" for the specific component (MonoBehaviour object)
  - {{<doc EditorWindow>}} Create custom windows
  - {{<doc SettingsProvider>}} Create custom view in Preferences or ProjectSettings windows
  - {{<doc PropertyDrawer>}} Customize input field for your serializable c# type
  - {{<doc EditorApplication-projectWindowItemOnGUI>}} Customize project window items
  - {{<doc EditorApplication-hierarchyWindowItemOnGUI>}} Customize hierarchy window items
  - {{<doc Editor-finishedDefaultHeaderGUI>}} Add UI to the default header drawer in Inspector Window

- Draw GUI
  - IMGUI
    - Static classes: {{<doc GUI>}}, {{<doc GUILayout>}}, {{<doc EditorGUI>}}, {{<doc EditorGUILayout>}}
  - UI Toolkit        
    - Namespaces `UnityEngine.UIElements` and `UnityEditor.UIElements`

## Scene View

- {{<doc Gizmos>}} Draw things like tool handles or bounding boxes
- {{<doc Overlays.Overlay>}} Panels like tool options or data

## Serialization

- {{<doc JsonUtility>}}
- {{<doc SerializedObject>}} & {{<doc SerializedProperty>}} When you want to modify a component/asset avoid modifying it directly, instead use this API to properly save assets, scenes, support {{<doc Undo>}} etc.
- {{<doc Undo>}} When modifying scene objects or assets use this class to support undo feature.

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
> - Icons for the dark theme are indicated by the prefix:
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
- {{<doc PluginImporter.SetIcon>}} Sets the custom icon to associate with a MonoScript imported by a managed plugin.
- {{<doc EditorGUIUtility.SetIconForObject>}} Set icon texture for GameObject or MonoScript.
- {{<doc EditorGUIUtility.SetIconSize>}} Change IMGUI size of the icon.

## Misc
- {{<doc EditorGUIUtility.hierarchyMode>}} Reason why `Foldout` can behave differently in inspector view. [Unity Answers](https://answers.unity.com/questions/1320999/editorguifoldout-docs-wrong.html)
- `EditorInternalUtility.IsValidFileName`
- `ScriptableSingleton<T>`


## Advanced / Tricks

- {{<doc IMGUI.Controls.TreeView>}} IMGUI TreeView
- {{<doc IMGUI.Controls.AdvancedDropdown>}} Dropdown with pages as in "Add Component" button.
- Open _About Unity_ menu and type "internal" to toggle editors developer mode.
  - Reload and inspect the editor window from its tab context menu
  - Enable "Debug-Internal" inspector mode from the tab context menu
  - Bunch of other internal tools, you can look for those in [Unity C#Reference repo](https://github.com/Unity-Technologies/UnityCsReference)

## Resources

- [Unity Editor Foundations](https://www.foundations.unity.com/)
- [Unity C#Reference repo](https://github.com/Unity-Technologies/UnityCsReference)
- [My GitHub projects](https://github.com/ernsur "https://github.com/ernsur") & [Stars](https://github.com/ErnSur?tab=stars "https://github.com/ErnSur?tab=stars")
- [Unity Forums](https://forum.unity.com/forums/ui-toolkit.178/)

# Useful Internals

Those are internal classes/methods that I found and thought might be useful. As with any internal stuff your need to keep in mind that this functionality is subject to change.

- `ScriptAttributeUtility.GetFieldInfoFromProperty`
- `ScriptAttributeUtility.GetDrawerTypeForType`
- You can create an assembly with a name like "Unity.InternalAPIEditorBridge.<index>" that has access to the editor's internal API. To find possible indexes search [editor source code](https://github.com/Unity-Technologies/UnityCsReference/blob/master/Editor/Mono/AssemblyInfo/AssemblyInfo.cs)