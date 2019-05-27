
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
UCrewFunctions::AreCharactersInSameCrew | Check if bith players are in the same crew
UGameStateFunctionLibrary::GetAthenaGameStateFromWorld(uWord) | Get the AAthenaGameState
UKismetTextLibrary::Conv_TextToString | Convert FText to FString
UVectorMaths::Distance | Calculate distance between two FVector
URotationMaths::RotatorToQuat | Convert FRotator to FQuat

## Math classes
#### UKismetMathLibrary
The class got a lot of math functions to work with algebra in UE.

**Some useful functions, see in the SDK for more:**

Function | Description
-------- | ----------------
FindLookAtRotation | Find a rotation for an object at Start location to point at Target location. (good for aimbot)
Conv_VectorToRotator | Return the FRotator orientation corresponding to the direction in which the vector points. (Conv_VectorToRotator(cameraLocation - targetLocation) rotation for an aimbot)