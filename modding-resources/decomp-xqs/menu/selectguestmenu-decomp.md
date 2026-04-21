---
title: SelectGuestMenu Decompilation
layout: default
has_children: true
parent: Menu XQs
grand_parent: Decompiled/Sample XQs
---

# SelectGuestMenu Decompilation
This section contains a decompilation of Yo-kai Watch 2's SelectGuestMenu which allows you to change your Companion Yo-kai. As this is a decompilation page, the explanations are in the form of comments placed within the source. This source assumes you are using the latest `methodMappings.json`. The source can be found below:

```php
/*
 * SelectGuestMenu()
 * This is the main func for the menu, being the entry point of this script where everything is initialised and setup
 * Also holds the main loop so yeah.
*/
SelectGuestMenu()
{
	$local1 = set_user_input_lock_state(1); // lock user input
	$global3 = 1; // $global3 = menuState (0 = closed, 1 = opening, 2 = active, 3 = resuming (from state 4), 4 = inSubMenu aka selecting custom Yo-kai), we're initialising to state 1 (opening) because well it's opening :p
	$global0 = 0; // $global0 = storedColumn, used only for a simple UX feature in GuestMenu_Select, initialised to 0 since the default selected button is at column 0
	$global1 = 0; // $global1 = prevSelectedIndex
	$global2 = 0; // $global2 = prevSelectedRow
	$local1 = yw2.seq.prog_common_011d.BrightFadeB(1, 15f); // fade in from black
	$local1 = set_overworld_rendering_composite_mode(0, 1); // render the overworld on the top screen
	$local2 = anm_is_exist("mm_res_aanm"); // if the anm dosen't exist
	$local1 = not $local2;
	if not $local1 goto "@000@"h;
	$local1 = yw2.seq.prog_menu_006c.Menu_BgbuildAnmByGameData("mm_res_aanm", "data/menu/mm_top_u00_<LG><GENDER>");
"@000@":
	$local2 = anm_is_exist("mm_res_banm"); // if the anm dosen't exist
	$local1 = not $local2;
	if not $local1 goto "@001@"h;
	$local1 = yw2.seq.prog_menu_006c.Menu_BgbuildAnmByGameData("mm_res_banm", "data/menu/mm_top_d00_<LG><GENDER>"); // then load it
"@001@":
	$local2 = anm_is_exist("mm_res_aanm_skin"); // if the anm dosen't exist
	$local1 = not $local2;
	if not $local1 goto "@002@"h;
	$local1 = yw2.seq.prog_menu_006c.Menu_BgbuildAnmByGameData("mm_res_aanm_skin", "data/menu/mm_top_us00<SKIN>"); // then load it
"@002@":
	$local2 = anm_is_exist("mm_res_banm_skin"); // if the anm dosen't exist
	$local1 = not $local2;
	if not $local1 goto "@003@"h;
	$local1 = yw2.seq.prog_menu_006c.Menu_BgbuildAnmByGameData("mm_res_banm_skin", "data/menu/mm_top_ds00<SKIN>_<LG>"); // then load it
"@003@":
	$local1 = yw2.seq.prog_menu_006c.Menu_BgbuildAnmByGameData("guest_res_anm_b", "data/menu/attend_d00_<LG><GENDER>"); // load the anm for the buttons
	$local1 = yw2.seq.prog_common_011d.WaitBGBuild(); // wait for the background to finish loading
	$local1 = GuestMenu_SetupScrnB(); // setup the menu (bottom screen)
	yield; // wait a frame
	$local1 = yw2.seq.prog_common_011d.BrightFadeN(1, 15f);  // fade in the background
	$global3 = 2; // set menu state to 2 (active)
	$local1 = set_user_input_lock_state(0); // unlock user input
"@004@":
	$local1 = $global3 != 0;
	if not $local1 goto "@005@"h; // if the menu isn't open then
	$local1 = $global3 == 3; // check if the menu is resuming
	if not $local1 goto "@006@"h; // if not
	$local1 = GuestMenu_Resume(); // resume
"@006@":
	yield; // else wait a frame
	goto "@004@"h; // and loop
"@005@": // if the menu IS open then
	$local1 = set_user_input_lock_state(1); // lock user input
	$local1 = yw2.seq.prog_common_011d.BrightFadeB(1, 15f); // fade from black
}

// I'm not even going to write an explanation here
GuestMenu_SetupScrnA()
{ // SetupScrnA is empty because the top screen is only used to render the overworld, with no additional UI elements placed
}

/*
 * GuestMenu_SetupScrnB()
 * This func initialises alot of the UI elements on the bottom screen
 * also hi
*/
GuestMenu_SetupScrnB()
{ // bottom screen setup which setsup the UI elements
	$local1 = yw2.seq.prog_menu_006c.MenuMain_BGCreate(); // sets up the Yo-kai Pad Background/Wallpaper
	$local1 = set_element_opacity("mm_obj_abg", 0); // hide the top-screen background (the one that covers the overworld) since it's not wanted in this menu
	$local1 = set_anm_res_animated("mm_obj_btitle", "title_attend"); 
	$local1 = set_element_opacity("mm_obj_btitle", 1f); // show the bottom screen title since it's hidden by default
	$local1 = yw2.seq.prog_menu_006c.MenuCmd_MatrixBtnSetup(3, 2, 1, "GuestMenu_Select", 0); // create a 3x2 grid of matrix button's and set it's callback to GuestMenu_Select
	$local1 = yw2.seq.prog_menu_006c.Menu_CreateMatrixBtnPushLinkOff(0, 0, "guest_obj_b_btn_wisper", "guest_res_anm_b", 200, 43, 1, "GuestMenu_DecideWisper", 2, "btn_a_wis", 0, 0, 0, 0, 300, 166, 0.9f); // create the first button (Whisper) at column 0 row 0 (0, 0)
	$object0 = 0; // i = 0
"@000@":
	$local1 = $object0 < 3; // while(i < 3)
	if not $local1 goto "@001@"h;
	$local1 = yw2.seq.prog_menu_006c.Menu_MatrixBtnSet(0xD8FE9751h, $object0, 0); // TODO: document
	$local1 = yw2.seq.prog_menu_006c.MenuCmd_MatrixBtnSetBtnType(2, $object0, 0); // TODO: document
	$object0++; // i++ (increment i)
	goto "@000@"h; // and loop
"@001@":
	$local1 = yw2.seq.prog_menu_006c.Menu_CreateMatrixBtnPushLinkOff(0, 1, "guest_obj_b_btn_etc", "guest_res_anm_b", 200, 43, 1, "GuestMenu_DecideEtc", 2, "btn_a_other", 0, 0, 0, 0, 300, 166, 0.9f); // create the second button (Other, internally called "Etc") at column 0 row 1 (0, 1)
	$local1 = yw2.seq.prog_menu_006c.Menu_CreateMatrixBtnPushLinkOff(1, 1, "guest_obj_b_btn_jiba", "guest_res_anm_b", 200, 43, 1, "GuestMenu_DecideJiba", 2, "btn_a_jiba", 0, 0, 0, 0, 300, 166, 0.9f); // create the third button (Jibanyan) at column 1 row 1 (1, 1)
	$local1 = yw2.seq.prog_menu_006c.Menu_CreateMatrixBtnPushLinkOff(2, 1, "guest_obj_b_btn_nothing", "guest_res_anm_b", 200, 43, 1, "GuestMenu_DecideNothing", 2, "btn_a_remove", 0, 0, 0, 0, 300, 166, 0.9f); // create the fourth button (None, internally called "Nothing") at column 2 row 1 (2, 1)
	$local1 = yw2.seq.prog_menu_006c.Menu_MatrixBtnSetFocus(0, 0); // set the default selected button to the first one (Whisper) as Whisper is at (0, 0) or column 0 row 0
	$local1 = yw2.seq.prog_menu_006c.Menu_CreateMatrixDecideBtn("guest_select_decide", 200, 43, "mm_res_banm", 1, 0, 0, 0); 
	$local1 = yw2.seq.prog_menu_006c.CreateMenuBtnEx("guest_obj_b_btn_back", 200, 43, "mm_res_banm", "btn_b_on", "btn_b_off", "GuestMenu_Back", 1, 3, 7, 0, 0, 0, 76, 32); // create the back button at the bottom right of the screen, which has a callback pointing to GuestMenu_Back
}

// too lazy to write an explanation for this one - I'll do it once I'm done with the rest IF I remember to atleast
GuestMenu_ReserveResume()
{
	$local1 = get_ui_element_property("obj_guest_attend_check", 8); // if property 8 is true
	if not $local1 goto "@000@"h;
	$global3 = 0; // if so, set menu state to 0 (closed) and
	goto "@001@"h; // return
"@000@": // otherwise
	$global3 = 3; // set menu state to 3 (resuming) so that the menu will resume instead of opening from the start when opened again
"@001@":
}

/*
 * GuestMenu_Resume()
 * This func is the callback for resuming the menu from the "select custom Yo-kai" sub-menu, it plays some fade effects, sets the menu state to 2 (active) again and unlocks user input
*/
GuestMenu_Resume()
{
	$local1 = set_element_opacity("mm_obj_abg", 0); // hide the top-screen background again
	$local1 = sub5323(1, 43, 1f, 20f); // play some fade effect
	$local1 = yw2.seq.prog_common_011d.BrightFadeAllN(20f); // fade in from white for 20 frames (1/3 of a second during UIs, since UIs run at 60FPS)
	$global3 = 2; // set menu state to 2 (active) 
	$local1 = set_user_input_lock_state(0); // unlock user input
}

/* 
 * GuestMenu_Back()
 * This func is the callback for the back button, it simply sets the menu state to 0 (closed), the main loop handles ze rest
*/
GuestMenu_Back()
{
	$global3 = 0; // set menu state to 0 (closed)
}

/* 
 * GuestMenu_Select(selectedIndex, unused, selectionEvent)
 * This func handles cursor movement within the 3x2 grid of buttons
 * This is pretty beefy lol, it tracks current and previous selectionIndex BUT
 * it also determines current row, plays sound based on row and event type etc
 * Notably, it has a pretty nice UX feature
 * oh yeah never explicitly returns
*/
GuestMenu_Select($param0, $param1, $param2)
{ // $param0 = selectedIndex, $param1 is unused and I'm too lazy to check what it is, $param2 = selectionEvent (0/1 = cursor movement, 2/3 = confirm / navigation)
	$object0 = $global1; // get the previously selected index
	$object1 = $global2; // get the previously selected row
	$object2 = $param0 / 3; // get the currently selected row by dividing the selected index by the number of columns (3)
	$global1 = $param0; // set prevSelectedIndex to selectedIndex for the next time this func is called
	$local1 = $object2 == 0; // if the currently selected row is the first row (Whisper) then continue
	if not $local1 goto "@000@"h; // otherwise goto @000@
	$local2 = $param2 == 2;
	$local3 = $param2 == 3; 
	$local1 = $local2 or $local3; // if the selectionEvent is either 2 OR 3 
	if not $local1 goto "@001@"h; //  then
	$local1 = yw2.seq.prog_menu_006c.Menu_MatrixBtnSelectPlaySE(); // play the sound effect for selecting a button
"@001@":
	goto "@002@"h; // and goto @002@
"@000@": // else if the currently selected row is NOT the first row (Whisper)
	$local1 = $param2 >= 0; // and the event is NOT < 0
	if not $local1 goto "@003@"h; 
	$local1 = yw2.seq.prog_menu_006c.Menu_MatrixBtnSelectPlaySE(); // play the sound effect for selecting a button
"@003@":
"@002@": // TODO: document the rest
	$local1 = $object1 != $object2; // if previousRow != currentRow
	if not $local1 goto "@004@"h; // aka the row has changed then
	$global2 = $object2; // set previousRow to currentRow
	$local1 = $object1 == 1; // if the previously selected row was the bottom row (non-Whisper row)
	if not $local1 goto "@005@"h; 
	$global0 = $object0 % 3; // then restore the previously selected column via selectedIndex % columnCount (3) and storing it in global0 (storedColumn) for later use when setting the focus again
	// note that this is needed because when you move from the top row to the bottom row and then back to the top row, you'd ideally want to restore the previously selected column in the top row - just nice UX lol
	goto "@006@"h; // and return from this function
"@005@": // OTHERWISE if the previously selected row was the top row (Whisper was selected)
	$local1 = $param2 >= 0; // and the event is NOT < 0
	if not $local1 goto "@007@"h; //  then
	$local1 = yw2.seq.prog_menu_006c.Menu_MatrixBtnSetFocus($global0, $global2); // set the focus to the button at (storedColumn, currentRow) which is the column we stored earlier when moving from the bottom row and the current row aka the top row
"@004@":  // and return from this function
"@007@": // otherwise return from the function
"@006@": // yay love returning from functions (Level5 imo should've added jump reuse in their compiler, since they so frequently set many jumps to the same point but whatever)
}


GuestMenu_Decide_Nothing()
{
	$object1 = sub13104($object0);
	$local1 = $object0 == 0;
	$local2 = $object1 == 0;
	$object2 = $local1 and $local2;
	if not $object2 goto "@000@"h;
	$local1 = GuestMenu_MessageInfoAlreadyGuest(0xFFCCD364h);
	goto "@001@"h;
"@000@":
	$local1 = yw2.seq.prog_menu_006c.MenuCmd_MatrixBtnStateCtrl(1);
	$object3 = GuestMenu_GetText(0x33A9137Dh);
	$object4 = yw2.seq.prog_menu_006c.Common_SelectYN($object3, 1);
	$local1 = yw2.seq.prog_menu_006c.MenuCmd_MatrixBtnStateCtrl(0);
	if not $object4 goto "@002@"h;
	$local1 = set_user_input_lock_state(1);
	$local1 = GuestMenu_ChangeExecParam(1, 0, 0);
	$global3 = 0;
	return 0;
"@002@":
"@001@":
	$local1 = set_user_input_lock_state(0);
}

GuestMenu_Decide_WisperJiba($param0, $param1)
{
	$object0 = crc32($param0); // hash the model name of the Yo-kai to get their BaseID
	$object2 = sub13104($object1); // $object1 = outGuestBaseID
	$object3 = 1;
	$local1 = $object2 == 0;
	$object3 &= $local1;
	$local1 = $object1 == $object0;
	$object3 &= $local1;
	if not $object3 goto "@000@"h;
	$local1 = GuestMenu_MessageInfoAlreadyGuest(0x30ACB816h);
	goto "@001@"h;
"@000@":
	$local1 = yw2.seq.prog_menu_006c.MenuCmd_MatrixBtnStateCtrl(1);
	$object4 = GuestMenu_GetText(0x38857000h);
	$object5 = GuestMenu_GetComvertMessage($object4, $param1);
	$object6 = yw2.seq.prog_menu_006c.Common_SelectYN($object5, 1);
	$local1 = yw2.seq.prog_menu_006c.MenuCmd_MatrixBtnStateCtrl(0);
	if not $object6 goto "@002@"h;
	$local1 = set_user_input_lock_state(1); // lock user input
	$local1 = GuestMenu_ChangeExecParam(1, 0, $object0);
	$global3 = 0;
	return 0;
"@002@":
"@001@":
	$local1 = set_user_input_lock_state(0); // unlock user input
}

GuestMenu_DecideWisper()
{
	$local1 = set_user_input_lock_state(1); // lock user input
	$local2 = GuestMenu_GetText(0x1B4E832Dh); // get Whisper's text - in English (engb and en) that's "Whisper"
	$local1 = spawn_coroutine("GuestMenu_Decide_WisperJiba", "y001000", $local2); // spawn a coroutine to handle this, we run GuestMenu_Decide_WhisperJiba passing whisper's Model (BaseID CRC-32 original string) and name
}

GuestMenu_DecideJiba()
{
	$local1 = set_user_input_lock_state(1); // lock user input
	$local2 = GuestMenu_GetText(0x6A4EB46Bh); // get Jibanyan's text - in English (engb and en) that's "Jibanyan"
	$local1 = spawn_coroutine("GuestMenu_Decide_WisperJiba", "y152000", $local2); // spawn a coroutine to handle this, we run GuestMenu_Decide_WhisperJiba passing Jibanyan's Model (BaseID CRC-32 original string) and name
}

GuestMenu_DecideEtc()
{
	$global3 = 4; // set menu state to 4 (inSubMenu) to indicate that we're now in the sub menu for selecting custom Yo-kai
	$local1 = sub5323(1, 43, 0f, 15f); // play some fade effect idk
	$local1 = yw2.seq.prog_common_011d.BrightFadeB(0, 15f); // fade out to black for 15 frames (1/4 of a second during UIs, since UIs run at 60FPS)
	$local1 = start_menu_event(2431, 2430); // start the menu event for the custom Yo-kai selection menu
	yield; // wait a frame
"@000@": // while(is_menu_event_over()) {}
	$local2 = is_menu_event_not_over(); // if the menu event isn't over then
	if $local2 goto "@000@"h; // loop
}

GuestMenu_DecideNothing()
{
	$local1 = set_user_input_lock_state(1); // lock user input
	$local1 = spawn_coroutine("GuestMenu_Decide_Nothing"); // spawn a coroutine to handle this, we run GuestMenu_Decide_Nothing
}

GuestMenu_ChangeExecParam($param0, $param1, $param2)
{
	$local1 = set_ui_element_property("obj_guest_attend_check", 8, $param0);
	$local1 = set_ui_element_property("obj_guest_attend_check", 9, $param1);
	$local1 = set_ui_element_property("obj_guest_attend_check", 10, $param2);
	$local1 = sub13105($param1);
}

GuestMenu_GetText($param0)
{ // $param0 = TextID
	$local1 = get_menu_text("selectguestmenu_text", $param0); // get the text with the specified TextID from data/res/text/menu/selectguestmenu_text_<locale>.cfg.bin where <locale> is a string such as engb
	return $local1; // and return it
}

GuestMenu_MessageInfoAlreadyGuest($param0)
{ // $param0 = TextID
	$object0 = GuestMenu_GetText($param0); // get the text with the specified TextID from data/res/text/menu/selectguestmenu_text_<locale>.cfg.bin where <locale> is a string such as engb
	$local1 = yw2.seq.prog_menu_006c.Menu_MessageInfoDraw($object0); // and draw it in a message box using the good ol' clasic Menu_MessageInfoDraw (defined in prog common)  
}

GuestMenu_GetComvertMessage($param0, $param1)
{
	$object0 = $param0;
	$object0 = set_game_variable($object0, 2, "YOKAI_NAME", $param1);
	return $object0;
}
```
