---
title: Blank Battle XQ
layout: default
has_children: true
parent: Modding Resources
---

# Blank Battle XQ

You can attach this to any battle in the encount table, then add your XQ code that should be triggered when the battle is won in the BattleEvent_OnBattleEndEvent next to the comment. 

*Tip: Click the copy button on the top right*

```

BattleEventInit()
{
	$local1 = log("BattleEventInit");
	$global9 = 0;
	$local1 = BattleEvent_OnInit();
	$local1 = sub15417(256, 0);
}

BattleEvent_Finalize()
{
	$local1 = log("BattleEvent_Finalize");
	$local1 = BattleEvent_OnFinalize();
}

BattleEvent_OnBattleEndEvent($param0, $param1, $param2)
{
	$local1 = log("BattleEvent_OnBattleEndEvent");
	$local1 = BattleEvent_OnBattleEnd($param0, $param1, $param2);
	$local2 = $global12_isVictory == 1;
	if not $local2 goto "end"h;
	//the code here will be ran upon victory
"end":
}

PlayFatalHitScene($param0, $param1, $param2, $param3, $param4, $param5, $param6, $param7, $param8)
{
	$local1 = sub14804($param0);
	$local1 = PlayCloseUpScene($param1, $param2, $param3, $param4, $param5, $param6, $param7, $param8);
	$local1 = sub15710(0.5f, 0.2f, 40f, 20f);
	$local1 = WaitFrame(1f);
	$local1 = sub2501(0, 0.8f);
	$local1 = WaitFrame(1f);
	$local1 = sub2502(0, 0f, 10f);
}

PlayCloseUpScene($param0, $param1, $param2, $param3, $param4, $param5, $param6, $param7)
{
	$local1 = sub15819($param0, 130);
	$local1 = sub15822($param0, "sy0500");
	$local1 = sub15814($param0, 30, $param3, $param4, $param5, $param1, $param2, $param6, $param7, 10, 10);
	$local2 = $param0 + 130;
	$local1 = sub15807($local2, 30, 0, 10f, 10f);
}

OnWeakPointDamaged_Common($param0)
{
	$local1 = sub14804($param0);
	$local1 = sub15800(0f, 90f);
	$local1 = sub15816(0f, 20f, 5f, 5f);
	$local1 = sub15818(0f, 20f, 0.9f);
	$local1 = sub15817(20f, 20, 1f, 0.7f);
	$local1 = BattleMenuHPBar_Boss_PlayDamage();
}

OnDeathEvent_Common($param0, $param1, $param2, $param3, $param4, $param5, $param6, $param7, $param8, $param9)
{
	if not $global9 goto "@000@"h;
	return 0;
"@000@":
	$global9 = 1;
	$local1 = sub15823();
	$object0 = sub15411($param1);
	$local1 = BattleMenuTarget_DeletePin($object0);
	$local1 = DeleteWandererSpirit();
	$local1 = sub15417(256, 1);
	$local1 = sub14804($param0);
	$local2 = $param8 - 50f;
	$local1 = sub15803(50f, $local2, 0f, 0f, 0f, $param1, $param2, 0.2f);
	$local1 = sub15817(60f, $param9, 0.8f, 0.5f);
	$local1 = StageEffect_FlashWithSE(50f, 20f, 1f);
	$local2 = $param8 + 10f;
	$local1 = sub15822($local2, "sy0502");
	$local2 = $param8 + 10f;
	$local1 = sub15818($local2, 60f, 0.8f);
	$local2 = $param8 + 10f;
	$local1 = sub15816($local2, 90f, 5f, 5f);
	$local2 = $param8 + 120;
	$local1 = WaitFrame($local2);
	$local1 = sub15417(256, 0);
}

StageEffect_FlashWithSE($param0, $param1, $param2)
{
	$global12_isVictory = 1;
}

ConvertCharaNameBossMessage($param0, $param1, $param2)
{
	$object1 = sub13732($param1, $param2);
	$local1 = sub15408($param0);
	$object0 = sub13665($local1);
	$object1 = sub13730($object1, 2, "CHARA_NAME", $object0);
	return $object1;
}

InitExtraObject()
{
	$global10 = new[2];
	$global11 = new[2];
	$object0 = 0;
"@000@":
	$local1 = $object0 < 2;
	if not $local1 goto "@001@"h;
	$global10[$object0] = 0;
	$global11[$object0] = 0;
	$object0++;
	goto "@000@"h;
"@001@":
}

BuildExtraObject($param0, $param1, $param2, $param3)
{
	$local1 = sub16512($param0, $param1);
	$local1 = sub16512($param2, $param3);
}

FinalizeExtraObject($param0, $param1)
{
	$local2 = crc32($param0);
	$local1 = sub3701($local2);
	$local2 = crc32($param1);
	$local1 = sub3701($local2);
}

CreateExtraObject($param0, $param1, $param2)
{
	$object0 = format("EXTRA_OBJ_%02d", $param0);
	$object1 = sub16501($object0, 0, 110, 21, 0);
	$local1 = sub6800($object1, $param1);
	$local1 = sub6031($object1, 0xA90612C0h);
	$local1 = sub6008($object1, 1);
	$local1 = sub6005($object1, 1);
	$local1 = sub11010($object1);
	$global11[$param0] = $object1;
	$local1 = sub3730($object1, $param2);
	$local1 = sub6046($object1, 8, $param0);
	$local1 = sub6046($object1, 9, 301);
	$local1 = sub6247($object1, -300f, 0f, 0f);
	$local4 = $global11[$param0];
	$local5 = $global10[$param0];
	$local1 = log("CreateExtraObject()", $param0, $object0, $local4, $local5);
}

SetExtraObjectEnable($param0, $param1)
{
	$local1 = $param0 < 2;
	if not $local1 goto "@000@"h;
	$local2 = $global10[$param0];
	$local1 = $local2 != $param1;
	if not $local1 goto "@001@"h;
	$global10[$param0] = $param1;
	if not $param1 goto "@002@"h;
	$local2 = $global11[$param0];
	$local3 = $global11[$param0];
	$local1 = sub6044($local2, 0x7314FA36h, $local3);
	goto "@003@"h;
"@002@":
	$local2 = $global11[$param0];
	$local3 = $global11[$param0];
	$local1 = sub6044($local2, 0xC26BE45Fh, $local3);
"@003@":
"@000@":
"@001@":
}

GetExtraObjectName($param0)
{
	$local1 = $param0 < 2;
	if not $local1 goto "@000@"h;
	$local1 = $global11[$param0];
	return $local1;
"@000@":
}

IsExtraObjectEnabled($param0)
{
	$local1 = $param0 >= 2;
	if not $local1 goto "@000@"h;
	return 0;
"@000@":
	$local1 = $global10[$param0];
	return $local1;
}

IsAllExtraObjectEnabled()
{
	$object0 = 0;
	$object1 = 0;
"@000@":
	$local1 = $object1 < 2;
	if not $local1 goto "@001@"h;
	$local2 = $global10[$object1];
	$local1 = $local2 == 1;
	if not $local1 goto "@002@"h;
	$object0 = 1;
	goto "@001@"h;
"@002@":
	$object1++;
	goto "@000@"h;
"@001@":
	return $object0;
}

SetExtraObjectPos($param0, $param1, $param2, $param3, $param4)
{
	$local1 = $param0 < 2;
	if not $local1 goto "@000@"h;
	$local2 = $global11[$param0];
	$local1 = sub6242($local2, $param1, $param2, $param3);
	$local2 = $global11[$param0];
	$local3 = ToRadian($param4);
	$local1 = sub6222($local2, 0f, $local3, 0f);
"@000@":
}

StartExtraObjectLoop($param0)
{
	$local1 = sub6247($param0, 0f, 0f, 0f);
	$local1 = sub6810($param0, 0x845BDF58h, 0x0000000Ch, 0);
"@000@":
	$local3 = sub6813($param0);
	$local2 = not $local3;
	if $local2 goto "@000@"h;
	$local1 = sub6810($param0, 0xC85C602Fh, 0, 0);
}

EndExtraObjectLoop($param0)
{
	$local1 = sub6810($param0, 0xB58185B1h, 0x0000000Ch, 0);
"@000@":
	$local3 = sub6813($param0);
	$local2 = not $local3;
	if $local2 goto "@000@"h;
	$object0 = sub6817($param0);
	$local1 = $object0 == 0xB58185B1h;
	if not $local1 goto "@001@"h;
	$local1 = sub6247($param0, -300f, 0f, 0f);
"@001@":
}

ExtraObjectTargetMove($param0, $param1, $param2, $param3, $param4)
{
	$local2 = $global11[$param0];
	$local1 = sub6241($local2, $object0, $object1, $object2);
	$local2 = $global11[$param0];
	$local3 = $param1 - $object0;
	$local4 = $param2 - $object1;
	$local5 = $param3 - $object2;
	$local1 = sub6248($local2, $local3, $local4, $local5, $param4, 0);
}

PlayCharge_x301()
{
	$local1 = PlayCharge_Ex(0xF447F0BCh, 6);
}

ShowCapsuleMessage($param0)
{
	$local1 = $param0 == 0x1ABCC573h;
	if not $local1 goto "@000@"h;
	$local2 = sub13732(0xE11720C5h);
	$local1 = ShowTopMessage($local2, 180, 0);
"@000@":
	$local1 = $param0 == 0x6DBBF5E5h;
	if not $local1 goto "@001@"h;
	$local2 = sub13732(0x1AB63E75h);
	$local1 = ShowTopMessage($local2, 180, 0);
"@001@":
	$local1 = $param0 == 0x86439F37h;
	if not $local1 goto "@002@"h;
	$local2 = sub13732(0x721BDC90h);
	$local1 = ShowTopMessage($local2, 180, 0);
"@002@":
	$local1 = $param0 == 0x7CAEFE7Ch;
	if not $local1 goto "@003@"h;
	$object0 = sub13732(0x161DA7E6h);
"@003@":
	$local1 = $param0 == 0x0BA9CEEAh;
	if not $local1 goto "@004@"h;
	$object0 = sub13732(0xD1BBC3A0h);
"@004@":
	$local1 = $param0 == 0xF4B2A45Fh;
	if not $local1 goto "@005@"h;
	$object0 = sub13732(0x426E34E6h);
"@005@":
	$local4 = $param0 == 0x7CAEFE7Ch;
	$local5 = $param0 == 0x0BA9CEEAh;
	$local2 = $local4 or $local5;
	$local3 = $param0 == 0xF4B2A45Fh;
	$local1 = $local2 or $local3;
	if not $local1 goto "@006@"h;
	$local1 = sub15408(6);
	$object1 = sub13665($local1);
	$object0 = set_game_variable($object0, 2, "CHARA_NAME", $object1);
	$local1 = ShowTopMessage($object0, 180, 0);
"@006@":
}

SetGasyaMode()
{
	if not $global4 goto "@000@"h;
	$global3 |= 2;
	$object0 = sub15402(6, 2, 0);
	$object1 = sub15402(6, 3, 0);
	$local1 = (float)$object0;
	$local2 = (float)$object1;
	$object2 = $local1 / $local2;
	$local1 = $object2 < 0.8f;
	if not $local1 goto "@001@"h;
	$global3 |= 4;
	goto "@002@"h;
"@001@":
	$global3 &= -5;
"@002@":
	$local1 = sub14005(0xC939DE59h, $global3);
"@000@":
}

BossEvent_RibRecoverCnt($param0)
{
	$local12 = $param0 == 0x1ABCC573h;
	$local13 = $param0 == 0x86439F37h;
	$local10 = $local12 or $local13;
	$local11 = $param0 == 0x6DBBF5E5h;
	$local8 = $local10 or $local11;
	$local9 = $param0 == 0x7CAEFE7Ch;
	$local6 = $local8 or $local9;
	$local7 = $param0 == 0xF4B2A45Fh;
	$local4 = $local6 or $local7;
	$local5 = $param0 == 0xEB7A5B90h;
	$local2 = $local4 or $local5;
	$local3 = $param0 == 0x95CD5B49h;
	$local1 = $local2 or $local3;
	if not $local1 goto "@000@"h;
	$local1 = $global8 > 0;
	if not $local1 goto "@001@"h;
	$global8--;
	$local1 = $global8 == 0;
	if not $local1 goto "@002@"h;
	$local1 = $param0 == 0x7CAEFE7Ch;
	if not $local1 goto "@003@"h;
"@003@":
	$local1 = sub15800(0f, 200f);
	$object0 = sub15411(6);
	$local1 = sub6043($object0, 0x3CA3C65Eh);
"@002@":
"@000@":
"@001@":
}

BossEventRibRecover()
{
	$object0 = sub15411(6);
	$local1 = sub15706(6, 2, 0);
	$local1 = sub15706(6, 1, 1);
	$object1 = sub15402(6, 3, 1);
	$local1 = sub15723(6, 1);
	$local1 = sub15707(6, 1, $object1);
	$local1 = sub15725(6, 12);
	$local1 = PinEvent_Delete(6, 2);
	$global2 = 0;
	$local1 = WaitFrame(10f);
	$object2 = sub15600(0xFF124EA8h, 0f, 0f, 0f);
	$local1 = sub15607($object2, $unk1, 0x64FAF507h, 0x00000767h);
	$local1 = sub15814(0, 30, 0, 0, 0, 6, 0xC91AEB7Bh, -15, 65, 10, 10);
	$local1 = sub15807(160, 30, 0, 10f, 10f);
	$local1 = WaitFrame(60f);
	$local2 = sub13732(0x010B526Fh);
	$local1 = ShowTopMessage($local2, 120, 0);
	$local1 = sub6865($object0, 0x08EB5CBDh, 1);
}

BossEvent_RibBreak_Begin()
{
	$global8 = 3;
	$object0 = sub15411(6);
	$local1 = sub6043($object0, 0xF435EBF2h);
}

BossEvent_RibBreak()
{
	$local1 = OnWeakPointDamaged_Common("");
	$local1 = WaitFrame(10f);
	$object0 = sub15600(0xC7BA0120h, 0f, 0f, 0f);
	$local1 = sub15607($object0, $unk1, 0x64FAF507h, 0x00000767h);
	$local1 = sub6865($unk1, 0x08EB5CBDh, 0);
	$local1 = WaitFrame(30);
	$local1 = DollyEvent_PlayWeakPoint(6, 0xC91AEB7Bh);
	$global2 = 1;
	$local1 = sub15706(6, 2, 1);
	$local1 = sub15706(6, 1, 0);
	$local1 = sub15725(6, 10);
	$local1 = PinEvent_Delete(6, 1);
}

DollyEvent_PlayGasya($param0, $param1, $param2, $param3, $param4, $param5, $param6, $param7, $param8, $param9, $param10, $param11, $param12)
{
	$local1 = sub15814($param0, $param10, $param3, $param4, $param5, $param1, $param2, $param6, $param8, $param11, $param12, $param7);
	$local1 = sub15805($param0, $param10, 25, $param11, $param12);
	$local3 = $param0 + $param10;
	$local2 = $local3 + 40f;
	$local1 = sub15814($local2, 25f, 10f, 4.5f, 0f, $param1, $param2, $param6, 75f, $param11, $param12, $param7);
	$local3 = $param0 + $param10;
	$local2 = $local3 + 40f;
	$local1 = sub15805($local2, 25f, 17, $param11, $param12);
	$local2 = $param0 + $param9;
	$local1 = sub15807($local2, $param10, 0, $param11, $param12);
}

DollyEvent_Gasya($param0)
{
	$local1 = DollyEvent_PlayGasya(120f, $param0, 0x7039D622h, 12f, 7f, 0f, -14f, -20f, 75f, 160f, 40f, 10f, 20f);
}

DollyEvent_PlayWeakPoint($param0, $param1)
{
	$local1 = sub15800(0f, 300f);
	$local1 = PlayFatalHitScene("", 0, $param0, $param1, 0, 0, 0, -15, 65);
}

PinEvent_Delete($param0, $param1)
{
	$object0 = sub15411($param0);
	$object1 = sub15450($object2);
	$local1 = $object2 == $param1;
	if not $local1 goto "@000@"h;
	$local1 = BattleMenuTarget_DeletePin($object0);
"@000@":
}

CrowEvent_Begin($param0)
{
	$global1 = 0;
	$object0 = sub15411($param0);
	$local1 = sub6043($object0, 0xB8482F6Dh);
}

CrowEvent_Wait($param0, $param1, $param2)
{
	$object0 = 0;
	$object1 = GetExtraObjectName(0);
	$object2 = 0;
"@000@":
	if not 1 goto "@001@"h;
	$local2 = sub6813($param0);
	$local3 = $object2 == 0;
	$local1 = $local2 and $local3;
	if not $local1 goto "@002@"h;
	$object0 = 80;
	$object2 = 1;
	$local1 = CrowEvent_ColEnable($object1, 1);
	$local1 = sub15824(0, 20, 0, -2f, 0, $object1, 0x7039D622h, 0, 45, 10, 10);
	$local1 = sub15807(80, 20, 0, 10, 10);
	$local1 = sub6810($object1, 0xCC1FD852h, 0x00000009h);
	$local1 = sub6811($object1, 1.5f);
	$local1 = sub14804("ex302000_m1");
"@002@":
	$local1 = $object0 > 0;
	if not $local1 goto "@003@"h;
	$local1 = sub6813($object1);
	if not $local1 goto "@004@"h;
	goto "@001@"h;
"@004@":
	$local1 = sub2120();
	$object0 -= $local1;
	$local1 = $object0 <= 0;
	if not $local1 goto "@005@"h;
	goto "@001@"h;
"@005@":
"@003@":
	if not $global1 goto "@006@"h;
	$local1 = sub15807(30, 20, 0, 10, 10);
	$local1 = sub6811($object1, 1f);
	return 1;
"@006@":
	$local1 = sub6814($param0, $param1, $object3);
	$local1 = $object3 >= $param2;
	if not $local1 goto "@007@"h;
"@007@":
	yield;
	goto "@000@"h;
"@001@":
	$local1 = sub6811($object1, 1f);
	return 0;
}

CrowEvent_ColEnable($param0, $param1)
{
	if not $param1 goto "@000@"h;
	$local1 = sub6051($param0, 1, 0, 0, 0, 16f);
	goto "@001@"h;
"@000@":
	$local1 = sub6050($param0);
"@001@":
}

CrowEvent_Damage()
{
	$object0 = GetExtraObjectName(0);
	$local1 = sub6819($object0, 0xF9B47AB2h, 0x00000005h);
	$local1 = WaitFrame(15);
	$local1 = sub14808("ex302000_m1");
	$local1 = sub14804("ex302000_m2");
	$local1 = sub6810($object0, 0x148BD6D6h, 0x00000009h);
	$object1 = GetExtraObjectName(1);
	$local1 = sub6816($object1, 0);
"@000@":
	$local3 = sub6813($object0);
	$local2 = not $local3;
	if $local2 goto "@000@"h;
	$local1 = sub6810($object0, 0x60284354h, 0x00000001h);
	$local1 = sub6248($object0, 0, 0, 400, 120, 0);
}

CrowEvent_TakeOff()
{
	$object0 = GetExtraObjectName(0);
	$object1 = GetExtraObjectName(1);
	$local1 = SetExtraObjectPos(0, 0f, 0f, 0f, 0f);
	$local1 = sub6227($object0, 0f, 0f, 0f);
	$local1 = sub6247($object0, 0f, 0f, 0f);
	$local1 = sub6810($object0, 0xFB7C88FEh, 0x0000000Ch);
	$local1 = sub6810($object1, 0xFB7C88FEh, 0x0000000Ch);
	$local1 = sub15750(0);
	$global5 = CrowEvent_Wait($object1, 0xFB7C88FEh, 0.7f);
	$local1 = sub15750($global5);
	$local1 = CrowEvent_ColEnable($object0, 0);
	$local1 = $global5 == 1;
	if not $local1 goto "@000@"h;
	$local1 = sub6043($object0, 0x1F0018FCh);
	goto "@001@"h;
"@000@":
	yield;
"@002@":
	$local3 = sub6813($object0);
	$local2 = not $local3;
	if $local2 goto "@002@"h;
	$local1 = sub6810($object0, 0x9B9E5312h, 0x00000009h);
	$local1 = sub6810($object1, 0x9B9E5312h, 0x00000009h);
"@001@":
	$object2 = sub15752(6);
	if not $object2 goto "@003@"h;
	$local1 = sub15400(1, 0, $object2);
"@003@":
}

BattleEvent_OnInit()
{
	$local1 = InitExtraObject();
	$local1 = BuildExtraObject("EXTRA_OBJ_RES", "data/character/x302000/x302000_p00.xc", "EXTRA_OBJ_MOT", "data/character/x302000/x302000_p20.xc");
	$local1 = BuildExtraObject("EXTRA_OBJ_RES2", "data/character/x304000/x304000_p00.xc", "EXTRA_OBJ_MOT2", "data/character/x304000/x304000_p20.xc");
	$local1 = sub16512("ex3", "data/character/x302000/x302000_p10.xc");
	$global3 = 1;
	$local1 = sub14005(0xC939DE59h, $global3);
	$global8 = 0;
}

BattleEvent_OnFinalize()
{
	$local1 = FinalizeExtraObject("data/character/x302000/x302000_p00.xc", "data/character/x302000/x302000_p20.xc");
	$local1 = FinalizeExtraObject("data/character/x304000/x304000_p00.xc", "data/character/x304000/x304000_p20.xc");
}

BattleEvent_OnStartEvent()
{
	$global12_isVictory = 0;
	$local1 = sub15749(6, 0f, 0f, -28f, 0f);
	$local1 = sub15806(0f, 16f, 76.6f, 0f, 14.5f, 45f, 0f, 30f);
	$local1 = sub15740(6);
	$local1 = sub15801(0f, 0f, 0f, 15.44f, 75.5f, 0, 0f, 0f);
	$local1 = sub15802(0f, 0f, 0f, 16.11f, 45f, 0, 0f, 0f);
	$local1 = sub15706(7, 0, 0);
	$local1 = sub15700(6, 7, 0x247DCA79h, 0x5013BAC1h, 0x1487E25Dh);
	$local1 = sub15725(6, 12);
	$local1 = sub15730(6, 2);
	$local1 = sub15730(6, 1);
	$local1 = sub15706(6, 2, 0);
	$global2 = 0;
	$object1 = sub15411(6);
	$object0 = sub13661(0xF447F0BCh, 3);
	$local1 = CreateExtraObject(0, "EXTRA_OBJ_RES", "EXTRA_OBJ_MOT");
	$local1 = CreateExtraObject(1, "EXTRA_OBJ_RES2", "EXTRA_OBJ_MOT2");
	$local1 = SetExtraObjectPos(0, 0f, -2000f, 0f, 0f);
	$local1 = SetExtraObjectPos(1, 0f, 0f, -32f, 0f);
	$object2 = GetExtraObjectName(0);
	$local1 = sub6810($object2, 0x43C3094Ah, 0x00000004h, 0);
	$local1 = sub6247($object2, 0f, 0f, 0f);
	$local1 = sub6202($object2, 1f, 1f, 1f);
	$local1 = sub3730($object2, "ex3");
	$object3 = GetExtraObjectName(1);
	$local1 = sub6810($object3, 0x43C3094Ah, 0x00000004h, 0);
	$local1 = sub6247($object3, 0f, 0f, 0f);
	$local1 = sub6202($object3, $object0, $object0, $object0);
	$local1 = sub6013($object2, $object3, 0xED2C1F6Fh);
	$local1 = CrowEvent_ColEnable($object2, 0);
	$global4 = 0;
	$local1 = sub15755(6, 1);
	$local1 = sub15470(0x1ABCC573h);
	$local1 = sub15470(0x6DBBF5E5h);
	$local1 = sub15470(0xF4B2A45Fh);
	$local1 = sub15470(0x86439F37h);
	$local1 = sub15470(0x7BC33A65h);
	$local1 = sub15470(0x0CC40AF3h);
	$local1 = sub15470(0x92A09F50h);
	$local1 = sub15470(0xE5A7AFC6h);
	$local1 = sub15470(0x7CAEFE7Ch);
	$local1 = sub15470(0x0BA9CEEAh);
	$local1 = sub15470(0x8CD66A08h);
	$local1 = sub15470(0x62D80B24h);
	$local1 = sub15470(0x15DF3BB2h);
	$local1 = sub15470(0x8BBBAE11h);
	$local1 = sub15470(0x49F558E7h);
	$local1 = sub15470(0x3EF26871h);
	$local1 = sub15470(0xA096FDD2h);
	$local1 = sub15747(7, 1);
	$global6 = 0;
	$global7 = 0;
}

BattleEvent_OnLastHit($param0, $param1, $param2, $param3, $param4, $param5)
{
	$local1 = $param3 == 6;
	if not $local1 goto "@000@"h;
	$object0 = sub15411(6);
	$object1 = sub15402(6, 2, 0);
	$local1 = $object1 <= 0;
	if not $local1 goto "@001@"h;
	if not $global9 goto "@002@"h;
	return 0;
"@002@":
	$local1 = OnDeathEvent_Common("", 6, 0xC91AEB7Bh, 90f, 80f, 0f, 60f, 180f, 202f, 90f);
	return 0;
"@001@":
	$local1 = $param4 == 1;
	if not $local1 goto "@003@"h;
	$local1 = $global2 == 0;
	if not $local1 goto "@004@"h;
	$object2 = sub15402(6, 2, 1);
	$local1 = $object2 <= 0;
	if not $local1 goto "@005@"h;
	$local1 = BossEvent_RibBreak_Begin();
"@004@":
"@005@":
"@003@":
"@000@":
}

BattleEvent_OnHit($param0, $param1, $param2, $param3, $param4)
{
	$local1 = $param3 == 6;
	if not $local1 goto "@000@"h;
	$local1 = sub15419($param0);
	if not $local1 goto "@001@"h;
	$local1 = SetGasyaMode();
	$object0 = sub15411(6);
	$local6 = $param4 == 0;
	$local7 = not $global6;
	$local4 = $local6 and $local7;
	$local5 = $param0 != 0x7CAEFE7Ch;
	$local2 = $local4 and $local5;
	$local3 = not $global7;
	$local1 = $local2 and $local3;
	if not $local1 goto "@002@"h;
	$local1 = sub6810($object0, 0x8D82876Ch, 0x00000009h);
	$local1 = sub6819($object0, 0xC85C602Fh, 0x00000001h);
"@002@":
	$local1 = $param0 == 0x7CAEFE7Ch;
	if not $local1 goto "@003@"h;
	$local1 = sub6810($object0, 0x565481FAh, 0x00000009h);
"@003@":
	if not $global6 goto "@004@"h;
	$local1 = OnWeakPointDamaged_Common("");
"@004@":
"@000@":
"@001@":
	$local4 = $param0 == 0xE2CA6BDFh;
	$local5 = $param0 == 0xFBD15A9Eh;
	$local2 = $local4 or $local5;
	$local3 = $param0 == 0xD0FC095Dh;
	$local1 = $local2 or $local3;
	if not $local1 goto "@005@"h;
	$local1 = sub15751(6);
"@005@":
	$local4 = $param0 == 0x7BC33A65h;
	$local5 = $param0 == 0x62D80B24h;
	$local2 = $local4 or $local5;
	$local3 = $param0 == 0x49F558E7h;
	$local1 = $local2 or $local3;
	if not $local1 goto "@006@"h;
	$local1 = not $global5;
	if not $local1 goto "@007@"h;
	$local1 = DollyEvent_Gasya(7);
"@007@":
	$local1 = sub15751(6);
"@006@":
	$local4 = $param0 == 0x0CC40AF3h;
	$local5 = $param0 == 0x15DF3BB2h;
	$local2 = $local4 or $local5;
	$local3 = $param0 == 0x3EF26871h;
	$local1 = $local2 or $local3;
	if not $local1 goto "@008@"h;
	$global4++;
	$local1 = $global4 >= 1000;
	if not $local1 goto "@009@"h;
	$global4 = 1000;
"@009@":
	$local1 = SetGasyaMode();
	$local1 = sub15751(6);
"@008@":
	$local1 = $param0 == 0x7CAEFE7Ch;
	if not $local1 goto "@010@"h;
	$local1 = sub15751(6);
"@010@":
	$local1 = $param0 == 0x0BA9CEEAh;
	if not $local1 goto "@011@"h;
	$local1 = sub15751(6);
	$global7 = 1;
"@011@":
	$local1 = $param0 == 0xD3B70DFCh;
	if not $local1 goto "@012@"h;
	$local1 = $global0 > 0;
	if not $local1 goto "@013@"h;
	$global0--;
	$local1 = $global0 <= 0;
	if not $local1 goto "@014@"h;
	$local1 = sub15715(6, 0, 0);
	$global6 = 0;
"@014@":
"@013@":
"@012@":
}

BattleEvent_OnExtraObjectHit($param0, $param1)
{
	$local1 = $param0 == 0;
	if not $local1 goto "@000@"h;
	$global1 = 1;
"@000@":
}

BattleEvent_OnActStart($param0, $param1)
{
}

BattleEvent_OnTurnStart($param0, $param1)
{
}

BattleEvent_OnExecuteCommand($param0, $param1, $param2, $param3, $param4)
{
	$local1 = $param1 == 6;
	if not $local1 goto "@000@"h;
	$local4 = $param0 == 0x0CC40AF3h;
	$local5 = $param0 == 0x15DF3BB2h;
	$local2 = $local4 or $local5;
	$local3 = $param0 == 0x3EF26871h;
	$local1 = $local2 or $local3;
	if not $local1 goto "@001@"h;
"@001@":
	$local4 = $param0 == 0xE2CA6BDFh;
	$local5 = $param0 == 0xFBD15A9Eh;
	$local2 = $local4 or $local5;
	$local3 = $param0 == 0xD0FC095Dh;
	$local1 = $local2 or $local3;
	if not $local1 goto "@002@"h;
	$local1 = sub15423(6, 0, $param0);
	$local1 = CrowEvent_Begin(6);
"@002@":
	$local1 = $param0 == 0xEB7A5B90h;
	if not $local1 goto "@003@"h;
	$global7 = 0;
"@003@":
	$local8 = $param0 == 0x1ABCC573h;
	$local9 = $param0 == 0x6DBBF5E5h;
	$local6 = $local8 or $local9;
	$local7 = $param0 == 0x86439F37h;
	$local4 = $local6 or $local7;
	$local5 = $param0 == 0x7CAEFE7Ch;
	$local2 = $local4 or $local5;
	$local3 = $param0 == 0x0BA9CEEAh;
	$local1 = $local2 or $local3;
	if not $local1 goto "@004@"h;
	$local1 = sub14804("ex305000_m1");
	$local1 = ShowCapsuleMessage($param0);
"@004@":
	$local1 = $param0 == 0xF4B2A45Fh;
	if not $local1 goto "@005@"h;
	$local1 = sub14804("ex305000_m2");
	$local1 = ShowCapsuleMessage($param0);
"@005@":
"@000@":
}

BattleEvent_OnCmdEnd($param0, $param1, $param2, $param3, $param4)
{
	$local1 = $param1 == 6;
	if not $local1 goto "@000@"h;
	$local1 = $param0 == 0x7CAEFE7Ch;
	if not $local1 goto "@001@"h;
	$global0 = 2;
	$local1 = sub15715(6, 0, 1);
	$global6 = 1;
"@001@":
	$local1 = BossEvent_RibRecoverCnt($param0);
"@000@":
}

BattleEvent_OnSpSkillDemoEnd($param0, $param1)
{
}

BattleEvent_OnSpSkillDemoStart($param0, $param1)
{
}

BattleEvent_OnActEnd($param0, $param1, $param2, $param3, $param4)
{
}

OnDeathEvent_x301($param0, $param1, $param2, $param3, $param4, $param5, $param6, $param7, $param8, $param9)
{
	if not $global9 goto "@000@"h;
	return 0;
"@000@":
	$global9 = 1;
	$local1 = sub15823();
	$object0 = sub15411($param1);
	$local1 = BattleMenuTarget_DeletePin($object0);
	$local1 = DeleteWandererSpirit();
	$local1 = sub15417(256, 1);
	$local1 = sub14804($param0);
	$local2 = $param8 - 10;
	$local2 = $param8 - 10;
	$local1 = StageEffect_FlashWithSE(50f, 20f, 1f);
	$local1 = sub15816($param7, 10f, 3f, 3f);
	$local2 = $param8 + 10f;
	$local1 = sub15822($local2, "sy0502");
	$local2 = $param8 + 10f;
	$local1 = sub15818($local2, 60f, 0.8f);
	$local2 = $param8 + 10f;
	$local1 = sub15816($local2, 90f, 5f, 5f);
	$local2 = $param8 + 120;
	$local1 = WaitFrame($local2);
	$local1 = sub15417(256, 0);
}
```
