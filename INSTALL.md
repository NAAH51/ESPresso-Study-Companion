# Install ESPresso on the LCDWiki 2.8-inch ESP32-S3 display

## 1. Install Arduino IDE and ESP32 support

1. Install the current Arduino IDE 2 from https://www.arduino.cc/en/software.
2. In IDE Settings/Preferences, add Espressif's stable Boards Manager URL: https://espressif.github.io/arduino-esp32/package_esp32_index.json.
3. In Boards Manager, install esp32 by Espressif Systems (latest stable). Select ESP32S3 Dev Module in Tools > Board. The tested core for this firmware is Arduino-ESP32 3.3.10; use that version if a future latest core fails to compile.

## 2. Install display libraries and manufacturer configuration

In Library Manager install LVGL 8.3.11 and FT6336-arduino. This exported UI is for LVGL 8; LVGL 9 is not compatible. Use the TFT_eSPI library and User_Setup.h pin/display configuration supplied in the LCDWiki display package: https://www.lcdwiki.com/2.8inch_ESP32-S3_Display. Its manufacturer demo contains the matching TFT_eSPI and FT6336 setup. Generic TFT_eSPI pin defaults will not drive this LCDWiki screen.

Use the manufacturer's LVGL config with 16-bit color and built-in Montserrat sizes 10, 12, 16, and 44 enabled. If LVGL cannot find lv_conf.h, follow https://docs.lvgl.io/8.3/ for placing it next to the Arduino LVGL library. Restart the IDE after library/config changes.

## 3. Open and upload the sketch

1. Download the repository's ESPresso source ZIP from the Code page and extract it.
2. Open ESPresso/ESPresso.ino. Keep the whole ESPresso folder together.
3. Connect the display and select its serial port in Tools > Port.
4. In Tools > Board options choose Flash Size: 16 MB, Partition Scheme: 16M Flash (3MB APP/9.9MB FATFS), and PSRAM: OPI PSRAM. The default app partition is too small for this firmware.
5. Click Verify, then Upload.

The tested setup used LVGL 8.3.11, Arduino-ESP32 3.3.10, TFT_eSPI 2.5.43 with LCDWiki's setup, and the manufacturer's FT6336 driver.

## 4. Edit the UI in SquareLine Studio

Install/open SquareLine Studio 1.6.2 from https://squareline.io/downloads and load SquareLine/SquareLine_Project.spj. It targets LVGL 8 and 240 x 320 portrait. Edit your layout, then export LVGL 8 files into ESPresso/.

For consistent readable text use 16 px headings and 12 px content and button labels. Keep the timer at 44 px. Center button labels and use short text that fits; the small timer controls use only + and −. Keep screen transitions off for smooth performance.

Keep widget names used by the Arduino feature files: StartPauseButton, ResetButton, SubjectNextButton, FocusMinus, FocusPlus, BreakMinus, BreakPlus, ProgressArc, StatsSummary, WeeklyBars, HistoryItems, and TodoItemsLabel. Keep ESPresso.ino, study_feature.cpp/.h, todo_feature.cpp/.h, touch.h, and gfx_config.h when replacing generated UI files.

## 5. Add tasks from your phone

Connect to Wi-Fi ESPresso-Todos (password ESPstudy123), stay connected if your phone warns about no internet, and open http://192.168.4.1. Add tasks individually or remove them one by one. Opening the page also syncs the device clock for daily statistics. Change the hotspot credentials in todo_feature.cpp before distributing a customized build.

## Troubleshooting

- lv_image_dsc_t unknown: install LVGL 8.3.11; LVGL 9 changed generated code.
- Sketch too large: select the 16 MB flash and 3 MB APP partition.
- Blank display or wrong touch orientation: verify LCDWiki User_Setup.h, gfx_config.h, and touch.h for the board revision.
- SquareLine preview compile error: check that its project target is LVGL 8, and remove raw line breaks from any quoted text in custom preview code.
