---
title: How to convert Mp4 to Moflex
layout: default
parent: Modding Guides
---


# How to convert Mp4 to Moflex
**original guide by @komazuraaa on discord**


**make sure your video is exported to 480p before doing this!!!**

1. Get mobiclip multicore encoder.

2. get runasdate

3. run mobiclip enocder inside runasdate and set the date to earlier than 2023

4. Give hte program the license file that it comes with

5.  load up the mgraph attached at the end of this message.

6. double click the video files block.

7. change the input video to your video.

8.  double click the encoder block.

9. change the output dir to your desired output directory.

10. click queue job.

11. find your output MOFLEX in your output directory.

12. replace any moflex in the game with your video!

# mgraph (copy and paste into a 'graph.mpgrah` file and load it in the program):
```json
[
   {
      "Data" : {
         "DisplayWarningMessage" : false,
         "Draft" : false,
         "EnableAviSave" : false,
         "EstimatedSizeMode" : 0,
         "FilenameBitrate" : true,
         "FilenameDraft" : true,
         "FilenameFps" : true,
         "FilenameOverwrite" : true,
         "FilenamePass" : true,
         "FilenameResolution" : true,
         "Input" : [
            {
               "AudioCompression" : 1,
               "Bitrate" : 0,
               "PassFilename" : "a3a7c1ed-7440-4089-9240-8d800b9e249e.pass",
               "StreamType" : 1,
               "Timeline" : false
            },
            {
               "AudioCompression" : 1,
               "Bitrate" : 0,
               "PassFilename" : "a68f62ba-5de1-4d9a-9c7c-dd36fc3e54dc.pass",
               "StreamType" : 0,
               "Timeline" : false
            },
            {
               "AudioCompression" : 1,
               "Bitrate" : 0,
               "PassFilename" : "16964235-b9aa-450d-8b99-473765524d87.pass",
               "StreamType" : 0,
               "Timeline" : false
            },
            {
               "AudioCompression" : 1,
               "Bitrate" : 0,
               "PassFilename" : "1fb962c5-7a6e-4141-955a-399ee7d98b83.pass",
               "StreamType" : 0,
               "Timeline" : false
            },
            {
               "AudioCompression" : 1,
               "Bitrate" : 0,
               "PassFilename" : "03053242-54e6-471c-a816-ea753b0b9314.pass",
               "StreamType" : 0,
               "Timeline" : false
            },
            {
               "AudioCompression" : 1,
               "Bitrate" : 0,
               "PassFilename" : "f167d241-9c0f-4c35-9da1-50f06b6ab471.pass",
               "StreamType" : 0,
               "Timeline" : false
            },
            {
               "AudioCompression" : 1,
               "Bitrate" : 0,
               "PassFilename" : "c31234b9-8da5-46ef-9b97-4ace6c0aec98.pass",
               "StreamType" : 0,
               "Timeline" : false
            },
            {
               "AudioCompression" : 1,
               "Bitrate" : 0,
               "PassFilename" : "bce1f1da-1548-4d95-840e-d1c144f2ea31.pass",
               "StreamType" : 0,
               "Timeline" : false
            }
         ],
         "KeyframeInterval" : 30.0,
         "MaxFileSize" : 128.0,
         "OutputDecoded" : true,
         "OutputDirectory" : "C:\\MobiclipOut",
         "OutputFilename" : "",
         "OutputFormat" : 0,
         "OutputLogInfo" : false,
         "PreloadTime" : 0.0,
         "Qmax" : 48,
         "Qmin" : 12,
         "SynchronizeTimelines" : true,
         "TargetPlatform" : 0,
         "TwoPass" : true,
         "Uid" : "4a61fb06-9ee6-4fdf-bd6a-8098f0c16fef",
         "WinX" : -1,
         "WinY" : -1
      },
      "Filter" : "MobiclipEncoder",
      "Uid" : 2457660243040,
      "x" : 351,
      "y" : 132
   },
   {
      "Data" : {
         "Directory" : "C:\\",
         "Filenames" : [ "SampleVid.mp4" ],
         "Index" : 0,
         "WinX" : -1,
         "WinY" : -1
      },
      "Filter" : "VideoFiles",
      "Next" : [
         {
            "Pin" : 0,
            "Uid" : 2457660243520
         }
      ],
      "Uid" : 2457660240880,
      "x" : 85,
      "y" : 362
   },
   {
      "Data" : {
         "AspectRatio" : 0.0,
         "ResizeMode" : 1,
         "ResizeType" : 2,
         "UserCrop" : {
            "Down" : 0,
            "Left" : 0,
            "Right" : 0,
            "Up" : 0
         },
         "UserCropEnabled" : false,
         "UserResize" : {
            "Height" : 240,
            "Width" : 800
         },
         "UserResizeEnabled" : true,
         "WinX" : -1,
         "WinY" : -1
      },
      "Filter" : "CropResize",
      "Next" : [
         {
            "Pin" : 0,
            "Uid" : 2457660243040
         }
      ],
      "Uid" : 2457660243520,
      "x" : 296,
      "y" : 293
   }
]
```
