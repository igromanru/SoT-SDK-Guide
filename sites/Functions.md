
# Functions and their usage
* [Back to main page](README.md)
* [Useful functions](#useful-functions)
* [Math classes](#math-classes)
* Usage

## Useful functions
Function | Description
-------- | ----------------
playerController->ProjectWorldLocationToScreen | WorldToScreen function
playerController->LineOfSightTo | Visibility check
UCrewFunctions::AreCharactersInSameCrew | Check if both players are in the same crew
UGameStateFunctionLibrary::GetAthenaGameStateFromWorld(uWord) | Get the AAthenaGameState
UKismetTextLibrary::Conv_TextToString | Convert FText to FString
UVectorMaths::Distance | Calculate distance between two FVector
URotationMaths::RotatorToQuat | Convert FRotator to FQuat
AActor->GetActorForwardVector | Normalized Forward Vector for an actor  
UKismetGuidLibrary::IsValid_Guid | Checks if a FGuid is valid
UKismetGuidLibrary::EqualEqual_GuidGuid | Compares if two FGuid's are equal
UKismetGuidLibrary::Conv_GuidToString | Converts a FGuid to FString

## Math classes
#### UKismetMathLibrary
The class got a lot of math functions to work with algebra and time in UE.

**Some useful functions, look in the SDK for more:**

Function | Description
-------- | ----------------
FindLookAtRotation | Find a rotation for an object at Start location to point at Target location. (good for aimbot)
Conv_VectorToRotator | Return the FRotator orientation corresponding to the direction in which the vector points. (Conv_VectorToRotator(targetLocation - cameraLocation) rotation for an aimbot)
Dot_VectorVector | Returns the dot product of two 3d vectors
GetHours | Returns hours as int from a FTimespan
GetMinutes | Returns minutes as int from a FTimespan
GetSeconds | Returns seconds as int from a FTimespan
