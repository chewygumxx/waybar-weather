<!-- vim:set expandtab shiftwidth=2 filetype=markdown: -->
<!-- SPDX-License-Identifier: GPL-3.0-only -->

<!--
   - 
   - ~chewygumxx/waybar-weather.git
   - ::: :/README.md
   - 
   -->

# waybar-weather

[Waybar Module] Returns current weather stats

A Bash script that queries the [Visual Crossing Weather API] for the current
conditions of a city and prints the JSON object [Waybar]'s ["Custom" module]
protocol expects, with the condition icon coloured inline via Pango markup.

[Visual Crossing Weather API]: <https://www.visualcrossing.com/weather-api>
[Waybar]: <https://github.com/Alexays/Waybar>
["Custom" module]: <https://github.com/Alexays/Waybar/wiki/Module:-Custom>

## Requirements

- `bash`
- `curl`
- `jq`
- A [Visual Crossing] API key

[Visual Crossing]: <https://www.visualcrossing.com/sign-up>

## Configuration

The script reads these environment variables:

| Variable               | Default       | Description                                                  |
|------------------------|---------------|--------------------------------------------------------------|
| `LOCATION`             | `Melbourne`   | City to query                                                |
| `UNITS`                | `metric`      | Visual Crossing `unitGroup`: `us`, `metric`, `uk`, or `base` |
| `ICON_SIZE`            | `100%`        | Pango markup [`font_size`] applied to icon                   |
| `VISUALCROSSING_APIKEY`| unset         | API key                                                      |

[`font_size`]: <https://docs.gtk.org/Pango/pango_markup.html#:~:text=font%5Fsize>

If `VISUALCROSSING_APIKEY` is unset, the key is read from a file named
`visualcrossing.apikey` next to the script.

## Usage

```sh
LOCATION=Melbourne ./waybar-weather
```

Wire it into Waybar's config as a custom module:

```jsonc
"custom/weather": {
    "format": "{}",
    "interval": 900,
    "return-type": "json",
    "exec-if": "ls \"${XDG_CONFIG_HOME:-$HOME/.config}/waybar/modules/waybar-weather/visualcrossing.apikey\"",
    "exec": "LOCATION=Melbourne UNITS=metric ICON_SIZE=130% \"${XDG_CONFIG_HOME:-$HOME/.config}/waybar/modules/waybar-weather/waybar-weather\""
}
```

`"return-type": "json"` is required, the module emits `{"text", "tooltip",
"class"}`, not a plain line of text. Leave `"escape"` unset (or `false`) so
the inline Pango markup in `text` renders instead of printing literally.

## License

GPL-3.0-only, see [LICENSE](./LICENSE).
