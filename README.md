Pegasus on Linux with Yuzu, Dolphin, Citra, melonDS, PCSX2, PPSSPP and etc

<p align="center">
  <img src="https://raw.githubusercontent.com/varlesh/pegasus-instructions/master/preview.gif" alt="preview"/>
</p>

## Install emulators, Pegasus, Skyscraper, MPV, Youtube-dl, FFmpeg & AntimicroX (additional):
```
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub
flatpak install org.pegasus_frontend.Pegasus org.DolphinEmu.dolphin-emu org.citra_emu.citra org.yuzu_emu.yuzu net.kuribo64.melonDS org.ppsspp.PPSSPP net.pcsx2.PCSX2 io.github.antimicrox.antimicrox
sudo apt install git mpv youtube-dl ffmpeg build-essential qt5-default
mkdir -p /tmp/skysource && /tmp/skysource
wget -q -O - https://raw.githubusercontent.com/muldjord/skyscraper/master/update_skyscraper.sh | bash
```

## Configure game database. Create meta-file on any roms directory.

Some examples: 

**/path/to/your/roms/wii/metadata.pegasus.txt:**
```
collection: Nintendo Wii
shortname: wii
extensions: iso, wbfs
launch: flatpak run org.DolphinEmu.dolphin-emu --config "Dolphin.Display.Fullscreen=True" "{file.path}"
```

**/path/to/your/roms/3ds/metadata.pegasus.txt:**
```
collection: Nintendo 3DS
shortname: 3ds
extensions: 3ds
launch: flatpak run org.citra_emu.citra "{file.path}"
```

**/path/to/your/roms/switch/metadata.pegasus.txt:**
```
collection: Nintendo Switch
shortname: switch
extensions: nsp
launch: flatpak run org.yuzu_emu.yuzu -f -g "{file.path}"
ignore-file: updates/game.nsp
```

### Give games cache metadata:
```
Skyscraper -p wii -i "/path/to/your/roms/wii" -f pegasus -s thegamesdb --refresh
```

Convert files from cache to Pegasus DB:
```
Skyscraper -p wii -i "/path/to/your/roms/wii" -f pegasus
```

**NOTE:** Switch roms not supported

## Now you can check all your metadata's and update/fix that with popular skrapper sites:

https://gamesdb.launchbox-app.com

https://www.mobygames.com

https://www.gametdb.com

https://www.screenscraper.fr

http://www.thegamesdb.net

Example section game:
```
game: Mario Kart Wii
file: /path/to/your/roms/wii/Mario Kart Wii.iso
description: Mario and friends once again jump into the seat of their go-kart machines for the first Wii installment of this popular franchise. New features this year are an online racing mode, new motorbike vehicle types, a special balancing system for new and veteran players, and (in its initial release) a special Mario Kart wheel packaged with the game.
release: 2008-04-27
developer: Nintendo
publisher: Nintendo
genre: Action, Racing
players: 4
assets.screenshot: /path/to/your/roms/wii/media/screenshots/Mario Kart Wii.png
assets.marquee: /path/to/your/roms/wii/media/marquees/Mario Kart Wii.png
```

## Grab video previews from youtube.

Ok, now we can make small video previews for all games. I'm not found better solution for that, then youtube-dl + ffmpeg:

### Found youtube gameplay video your game and download it (with 640x480 resolution):
```
cd /path/to/your/roms/wii/media/videos
youtube-dl -f 18 -o video.mp4 youtube_link
```

where:

-f = quality

-o  = output file

### Now you can play video on MPV and mark timeline for cutting:
```
mpv video.mp4
```

### Cut the video to any duration (1 min on example):
```
ffmpeg -ss 00:02:30 -t 00:01:00 -i video.mp4 -vcodec copy -acodec copy your_game.mp4
```

where:

-ss = time position

-t = duration

-i = input file

-vcodec = video codec

-acodec = audio codec

### added video preview link to Pegasus metadata.

Open metadata.pegasus.txt and added line to link video:

```
assets.video: /path/to/your/roms/wii/media/videos/your_game.mp4
```

## Configure gamepads & emulators.

Open any emulator and configure gamepads.

Set fullscreen mode on emulators (when not support command option):

Ryujinx need change value on **~/.config/Ryujinx/Config.json** to `"start_fullscreen": true,`

Citra need change value on **~/.var/app/org.citra_emu.citra/config/citra-emu/qt-config.ini** to:
```
fullscreen=true
fullscreen\default=false
```

PCSX2 need change value on **~/.var/app/net.pcsx2.PCSX2/config/PCSX2/inis/PCSX2_ui.ini** to `DefaultToFullscreen=enabled`

PPSSPP need change value on **~/.var/app/org.ppsspp.PPSSPP/config/ppsspp/PSP/SYSTEM/ppsspp.ini** to `FullScreen = True`

melonDS not have option or command for fullscreen mode, set F11 on application settings.

## Hotkeys for emulators.

Now you can set close & fullscreen keys to free button on your gamepad.

Open AntimicroX and set L3 (Alt+F4) for close and R3 (F11) for fullscreen.

These buttons are very rarely used in games and are quite suitable for these tasks.

## Recommendations

More info you can found on [Pegasus docs](https://pegasus-frontend.org/docs/)

You can disable dialog question about exit on emulator for comphortable usage.

If you need run Ryujinx instead Yuzu for a specific game, you can added launch option to game section. For example:
```
game: My Game
file: /path/to/your/roms/switch/your_game
launch: Ryujinx "{file.path}"
...
```
