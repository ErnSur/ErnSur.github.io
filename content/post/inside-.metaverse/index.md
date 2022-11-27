---
title: "Inside .metaverse"
description: "Hello"
draft: true
---

UnityEditor.ObjectWrapperJSON:{"guid":"c22777d6e868e4f2fb421913386b154e","localId":2100000,"type":2,"instanceID":21634}

# Unity Asset Serialization 

## Files

- .meta file
  - textures
- .asset
  - scriptableobjects
  - 
- prefab
  - What happen when you do ... on prefab/component:
    - JsonUtility.ToJson 
    - EditorJsonUtility.ToJson 
- scene
- anim

## Serialized data format

"When referencing objects outside this file, like a “Weapon” script referencing a “Bullet” Prefab, things get a little more complex. Remember that the File ID is local to the file, meaning it can be repeated in different files. In order to uniquely identify an object in another file, we need an additional ID or “GUID” that identifies the whole file instead of individual objects inside of it. Each asset has this GUID property defined in its meta file, which can be found in the same folder as the original file, with the exact same name plus a “.meta” extension added."

- obj reference :fileid, guid, type
Type 2: Assets that can be loaded directly from the Assets folder by the Editor, like Materials and .asset files
Type 3: Assets that have been processed and written in the Library folder, and loaded from there by the Editor, like Prefabs, textures, and 3D models


- UnityEngine.Object vs C# POCO
- dlls

## Utilities

- SerializedProperty
- Set Dirty
- gameobject Hide flags
- ISerializationCallbackReceiver

## Resources

- [Unity Serialization Blog Post](https://blog.unity.com/technology/understanding-unitys-serialization-language-yaml)
- [Class ID Reference](https://docs.unity3d.com/Manual/ClassIDReference.html)
