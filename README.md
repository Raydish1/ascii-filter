# ASCII Filter

ASCII Filter turns a photo into colored ASCII characters, a mosaic of lettered tiles, dithered squares, or a scatter of random letters on top of the original image. Bright areas let the photo show through, so the subject looks lit while the rest of the frame turns into text.

The whole tool is one file, `ascii-light.html`. It runs in the browser, has no build step, and never uploads your images.

## Run it

1. Clone the repo, or download `ascii-light.html`.
2. Open `ascii-light.html` in Chrome, Edge, Firefox, or Safari.

Fonts other than Courier New load from Google Fonts. Without an internet connection, those fonts fall back to your system's monospace font.

## Filter an image

1. Drop an image onto the page. You can also paste one with Ctrl+V, or click **Open image**.
2. In **Style**, choose **Characters**, **Mosaic**, **Squares**, or **Scatter**. Choosing a style applies that style's preset. The **Controls** section lists what each preset changes.
3. Adjust the sliders. The preview redraws as you drag.
4. Click **Save PNG**.

The PNG matches the preview frame. If **Shimmer** or **Drifting snow** is above 0, the frame changes over time, so save when the preview looks the way you want.

### Get the "lit statue" look

This look works best on a photo with one bright subject against darker surroundings, such as a cathedral interior, a statue in a light beam, or an overgrown ruin.

- To show more of the photo, lower **Photo shows through above**.
- To make the characters stand out, raise **Character glow** to about 1.6.
- If the text looks flat or uses only one or two characters, leave **Auto contrast** on and move **Contrast** left.
- To get crisp text on a dark background, lower **Background photo** to about 0.1.

### Get the "data sky" look

Use **Scatter** on a landscape with a bright sky. The style keeps the full photo and lays random letters over it. Near the light, letters are white and strong. Away from it, they take on the photo's color and fade out. Areas darker than **Letters start at brightness** get no letters.

- To clear letters from dark ground, raise **Letters start at brightness**.
- To spread letters further into darker sky, lower **Letters start at brightness**.
- To make letter brightness more uneven, raise **Randomness**.
- To get the binary-code feel, set **Characters** to `01`.

### Get the "glowing mosaic" look

Use **Mosaic** on an image with a few bright shapes on a dark background, such as neon, night scenes, or light painting. Every lit cell becomes a tinted tile with a character on it. The brightest tiles turn solid, with dark characters, so highlights read as blocks of light.

- To get more solid blocks, lower **Invert tiles above**.
- For a consistent color scheme across a set of images, set **Colors** to **Duotone** and pick two colors.
- To make the image feel alive, set **Shimmer** to about 1.4 and **Sparkles** to about 0.014.

## Controls

The defaults below are for **Characters** mode. Each style's preset changes these controls:

| Control | Characters | Mosaic | Squares | Scatter |
| --- | --- | --- | --- | --- |
| Cell size | 11 | 11 | 11 | 13 |
| Photo shows through above | 0.62 | 1 | 0.62 | 1 |
| Background photo | 0.35 | 0.12 | 0.35 | 1 |
| Text blend | Screen | Normal | Screen | Screen |
| Cell grid lines | Off | On | Off | Off |
| Letter spacing | 0.78 | 0.9 | | |
| Line height | 1.2 | 1.36 | | |
| Character ramp | `SS00088Xx+=-:. ` | ` .':;ot%SX8@#` | | |

### Shared controls

| Control | Default | What it does |
| --- | --- | --- |
| Style | Characters | Draws characters, lettered tiles, dithered squares, or scattered letters. |
| Cell size | 11 | Font size in Characters and Mosaic modes. Width and height of each square cell in Squares and Scatter modes. |
| Photo shows through above | 0.62 | Brightness above which the original photo replaces the effect. |
| Through softness | 0.18 | Width of the fade between the effect and the photo. |
| Background photo | 0.35 | Opacity of the photo behind the effect. |
| Character glow | 1.35 | Multiplier on character, tile, and square color. |
| Contrast | 1 | Gamma on brightness. Lower values push cells toward the bright end of the ramp. Not used by Scatter. |
| Auto contrast | On | Spreads cells evenly across the ramp by brightness rank. Not used by Scatter. |
| Text blend | Screen | How the effect combines with the photo: **Screen**, **Normal**, or **Add**. |

### Type (Characters, Mosaic, and Scatter)

