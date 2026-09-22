---
title: Phase
layout: default
has_children: true
parent: Modding Resources
---

# Phase
Phase (not to be confused with `QuestPhase` - which is 8-bit and denotes Quest progress) is a large unsigned 32-bit integer (a number between 0 and 4,294,967,295) which combined with flags and watch rank serves to tell the game how far you are into the story.

The current phase can be obtained in CExpressions via the `GetPhase()` function.

Phase is actually two numbers, the chapter number and the subphase; when `GetPhase` is called it returns `(ChapterNo * 1000) + SubPhase` for simplicity.
The following sub-pages will, for each game, list some common phase boundaries and what part of the story they belong to, for reference.
