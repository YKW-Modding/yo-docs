---
title: Useful Modding Tools!
layout: default
has_children: true
nav_order: 5
---

# Useful Tools
## File Format Handlers

### **Kuriimu2**
Kuriimu2 is a fan translation toolkit that supports nearly every non-model related YW file format.
It can be found [here](https://github.com/FanTranslatorsInternational/Kuriimu2) and uses a hyper-flexible plugin system so you can easily contribute to the project (I myself am planning to make a PR to add support for Icon CTPK reading)
> *Note: Kuriimu2 dosent support text editing as of yet but its predecesor Kuriimu does!*

### **Kuriimu**
Kuriimu, Karameru, and Kukkii combined form the predecesor to **Kuriimu2**. These have the benefit of text editing despite being older! They can be found [here](https://github.com/IcySon55/Kuriimu).

### **Pinguoin**
Pingouin is a tool to extract archives (XFSA, XPCK, XFSP and even good ol' ZIP). It can be found [here](https://github.com/Tiniifan/Pingouin) although Kuriimu2 should be used instead.

### **CfgBin Editor**
This tool allows you to read, modify and export/import the `cfg.bin` (Binary config) format used by the games to store serialised JSON-like data. Some files namely `npcbin` and `mapenv.bin` stil use this format despite the name. The current tool by onepiecefreak can be found [here](https://github.com/onepiecefreak3/CfgBinEditor) - the predecesor by Tiniifan can be found [here](https://github.com/Tiniifan/CfgBinEditor) and the original version by Togenyan can be found [here](https://github.com/togenyan/CfgBinEditor).

### **XtractQuery**
XtractQuery is a command-line decompiler for Level5's `.xq` scripts (XQSEQ/XQ32) by onepiecefreak. It can be found [here](https://github.com/onepiecefreak3/XtractQuery/releases).

### **Nyanko**
Nyanko is a version of CfgBin Editor that *only supports text editing* as it is its main purpose for use translating. It can be found [here](https://github.com/Tiniifan/Nyanko).

### **Studio Eleven**
A blender plugin to add support for Level5 Models and Animations, it can be found [here](https://github.com/Tiniifan/studio_eleven) and is the current best way to edit them (despite it having some bugs here and there).

### **Metanoia**
Modified versions of Metanoia exist as tools for reading/editing Yo-kai Watch models. Use Studio Eleven instead - although Tiniifans fork of Metanoia can be found [here](https://github.com/Tiniifan/Metanoia) and another fork can be found [here](https://github.com/YKW-Modding/Metanoia/releases).

### Looping Audio Converter
Looping Audio Converter allows you to read and modify `BCSTM` files - used by the game to store its audio: [Looping Audio Converter](https://github.com/libertyernie/LoopingAudioConverter).

### Citric Composer
Citric Composer is another tool that can do this - Tiniifans fork, which is up-to-date and fixes a bug in the original can be found [here](https://github.com/Tiniifan/Citric-Composer).

### BCSAR-View
BCSAR-View is a tool that can also be used for sound files although this is unrecommended due to being older, no longer maintained and having less customisability/utility compared to [Looping Audio Converter](https://github.com/libertyernie/LoopingAudioConverter). BCSar-View can still be found [here](https://github.com/thane98/BCSAR-View) however.

### Mobius
Mobius is a tool used for viewing and modifying `moflex` files that are found in Yo-kai Watch 3DS titles. 
To use this tool you must have `ffmpeg.exe` in the same folder as the executable. FFMPeg can be found [here](https://ffmpeg.org/download.html)
Mobius may give you an error or a corrupted output if you don't configure the config right, try opening the .config in notepad and using this line instead of the old one: 
`<add key="options" value="-preset ultrafast -crf 16"/>`
This should fix the corrupted output issue.
Mobius can be found [here](https://github.com/AdibSurani/Mobius).

## General Tools

### **Albatros**
A general modding tool for Yo-kai Watch. It hasnt been updated in a while and only supports Yo-kai editing as of now. It can be found [here](https://github.com/Tiniifan/Albatross).

### **LuaDeobfuscator**
LuaDeobfuscator is a command line tool to deobfuscate decompiled `.lua` files from Level5 mobile games. The tool expects decompilations from `.lua.bin` files by `unluac`. LuaDeobfuscator can be found [here](https://github.com/onepiecefreak3/LuaDeobfuscator).



lorem ipsum dolar sit amet: if you see this call me an idiot
😔
