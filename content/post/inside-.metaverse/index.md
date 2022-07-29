---
title: "Inside .metaverse"
description: "Hello"
draft: true
---


# Unity Assetx Serialization 

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

- fileid and guid
- UnityEngine.Object vs C# POCO
- dlls

## Utilities

- SerializedProperty
- Set Dirty
- gameobject Hide flags
- ISerializationCallbackReceiver

## Resources

- [Unity Serialization Blog Post](https://blog.unity.com/technology/understanding-unitys-serialization-language-yaml)