| Control | Default | What it does |
| --- | --- | --- |
| Font | Courier New | Monospace font. The menu groups serif and sans fonts. |
| Weight | Light | Thin, Light, or Regular. Courier Prime, Cutive Mono, Space Mono, and Xanh Mono have one weight only. |
| Letter spacing | 0.78 | Horizontal cell size as a multiple of the font size. Characters and Mosaic only. |
| Line height | 1.2 | Vertical cell size as a multiple of the font size. Characters and Mosaic only. |
| Character ramp | `SS00088Xx+=-:. ` | Characters from darkest cell to brightest cell. A space draws nothing. Characters and Mosaic only. |
| Reverse ramp | Off | Reverses the ramp order. Characters and Mosaic only. |

### Mosaic mode

| Control | Default | What it does |
| --- | --- | --- |
| Invert tiles above | 0.86 | Brightness above which a tile turns solid and its character turns dark. Set it to the maximum to turn this off. |
| Tile strength | 1 | Multiplier on the opacity of the tinted tiles. At 0, only characters remain. |

### Squares mode

| Control | Default | What it does |
| --- | --- | --- |
| Gap between squares | 0.35 | Empty space around each square, as a share of the cell. |
| Density | 1 | Multiplier on how likely a cell is to draw a square. |
| Black and white | On | Draws squares in gray instead of the photo's colors. |

### Scatter mode

| Control | Default | What it does |
| --- | --- | --- |
| Characters (picked at random) | `01Xx#@+*` | Each cell draws one of these characters, chosen at random. |
| Letter size in cell | 0.7 | Font size as a share of the cell. Each letter sits centered in its own square cell, so lower values leave more space between letters. |
| Amount | 0.85 | Overall letter opacity. |
| Randomness | 0.75 | How much letter opacity varies from cell to cell. At 0, opacity follows brightness exactly. |
| Letters start at brightness | 0.35 | Brightness below which no letters appear. Above it, opacity and whiteness rise to full at the brightest point. |
| Lighten letters | 0.9 | How white the letters get at the brightest point. Letters near the cutoff keep the photo's color. |
| Smooth light | 4 | Blur radius, in cells, on the brightness map. Higher values make letters follow the broad light instead of local detail. |

### Color

| Control | Default | What it does |
| --- | --- | --- |
| Colors | From the photo | **From the photo** uses each cell's own color. **Duotone** ignores the photo's colors and maps brightness onto a gradient between two colors. The background photo is recolored to match. |
| Dark, Bright | `#7b5cff`, `#5ce2f0` | The two duotone colors, for the darkest and the brightest cells. Shown only when **Colors** is **Duotone**. |
| Sparkles | 0 | Share of mid-brightness cells that take an accent color (pink, yellow, green, or violet). Not used by Squares. |

### Motion

| Control | Default | What it does |
| --- | --- | --- |
| Shimmer (swaps per second) | 0 | How often each cell swaps its character, square, or sparkle. Cells swap at staggered times, so changes roll across the image. At 0, the image is still. |
| Drifting snow | 0 | Number of dim specks (`. - ' ,`) that fall slowly behind the effect. |

### Background

| Control | Default | What it does |
| --- | --- | --- |
| Cell grid lines | Off | Draws faint lines along the cell edges. |
| Vignette | 0 | How dark the edges of the image get. |
| Max output width | 1920 | Width cap for the canvas and the saved PNG, in pixels. |

## How the effect works

The filter scales the image down so that each pixel stands for one cell. That pixel's color and brightness drive the cell:

- **Characters mode** picks a character from the ramp by brightness and colors it with the cell's color. Dark cells get dense characters like `S` and `0`. Bright cells get light ones like `+` and `:`.
- **Mosaic mode** fills each lit cell with a tinted tile, 1 pixel smaller than the cell so a thin gap shows between tiles. It then draws the ramp character on the tile in a lighter tint. Tiles brighter than **Invert tiles above** turn solid with dark characters.
- **Squares mode** gives each cell a fixed random threshold and draws a square when the cell is brighter than that threshold. Bright areas fill in and dark areas thin out. This technique is called dithering.
- **Scatter mode** picks a random character for each cell. It blurs the brightness map, then measures how far each cell sits above **Letters start at brightness**. That value sets both the letter's opacity and how close its color is to white. A random amount then scales the opacity, with most letters faint and a few strong. Letter brightness looks random up close but still follows the image's light.

A second pass builds a mask from the bright cells. The filter draws the original photo through that mask on top of the effect, so the brightest parts of the image stay photographic.

### Randomness that holds still

Every random choice comes from a hash of the cell's column, row, and a time step. The time step changes only when **Shimmer** is above 0, so a still image keeps the same pattern while you adjust other sliders. With **Shimmer** on, each cell steps on its own staggered clock, and characters swap only between a level's character and the next lighter one, so areas keep their brightness while they move.

The Mosaic tiles, the two-character shimmer, the sparkles, the snow, and the grid lines are inspired by the ASCII jellyfish background in HorizonX's Kurage Dashboard template.
