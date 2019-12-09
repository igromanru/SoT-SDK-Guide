
# Engine Drawing
* [Back to main page](README.md)
* [Draw with UCanvas](#draw-with-ucanvas)
  * [Draw a text]()
  * [Draw a line]()
  * [Draw a box]()
* [Use AHUD to draw](#use-ahud-to-draw)
  * [Get AHUD]()
  * [Draw a rectangle]()

## Draw with UCanvas
If we hook the PostRender function from UGameViewportClient, we got a UCanvas pointer that comes as parameter.
#### Draw a text
UCanvas*->K2_DrawText
```cpp
// Get somewhere at the beginning the font once
auto defaultFont = UObject::FindObject<UFont>("Font Roboto.Roboto");
// ...
auto myText = std::wstring(L"Hallo World");
canvas->K2_DrawText(defaultFont, FString(myText.c_str()), FVector2D(100, 100), White, 1.f, Black, FVector2D(1.f, 1.f), true, true, true, Black);
```

#### Draw a line
UCanvas*->K2_DrawLine
```cpp
FLinearColor white(1.f,1.f,1.f,1.f);
canvas->K2_DrawLine(FVector2D(10, 10), FVector2D(100, 100), 1.f, white);
```
#### Draw a box
UCanvas*->K2_DrawBox can draw only white boxes, so we have make an own function. 

```cpp
void DrawBox(UCanvas* canvas, const FVector2D& min, const FVector2D& max, const FLinearColor& color, float thickness)
{
    canvas->K2_DrawLine(min, FVector2D(max.X, min.Y), thickness, color); // top border
    canvas->K2_DrawLine(min, FVector2D(min.X, max.Y), thickness, color); // left border
    canvas->K2_DrawLine(FVector2D(min.X, max.Y), max, thickness, color); // bottom border
    canvas->K2_DrawLine(FVector2D(max.X, min.Y), max, thickness, color); // right border
}
// usage
FLinearColor red(1.f, 0, 0, 1.f);
DrawBox(canvas, FVector2D(10, 10), FVector2D(100, 100), red, 1.f);
```

## Use AHUD to draw
Assign the current UCanvas from the PostRender function, then you can draw with AHUD.
#### Get AHUD
```cpp
PlayerController->MyHUD
```
#### Draw a rectangle
```cpp
PlayerController->MyHUD->Canvas = canvas;
PlayerController->MyHUD->DrawRect(color, x, y, width, heigth);
```
