# Weather plugin hardware test

Record the companion OS and version, ESP32 board variant, firmware version,
and whether `get_info` reports `"sceneProtocol":1`.

1. Inspect `weather.aimplugin` in **Plugins**. Confirm Weather 1.1.0, author,
   `https://api.open-meteo.com`, its SHA-256 and unsigned status. Install it.
2. Place Weather in a new **Display** window and select it. Check city,
   temperature, condition, today's high and low, wind, humidity and
   Open-Meteo attribution on the actual panel. Take a photo.
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

The preview PNGs and firmware scene ACKs do not establish what the physical
display shows. Attach the photo and note any failed step before release.
