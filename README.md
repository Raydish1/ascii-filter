# ASCII Filter

ASCII Filter turns a photo into colored ASCII characters, dithered squares, or a scatter of random letters that sit on top of the original image. Bright areas let the photo show through, so the subject looks lit while the rest of the frame turns into text.

The whole tool is one file, `ascii-light.html`. It runs in the browser, has no build step, and never uploads your images.

## Run it

1. Clone the repo, or download `ascii-light.html`.
2. Open `ascii-light.html` in Chrome, Edge, Firefox, or Safari.

Fonts other than Courier New load from Google Fonts. Without an internet connection, those fonts fall back to your system's monospace font.

## Filter an image

1. Drop an image onto the page. You can also paste one with Ctrl+V, or click **Open image**.
2. In **Style**, choose **Characters**, **Squares**, or **Scatter**. Choosing a style resets **Cell size**, **Photo shows through above**, and **Background photo** to that style's defaults.
3. Adjust the sliders. The preview redraws as you drag.
4. Click **Save PNG**.

The PNG matches the preview frame. If **Flicker** is above 0, set it to 0 first so you get the frame you expect.

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

## Controls

The defaults below are for **Characters** mode. **Scatter** sets **Cell size** to 13, **Photo shows through above** to 1, and **Background photo** to 1.

### Shared controls

| Control | Default | What it does |
| --- | --- | --- |
| Style | Characters | Draws characters, dithered squares, or scattered letters. |
| Cell size | 11 | Font size in Characters mode. Width and height of each square cell in Squares and Scatter modes. |
| Photo shows through above | 0.62 | Brightness above which the original photo replaces the effect. |
| Through softness | 0.18 | Width of the fade between the effect and the photo. |
| Background photo | 0.35 | Opacity of the photo behind the effect. |
| Character glow | 1.35 | Multiplier on character and square color. |
| Contrast | 1 | Gamma on brightness. Lower values push cells toward the bright end of the ramp. Not used by Scatter. |
| Auto contrast | On | Spreads cells evenly across the ramp by brightness rank. Not used by Scatter. |
| Text blend | Screen | How the effect combines with the photo: **Screen**, **Normal**, or **Add**. |
| Flicker | 0.04 | Share of cells that change each frame, about 11 times a second. |
| Max output width | 1920 | Width cap for the canvas and the saved PNG, in pixels. |

### Text controls (Characters and Scatter)

| Control | Default | What it does |
| --- | --- | --- |
| Font | Courier New | Monospace font. The menu groups serif and sans fonts. |
| Weight | Light | Thin, Light, or Regular. Courier Prime, Cutive Mono, Space Mono, and Xanh Mono have one weight only. |

### Characters mode

| Control | Default | What it does |
| --- | --- | --- |
| Letter spacing | 0.78 | Horizontal cell size as a multiple of the font size. |
| Line height | 1.2 | Vertical cell size as a multiple of the font size. |
| Character ramp | `SS00088Xx+=-:. ` | Characters from darkest cell to brightest cell. A space draws nothing. |
| Reverse ramp | Off | Reverses the ramp order. |

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

## How the effect works

The filter scales the image down so that each pixel stands for one cell. That pixel's color and brightness drive the cell:

- **Characters mode** picks a character from the ramp by brightness and colors it with the cell's color. Dark cells get dense characters like `S` and `0`. Bright cells get light ones like `+` and `:`.
- **Squares mode** gives each cell a fixed random threshold and draws a square when the cell is brighter than that threshold. Bright areas fill in and dark areas thin out. This technique is called dithering.
- **Scatter mode** picks a random character for each cell. It blurs the brightness map, then measures how far each cell sits above **Letters start at brightness**. That value sets both the letter's opacity and how close its color is to white. A random amount then scales the opacity, with most letters faint and a few strong. Letter brightness looks random up close but still follows the image's light.

A second pass builds a mask from the bright cells. The filter draws the original photo through that mask on top of the effect, so the brightest parts of the image stay photographic.
