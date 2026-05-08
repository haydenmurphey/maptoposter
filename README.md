# City Map Poster Generator

Generate beautiful, minimalist map posters for any city in the world.

<img src="posters/singapore_neon_cyberpunk_20260118_153328.png" width="250">
<img src="posters/dubai_midnight_blue_20260118_140807.png" width="250">

## Examples


| Country      | City           | Theme           | Poster |
|:------------:|:--------------:|:---------------:|:------:|
| USA          | San Francisco  | sunset          | <img src="posters/san_francisco_sunset_20260118_144726.png" width="250"> |
| Spain        | Barcelona      | warm_beige      | <img src="posters/barcelona_warm_beige_20260118_140048.png" width="250"> |
| Italy        | Venice         | blueprint       | <img src="posters/venice_blueprint_20260118_140505.png" width="250"> |
| Japan        | Tokyo          | japanese_ink    | <img src="posters/tokyo_japanese_ink_20260118_142446.png" width="250"> |
| India        | Mumbai         | contrast_zones  | <img src="posters/mumbai_contrast_zones_20260118_145843.png" width="250"> |
| Morocco      | Marrakech      | terracotta      | <img src="posters/marrakech_terracotta_20260118_143253.png" width="250"> |
| Singapore    | Singapore      | neon_cyberpunk  | <img src="posters/singapore_neon_cyberpunk_20260118_153328.png" width="250"> |
| Australia    | Melbourne      | forest          | <img src="posters/melbourne_forest_20260118_153446.png" width="250"> |
| UAE          | Dubai          | midnight_blue   | <img src="posters/dubai_midnight_blue_20260118_140807.png" width="250"> |
| USA          | Seattle        | emerald         | <img src="posters/seattle_emerald_20260124_162244.png" width="250"> |

## Installation

```bash
pip install -r requirements.txt
```

## Usage

```bash
python create_map_poster.py --city <city> --country <country> [options]
```

### Options

| Option | Short | Description | Default |
|--------|-------|-------------|---------|
| `--city` | `-c` | City name | required |
| `--country` | `-C` | Country name | required |
| `--country-label` | | Override country text displayed on poster | |
| `--theme` | `-t` | Theme name | `feature_based` |
| `--distance` | `-d` | Map radius in meters | `29000` |
| `--list-themes` | | List all available themes | |
| `--all-themes` | | Generate posters for all available themes | |
| `--width` | `-W` | Poster width in inches — sets physical print size | `12` |
| `--height` | `-H` | Poster height in inches — sets physical print size | `16` |
| `--dpi` | `-D` | Output DPI for PNG (use `150` for preview, `300` for print) | `300` |
| `--upscale` | `-u` | Post-render Lanczos upscale factor, e.g. `2.0` for 2× (PNG only) | `1.0` |
| `--format` | `-f` | Output format: `png`, `svg`, or `pdf` | `png` |

### Poster Size & Resolution

The script renders directly at the size you specify — no external upscaling needed. The output pixel dimensions are simply:

```
width_px  = --width  × --dpi
height_px = --height × --dpi
```

**Common print sizes at 300 DPI:**

| Print size | `-W` | `-H` | Output pixels | Use case |
|------------|------|------|---------------|----------|
| 8×10 in | `8` | `10` | 2400×3000 | Desk / small frame |
| 11×14 in | `11` | `14` | 3300×4200 | Standard small poster |
| 12×16 in | `12` | `16` | 3600×4800 | Default |
| 18×24 in | `18` | `24` | 5400×7200 | Classic poster |
| 24×30 in | `24` | `30` | 7200×9000 | Large wall poster |
| 24×36 in | `24` | `36` | 7200×10800 | Full-size gallery poster |

**Digital / screen targets at 300 DPI:**

| Target | `-W` | `-H` | Output pixels |
|--------|------|------|---------------|
| Instagram post | `3.6` | `3.6` | 1080×1080 |
| Mobile wallpaper | `3.6` | `6.4` | 1080×1920 |
| 4K wallpaper | `12.8` | `7.2` | 3840×2160 |
| A4 print | `8.3` | `11.7` | 2480×3508 |

### Using `--dpi` and `--upscale`

**`--dpi`** controls the rendering resolution directly. Lower DPI renders faster and produces smaller files — useful for iterating on city/theme combinations before committing to a full-resolution render.

