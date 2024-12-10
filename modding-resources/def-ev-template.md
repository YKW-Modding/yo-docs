---
title: Blank Event XQ
layout: default
has_children: true
parent: Modding Resources
---

# Blank Event XQ

Blank event template for doing basic dialogue. Tested on YW1 & 2.

*Tip: Click the copy button on the top right*

```php
Main()
{
    //Anything that should happen before the talking starts
    $local1 = log("do stuff here");
    //Event:
    $local1 = StartTalkEvent();
    $local1 = BrightFadeAllN(30f);
    $local1 = EventTalk_CameraZoomStartNoWait();
    //Instead of "1", you should use the unhashed text section ID from your event text cfgbin
    $local1 = EventTalkRun("1");
    //make sure to register talkers in the washamap !
    $local1 = EventTalk_CameraZoomEndNoWait();
    $local1 = EventTalk_CameraZoomWait();
    $local1 = EndTalkEvent();
    $local1 = sub2502(1, -1f, 0f);

    //Anything that should happen after the talking is done
    $local1 = log("do stuff here");

}
```
