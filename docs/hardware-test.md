# Hardware test guide

To check a package on an ESP32, note the companion OS and version, board
variant and firmware version. The firmware should report `"sceneProtocol":1`
in `get_info`.

1. Inspect the chosen `.aimplugin` file in **Plugins**. Check its version,
   author, `https://api.open-meteo.com` data origin and SHA-256, then install it.
2. Place Weather in a new **Display** window and select it. Check city,
   temperature, condition, today's high and low, wind, humidity and
   Open-Meteo attribution on the display.
3. On a CYD, rotate through portrait and both landscape modes. On an S3,
   check the square layout. Ensure text is readable and does not overlap.
   Switch to another window with touch, then use automatic switching. The AI
   and clock windows must retain their content.
4. Change latitude and longitude to another location and save. The next
   weather frame should change. Disconnect the internet and save again to
   trigger a refresh; Weather should show a data error. Reconnect and save
   once more to verify recovery.
5. Restart the companion and power cycle the display. Confirm the plugin
   settings, window placement and switching mode survive. Remove Weather and
   confirm its assigned windows become clocks.


## Intelligent switching (v1.4.0)

Install `weather-intelligent.aimplugin` in a companion that supports plugin
format 2. Assign it to a window and choose Intelligent switching. A first
successful weather fetch establishes the baseline. When the reported WMO code
changes from 0-48 to 51 or higher, Weather should be selected after the
minimum dwell. While precipitation continues, subsequent updates should not
switch again. A change from precipitation to code 95 or higher should request
a new switch, subject to the companion's cooldown and manual touch hold.
For a deterministic Windows hardware test, set `AIMONITOR_PLUGIN_FIXTURE_DIR`
to a local directory before starting the companion and put a copy of `fixture.json`
there named `org.aimonitor.weather.json`. The companion reads that file on each
plugin fetch. Change `current.weather_code` from 2 to 61, then to 95, waiting
for a fetch after each edit (up to 15 minutes with the release package). Do
not save plugin settings between samples: that resets the trigger baseline.
The Mac companion fetches the live Open-Meteo endpoint; this fixture override
is currently Windows-only. Without a fixture, wait for actual weather changes.
