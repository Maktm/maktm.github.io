---
layout: post
title:  "Exported Functions from Dunia.dll"
date:   2015-11-10
categories: jekyll update
---

This post is for anyone that decides to toy around with Far Cry 2 in the future (or now). If you're looking for a game to reverse then you should take this into consideration.

First of all there's already a post for calling engine functions here:

http://www.unknowncheats.me/forum/fa...tions-wip.html

But it only mentions 2 functions and uses inline assembler (which is not necessary). The entire game is within Dunia.dll (FarCry2.exe is a hollow process that puts together the different modules) and with dumpbin you can see the exported functions. Here's what I got from dumpbin:

In order to call the functions just use GetProcAddress.

In order to get the unmangled version of the functions you can do some stuff but I can't remember them off the top of my head.

I haven't done anything else with the game other than see the modules using CFFExplorer and dump parts of it using dumpbin but I'll post here if I get bored again and go back to looking at the game.

Exported functions:

```
1    0 00A38560 ??0CThreadSndInformer@@QAE@XZ

2    1 00A38560 ??0CThreadToolInformer@@QAE@XZ

3    2 00A38540 ??4CThreadInformer@@QAEAAV0@ABV0@@Z

4    3 00A38540 ??4CThreadSndInformer@@QAEAAV0@ABV0@@Z

5    4 00A38540 ??4CThreadToolInformer@@QAEAAV0@ABV0@@Z

6    5 00A1CC8E ??4_Init_locks@std@@QAEAAV01@ABV01@@Z

7    6 00A38520 ??8sndCString@@QBEHABV0@@Z

8    7 00A38500 ??8sndCString@@QBEHPBD@Z

9    8 00A6BA60 ?GetLastFile@CThreadInformer@@QBEPADXZ

10    9 00ACA5E0 ?GetLastLine@CThreadInformer@@QBEKXZ

11    A 003FC9A0 ?GetLastThread@CThreadInformer@@QBEKXZ

12    B 00006530 ?RunGame@@YA_NPAUHINSTANCE__@@PBD@Z

13    C 00001CE0 AddFunctionCB

14    D 00001BF0 CloseDuniaEngine

15    E 00656330 FCB_Benchmark_GetHwNbCore

16    F 00656380 FCB_Benchmark_GetMaxTextureHeight

17   10 00656350 FCB_Benchmark_GetMaxTextureWidth

18   11 006563B0 FCB_Benchmark_GetOptimalResolution

19   12 00656320 FCB_Benchmark_GetRenderScore

20   13 00656310 FCB_Benchmark_IsD3D10Supported

21   14 006562F0 FCB_Benchmark_SystemDetectionInit

22   15 00656300 FCB_Benchmark_SystemDetectionShutdown

23   16 00882FC0 FCE_Brush_Create

24   17 0032EA30 FCE_Brush_Destroy

25   18 0087FA00 FCE_BudgetManager_GetMemoryUsage

26   19 0087FA10 FCE_BudgetManager_GetObjectUsage

27   1A 00881E10 FCE_Camera_GetAngles

28   1B 00881ED0 FCE_Camera_GetFOV

29   1C 00881E40 FCE_Camera_GetFrontVector

30   1D 00881D90 FCE_Camera_GetPos

31   1E 00881E70 FCE_Camera_GetRightVector

32   1F 0087F020 FCE_Camera_GetSpeed

33   20 00881EA0 FCE_Camera_GetUpVector

34   21 0087EFE0 FCE_Camera_Input_Forward

35   22 0087F000 FCE_Camera_Input_Lateral

36   23 008854C0 FCE_Camera_Rotate

37   24 00883C40 FCE_Camera_SetAngles

38   25 00881DC0 FCE_Camera_SetPos

39   26 0087F030 FCE_Camera_SetSpeed

40   27 0087F050 FCE_Camera_SetSpeedFactor

41   28 0087F560 FCE_CollectionManager_AssignCollectionId

42   29 0087F5F0 FCE_CollectionManager_ClearMaskId

43   2A 0087F540 FCE_CollectionManager_GetCollectionEntryFromId

44   2B 00882780 FCE_CollectionManager_UpdateCollections

45   2C 0087F590 FCE_CollectionManager_WriteMaskCircle

46   2D 0087F5C0 FCE_CollectionManager_WriteMaskSquare

47   2E 00887420 FCE_Collection_Paint

48   2F 00882820 FCE_Collection_Paint_End

49   30 00883A10 FCE_Core_GetAnglesFromAxis

50   31 00882F50 FCE_Core_GetAnglesFromDir

51   32 00883940 FCE_Core_GetAxisFromAngles

52   33 00883530 FCE_Core_Points_Create

53   34 00883B40 FCE_Core_Points_Destroy

54   35 0087ECB0 FCE_Document_ClearSnapshot

55   36 00669070 FCE_Document_Export

56   37 0087ECE0 FCE_Document_FinalizeMap

57   38 008819C0 FCE_Document_GetAuthorName

58   39 0087EC40 FCE_Document_GetBattlefieldSize

59   3A 008819A0 FCE_Document_GetCreatorName

60   3B 00881980 FCE_Document_GetMapName

61   3C 0087EC70 FCE_Document_GetPlayerSize

62   3D 00881A50 FCE_Document_GetSnapshotAngle

63   3E 008819E0 FCE_Document_GetSnapshotPos

64   3F 0087ECA0 FCE_Document_IsSnapshotSet

65   40 0087EBF0 FCE_Document_Load

66   41 0087EBE0 FCE_Document_Reset

67   42 0087EC10 FCE_Document_Save

68   43 00886F40 FCE_Document_SetAuthorName

69   44 0087EC50 FCE_Document_SetBattlefieldSize

70   45 00886F10 FCE_Document_SetCreatorName

71   46 00887010 FCE_Document_SetMapName

72   47 0087EC80 FCE_Document_SetPlayerSize

73   48 00883BF0 FCE_Document_SetSnapshotAngle

74   49 00881A10 FCE_Document_SetSnapshotPos

75   4A 0087ECC0 FCE_Document_TakeSnapshot

76   4B 0087EC30 FCE_Document_Validate

77   4C 00881A80 FCE_Draw_Arrow

78   4D 0087EFA0 FCE_Draw_BeginGroup

79   4E 00881B50 FCE_Draw_Dot

80   4F 0087EFC0 FCE_Draw_EndGroup

81   50 00880560 FCE_Draw_ScreenCircleOutlined

82   51 008805F0 FCE_Draw_ScreenRectangleOutlined

83   52 00881C00 FCE_Draw_SegmentedLineSegment

84   53 00880680 FCE_Draw_Terrain_Circle

85   54 00880720 FCE_Draw_Terrain_Square

86   55 00881CE0 FCE_Draw_WireBoxFromBottomZ

87   56 008807C0 FCE_Draw_WireRegionFromTerrain

88   57 0087EF40 FCE_EditorSettings_GetEngineQuality

89   58 0087EE40 FCE_EditorSettings_GetGridResolution

90   59 0087EEB0 FCE_EditorSettings_IsAutoSnappingObjects

91   5A 0087EED0 FCE_EditorSettings_IsAutoSnappingObjectsRotation

92   5B 0087EEF0 FCE_EditorSettings_IsAutoSnappingObjectsTerrain

93   5C 0087EF10 FCE_EditorSettings_IsCameraClippedTerrain

94   5D 0087ECF0 FCE_EditorSettings_IsCollectionVisible

95   5E 0087ED20 FCE_EditorSettings_IsFogVisible

96   5F 0087EE10 FCE_EditorSettings_IsGridVisible

97   60 0087EDB0 FCE_EditorSettings_IsIconsVisible

98   61 0087EE70 FCE_EditorSettings_IsInvincible

99   62 0087EF70 FCE_EditorSettings_IsKillDistanceOverride

100   63 0087ED50 FCE_EditorSettings_IsShadowVisible

101   64 0087EE90 FCE_EditorSettings_IsSnappingObjectsToTerrain

102   65 0087EDE0 FCE_EditorSettings_IsSoundEnabled

103   66 0087ED80 FCE_EditorSettings_IsWaterVisible

104   67 0087EEC0 FCE_EditorSettings_SetAutoSnappingObjects

105   68 0087EEE0 FCE_EditorSettings_SetAutoSnappingObjectsRotation

106   69 0087EF00 FCE_EditorSettings_SetAutoSnappingObjectsTerrain

107   6A 0087EF20 FCE_EditorSettings_SetCameraClipTerrain

108   6B 0087EF50 FCE_EditorSettings_SetEngineQuality

109   6C 0087EE50 FCE_EditorSettings_SetGridResolution

110   6D 0087EE80 FCE_EditorSettings_SetInvincible

111   6E 0087EF80 FCE_EditorSettings_SetKillDistanceOverride

112   6F 0087EEA0 FCE_EditorSettings_SetSnapObjectsToTerrain

113   70 0087EDF0 FCE_EditorSettings_SetSoundEnabled

114   71 0087ED00 FCE_EditorSettings_ShowCollections

115   72 0087ED30 FCE_EditorSettings_ShowFog

116   73 0087EE20 FCE_EditorSettings_ShowGrid

117   74 0087EDC0 FCE_EditorSettings_ShowIcons

118   75 0087ED60 FCE_EditorSettings_ShowShadow

119   76 0087ED90 FCE_EditorSettings_ShowWater

120   77 0087EBA0 FCE_Editor_EnableUI_Callback

121   78 0087EB40 FCE_Editor_Event_Callback

122   79 0087EBC0 FCE_Editor_GetFrameTime

123   7A 00881630 FCE_Editor_GetScreenPointFromWorldPos

124   7B 008816B0 FCE_Editor_GetWorldRayFromScreenPoint

125   7C 00883BD0 FCE_Editor_IsIngame

126   7D 0087EAF0 FCE_Editor_IsInitialized

127   7E 0087EBB0 FCE_Editor_IsLoadPending

128   7F 0087EB60 FCE_Editor_LoadCompleted_Callback

129   80 008817E0 FCE_Editor_RayCastPhysics

130   81 008818B0 FCE_Editor_RayCastPhysics2

131   82 00881740 FCE_Editor_RayCastTerrain

132   83 0087EB80 FCE_Editor_SaveCompleted_Callback

133   84 00883B90 FCE_Editor_ToggleIngame

134   85 0087EB30 FCE_Editor_Update_Callback

135   86 0087EBD0 FCE_Editor_ValidateIngame

136   87 0087EA30 FCE_Engine_AutoAcquireInput

137   88 00886E00 FCE_Engine_GetPersonalPath

138   89 0087EAC0 FCE_Engine_GetStormFactor

139   8A 0087EA50 FCE_Engine_GetTimeOfDay

140   8B 00880520 FCE_Engine_IsConsoleOpen

141   8C 0087EAD0 FCE_Engine_SetStormFactor

142   8D 0087EA80 FCE_Engine_SetTimeOfDay

143   8E 0087EA00 FCE_Engine_UpdateViewport

144   8F 0087F4E0 FCE_Gizmo_Create

145   90 00880910 FCE_Gizmo_Destroy

146   91 0087F500 FCE_Gizmo_GetActive

147   92 00882620 FCE_Gizmo_GetAxis

148   93 008825D0 FCE_Gizmo_GetPos

149   94 0087F530 FCE_Gizmo_Hide

150   95 00882720 FCE_Gizmo_HitTest

151   96 0087F520 FCE_Gizmo_Redraw

152   97 0087F510 FCE_Gizmo_SetActive

153   98 008826B0 FCE_Gizmo_SetAxis

154   99 008825F0 FCE_Gizmo_SetPos

155   9A 00880C50 FCE_ImageMap_Clone

156   9B 00880B20 FCE_ImageMap_ConvertTo24bit

157   9C 0032EA30 FCE_ImageMap_Destroy

158   9D 0087F9E0 FCE_ImageMap_GetSize

159   9E 00880870 FCE_Inventory_Collection_GetBurnProfile

160   9F 00881F20 FCE_Inventory_Collection_GetChild

161   A0 008833C0 FCE_Inventory_Collection_GetChildCount

162   A1 00881F40 FCE_Inventory_Collection_GetDisplay

163   A2 008808B0 FCE_Inventory_Collection_GetParent

164   A3 00880860 FCE_Inventory_Collection_GetRoot

165   A4 00669070 FCE_Inventory_Object_AddPivot

166   A5 00669070 FCE_Inventory_Object_ClearPivots

167   A6 00881EE0 FCE_Inventory_Object_GetChild

168   A7 008833A0 FCE_Inventory_Object_GetChildCount

169   A8 00881F00 FCE_Inventory_Object_GetDisplay

170   A9 0087F070 FCE_Inventory_Object_GetId

171   AA 008808B0 FCE_Inventory_Object_GetParent

172   AB 006D6580 FCE_Inventory_Object_GetPivotCount

173   AC 00880850 FCE_Inventory_Object_GetRoot

174   AD 006A9BC0 FCE_Inventory_Object_GetZOffset

175   AE 0087F080 FCE_Inventory_Object_IsAutoOrientation

176   AF 003D6120 FCE_Inventory_Object_IsAutoPivot

177   B0 00669070 FCE_Inventory_Object_SavePivots

178   B1 00669070 FCE_Inventory_Object_SetAutoPivot

179   B2 00669070 FCE_Inventory_Object_SetPivot

180   B3 00669070 FCE_Inventory_Object_SetPivots

181   B4 00669070 FCE_Inventory_Object_SetZOffset

182   B5 00881F80 FCE_Inventory_Spline_GetChild

183   B6 00883400 FCE_Inventory_Spline_GetChildCount

184   B7 00881FC0 FCE_Inventory_Spline_GetDisplay

185   B8 008808B0 FCE_Inventory_Spline_GetParent

186   B9 008808A0 FCE_Inventory_Spline_GetRoot

187   BA 00881F60 FCE_Inventory_Texture_GetChild

188   BB 008833E0 FCE_Inventory_Texture_GetChildCount

189   BC 00881FC0 FCE_Inventory_Texture_GetDisplay

190   BD 008808B0 FCE_Inventory_Texture_GetParent

191   BE 00880890 FCE_Inventory_Texture_GetRoot

192   BF 00881FA0 FCE_Inventory_Wilderness_GetChild

193   C0 00883420 FCE_Inventory_Wilderness_GetChildCount

194   C1 00881FC0 FCE_Inventory_Wilderness_GetDisplay

195   C2 008808B0 FCE_Inventory_Wilderness_GetParent

196   C3 008808C0 FCE_Inventory_Wilderness_GetRoot

197   C4 008822F0 FCE_ObjectManager_GetObjectFromScreenPoint

198   C5 0087F240 FCE_ObjectManager_GetObjectsFromMagicWand

199   C6 0087F200 FCE_ObjectManager_GetObjectsFromScreenRect

200   C7 0087F260 FCE_ObjectManager_UnfreezeObjects

201   C8 0087F440 FCE_ObjectRenderer_Clear

202   C9 0087F4D0 FCE_ObjectRenderer_ClearSnapshot

203   CA 0087F4B0 FCE_ObjectRenderer_GetSnapshot

204   CB 0087F4C0 FCE_ObjectRenderer_GetSnapshotEntry

205   CC 0087F490 FCE_ObjectRenderer_IsSnapshotReady

206   CD 0087F470 FCE_ObjectRenderer_RenderObject

207   CE 0087F450 FCE_ObjectRenderer_SetActive

208   CF 0087F2A0 FCE_ObjectSelection_Add

209   D0 0087F2C0 FCE_ObjectSelection_AddSelection

210   D1 0087F290 FCE_ObjectSelection_Clear

211   D2 0087F3D0 FCE_ObjectSelection_ClearState

212   D3 0087F350 FCE_ObjectSelection_Clone

213   D4 0087F380 FCE_ObjectSelection_ComputeCenter

214   D5 0087F270 FCE_ObjectSelection_Create

215   D6 0087F370 FCE_ObjectSelection_Delete

216   D7 008808D0 FCE_ObjectSelection_Destroy

217   D8 0087F390 FCE_ObjectSelection_DropToGround

218   D9 008808F0 FCE_ObjectSelection_Get

219   DA 00882370 FCE_ObjectSelection_GetCenter

220   DB 008823C0 FCE_ObjectSelection_GetComputeCenter

221   DC 00882360 FCE_ObjectSelection_GetCount

222   DD 0087F3C0 FCE_ObjectSelection_GetPhysEntities

223   DE 0087F320 FCE_ObjectSelection_GetValidObjects

224   DF 00882400 FCE_ObjectSelection_GetWorldBounds

225   E0 0087F3E0 FCE_ObjectSelection_LoadState

226   E1 00882470 FCE_ObjectSelection_MoveTo

227   E2 0087F340 FCE_ObjectSelection_RemoveInvalidObjects

228   E3 00884180 FCE_ObjectSelection_Rotate

229   E4 00884200 FCE_ObjectSelection_Rotate3

230   E5 00884290 FCE_ObjectSelection_RotateCenter

231   E6 00884320 FCE_ObjectSelection_RotateGimbal

232   E7 008842E0 FCE_ObjectSelection_RotateLocal3

233   E8 0087F3F0 FCE_ObjectSelection_SaveState

234   E9 00882390 FCE_ObjectSelection_SetCenter

235   EA 0087F3B0 FCE_ObjectSelection_SnapToClosestObjects

236   EB 008824B0 FCE_ObjectSelection_SnapToPivot

237   EC 0087F2E0 FCE_ObjectSelection_ToggleObject

238   ED 0087F300 FCE_ObjectSelection_ToggleSelection

239   EE 0087F400 FCE_ObjectViewer_SetActive

240   EF 0087F420 FCE_ObjectViewer_SetObject

241   F0 0087F0E0 FCE_Object_AddRef

242   F1 0087F110 FCE_Object_Clone

243   F2 00884090 FCE_Object_ComputeAutoOrientation

244   F3 00881FE0 FCE_Object_Create_FromEntry

245   F4 0087F0A0 FCE_Object_Destroy

246   F5 0087F1C0 FCE_Object_DropToGround

247   F6 008820D0 FCE_Object_GetAngles

248   F7 00882100 FCE_Object_GetBounds

249   F8 00882220 FCE_Object_GetClosestPivot

250   F9 0087F130 FCE_Object_GetEntry

251   FA 0087F1F0 FCE_Object_GetPhysEntities

252   FB 00882180 FCE_Object_GetPivot

253   FC 00882060 FCE_Object_GetPos

254   FD 0087F120 FCE_Object_IsLoaded

255   FE 0087F140 FCE_Object_IsVisible

256   FF 0087F0F0 FCE_Object_Release

257  100 00884050 FCE_Object_SetAngles

258  101 0087F1A0 FCE_Object_SetFreeze

259  102 0087F180 FCE_Object_SetHighlight

260  103 00882090 FCE_Object_SetPos

261  104 0087F160 FCE_Object_SetVisible

262  105 0087F1E0 FCE_Object_SnapToClosestObject

263  106 00883530 FCE_PhysEntityVector_Create

264  107 00884460 FCE_PhysEntityVector_Destroy

265  108 00882870 FCE_ScriptFunction_GetDescription

266  109 0087F770 FCE_ScriptFunction_GetName

267  10A 008808B0 FCE_ScriptFunction_GetPrototype

268  10B 00880B00 FCE_Script_GetFunction

269  10C 008829B0 FCE_Script_GetNumFunctions

270  10D 00884410 FCE_Snapshot_Create

271  10E 008855A0 FCE_Snapshot_Destroy

272  10F 00880A20 FCE_Snapshot_GetData

273  110 0087F8F0 FCE_SplineController_ClearSelection

274  111 0087F8C0 FCE_SplineController_Create

275  112 0087F930 FCE_SplineController_DeleteSelection

276  113 0032EA30 FCE_SplineController_Destroy

277  114 0087F900 FCE_SplineController_IsSelected

278  115 00882950 FCE_SplineController_MoveSelection

279  116 00880AA0 FCE_SplineController_SelectFromScreenRect

280  117 0087F910 FCE_SplineController_SetSelected

281  118 0087F8E0 FCE_SplineController_SetSpline

282  119 0087F940 FCE_SplineManager_CreateRoad

283  11A 0087F960 FCE_SplineManager_DestroyRoad

284  11B 0087F990 FCE_SplineManager_GetPlayableZone

285  11C 0087F980 FCE_SplineManager_GetRoadFromId

286  11D 0087F860 FCE_SplineRoad_GetEntry

287  11E 0087F880 FCE_SplineRoad_GetWidth

288  11F 0087F870 FCE_SplineRoad_SetEntry

289  120 0087F890 FCE_SplineRoad_SetWidth

290  121 0087F8B0 FCE_SplineZone_Reset

291  122 00883440 FCE_Spline_AddPoint

292  123 0087F7E0 FCE_Spline_Clear

293  124 0087F7C0 FCE_Spline_Create

294  125 0032EA30 FCE_Spline_Destroy

295  126 0087F840 FCE_Spline_Draw

296  127 0087F830 FCE_Spline_FinalizeSpline

297  128 00882870 FCE_Spline_GetNumPoints

298  129 008834C0 FCE_Spline_GetPoint

299  12A 00882880 FCE_Spline_HitTestPoints

300  12B 008828F0 FCE_Spline_HitTestSegments

301  12C 00883480 FCE_Spline_InsertPoint

302  12D 0087F810 FCE_Spline_OptimizePoint

303  12E 0087F7F0 FCE_Spline_RemovePoint

304  12F 0087F800 FCE_Spline_RemoveSimilarPoints

305  130 008834F0 FCE_Spline_SetPoint

306  131 0087F820 FCE_Spline_UpdateSpline

307  132 00880A50 FCE_Spline_UpdateSplineHeight

308  133 0087F640 FCE_TerrainManager_AssignTextureId

309  134 0087F660 FCE_TerrainManager_ClearTextureId

310  135 0087F610 FCE_TerrainManager_GetHeightAt

311  136 0087F630 FCE_TerrainManager_GetTextureEntryFromId

312  137 0087F680 FCE_TerrainManager_GetWaterLevel

313  138 0087F690 FCE_TerrainManager_SetWaterLevel

314  139 008870B0 FCE_Terrain_Bump

315  13A 008832A0 FCE_Terrain_Bump_End

316  13B 00883F90 FCE_Terrain_Erosion

317  13C 00883390 FCE_Terrain_Erosion_End

318  13D 00887240 FCE_Terrain_Grab

319  13E 00887170 FCE_Terrain_Grab_Begin

320  13F 008832E0 FCE_Terrain_Grab_End

321  140 008876A0 FCE_Terrain_Noise

322  141 00883330 FCE_Terrain_Noise_Begin

323  142 00883370 FCE_Terrain_Noise_End

324  143 008870F0 FCE_Terrain_RaiseLower

325  144 008832B0 FCE_Terrain_RaiseLower_End

326  145 00887290 FCE_Terrain_Ramp

327  146 00887130 FCE_Terrain_SetHeight

328  147 008832D0 FCE_Terrain_SetHeight_End

329  148 00887260 FCE_Terrain_Smooth

330  149 00883300 FCE_Terrain_Smooth_End

331  14A 008873D0 FCE_Terrain_Terrace

332  14B 00883310 FCE_Terrain_Terrace_End

333  14C 00887460 FCE_Texture_Paint

334  14D 008874F0 FCE_Texture_PaintConstraints

335  14E 00880930 FCE_Texture_PaintConstraints_Begin

336  14F 008843F0 FCE_Texture_PaintConstraints_End

337  150 008843E0 FCE_Texture_Paint_End

338  151 0087F6E0 FCE_UndoManager_CommitUndo

339  152 00882840 FCE_UndoManager_GetRedoCount

340  153 00882830 FCE_UndoManager_GetUndoCount

341  154 0087F6D0 FCE_UndoManager_RecordUndo

342  155 0087F6C0 FCE_UndoManager_Redo

343  156 0087F6B0 FCE_UndoManager_Undo

344  157 0087F780 FCE_ValidationRecord_GetFlags

345  158 00882850 FCE_ValidationRecord_GetMessage

346  159 0087F790 FCE_ValidationRecord_GetObject

347  15A 0087F770 FCE_ValidationRecord_GetSeverity

348  15B 0032EA30 FCE_ValidationReport_Destroy

349  15C 0087F740 FCE_ValidationReport_GetCount

350  15D 0087F750 FCE_ValidationReport_GetRecord

351  15E 0087F6F0 FCE_Validation_Game

352  15F 00880980 FCE_Validation_GameMode

353  160 00669070 FCE_Wilderness_Desert

354  161 0087F9A0 FCE_Wilderness_Script

355  162 0087F9C0 FCE_Wilderness_ScriptBuffer

356  163 00882980 FCE_Wilderness_ScriptEntry

357  164 007F69C0 FCS_Server_CanCallVote

358  165 007F64B0 FCS_Server_ConsoleAutoComplete

359  166 007F63D0 FCS_Server_Console_Callback

360  167 007F6560 FCS_Server_CreateConsoleObserver

361  168 007F6A60 FCS_Server_CreateMatchStats

362  169 007F6720 FCS_Server_CreatePlayerStats

363  16A 007F6480 FCS_Server_DeleteConsoleObserver

364  16B 007F6860 FCS_Server_DeleteMatchStats

365  16C 0032E7A0 FCS_Server_DeletePlayerStats

366  16D 007F6440 FCS_Server_EnablePunkbuster

367  16E 007F6920 FCS_Server_ExecuteConsole

368  16F 007F66B0 FCS_Server_GetMatchStats_Map

369  170 007F66D0 FCS_Server_GetMatchStats_Mode

370  171 007F64E0 FCS_Server_GetMatchStats_PlayerId

371  172 007F6620 FCS_Server_GetMatchStats_PlayerName

372  173 007F6500 FCS_Server_GetMatchStats_PlayerScore

373  174 007F6650 FCS_Server_GetMatchStats_PlayerTeam

374  175 007F6520 FCS_Server_GetMatchStats_PlayerVIP

375  176 007F6610 FCS_Server_GetMatchStats_PlayersCount

376  177 007F6690 FCS_Server_GetMatchStats_TeamName

377  178 007F6540 FCS_Server_GetMatchStats_TeamScore

378  179 007F6680 FCS_Server_GetMatchStats_TeamsCount

379  17A 008808B0 FCS_Server_GetPlayerStats_Deaths

380  17B 0087F770 FCS_Server_GetPlayerStats_Kills

381  17C 007F6420 FCS_Server_GetPlayerStats_Losses

382  17D 0087F130 FCS_Server_GetPlayerStats_Rank

383  17E 007F6400 FCS_Server_GetPlayerStats_Revives

384  17F 007F63F0 FCS_Server_GetPlayerStats_Suicides

385  180 00882870 FCS_Server_GetPlayerStats_TeamKills

386  181 007F6410 FCS_Server_GetPlayerStats_Wins

387  182 007F6430 FCS_Server_GetPlayerStats_Xp

388  183 007F63E0 FCS_Server_Message_Callback

389  184 00001CA0 GetCRCFromString

390  185 00002260 GetExePath

391  186 00004920 InitDuniaEngine

392  187 00006070 LocalizeText

393  188 00001CB0 PrintToConsole

394  189 00001CD0 RegisterGameFunctionProvider

395  18A 00001BD0 RunDuniaEngine

396  18B 00A5B510 SND_Is_SSE_Supported

397  18C 00A5B5C0 SND_fn_bTestSnd_MMX

398  18D 00A5B5A0 SND_fn_bTestSnd_Pentium

399  18E 00A5B6F0 SND_fn_bTestSnd_Win32

400  18F 00A5B600 SND_fn_bTestSnd_WinMM

401  190 00A5B750 SND_fn_bTestSnd_WinNT4

402  191 00001C00 ShutdownDuniaEngine

403  192 00002A70 SwitchContext

404  193 00001BC0 TickDuniaEngine
```