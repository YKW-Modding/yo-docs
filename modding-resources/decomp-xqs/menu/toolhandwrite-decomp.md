---
title: tool_hand_write Decompilation (YW1s)
layout: default
parent: Menu XQs
grand_parent: Decompiled/Sample XQs
---

# tool_hand_write Decompilation (YW1s)
This section contains a decompilation of Yo-kai Watch 1's debug tool called tool_hand_write, used for testing the drawing minigame, the explanations are in the form of comments placed within the source. This source assumes you are using the latest `methodMappings.json`. The source can be found below:

```php
/*
 * Main()
 * Entry point for ze tool.
 * Loads the tool's own ANM resource and the battle ANM resource (which holds the waza frames),
 * It also sets up all UI elements, then enters a loop allowing you to navigate between 'waza' frames using the left/right controller inputs.
 */
Main()
{
	$local1 = set_user_input_lock_state(1); // lock user input during setup
	$global0 = 0; // $global0 = currentWazaIndex; the index of the waza frame currently being edited, initialised to 0
	$local1 = load_spr_image("res_anm_dbg", "_debug/tool/tool_hand_write"); // load the tool's own ANM resource (contains the button textures etc.)
	$local1 = load_spr_image("res_anm_btl", "data/menu/battle_d00"); // load the battle ANM resource which contains the waza frames (waza_g3_01, waza_g3_02, etc.)
"@000@": // wait for both resources to finish loading
	$local5 = sub2302();
	$local6 = sub2303();
	$local3 = $local5 or $local6; 
	$local4 = sub2304();
	$local2 = $local3 or $local4; // is anything still loading?
	if $local2 goto "@000@"h; // if so, loop else continue
	yield; // wait a frame so everything is ready
	$local1 = create_element("btn_save", 1, 131, 1, 0, "Btn_Save", "ok_off", "ok_on"); // create the save button on the bottom screen
	$local1 = CreateBtn("btn_save", "res_anm_dbg"); // set up its ANM and callbacks
	$local1 = sub5051("btn_save", 1, 0, 0, 56, 23); // set the save button's interactive hitbox
	$local1 = move_element("btn_save", 260, 210); // place it at the bottom right of the bottom screen
	$local1 = create_element("btn_add", 1, 131, 1, 0, "", "add_off", "add_on"); // create the add-point button; note: no click callback since the touch callback handles the full drag interaction
	$local1 = set_anm_res("btn_add", "res_anm_dbg"); // assign the ANM resource
	$local2 = get_ui_element_property("btn_add", 6); // get the default idle animation?
	$local1 = set_anm_res_animated("btn_add", $local2); // and play it
	$local1 = sub5034("btn_add", "BtnAdd_Nrm", "BtnAdd_Tch"); // assign callbacks
	$local1 = move_element("btn_add", 4, 210); // position it at the bottom left of the bottom screen
	$local1 = create_element("btn_del", 1, 131, 1, 0, "Btn_Delete", "del_off", "del_on"); // create the delete-last-point button
	$local1 = CreateBtn("btn_del", "res_anm_dbg"); // set up its ANM and callbacks
	$local1 = sub5051("btn_del", 1, 0, 0, 56, 23); // set the delete button's interactive hitbox
	$local1 = move_element("btn_del", 70, 210); // position it to the right of the add button
	$local1 = create_element("obj_bg", 1, 0, 0); // create the background element on the bottom screen
	$local1 = set_anm_res("obj_bg", "res_anm_btl"); // assign the battle ANM resource to it (the waza frames are displayed here)
	$local1 = move_element("obj_bg", 160, 120); // centre it on the bottom screen
	$local1 = set_element_scale("obj_bg", 1.5f, 1.5f); // scale it up 1.5x so the waza frame fills the screen better
	$local1 = create_element("msg_anm", 0, 0, 0); // create a text label on the top screen to display the current waza frame name
	$local1 = move_element("msg_anm", 960, 540); // position it on the top screen
	$local1 = set_text_style("msg_anm", 64); // configure text
	$local1 = set_text_colour("msg_anm", 2); // apply text colour
	$local1 = sub5942("msg_anm", 1); // configure text element (likely horizontal alignment or wrap setting)
	$local1 = sub5943("msg_anm", 1); // configure text element (likely vertical alignment or wrap setting)
	$local1 = BuildScreenBObj($global0); // build the point objects for the initial waza frame (index 0)
	$local1 = sub2512(0f, 5f); // fade in over 5 frames?
"@001@": 
	$local2 = sub2519(); // is fade not over
	if $local2 goto "@001@"h; // if so, loop
	$local1 = set_user_input_lock_state(0); // otherwise unlock user input and continue
"@002@": // main input loop
	if not 1 goto "@003@"h; // while(true) {
	$local1 = get_controller_input(0); // check if D-Pad left was pressed
	if not $local1 goto "@004@"h; // if not then goto @004@ otherwise continue
	$local1 = $global0 - 1; // decrement index
	$object0 = math_max($local1, 0); // clamp to minimum 0 so we don't go below the first waza
	$local1 = IsAnmExist($object0); // check if the waza frame at the new index actually exists in the ANM resource
	if not $local1 goto "@005@"h;
	$global0 = $object0; // update currentWazaIndex
	$local1 = BuildScreenBObj($global0); // rebuild the point objects for the new waza frame
"@004@":
"@005@":
	$local1 = get_controller_input(1); // check if D-Pad right (next waza) was pressed
	if not $local1 goto "@006@"h;
	$local1 = $global0 + 1; // increment index
	$object0 = math_min($local1, 31); // clamp to maximum 31 (32 waza frames supported, 0-31)
	$local1 = IsAnmExist($object0); // check if the waza frame at the new index actually exists in the ANM resource
	if not $local1 goto "@007@"h;
	$global0 = $object0; // update currentWazaIndex
	$local1 = BuildScreenBObj($global0); // rebuild the point objects for the new waza frame
"@007@":
"@006@":
	yield; // wait a frame
	goto "@002@"h; // and loop
"@003@":
	$local1 = set_user_input_lock_state(1); // lock user input during fade out
	$local1 = sub2512(-1f, 5f); // fade out over 5 frames?
"@008@": // wait for the fade out to finish
	$local2 = sub2519(); // is fade not over
	if $local2 goto "@008@"h; // if so, loop
}

/*
 * SubLoop()
 * A coroutine that runs in the background, visually animating the point objects for the current waza frame by alternating their tint between red (1,0,0) and gold (1,0.7,0).
 * The tint flips every 5 frames, cycling through all points in sequence.
 * also hi guys!
 */
SubLoop()
{
	$object0 = 0; // $object0 = colourPhase; toggles between 0 (gold) and 1 (red) to alternate point colours
	$object1 = 0; // $object1 = currentPointIndex; the index of the point currently being coloured
"@000@": // main loop
	if not 1 goto "@001@"h; // while(true) {}
	$object2 = run_engine_command(0, $global0); // get the total number of points for the current waza frame
	$object3 = format("obj_pnt%03d", $object1); // format the element name for the current point e.g. "obj_pnt000"
	$local1 = does_element_exist($object3); // check if this point's UI element exists
	if not $local1 goto "@002@"h; // if it doesn't, loop back to the first point
	if not $object0 goto "@003@"h; // if colourPhase is 0 then use gold, otherwise use red
	$local1 = set_tint($object3, 1f, 0f, 0f); // tint the point red (#FF0000)
	goto "@004@"h;
"@003@":
	$local1 = set_tint($object3, 1f, 0.7f, 0f); // tint the point gold aka #FFB300 (well it's B2.5 but yeah :p)
"@004@":
	$object1++; // advance to the next point
	$local1 = $object1 >= $object2; // if we've coloured all points
	if not $local1 goto "@005@"h;
	$object1 = 0; // reset back to the first point
	$object0 = 1 - $object0; // and flip the colour phase
"@005@": // otherwise (or after doing so)
	$local1 = WaitFrame(5); // wait 5 frames before colouring the next point
	goto "@006@"h; // goto @006@
"@002@":
	$object1 = 0; // the element didn't exist (likely deleted), reset the point index back to 0
"@006@":
	yield; // wait a frame
	goto "@000@"h; // and loop
"@001@":
}

/*
 * BuildScreenBObj($param0)
 * Builds (or rebuilds lol) all point objects for the specified waza frame index
 * Stops the SubLoop coroutine, clears existing point elements, loads the correct waza frame into the background, then creates a point object for each stored point in the engine's data for that waza. 
 * then restarts SubLoop.
 */
BuildScreenBObj($param0)
{ // $param0 = wazaIndex
	$local1 = set_coroutine(""); // stop the currently running coroutine (SubLoop) so it doesn't interfere during rebuild
	$local1 = sub5310(1, 3); // delete all existing point UI elements (clears the previous waza frame's points from the screen)
	$local1 = $param0 + 1; // waza frame names are 1-indexed (waza_g3_01 not waza_g3_00)
	$object0 = format("waza_g3_%02d", $local1); // format the ANM frame name, for example "waza_g3_01"
	$local1 = set_anm_res_animated("obj_bg", $object0); // update the background to display the correct waza frame
	$local1 = attach_text("msg_anm", $object0); // display the waza frame name as text on the top screen label
	$object1 = run_engine_command(0, $param0); // get the number of stored points for this waza frame from the engine
	$object2 = 0; // $object2 = i
"@000@": // for each stored point
	$local1 = $object2 < $object1; // while i < pointCount
	if not $local1 goto "@001@"h;
	$local1 = run_engine_command(10, $param0, $object2, $object3, $object4); // get the position of point i; outputs X into $object3 and Y into $object4
	$object5 = format("obj_pnt%03d", $object2); // format the element name for this point so for example "obj_pnt000"
	$local2 = does_element_exist($object5); // check if the element already exists (shouldn't after the clear above, but guard anyway)
	$local1 = not $local2;
	if not $local1 goto "@002@"h;
	$local1 = CreatePointObj($object5, $object3, $object4, $object2); // create the point UI element at the stored position
"@002@":
	$object2++; // i++
	goto "@000@"h; // loop
"@001@":
	$local1 = set_coroutine("SubLoop"); // restart SubLoop since the point elements are in place
}

/*
 * CreatePointObj(elementNam, xpos, ypos, pointIdx)
 * Creates a single draggable point UI element at the specified position.
 * The element is rendered as a small 4x4 red square and is interactive:
 * touching and holding it enters PointObj_Slide which lets the user drag it.
 */
CreatePointObj($param0, $param1, $param2, $param3)
{ // $param0 = elementName, $param1 = xPos, $param2 = yPos, $param3 = pointIndex
	$local1 = create_element($param0, 1, 131, 3, $param3); // create the element on the bottom screen; the 5th argument stores the point index into the element (accessible via $unk4 in callbacks)
	$local1 = sub5420($param0, -2, -2, 4, 4); // set the element's visual rect to a 4x4 square centred on its position
	$local1 = set_tint($param0, 1f, 0f, 0f); // tint it red (SubLoop will animate this)
	$local1 = move_element($param0, $param1, $param2); // position the element at the stored point coordinates
	$local1 = sub5034($param0, "PointObj_Normal", "PointObj_Slide"); // assign pen callbacks
	$local1 = sub5051($param0, 1, -5, -5, 10, 10); // set the interactive hitbox to a 10x10 area centred on the element, making it easier to tap
}

/*
 * PointObj_Normal()
 * Empty because no code is needed
 */
PointObj_Normal()
{
}

/*
 * PointObj_Slide()
 * Drag callback for a point element. Runs as long as the user holds the point, continuously moving the element to the current pen position and updating the engine's stored coordinates for this point so the data stays in sync with the visual.
 */
PointObj_Slide()
{
"@000@": // drag loop: runs every frame while the user holds the point
	if not 1 goto "@001@"h; // (unconditional loop, exits only when the pen is released and the engine stops calling this callback)
	$object0 = ctrl_pen_get_lx(); // get the current pen X position
	$object1 = ctrl_pen_get_ly(); // get the current pen Y position
	$local1 = move_element($unk0, $object0, $object1); // move this point's UI element to the pen position, $unk0 is the element's name (injected by the engine into the callback's scope - $unk should be renamed to $engine)
	$local1 = run_engine_command(11, $global0, $unk4, $object0, $object1); // update the engine's stored position for this point; $unk4 is the point index stored when CreatePointObj called create_element
	yield; // wait a frame
	goto "@000@"h; // and loop
"@001@":
}

/*
 * CreateBtn($param0, $param1)
 * Helper that sets up a standard clickable button element with ANM, default animation,
 * size/anchor and the standard 3-state click callbacks (normal, touch, click).
 */
CreateBtn($param0, $param1)
{ // $param0 = elementName, $param1 = anmResourceName
	$local1 = set_anm_res($param0, $param1); // assign the ANM resource to the button
	$local3 = get_ui_element_property($param0, 6); // get the default idle animation from the ANM resource
	$local1 = set_anm_res_animated($param0, $local3); // play the idle animation
	$local1 = sub5252($param0);
	$local1 = assign_advanced_ui_callback($param0, "Btn_ClkNormal", "Btn_ClkTouch", "Btn_ClkClick");
}

Btn_ClkNormal()
{
	$local1 = set_anm_res_animated($unk0, $unk6); // restores the idle anim; $unk0 = element name, $unk6 = idle anim 

Btn_ClkTouch()
{
	$local1 = set_anm_res_animated($unk0, $unk7); // plays the pressed anim; $unk7 is the pressed anim
}

Btn_ClkClick()
{
	$local1 = set_anm_res_animated($unk0, $unk6); // restore idle animation on click
	$local1 = spawn_coroutine($unk5, $unk0); // spawn the button's specific click handler as a coroutine; $unk5 = click callback name set at create_element time; $unk0 = element name passed as argument
}

// Idle callback for the add-point button; restores the idle animation
BtnAdd_Nrm()
{
	$local1 = set_anm_res_animated($unk0, $unk6); // restore the idle animation
}

/*
 * BtnAdd_Tch()
 * Touch/hold callback for the add-point button.
 * On the first touch this begs the engine to allocate a new point slot for the current waza, then makes a point UI element at the current pen (touch) pos.
 * While held this moves the point to the pen position and syncs the engine data :p
 */
BtnAdd_Tch()
{
	$local1 = set_anm_res_animated($unk0, $unk7); // play the pressed animation
	$object0 = run_engine_command(0, $global0); // get the current point count for this waza (this will be the index of the new point)
	$local1 = run_engine_command(1, $global0); // tell the engine to add a new point slot for the current waza
	$global1 = format("obj_pnt%03d", $object0); // $global1 = newPointElementName, so for example "obj_pnt003"
	$local2 = does_element_exist($global1); // check if the element already exists - this shouldn't iirc for a brand new point but Level5 is always safe
	$local1 = not $local2;
	if not $local1 goto "@000@"h;  // if not then
	$local3 = ctrl_pen_get_lx(); // get the initial X pos for placement
	$local4 = ctrl_pen_get_ly(); // get the initial Y pos for placement
	$local1 = CreatePointObj($global1, $local3, $local4, $object0); // create the point element at the pen position with the new point index
"@000@": // else (or after doing the above)
"@001@": // enter the drag loop
	if not 1 goto "@002@"h; // while(true)
	$object1 = ctrl_pen_get_lx(); // current pen X
	$object2 = ctrl_pen_get_ly(); // current pen Y
	$local1 = move_element($global1, $object1, $object2); // move the new point element to the pen position
	$local1 = run_engine_command(11, $global0, $object0, $object1, $object2); // sync the new point's position to the engine's data
	yield; // wait a frame
	goto "@001@"h; // and loop
"@002@":
}

/*
 * Btn_Delete()
 * Click callback for the delete button.
 * Removes the last point from the current waza by telling the engine to pop it, then deletes the actual UI element from the screen.
 */
Btn_Delete()
{
	$object0 = run_engine_command(0, $global0); // get the current point count for this waza
	$local1 = $object0 == 0; // if there are no points then
	if not $local1 goto "@000@"h;
	return 0; // there's nothing to delete so it returns
"@000@": // otherwise
	$local1 = run_engine_command(2, $global0); // tell the engine to remove the last point from the current waza
	$local1 = $object0 - 1; // compute the index of the point that was just removed (last index = count - 1)
	$object1 = format("obj_pnt%03d", $local1); // format its element name
	$local1 = does_element_exist($object1); // check if the element exists (it should, but again Level5 is safe)
	if not $local1 goto "@001@"h;
	$local1 = delete_element($object1); // delete the point's UI element from the screen
"@001@":
}

/*
 * Btn_Save()
 * Click callback for the save button.
 * Tells the engine to serialise and save all waza point data to disk?
 * Atleast I'm assuming it saves to your save - I don't remember
 */
Btn_Save()
{
	$local1 = run_engine_command(20); // save all waza point data
}

/*
 * WaitFrame($param0)
 * Waits for the specified number of frames, accounting for delta time
 * so the wait is frame-rate aware. Falls back to a single yield if param0 <= 0.
 * Also checks sub1010() (is_paused?) to avoid counting certain frames.
 */
WaitFrame($param0)
{ // $param0 = frameCount
	$local1 = $param0 > 0; // if frameCount is 0 or negative there's nothing to wait for
	if not $local1 goto "@000@"h;
	$object0 = $param0; // $object0 = remainingFrames
"@001@":
	$local2 = $object0 > 0; // while there are remaining frames
	$local4 = sub1010(); // check if the game is paused?
	$local3 = not $local4; // only count down if NOT 
	$local1 = $local2 and $local3; // continue if (remainingFrames > 0) AND (not (paused?))
	if not $local1 goto "@002@"h;
	$local1 = get_delta_time();
	$object0 -= $local1; // subtract from remaining frames
	yield; // wait a frame
	goto "@001@"h; // and loop
"@002@":
	$local1 = $object0 == $param0; // if we never actually decremented thn well yield once anyway to guarantee at least one frame of wait
	if not $local1 goto "@003@"h;
	yield;
"@003@":
"@000@":
}

/*
 * IsAnmExist($param0)
 * Checks whether the waza frame at the specified index exists within the loaded battle ANM resource.
 * Waza frames are named waza_g3_01 through waza_g3_32 (1-indexed).
 * Returns 1 if the frame exists, 0 otherwise.
 */
IsAnmExist($param0)
{ // $param0 = wazaIndex (0-based)
	$local1 = $param0 + 1; // convert to 1-based index to match the ANM frame naming convention
	$object0 = format("waza_g3_%02d", $local1); // format the frame name so like "waza_g3_01"
	$local1 = sub3544("res_anm_btl", $object0, $object1, $object2); // check if this frame exists within the loaded battle ANM resource; returns 1 if found
	return $local1; // return the result
}
```
