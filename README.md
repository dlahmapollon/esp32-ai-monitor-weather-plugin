# AI Monitor Weather plugin

Weather for the [AI Monitor](https://github.com/tobymarks/esp32-ai-monitor)
ESP32 display. With compatible firmware, the companion app retrieves current
conditions from [Open-Meteo](https://open-meteo.com/en/docs) and sends the
weather view to the display over USB. No API key, ESP32 Wi-Fi connection or
firmware reflash is needed.

The plugin packages are declarative `.aimplugin` archives containing only
`plugin.json`. They run no third-party code on the computer or ESP32.

## Install

Choose the package that matches your companion app. Copy its **direct URL**;
the GitHub repository page URL cannot be used in the app.

| Package | Features | Companion support |
| --- | --- | --- |
| `weather.aimplugin` (v1.1.0) | Weather view in English, dark layout | Display plugins |
| `weather-localized.aimplugin` (v1.2.0) | German and English text, dark layout | Display plugins and plugin localization |
| `weather-localized-light.aimplugin` (v1.3.0) | German and English text, dark and light layouts | Display plugins, plugin localization and light scenes |
| `weather-intelligent.aimplugin` (v1.4.0) | v1.3.0 features plus intelligent weather triggers | Companion with plugin format 2 and Intelligent switching |

**v1.1.0**

```text
https://raw.githubusercontent.com/dlahmapollon/esp32-ai-monitor-weather-plugin/main/weather.aimplugin
```

**v1.2.0 — localized**

```text
https://raw.githubusercontent.com/dlahmapollon/esp32-ai-monitor-weather-plugin/main/weather-localized.aimplugin
```

**v1.3.0 — localized with light theme**

```text
https://raw.githubusercontent.com/dlahmapollon/esp32-ai-monitor-weather-plugin/main/weather-localized-light.aimplugin
```

**v1.4.0 — intelligent weather switching (test branch)**

```text
https://raw.githubusercontent.com/dlahmapollon/esp32-ai-monitor-weather-plugin/codex/intelligent-weather-triggers/weather-intelligent.aimplugin
```

The URL will use `main` after this branch is merged.

1. Open **Plugins** in the Mac or Windows companion app and paste the chosen URL.
2. Choose **Inspect**, review the package details, then choose **Install**.
3. In **Display**, add **Weather** to a window and select it. Weather also works
   with timed window switching.
4. In **Plugins**, set the city label, latitude and longitude. The city is a
   label; the coordinates determine the weather location. Use ASCII letters
   for the label, as required by scene protocol version 1.

You can also download a package from this repository and install it as a local
file. The companion and firmware must support display plugins (`sceneProtocol: 1`).

The weather view refreshes about every 15 minutes while assigned to a window.
It shows temperature, condition, today's high and low, wind, humidity and
Open-Meteo attribution. Portrait, landscape and square displays have separate
layouts. Version 1.3.0 follows the selected display theme, including the system
theme setting. Version 1.4.0 also requests an intelligent switch when current
weather first changes from dry or foggy to precipitation (Open-Meteo WMO code
51 or higher), or when ongoing precipitation first becomes a thunderstorm
(code 95 or higher). The first successful fetch only establishes a baseline;
unchanged rain or storm reports do not keep switching the display. A later
dry-to-rain transition can trigger again, subject to the companion cooldown.
The companion also enforces its minimum dwell and manual touch hold. Assign
Weather to a window and select Intelligent switching to use these triggers.
German display text uses ASCII spellings such as `bewoelkt`
because scene protocol version 1 does not support umlauts.

## Build and validate

Python's standard library builds the deterministic v1.4.0 package:

```sh
python3 scripts/package.py
python3 scripts/package.py --check
```

`fixture.json` contains sample weather data. With an AI Monitor source checkout
next to this repository that supports localization and light scenes, inspect
and render the package with the shared plugin helper:

```sh
cargo run --quiet --locked --manifest-path ../esp32-ai-monitor/companion-windows/Cargo.toml \
  -p aimonitor-plugin-host -- inspect weather-intelligent.aimplugin
cargo run --quiet --locked --manifest-path ../esp32-ai-monitor/companion-windows/Cargo.toml \
  -p aimonitor-plugin-host -- render weather-intelligent.aimplugin - all fixture.json --locale=de --theme=light
```

### Dark layout samples

<img src="previews/portrait.png" alt="Portrait weather view" width="160">
<img src="previews/landscape.png" alt="Landscape weather view" width="213">
<img src="previews/square.png" alt="Square weather view" width="240">

## Data source and trust

The companion requests weather from Open-Meteo only while the view is assigned.
Latitude and longitude are included in that HTTPS request. Network errors and
stale data appear as status screens on the display.

Open-Meteo's [free API terms](https://open-meteo.com/en/terms) restrict free use
to non-commercial projects and require attribution. The weather view displays
"Weather by Open-Meteo.com". Packages are unsigned; the companion displays a
SHA-256 checksum during inspection and checks it again at installation.
