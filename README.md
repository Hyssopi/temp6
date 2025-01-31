# Fire Emblem Tile Map Editor
Tile map editor for Fire Emblem GBA games.
- Fire Emblem 6: The Binding Blade
- Fire Emblem 7: The Blazing Blade
- Fire Emblem 8: The Sacred Stones

![splash](help/images/main.png)

Allows users to create, modify, output and load Fire Emblem GBA maps. Features an auto-generate feature, but recommended that users manually fix it up after auto-generation.

Tile data is extracted from chapter maps of the Fire Emblem GBA games. Each tile's neighboring tiles are remembered when extracted from the chapter maps. This allows the user to know which tile can fit with another tile, as well as allow the auto-generate feature.

## Working Demo
https://www.youtube.com/watch?v=N78gV8VUhXg

## Try It Out
https://hyssopi.github.io/Fire-Emblem-Tile-Map-Editor/

## Prerequisites
(None)

## Build
(None)

## Run
Open `index.html` in a web browser.

## Test
(None)

## Miscellaneous Details
Getting map images from the game to the Fire Emblem Tile Map Editor.

#### Emulator
- Switch to only show `Background 3` in the emulator. This is the background layer that has the map image.
- Take screen captures of Background 3, keeping in mind that some tiles are animated and therefore has to be timed right (pause the game) when taking a screen capture.
- Some backgrounds have some overlay over it and cannot have a screen capture taken. For example: Fire Emblem 6 Chapter 08x. Has blinking red on Background 3 to simulate the fire effects.
- Some levels are fog of war which is still shown on the tiles of the maps in Background 3 layer. Therefore recommended to use Thief units with Torch item with Cheats (unit turn is always available to move).
- Some levels have parallax backgrounds. For example: Fire Emblem 6 Chapter 16x, Fire Emblem 6 Chapter 24, and Fire Emblem 6 Final Chapter.

#### GIMP
- Stitch screen captures together to form the complete map.
- Double check that maps with animated tiles are properly stitched.

#### References/Images (24-Bit Color Depth)
- Emulators such as mGBA use 24-bit color depth instead of the original GBA 15-bit color depth.
- Place completed stitched maps to the `References\Images (24-Bit Color Depth)` folder.

#### Map Extractor
- Inputs:
  - References\Images (24-Bit Color Depth)
  - tile\images
- Outputs:
  - References\Images (15-Bit Color Depth)
  - References\Fire Emblem Map JSON Files
  - tiles\tileReferences.json
  - tile\images
  - tileHashesSortedByColor.txt
  - tileHashesByMapScript.txt

- Map Extractor Runner
1. Delete `References\Images (15-Bit Color Depth)`
2. Convert 24-bit color depth map images from `References\Images (24-Bit Color Depth)` to 15-bit color depth map images and output/save the converted map images to `References\Images (15-Bit Color Depth)`
3. Extract `TileData` data from the map images in `References\Images (15-Bit Color Depth)` and any existing tile images in `tiles\images`, then store it in `SortedDictionary<string, TileData> allUniqueTileData`
4. Output/save the tile images to `tiles\images`
5. Use `allUniqueTileData` to output the `tileReferences.json` in `tiles`
6. Delete `References\Fire Emblem Map JSON Files`
7. Generate Map JSON files of `References\Images (15-Bit Color Depth)` into `References\Fire Emblem Map JSON Files`
8. Output `tileHashesSortedByColor.txt` in the base directory (Desktop)
9. Output `tileHashesByMapScript.txt` in the base directory (Desktop)
10. Print debug information to the console
11. Run various checks and output the results, should be all `True`

- Map Extractor Runner console output provides information
  - Cursor Tile Hash
  - Empty Tile Hash
  - List of groups

- Update `ALL_TILE_HASHES` in `tileSortHelperStyle1.html` with the output `tileHashesSortedByColor.txt`.
- Update `TILE_GROUP_INPUT_OPTIONS` in both `tileSortHelperStyle1.html` and `tileSortHelperStyle2.html` with the list of groups from the Map Extractor Runner console output.
- The output `tileHashesByMapScript.txt` is not used.

#### Tile Sort Helper
- Style1
  - Displays the tiles based on `group`. Clicking tiles on the right will move it to the left. Tiles on the left are used to generate a batch script to move the tile images to the new specified group.
  - For example, choosing `UNDEFINED` as the input will display all the UNDEFINED tile images in the `tiles\images\UNDEFINED` folder. Clicking on tiles on the right will move it to the left. Then choosing `FLOOR` as the output group will update the batch move script. Copy the batch script into a command prompt to move the tile images from `UNDEFINED` to `FLOOR`.
  - Not recommended to use this style. Better to use Style2.
- Style2
  - Load map JSON file (for example, `References\Fire Emblem Map JSON Files\Fire Emblem 6\Chapters\07\001.png.json`). Display the tiles of the map on top. Clicking on tiles on the top will move it to the bottom. Tiles on the bottom and the output radio button group are used to generate a batch script to move the tile images to the new specified group. Any changes should automatically copy the batch move script output to clipboard. Execute it by pasting into the command prompt to move the tile images.
  - Any tiles that are not in the UNDEFINED folder (meaning they have already been sorted), will be replaced with an EMPTY tile.
  - Use the Emulator with the game and chapter running as a reference when sorting.

#### Map Extractor
- Execute Map Extractor Runner again to update any sorting changes from the `Tile Sort Helper`.

#### Fire Emblem Tile Map Editor
- Uses `tiles\images` for tile images, `tiles\tileReferences.json` for tile data
- Uses `help` to open up the help page
- Chapter maps are available to load in `References\Fire Emblem Map JSON Files`
