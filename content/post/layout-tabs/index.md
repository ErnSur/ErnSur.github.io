---
title: "Side project: Layout Tabs"
draft: true
---

As much time as I had working in Unreal Engine I had also the opportunites to hate and love some of its features.

So I decided to try and port one of them to Unity.

The [Editor Tabs](https://docs.unrealengine.com/4.27/en-US/Basics/UI/InterfaceOverview/#editortabs)

I started with a simple PoC:
Each layout tab would load the editor layout just as layout dropdown does. The difference would be that before switching from layout A to layout B I would save it.
<Explain logic>
It seemed to work just fine


BUT

in 2021 Unity disabled the option to set min size of docked editor widnwos

so I thought to add those tabs to main toolbar


it seemd to work without that much hussle in spite of using reflection. Or so I thought.
