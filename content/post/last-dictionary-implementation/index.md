---
title: "Last Dictionary Implementation"
draft: true
---

With Unity 2020.1 we finally got serialization for generic types, with it I think it the time to create a definitive serializable dicrionary type for unity.

there has allways been some solution out there that provided serializable dictionary type with property drawer so that you could edit keys and values form the inspector. None of them are ideal tho.

Beside the actual API the UX of of p mentioned propertty drawers leaves a lot of room for improvements.

Well there is one implementation that has it moslty right:

<Odin Inspector iamge>

This is obviously Odin Inspector, but this is a paid solution that is not open source.
So my goal right now is to create the best implementation of serializable dictionary for Unity with MIT license.
This should be easy.

Tasks that I know from the start I want to do:

Property drawe:
- inline key and value if both of them are one line property drawers.
    - create a foldout if there are not
- IDictionary, API shoult not differ form regulat System.Generics.Collections impelemntation

