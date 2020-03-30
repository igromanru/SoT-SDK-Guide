# Sea of Thieves SDK Guide

## Index

* General Information
  * [SDK Dump](#sdk)
  * [FindPattern Signatures](#findpattern-signatures)
  * [Distance to Meter scale](#distance-to-meter-scale)
  * [Actor Array location](#actor-array-location)
  * [Get UAthenaGameViewportClient and PostRender address](#get-uathenagameviewportclient-and-postrender-address)
  * [Get UWord and GameInstance](#get-uword-and-gameinstance)
  * [ULocalPlayer, APlayerController and AAthenaPlayerCharacter from LocalPlayer](#ulocalplayer-aplayercontroller-and-aathenaplayercharacter-from-localplayer)
  * [Fonts](#fonts)
* [Engine Drawing](sites/EngineDrawing.md)
* [Actors and their usage](sites/Actors.md)
* [Functions and their usage](sites/Functions.md)
* [Features](sites/Features.md)
* [Credits](#credits)

## SDK
[SoT SDK Dump](https://github.com/pubgsdk/SoT-SDK)  

## FindPattern Signatures
**v2.0.10**
```
GObjects:
89 0D ? ? ? ? 48 8B DF 48 89 5C 24
offset = address + 2
(UObject::GObjects)address + 6 + offset
\x89\x0D\x00\x00\x00\x00\x48\x8B\xDF\x48\x89\x5C\x24
xx????xxxxxxx

GNames
48 8B 1D ? ? ? ? 48 85 ? 75 3A
offset = address + 3
((FName::GNames))(*)address + 7 + offset
\x48\x8B\x1D\x00\x00\x00\x00\x48\x85\x00\x75\x3A
xxx????xx?xx

UWorld:
48 8B 05 ? ? ? ? 48 8B 88 ? ? ? ? 48 85 C9 74 06 48 8B 49 70
offset = address + 3
(UWorld)(*)address + 7 + offset
\x48\x8B\x05\x00\x00\x00\x00\x48\x8B\x88\x00\x00\x00\x00\x48\x85\xC9\x74\x06\x48\x8B\x49\x70
xxx????xxx????xxxxxxxxx
```

## Distance to Meter scale
The distance scale in UE4 is 100 units = 1m.  
So distance between two FVector's * 0.01f = distance in meter  
(You can also use / 100.f, but multiplication is faster than division and has no problems with 0)
```cpp
auto DistanceScale = 0.01f;
auto distanceInMeter = UVectorMaths::Distance(cameraLocation, enemyLocation) * DistanceScale;
```

## Actor Array location
Like in all other UE4 games, the actor array is in ULevel under offset **0xA0**.  
The SDK Generator can't find it, so it's always hidden in UnknownData bytes block.  
It has to be fixed manually.  
Create a second UnknownData that will be placed under the actor array, change the size of UnknownData00 and add the actor array.  
**Calculating**  
UnknownData00 new size = AActors offset - UnknownData00 offset  
UnknownData10 offset = AActors offset + AActors size  
UnknownData10 size = ActorCluster offset - UnknownData10 offset  

Before:
```cpp
unsigned char                                      UnknownData00[0xA0];                                      // 0x0028(0x00A0) MISSED OFFSET
class ULevelActorContainer*                        ActorCluster;                                             // 0x00C8(0x0008)
```
After:
```cpp
unsigned char                                      UnknownData00[0x78];                                      // 0x0028(0x0078) MISSED OFFSET
TArray<class AActor*>                              AActors;                                                  // 0x00A0(0x0010)
unsigned char                                      UnknownData10[0x18];                                      // 0x00B0(0x0018) MISSED OFFSET
class ULevelActorContainer*                        ActorCluster;                                             // 0x00C8(0x0008)
```

## Get UAthenaGameViewportClient and PostRender address
```cpp
auto AthenaGameViewportClient = UObject::FindObject<UAthenaGameViewportClient>("AthenaGameViewportClient Transient.AthenaGameEngine_1.AthenaGameViewportClient_1");

const size_t PostRenderIndex = 88;
const auto vmtPostRender  = *reinterpret_cast<uintptr_t***>(AthenaGameViewportClient) + PostRenderIndex;
```
#### PostRender hook
```cpp
typedef void(__thiscall *tPostRender)(UGameViewportClient* uObject, UCanvas* Canvas);
tPostRender OriginalPostRender;

// Function that got called at the end of rendering, perfect to draw our overlay
void HookedPostRender(UGameViewportClient* thisPointer, UCanvas* canvas)
{			
    // our code here (Overlay, ESP etc.)
    OriginalPostRender(thisPointer, canvas);
}
```

## Get UWord and GameInstance
```cpp
auto uWord = AthenaGameViewportClient->World;
auto gameInstance = AthenaGameViewportClient->GameInstance;
```

## ULocalPlayer, APlayerController and AAthenaPlayerCharacter from LocalPlayer
#### ULocalPlayer
```cpp
auto localPlayer = AthenaGameViewportClient->GameInstance->LocalPlayers[0];
```

#### APlayerController
```cpp
// AthenaGameViewportClient->GameInstance->LocalPlayers[0]->PlayerController
auto playerController = localPlayer->PlayerController;
```
#### AAthenaPlayerCharacter
```cpp
// AthenaGameViewportClient->GameInstance->LocalPlayers[0]->PlayerController->Pawn
auto localPlayerActor = (AAthenaPlayerCharacter*)playerController->Pawn;
// or
auto localPlayerActor = (AAthenaPlayerCharacter*)playerController->K2_GetPawn();
```
## Fonts
Two methods to get a font for engine drawing.
#### 1. Find a font object by name
Available fonts / UFont names
```
Font Engine.Default__Font
Font Roboto.Roboto
Font RobotoDistanceField.RobotoDistanceField
Font RobotoTiny.RobotoTiny
Font RobotoMono.RobotoMono
Font MapFont.MapFont
Font RiddleMapFont.RiddleMapFont
Font Windlass.Windlass
Font PerfCounterFont.PerfCounterFont
```
To get a font:
```cpp
auto font = UObject::FindObject<UFont>("Font Roboto.Roboto");
```
#### 2. Get a font from the UEngine
Each UE4 game got an instance of the UEngine class with default fonts set.
The [UAthenaGameEngine](#get-uathenagameviewportclient-and-postrender-address) class derives from UEngine.  
Get a default font from UAthenaGameEngine:
```cpp
auto font = AthenaGameEngine->MediumFont;
```
Check out the UEngine class in the SoT SDK dump for more fonts.

## Credits
Name | Reason
---- | ---------
igromanru | SDK Dump, this guide and most in information
gummy8unny | Open source external, Ship water level and many other contributions
xyz12 | Help with compilable SDK, public release and many other contributions
Janck7 | Bones dump, hints for some functions, his ReClass file
sotgamer91 | TableMap pins, Levels array and other contributions

### Special thanks to the OSH Community
Name | Reason
---- | ---------
KN4CK3R | SDK Generator and <span>ReClass</span>.NET
Dr_P3pp3r | Help with SDK Generator, Unreal Engine, C++ and ASM
SilverDeath | C++, ASM, Math and some UE SDK stuff
Jeon | C++ and ASM