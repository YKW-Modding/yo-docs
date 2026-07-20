---
title: Blank Event XQ
layout: default
has_children: true
parent:	Event/Battle XQs
grand_parent: Decompiled/Sample XQs
---

# Blank Event XQ

Blank event template for doing basic dialogue. Tested on YW1 & 2.

> [!NOTE]
> You can click the clipboard icon on the top right of the code block below to copy the text.

```php
Main()
{
    // insert here any logic you want to occur before the event
    $local1 = log("do stuff here");

    // now we have the event logic
    $local1 = StartTalkEvent();
    $local1 = BrightFadeAllN(30f);
    $local1 = EventTalk_CameraZoomStartNoWait();
    $local1 = EventTalkRun("ev77_1000_010"); // REPLACE THIS
    // note: MAKE SURE to register talkers in the washamap!
    $local1 = EventTalk_CameraZoomEndNoWait();
    $local1 = EventTalk_CameraZoomWait();
    $local1 = EndTalkEvent();
    $local1 = sub2502(1, -1f, 0f);

    // insert here any logic you want to occur after the event
    $local1 = log("do stuff here");
}
```
