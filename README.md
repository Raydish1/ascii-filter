# ASCII Filter

ASCII Filter turns a photo into colored ASCII characters or dithered squares that sit on top of the original image. Bright areas let the photo show through, so the subject looks lit while the rest of the frame turns into text.

The whole tool is one file, `ascii-light.html`. It runs in the browser, has no build step, and never uploads your images.

## Run it

1. Clone the repo, or download `ascii-light.html`.
2. Open `ascii-light.html` in Chrome, Edge, Firefox, or Safari.

Fonts other than Courier New load from Google Fonts. Without an internet connection, those fonts fall back to your system's monospace font.

## Filter an image

1. Drop an image onto the page. You can also paste one with Ctrl+V, or click **Open image**.
2. In **Style**, choose **Characters** or **Squares**.
3. Adjust the sliders. The preview redraws as you drag.
4. Click **Save PNG**.

The PNG matches the preview frame. If **Flicker** is above 0, set it to 0 first so you get the frame you expect.

### Get the "lit statue" look

This look works best on a photo with one bright subject against darker surroundings, such as a cathedral interior, a statue in a light beam, or an overgrown ruin.

- To show more of the photo, lower **Photo shows through above**.
- To make the characters stand out, raise **Character glow** to about 1.6.
- If the text looks flat or uses only one or two characters, leave **Auto contrast** on and move **Contrast** left.
- To get crisp text on a dark background, lower **Background photo** to about 0.1.

## Controls

### Shared controls

| Control | Default | What it does |
| --- | --- | --- |
| Style | Characters | Draws characters or dithered squares. |
| Cell size | 11 | Font size in Characters mode. Square size in Squares mode. |
| Photo shows through above | 0.62 | Brightness above which the original photo replaces the effect. |
| Through softness | 0.18 | Width of the fade between the effect and the photo. |
| Background photo | 0.35 | Opacity of the photo behind the effect. |
| Character glow | 1.35 | Multiplier on character and square color. |
| Contrast | 1 | Gamma on brightness. Lower values push cells toward the bright end of the ramp. |
| Auto contrast | On | Spreads cells evenly across the ramp by brightness rank. |
| Text blend | Screen | How the effect combines with the photo: **Screen**, **Normal**, or **Add**. |
| Flicker | 0.04 | Share of cells that change each frame, about 11 times a second. |
| Max output width | 1920 | Width cap for the canvas and the saved PNG, in pixels. |

### Characters mode

| Control | Default | What it does |
| --- | --- | --- |
| Font | Courier New | Monospace font. The menu groups serif and sans fonts. |
| Weight | Light | Thin, Light, or Regular. Courier Prime, Cutive Mono, Space Mono, and Xanh Mono have one weight only. |
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

## How the effect works

The filter scales the image down so that each pixel stands for one cell. That pixel's color and brightness drive the cell:

- **Characters mode** picks a character from the ramp by brightness and colors it with the cell's color. Dark cells get dense characters like `S` and `0`. Bright cells get light ones like `+` and `:`.
- **Squares mode** gives each cell a fixed random threshold and draws a square when the cell is brighter than that threshold. Bright areas fill in and dark areas thin out. This technique is called dithering.

A second pass builds a mask from the bright cells. The filter draws the original photo through that mask on top of the effect, so the brightest parts of the image stay photographic.
