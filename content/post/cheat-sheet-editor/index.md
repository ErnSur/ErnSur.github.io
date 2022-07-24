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

## UI

- Entry points:
  - {{<doc MenuItem>}} Create context menu items
  - {{<doc Editor>}} Create custom "Inspector view" for specific component (MonoBehaviour object)
  - {{<doc EditorWindow>}} Create custom windows
  - {{<doc SettingsProvider>}} Create custom view in Preferences or ProjectSettings windows
  - {{<doc PropertyDrawer>}} Customize input field for your serializable c# type
  - {{<doc EditorApplication-projectWindowItemOnGUI>}} Customize project window items
  - {{<doc EditorApplication-hierarchyWindowItemOnGUI>}} Customize hierarchy window items

- Draw GUI
  - IMGUI
    - Static classes: {{<doc GUI>}}, {{<doc GUILayout>}}, {{<doc EditorGUI>}}, {{<doc EditorGUILayout>}}
  - UI Toolkit        
    - Namespaces `UnityEngine.UIElements` and `UnityEditor.UIElements`

- Misc
  - {{doc EditorApplication}}
    - delayCall
  - EditorCoroutines

## Scene View

- {{<doc Gizmos>}} Draw things like tool handles or bounding boxes
- {{<doc Overlays.Overlay>}} Panels like tool options or data

## Serialization

- {{<doc JsonUtility>}}
- {{<doc SerializedObject>}} & {{<doc SerializedProperty>}} When you want to modify component/asset avoid modifying it directly, instead use this API to properly save assets, scenes, support {{<doc Undo>}} etc.
- {{<doc Undo>}} When modifying scene objects or assets use this class to support undo feature.

## Native Popups

- {{<doc EditorUtility.DisplayDialog>}} Get confirmation from user
- {{<doc EditorUtility.DisplayPopupMenu>}} Open context menu
- Make user select path
  - {{<doc EditorUtility.OpenFilePanel>}}
  - {{<doc EditorUtility.OpenFilePanelWithFilters>}}
  - {{<doc EditorUtility.OpenFolderPanel>}}
  - {{<doc EditorUtility.SaveFilePanel>}}
  - {{<doc EditorUtility.SaveFilePanelInProject>}}
  - {{<doc EditorUtility.SaveFolderPanel>}}
- Progress bar
  - {{<doc EditorUtility.DisplayProgressBar>}}
  - {{<doc EditorUtility.ClearProgressBar>}}

## Advanced / Tricks

- {{<doc IMGUI.Controls.TreeView>}} IMGUI TreeView
- {{<doc IMGUI.Controls.AdvancedDropdown>}} Dropdown with pages as in "Add Component" button.
- {{<doc EditorGUIUtility.IconContent>}} Load icon texture from editor resources. More at [Editor Icon Browser](https://github.com/ErnSur/unity-editor-icons)
- Open _About Unity_ menu and type "internal" to toggle editors developer mode.
  - Reload and inspect editor window from its tab context menu
  - Enable "Debug-Internal" inspector mode from tab context menu
  - Bunch of other internal tools, you can look for those in [Unity C#Reference repo](https://github.com/Unity-Technologies/UnityCsReference)

## Resources

- [Unity C#Reference repo](https://github.com/Unity-Technologies/UnityCsReference)
- [My GitHub projects](https://github.com/ernsur "https://github.com/ernsur") & [Stars](https://github.com/ErnSur?tab=stars "https://github.com/ErnSur?tab=stars")
- [Unity Forums](https://forum.unity.com/forums/ui-toolkit.178/ "https://forum.unity.com/forums/ui-toolkit.178/")

## Examples

```cs
using UnityEditor;
using UnityEditorInternal;
using UnityEngine;

internal static class ContextMenuItems
{
    private const string RevertPrefabTransformMenuName = "CONTEXT/RectTransform/Revert Prefab Transform";

    [MenuItem("GameObject/Copy Transform #^C")]
    public static void CopyTransform(MenuCommand command)
    {
        var obj = command.context;
        if (obj == null)
            obj = Selection.activeGameObject;
        if (obj is GameObject go)
        {
            ComponentUtility.CopyComponent(go.transform);
            var sceneView = SceneView.lastActiveSceneView;
            if (sceneView != null)
                sceneView.ShowNotification(new GUIContent($"Copied {go.name} Transform"));
        }
    }

    [MenuItem("GameObject/Paste Transform #^V")]
    public static void PasteTransform(MenuCommand command)
    {
        var obj = command.context;
        if (obj == null)
            obj = Selection.activeGameObject;
        if (obj is GameObject go)
        {
            ComponentUtility.PasteComponentValues(go.transform);
            var sceneView = SceneView.lastActiveSceneView;
            if (sceneView != null)
                sceneView.ShowNotification(new GUIContent($"Paste {go.name} Transform"));
        }
    }

    [MenuItem(RevertPrefabTransformMenuName)]
    private static void RevertPrefabTransform(MenuCommand command)
    {
        var serObj = new SerializedObject(command.context);
        var prop = serObj.GetIterator();
        while (prop.NextVisible(true))
        {
            PrefabUtility.RevertPropertyOverride(prop, InteractionMode.UserAction);
        }
    }

    [MenuItem(RevertPrefabTransformMenuName, validate = true)]
    private static bool RevertPrefabTransformValidate(MenuCommand command)
    {
        var obj = command.context;
        return PrefabUtility.IsPartOfPrefabInstance(obj);
    }
}
```