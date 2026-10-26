```text
src/
  model/                 <- zero raylib
    config.hpp           (constexpr)
    stamina.hpp/.cpp
    tilemap.hpp/.cpp     (TileType, isSolidAt, spawnPoint)
    player_input.hpp     (class/struct)
    player.hpp/.cpp
  view/                  <- raylib
    player_sprite.hpp/.cpp   (textures + animation)
    world_view.hpp/.cpp      (drawWorld, tile colors)
    hud.hpp/.cpp             (drawStaminaBar)
  controller/
    input.hpp/.cpp      (readPlayerInput)
  main.cpp              (windows, render target, loop)
tests/  third_party/doctest.h  docs/  Makefile
```
