
# Engine Drawing

## Draw with UCanvas

## Use AHUD to draw
Assign the current UCanvas from the PostRender function, then you can draw with AHUD.
#### Get AHUD
```cpp
PlayerControlle->MyHUD
```
#### Draw
```cpp
PlayerControlle->MyHUD->Canvas = canvas;
PlayerControlle->MyHUD->DrawRect(color, x, y, width, heigth);
```