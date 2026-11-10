# Spotify-HUD-for-3.5-inch-CYD[README.md](https://github.com/user-attachments/files/34618763/README.md)
# Spotify HUD

A little touchscreen remote for Spotify on your desk, built on the Freenove 3.5" ESP32-S3 display. **Works with a free Spotify account** – no Spotify developer account or Premium needed.

- Album art, song name and artist (Japanese, Korean and other scripts work)
- Play / pause, next, previous, shuffle, repeat, like, and a progress bar you can drag
- PC volume or Spotify-only volume slider
- Your playlists with covers: open one, scroll its songs (any length), search, and tap a song to play it
- Background colour that follows the album art, beeps through the board's speaker
- A clock screen with the weather and the last song when nothing is playing
- Connects over USB, or WiFi if you add your network

It works through a small Python helper on your Windows PC. The helper reads what's playing from Windows' media controls and presses Spotify's buttons for you in the desktop app.

## What you need

- **Board:** Freenove ESP32-S3 Display 3.5" (FNK0104N, ST77922, 320×480 capacitive touch) – [Amazon](https://www.amazon.com/dp/B0H1VHJHS4)
- The speaker that comes with the board (optional, for the beeps)
- A Windows 10/11 PC with the **Spotify desktop app** and Python 3
- Optional: the 3D-printed case and stand (link to your MakerWorld page)

## 1. Set up the board (Arduino IDE)

1. Install the **esp32 by Espressif** boards package (Tools > Board > Boards Manager).
2. From Freenove's download for this board ([Freenove_ESP32_S3_Display](https://github.com/Freenove/Freenove_ESP32_S3_Display)), open `Libraries/FNK0104N` and add these with *Sketch > Include Library > Add .ZIP Library*:
   `TFT_eSPI_v2.5.43.zip`, `TFT_eSPI_Setups_v1.3.zip`, `lvgl_v8.4.0.zip`
3. Optional, for WiFi: copy `wifi_config.example.h` to `wifi_config.h` and put in your WiFi name and password. Without it, the board uses the USB cable.
4. Open `SpotifyHUD.ino` and choose these settings in the Tools menu:
   - Board: **ESP32S3 Dev Module**
   - USB CDC On Boot: **Enabled**
   - PSRAM: **OPI PSRAM**
   - Partition Scheme: **Huge APP**
5. Upload.

Screen upside down? Set `FLIP_SCREEN` to 1 in `display.h`.

## 2. Set up the PC

1. In a Command Prompt:
   ```
   pip install pyserial pillow pystray comtypes pycaw psutil winrt-runtime winrt-Windows.Foundation winrt-Windows.Media winrt-Windows.Media.Control winrt-Windows.Storage.Streams
   ```
2. Put your playlists in `playlists.txt`, one per line, as `Name: link`. To get the link: right-click the playlist in Spotify, then Share > Copy link to playlist.
3. Start the helper:
   - `start_helper.bat` runs it quietly, with a green icon by the clock.
   - `install_autostart.bat` also makes it start every time you log in.
   - `python spotify_helper.py` runs it in a window, so you can watch what it does.

Before uploading to the board again, right-click the green icon and choose **Pause**, so the USB port is free. Un-pause it afterwards.

**Weather:** change `WEATHER_LATITUDE` / `WEATHER_LONGITUDE` near the top of `spotify_helper.py` to your city (it defaults to Sydney).

## Tips

- If Windows blocks the `.bat` files after unzipping, open PowerShell in the folder and run `Get-ChildItem -Recurse | Unblock-File`.
- Big playlists: Spotify's public page only lists the first 100 songs. The helper reads the rest from the Spotify app once, when it starts. You'll see the Spotify window scroll through the playlist for a few seconds.
- If something's wrong, right-click the green icon and choose **Open log**.

## Licence

MIT – see `LICENSE`. Not affiliated with Spotify or Freenove.
