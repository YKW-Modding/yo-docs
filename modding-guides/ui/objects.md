---
title: "§2: Objects"
layout: default
parent: UIs
grand_parent: Modding Guides
nav_order: 2
---

# §2: Objects

## Introduction
All content on a screen can be defined as one of the following three types:
  * Objects
    * There are several types of Objects, this guide will cover `Object2D`, `Object3D` and `Mobiclip`, as others are not strictly necessary.
    * Objects are the basis on which a UI rests, any other form of visual content is attached to an Object. For example, images are simply an `Object2D` with an attached image resource.
  * Object properties
    * This includes, but is not limited to: position, screen, rotation, tint, scale, and text.
    * Keep in mind that text is not an individual component, it is merely a property of an `Object2D`.
  * Resources
    * This includes Images, Fonts, T2bs (`.cfg.bin`), Models, ANM resources, XSky files, and much more.

This categorisation does not include auxiliary content not rendered to a screen such as Audio, Filters and Cameras.
Additionally, not all resources are visual in nature. For example, t2b files (`.cfg.bin`) are merely data stores, and are therefore fundamentally not directly visible on screen.

## Behaviour & Usage

An Object is not inherently visible, its properties determine what is rendered to the screen.
For instance, to render an image one may create an Object2D, load the image resource and assign it to said Object2D.

An Object is identified by its name, or more precisely, the CRC-32 hash of the name.
Please note the following terminology: screen A (0) refers to the top screen whereas screen B (1) refers to the bottom.
Additionally, note that the position `(0, 0)` actually refers to the top-left corner of the screen as increasing the YPos moves an Object downwards, not upwards within Level5's engine.

## Core Instructions

To create an Object, one may use `create_*` instructions, namely:
  * `create_object_2d(string/hash: name, int: screen, int: XPos, int: YPos)`
    * Note that there are more parameters, but they are optional and thus will not be covered in this page.
  * `create_object_3d(string/hash: name, int: screen, int: XPos, int: YPos)`

To delete Object(s), one may use `*_delete` and `*_delete_all` instructions, namely:
  * `object_2d_delete(string/hash: name)`
  * `object_2d_delete_all()`
  * `object_3d_delete(string/hash: name)`
  * `object_3d_delete_all()`

> [!WARNING]
> It is preferred to check if an Object exists before deleting it. You can do so using the instructions below:

To check if an Object exists, one may use `*_is_exist` instructions, namely:
  * `object_2d_is_exist(string/hash: name)`
  * `object_3d_is_exist(string/hash: name)`

## Text
As mentioned above, text is a property of an Object2D, it can be assigned using `set_object_2d_text(string/hash: name, string: text)`.
Text does not automatically wrap, one must insert line breaks manually to account for this.
Please note that `set_object_2d_text` does not automatically escape Control Codes, Ruby Text (In JPN copies) or Glyphs (see [this page](../modding-resources/ctrl-codes.html)).
Therefore, these will still take effect. If directly linked to user input, this could lead to outcomes such as playing of unwanted audio.
Instead of utilising Colour Control Codes, one may set the colour of text using `object_2d_set_text_color`, which is often preferred.
This instruction has two different behaviours depending on the amount of arguments passed. Both will be shown below: <!-- TODO: list palette enum -->
  * `object_2d_set_text_color(string/hash: name, int: palette)`
  * `object_2d_set_text_color(string/hash: name, float: r, float: g, float: b)`
    * These three parameters are expected to be in range `0f`-`1f`. This is the recommended variant, and is the most common within Level5's own code.

Finally, the font, along with other properties, can be changed using `set_text_format`, although this will be explained in detail in a later section.

## Examples

First, let's expand the previous example from §1 to render some text!
Now after clicking A, after the modal appears, the words "Hello, world!" will appear at coordinates (100, 100) on screen A (the top screen).

```php
NotARealMenu()
{
  while(!ctrl_btn_held(5)) {
    yield;
    if ctrl_btn_held(6) {
      Menu_MessageInfoDraw("You have pressed A!");
      create_object_2d("HelloText", 0, 100, 100);
      set_text_format("HelloText", 512); // required for the text to render
      object_2d_set_text_color("HelloText", 1f, 0.5f, 0f); // we set colour before writing text, otherwise it will not apply to the text
      set_object_2d_text("HelloText", "Hello, world!");
      object_2d_move("HelloText", 100, 100); // sync the text position and object position so the text does not render at (0, 0)
    }
  }
}
```

<!-- TODO: expand ## Examples -->

<!-- TODO: cover mobiclip and its hybrid behaviour -->
