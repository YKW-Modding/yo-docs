---
title: XQ Mega Guide
layout: default
parent: Modding Guides
---

**Original guide by @komazuraaa. Guide INCOMPLETE.**
# Guide 0 - Setting up the decompiler
Step 1. Get XtractQuery from [here](https://github.com/onepiecefreak3/XtractQuery/releases/latest).
Step 2. Now we will set up XQ references, which are essentially hints that let us know what a function does. to do that, extract the entire `seq` folder of the game you are modding, and put it in a folder next to the executable called `reference`.
Step 4. Installing XtractQuery to PATH: Copy the path to your XtractQuery installation directory, which should have the XtractQuery executable and the reference folder. then, in your Windows search, look up "edit the system environment variables". open it, and click "Environment variables..." in the bottom right corner. Now, in the `User variables for [USER}` viewport, select the `Path` variable and click edit. Now click `New`, and paste the path to your XtractQuery installation directory. Click OK, then OK again, and exit the environment variables window. Now XtractQuery is fully installed.

Decompiling a XQ - to decompile any XQ, open Powershell or Command prompt in the folder with the XQ you wanna decompile and write: `XtractQuery -o e -f [filename.xq]`

Compiling a XQ -  to compile any XQ, open Powershell or Command prompt in the folder with the XQ you wanna compile and write: `XtractQuery -o c -t xq32 -f [filename.xq.txt]`

There you go! Now you know how to set up the decompiler and and decompile and compile any XQ!

# Guide 1 - Basic XQ syntax
This section of the guide assumes that you have basic programming knowledge with a language like Python or C-Sharp. If you don't have any programming experience,  I recommend [this](https://www.youtube.com/watch?v=fWjsdhR3z3c) video.

# Declaring variables
in XQ, variables don't have normal names like in other programming languages. to distinguish your variable from other variables, you need to call your variable `$local` and then a number. It's good practice to always go up with the numbers. for example, if you see that the variable with the biggest number in the current function is `$local10`, don't make something like `$local88` because you feel quirky, do `$local11`. Not following this convention could lead to unexpected behavior. There are other types of variables other than `local`, I'll go over them in a bit.

# Declaring functions
XQ function declarations are similar to languages like C# or C++, but XQ is dynamically written, so it's a little bit different.
for example, in C#, if I want to declare a function that returns a string, I would do
```cs
string foo() {
   return "bar";
}
```
but in XQ, functions can return any type and you don't need to specify types, so it would look like this:
```xq
foo() {
  return "bar";
}
```
like other C-like languages, statements in XQ have to end with a semicolon.
When declaring a function in XQ, parameters don't have names, like local variables, they are simply called `$param[number]`. for example, this is how a function greeting the user would look like in C#:
```cs
string greet(string name) {
  string result = "Hello, " + name;
  return result;
}
```
in XQ, it would look like this:
```xq
greet($param0) {
  $local1 = format("Hello, %s!", $param0);
  return $local1;
}
```

# Instructions
I would write a guide on this, but the creator of XtractQuery, onepiecefreak, has already done a fantastic job on that, so please read [this](https://github.com/onepiecefreak3/XtractQuery/blob/master/ScriptSpecification.md#instructions) to understand all the base instructions in XQ, This is super important.

# Comments
You can define single line comments with `//` and multiline comments with `/* comment */`
# Conditional statements
In XQ, you can't do do `if` blocks like in C#, you need to use a conditional goto, furthermore, in XQ, bool literals are not allowed. so you have to prepare the conditional beforehand for example, if in C# it would be like this:
```c#
string name = "john";
if(name == "john") {
  Console.WriteLine("hey john");
} else {
  Environment.Exit(1);
}
```
in XQ it would look like this:
```xq
$local1 = "john";
$local2 = $local1 == "john";
if $local2 goto "johnYes"h;
goto "johnNo"h; // Essentially acts as an `else` statement because if the previous condition was true it would already jump to "johnYes".
"johnYes"h:
  $local1 = log("hey john");
"johnNo"h:
  exit;
```
# Hash values
Unsigned integers or hex values have to end with `h`, as well as label literals. Hash values can also start with `0x` and have hex codes instead of decimal values.
# Stuff worth noting.
In XQs, even though some functions return void, every instruction has to be binded to a variable. You can't just do `PlayMovie()`, you need to do `$local1 = PlayMovie();`, for example.
This is only the tip of the iceberg; please visit the script specification on the XtractQuery github for more in-depth info about the syntax.

## if you have any questions, please ping me, I am free almost all the time and I love answering questions.

# Guide 2 - Useful functions for real modding
This guide will go over stuff you need to know if you want to actually mod the game.This includes functions you can call to activate dialouge, give yokais, give items, and more.

# Giving a yokai
*Function type*: Reference function

Parameters:

ParamID (type unsigned int, end it with h, it's recommended that you prefix it with 0x to use a hex value instead of a decimal value. 
[ParamIDs for YW1](https://pastebin.com/M6SX8nih)
[ParamIDs for YW2](https://pastebin.com/GCe4D2DC) 
[ParamIDs for YW3](https://pastebin.com/iYBphgMz)
unk
Level of the yokai (type int)
unk
unk
unk

Example:
```xq
// 0x79F3AA36 is Pandle, and it's given at Level 3.
$local1 = AddPartyEx(0x79F3AA36h,0,1,3,1,0);
```
# Playing a movie
*Function type*: Reference function

Parameters:
Moflex name without extension (type string)
Bcstm name without extension (type string)
unk
unk
unk
unk
unk
unk
CfgBin for subtitles
Example:
```xq
//This plays ProjectNateOpening.moflex with the sound for ev50_2540 with no subtitles (NULL).
$local1 = MovieRunEx2("ProjectNateOpening", "ev50_2540", 15, 15, 0, 0, 0, -1, "NULL", 0);
```
# Start event
*Function type* : Reference function

Parameters:
Event name (type string)

Example:
```xq
$local1 = RunEvent("ev01200");
```

# Giving a Item
(contribution by @thewadi )
*Function type*: Reference function

Parameters:

ItemID (type unsigned int, end it with h, it's recommended that you prefix it with 0x to use a hex value instead of a decimal value)
Number of items given per call 
unk
unk
unk
unk

Example:
```xq
// 0x6B9A7A3D is the ItemID, and it gives the item corresponding to the ID once per call

$local1 = AddItem(0x6B9A7A3D,1,1,1,1,0);
```

# Getting the player's gender

*Function type*: Engine function

No parameters

Example:
```xq
$local2 = getPlayerGender();
//local2 will be 2 if it's a girl,and another number if it's a boy.
```

# Display chapter title

*Function type*: Engine function

unk
Chapter number

Example:
```xq
//will display the chapter title for chapter 2
$local1 = display_chapter_title(0,2);
```
