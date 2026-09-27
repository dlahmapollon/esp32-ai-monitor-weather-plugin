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
