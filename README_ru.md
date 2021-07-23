Pegasus на Linux с Yuzu, Dolphin, Citra, melonDS, PCSX2, PPSSPP и т.д.

<p align="center">
  <img src="https://raw.githubusercontent.com/varlesh/pegasus-instructions/master/preview.gif" alt="preview"/>
</p>

## Установите эмуляторы, Pegasus, Skyscraper, MPV, Youtube-dl, FFmpeg и AntimicroX (дополнительно):
```
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub
flatpak install org.pegasus_frontend.Pegasus org.DolphinEmu.dolphin-emu org.citra_emu.citra org.yuzu_emu.yuzu net.kuribo64.melonDS org.ppsspp.PPSSPP net.pcsx2.PCSX2 io.github.antimicrox.antimicrox
sudo apt install git mpv youtube-dl ffmpeg build-essential qt5-default
mkdir -p /tmp/skysource && /tmp/skysource
wget -q -O - https://raw.githubusercontent.com/muldjord/skyscraper/master/update_skyscraper.sh | bash
```

## Настройте базу данных игр. Создайте мета-файл в каждой папке с ромами.

Некоторые примеры: 

**/путь/к/вашим/ромам/wii/metadata.pegasus.txt:**
```
collection: Nintendo Wii
shortname: wii
extensions: iso, wbfs
launch: flatpak run org.DolphinEmu.dolphin-emu --config "Dolphin.Display.Fullscreen=True" "{file.path}"
```

**/путь/к/вашим/ромам/3ds/metadata.pegasus.txt:**
```
collection: Nintendo 3DS
shortname: 3ds
extensions: 3ds
launch: flatpak run org.citra_emu.citra "{file.path}"
```

**/путь/к/вашим/ромам/switch/metadata.pegasus.txt:**
```
collection: Nintendo Switch
shortname: switch
extensions: nsp
launch: flatpak run org.yuzu_emu.yuzu -f -g "{file.path}"
ignore-file: updates/game.nsp
```

### Получите кеш метаданных:
```
Skyscraper -p wii -i "/путь/к/вашим/ромам/wii" -f pegasus -s thegamesdb --refresh
```

Сконвертируйте кеш в метаданные Pegasus:
```
Skyscraper -p wii -i "/путь/к/вашим/ромам/wii" -f pegasus
```

**ПРИМЕЧАНИЕ:** Switch ромы не поддерживаются

## Теперь вы можете проверить все метаданные и при необходимость обновить/исправить, используя популярные скрапер-сайты:

https://gamesdb.launchbox-app.com

https://www.mobygames.com

https://www.gametdb.com

https://www.screenscraper.fr

http://www.thegamesdb.net

Пример секции игры:
```
game: Mario Kart Wii
file: /путь/к/вашим/ромам/wii/Mario Kart Wii.iso
description: Марио и его друзья снова запрыгивают в свои картинговые авто для первого выпуска этой популярной франшизы на Wii. Среди нововведений этого года - режим онлайн-гонок, новые типы мотоциклов, специальная система балансировки для начинающих и опытных игроков и (в первоначальном выпуске) специальное колесо Mario Kart, входящее в комплект игры.
release: 2008-04-27
developer: Nintendo
publisher: Nintendo
genre: Action, Racing
players: 4
assets.screenshot: /путь/к/вашим/ромам/wii/media/screenshots/Mario Kart Wii.png
assets.marquee: /путь/к/вашим/ромам/wii/media/marquees/Mario Kart Wii.png
```

## Скачиваем видео с Youtube

Отлично, теперь мы можем создать небольшое видео превью для всех игр. Я не нашел ничего лучше, чем youtube-dl + ffmpeg:

### Найдите геймплей видео в Youtube и загрузите его (с разрешением 640х480):
```
cd /путь/к/вашим/ромам/wii/media/videos
youtube-dl -f 18 -o video.mp4 youtube_link
```

где:

-f = качество

-o  = выходной файл

### Теперь вы можете воспроизвести видео в MPV для определения метки обрезки:
```
mpv video.mp4
```

### Обрезаем видео любой длительности (1 мин в примере):
```
ffmpeg -ss 00:02:30 -t 00:01:00 -i video.mp4 -vcodec copy -acodec copy your_game.mp4
```

где:

-ss = позиция

-t = продолжительность

-i = выходной файл

-vcodec = видео кодек

-acodec = аудио кодек

### Добавляем ссылку на превью видео в метаданные Pegasus.

Открываем metadata.pegasus.txt и добавляем строчку в секцию нужной игры:

```
assets.video: /путь/к/вашим/ромам/wii/media/videos/your_game.mp4
```

## Настроить геймпады и эмуляторы.

Откройте каждый эмулятор и настройте их.

Установите полноэкранный режим в эмуляторах (если они не поддерживают такую опцию в терминале):

В Ryujinx нужно изменить в **~/.config/Ryujinx/Config.json** на `"start_fullscreen": true,`

В Citra нужно изменить в **~/.var/app/org.citra_emu.citra/config/citra-emu/qt-config.ini** на:
```
fullscreen=true
fullscreen\default=false
```

В PCSX2 нужно изменить в **~/.var/app/net.pcsx2.PCSX2/config/PCSX2/inis/PCSX2_ui.ini** на `DefaultToFullscreen=enabled`

В PPSSPP нужно изменить в **~/.var/app/org.ppsspp.PPSSPP/config/ppsspp/PSP/SYSTEM/ppsspp.ini** на `FullScreen = True`

melonDS не имеет опции или команды для полноэкранного режима, установите F11 в настройках программы.

## Горячие клавиши для эмуляторов.

Теперь вы можете установить комбинации клавиш закрытия и полноэкранного режима на свободные кнопки вашего геймпада.

Откройте AntimicroX и установите на L3 (Alt+F4) для закрытия и R3 (F11) для полного экрана.

Эти кнопки очень редко используются в играх и вполне подходят под эти задачи.

## Рекомендации

Более подробную информацию вы можете найти в [мануалах Pegasus](https://pegasus-frontend.org/docs/)

Вы можете отключить диалоговый вопрос о выходе из эмулятора для удобного использования.

Если вам нужно запустить Ryujinx вместо Yuzu для конкретной игры, вы можете добавить опцию запуска в секцию игры. Например:
```
game: My Game
file: /путь/к/вашим/ромам/switch/your_game
launch: Ryujinx "{file.path}"
...
```