**`--upscale`** applies a [Pillow LANCZOS](https://pillow.readthedocs.io/en/stable/handbook/concepts.html#filters-comparison-table) upscale pass after the render. LANCZOS is the highest-quality resampler available in Pillow and works well on map content (crisp lines, hard edges). The intended workflow is: render fast at low DPI, then upscale once to print resolution.

```bash
# Render at half resolution for speed, then upscale 2× → same final pixel count as 300 DPI
python create_map_poster.py -c "Paris" -C "France" -t pastel_dream --dpi 150 --upscale 2.0

# Render native at 300 DPI — no upscaling step, best quality
python create_map_poster.py -c "Paris" -C "France" -t pastel_dream --dpi 300
```

Both approaches produce similar output sizes. The native render at 300 DPI is higher fidelity; the 150 DPI + 2× upscale is faster for large poster dimensions.

### Examples

```bash
# Iconic grid patterns
python create_map_poster.py -c "New York" -C "USA" -t noir -d 12000           # Manhattan grid
python create_map_poster.py -c "Barcelona" -C "Spain" -t warm_beige -d 8000   # Eixample district

# Waterfront & canals
python create_map_poster.py -c "Venice" -C "Italy" -t blueprint -d 4000       # Canal network
python create_map_poster.py -c "Amsterdam" -C "Netherlands" -t ocean -d 6000  # Concentric canals
python create_map_poster.py -c "Dubai" -C "UAE" -t midnight_blue -d 15000     # Palm & coastline

# Radial patterns
python create_map_poster.py -c "Paris" -C "France" -t pastel_dream -d 10000   # Haussmann boulevards
python create_map_poster.py -c "Moscow" -C "Russia" -t noir -d 12000          # Ring roads

# Organic old cities
python create_map_poster.py -c "Tokyo" -C "Japan" -t japanese_ink -d 15000    # Dense organic streets
python create_map_poster.py -c "Marrakech" -C "Morocco" -t terracotta -d 5000 # Medina maze
python create_map_poster.py -c "Rome" -C "Italy" -t warm_beige -d 8000        # Ancient layout

# Coastal cities
python create_map_poster.py -c "San Francisco" -C "USA" -t sunset -d 10000    # Peninsula grid
python create_map_poster.py -c "Sydney" -C "Australia" -t ocean -d 12000      # Harbor city
python create_map_poster.py -c "Mumbai" -C "India" -t contrast_zones -d 18000 # Coastal peninsula

# River cities
python create_map_poster.py -c "London" -C "UK" -t noir -d 15000              # Thames curves
python create_map_poster.py -c "Budapest" -C "Hungary" -t copper_patina -d 8000  # Danube split

# List available themes
python create_map_poster.py --list-themes

# Generate posters for every theme
python create_map_poster.py -c "Tokyo" -C "Japan" --all-themes

# Print-ready poster sizes (no external upscaling needed)
python create_map_poster.py -c "Paris" -C "France" -t pastel_dream -d 10000 -W 18 -H 24      # 18×24 in @ 300 DPI
python create_map_poster.py -c "Tokyo" -C "Japan" -t japanese_ink -d 15000 -W 24 -H 36       # 24×36 in @ 300 DPI

# Quick preview at low DPI, then 2× Lanczos upscale via Pillow
python create_map_poster.py -c "New York" -C "USA" -t noir -d 12000 --dpi 150 --upscale 2.0
```

### Distance Guide

| Distance | Best for |
|----------|----------|
| 4000-6000m | Small/dense cities (Venice, Amsterdam center) |
| 8000-12000m | Medium cities, focused downtown (Paris, Barcelona) |
| 15000-20000m | Large metros, full city view (Tokyo, Mumbai) |

## Themes

17 themes available in `themes/` directory:

| Theme | Style |
|-------|-------|
| `feature_based` | Classic black & white with road hierarchy |
| `gradient_roads` | Smooth gradient shading |
| `contrast_zones` | High contrast urban density |
| `noir` | Pure black background, white roads |
| `midnight_blue` | Navy background with gold roads |
| `blueprint` | Architectural blueprint aesthetic |
| `neon_cyberpunk` | Dark with electric pink/cyan |
| `warm_beige` | Vintage sepia tones |
| `pastel_dream` | Soft muted pastels |
| `japanese_ink` | Minimalist ink wash style |
| `emerald`      | Lush dark green aesthetic |
| `forest` | Deep greens and sage |
| `ocean` | Blues and teals for coastal cities |
| `terracotta` | Mediterranean warmth |
| `sunset` | Warm oranges and pinks |
| `autumn` | Seasonal burnt oranges and reds |
| `copper_patina` | Oxidized copper aesthetic |
| `monochrome_blue` | Single blue color family |

## Output

Posters are saved to `posters/` directory with format:
```
{city}_{theme}_{YYYYMMDD_HHMMSS}.png
```

## Adding Custom Themes

Create a JSON file in `themes/` directory:

```json
{
  "name": "My Theme",
  "description": "Description of the theme",
  "bg": "#FFFFFF",
  "text": "#000000",
  "gradient_color": "#FFFFFF",
  "water": "#C0C0C0",
  "parks": "#F0F0F0",
  "road_motorway": "#0A0A0A",
  "road_primary": "#1A1A1A",
  "road_secondary": "#2A2A2A",
  "road_tertiary": "#3A3A3A",
  "road_residential": "#4A4A4A",
  "road_default": "#3A3A3A"
}
```

## Project Structure

```
map_poster/
├── create_map_poster.py          # Main script
├── themes/               # Theme JSON files
├── fonts/                # Roboto font files
├── posters/              # Generated posters
└── README.md
```

## Hacker's Guide

Quick reference for contributors who want to extend or modify the script.

### Architecture Overview

```
┌─────────────────┐     ┌──────────────┐     ┌─────────────────┐
│   CLI Parser    │────▶│  Geocoding   │────▶│  Data Fetching  │
│   (argparse)    │     │  (Nominatim) │     │    (OSMnx)      │
└─────────────────┘     └──────────────┘     └─────────────────┘
                                                     │
                        ┌──────────────┐             ▼
                        │    Output    │◀────┌─────────────────┐
                        │  (matplotlib)│     │   Rendering     │
                        └──────────────┘     │  (matplotlib)   │
                                             └─────────────────┘
```

### Key Functions

| Function | Purpose | Modify when... |
|----------|---------|----------------|
| `get_coordinates()` | City → lat/lon via Nominatim | Switching geocoding provider |
| `create_poster()` | Main rendering pipeline | Adding new map layers |
| `get_edge_colors_by_type()` | Road color by OSM highway tag | Changing road styling |
| `get_edge_widths_by_type()` | Road width by importance | Adjusting line weights |
| `create_gradient_fade()` | Top/bottom fade effect | Modifying gradient overlay |
| `load_theme()` | JSON theme → dict | Adding new theme properties |

**`create_poster()` signature:**
```python
create_poster(city, country, point, dist, output_file, output_format,
              width=12, height=16, dpi=300, upscale=1.0,
              country_label=None, name_label=None)
```
- `width` / `height` — matplotlib figure size in inches; directly sets output pixel count at the given DPI
- `dpi` — passed to `plt.savefig()` for PNG; controls rendering resolution
- `upscale` — if `!= 1.0`, opens the saved PNG with Pillow and resizes using `Image.LANCZOS` before overwriting the file

### Rendering Layers (z-order)

```
z=11  Text labels (city, country, coords)
z=10  Gradient fades (top & bottom)
z=3   Roads (via ox.plot_graph)
z=2   Parks (green polygons)
z=1   Water (blue polygons)
z=0   Background color
```

### OSM Highway Types → Road Hierarchy

```python
# In get_edge_colors_by_type() and get_edge_widths_by_type()
motorway, motorway_link     → Thickest (1.2), darkest
trunk, primary              → Thick (1.0)
secondary                   → Medium (0.8)
tertiary                    → Thin (0.6)
residential, living_street  → Thinnest (0.4), lightest
```

### Adding New Features

**New map layer (e.g., railways):**
```python
# In create_poster(), after parks fetch:
try:
    railways = ox.features_from_point(point, tags={'railway': 'rail'}, dist=dist)
except:
    railways = None

# Then plot before roads:
if railways is not None and not railways.empty:
    railways.plot(ax=ax, color=THEME['railway'], linewidth=0.5, zorder=2.5)
```

**New theme property:**
1. Add to theme JSON: `"railway": "#FF0000"`
2. Use in code: `THEME['railway']`
3. Add fallback in `load_theme()` default dict

### Typography Positioning

All text uses `transform=ax.transAxes` (0-1 normalized coordinates):
```
y=0.14  City name (spaced letters)
y=0.125 Decorative line
y=0.10  Country name
y=0.07  Coordinates
y=0.02  Attribution (bottom-right)
```

### Useful OSMnx Patterns

```python
# Get all buildings
buildings = ox.features_from_point(point, tags={'building': True}, dist=dist)

# Get specific amenities
cafes = ox.features_from_point(point, tags={'amenity': 'cafe'}, dist=dist)

# Different network types
G = ox.graph_from_point(point, dist=dist, network_type='drive')  # roads only
G = ox.graph_from_point(point, dist=dist, network_type='bike')   # bike paths
G = ox.graph_from_point(point, dist=dist, network_type='walk')   # pedestrian
```

### Performance Tips

- Large `dist` values (>20km) = slow downloads + memory heavy
- Cache coordinates locally to avoid Nominatim rate limits
- Use `network_type='drive'` instead of `'all'` for faster renders
- Use `--dpi 150` for quick previews; `--dpi 300` (default) for print-ready output
- For large poster dimensions (24×36+), `--dpi 150 --upscale 2.0` renders faster than native 300 DPI with comparable quality
