# STM32F7 Blinky
 
Ein einfaches STM32F7-Testprojekt mit blinkender LED und UART-Ausgabe. Es dient als
Referenz-Application für den [STM32 XModem-Bootloader](https://github.com/Diveturtle93/STM32_BootloaderXModem)
und zeigt, wie ein STM32F7-Projekt korrekt für den Betrieb hinter einem Bootloader
konfiguriert wird.
 
Das Projekt basiert auf dem **STM32F767ZIT** und wurde auf einem **Nucleo-F767ZI**-Board
getestet.
 
## Beschreibung
 
Die Application blinkt eine LED im 500-ms-Takt und gibt zyklisch eine Statusmeldung über
UART aus. Der primäre Zweck ist die Verifikation, dass der XModem-Bootloader eine neue
Firmware korrekt in den Flash schreibt und danach zuverlässig in die Application springt.
 
## Hardware
 
| Signal  | Pin   | Beschreibung                  |
|---------|-------|-------------------------------|
| LED     | PB14  | Onboard-LED (LD3, rot)        |
| UART TX | PA2   | UART2 Transmit (über ST-Link) |
| UART RX | PA3   | UART2 Receive (über ST-Link)  |
 
## Flash-Konfiguration
 
Die Application ist für den Betrieb hinter dem XModem-Bootloader konfiguriert.
Die Flash-Startadresse ist im Linker-Skript `STM32F767ZITX_FLASH.ld` auf `0x08008000`
gesetzt:
 
```
FLASH (rx) : ORIGIN = 0x08008000, LENGTH = 32K
```
 
Die Vektortabelle in `system_stm32f7xx.c` ist entsprechend auf das Offset `0x00008000`
verschoben:
 
```c
#define USER_VECT_TAB_ADDRESS
#define VECT_TAB_OFFSET  0x00008000U
```
 
## Projektstruktur
 
| Ordner / Datei              | Beschreibung                                          |
|-----------------------------|-------------------------------------------------------|
| `Core/`                     | Anwendungscode (`main.c`, Interrupts, Systemkonfiguration) |
| `Drivers/`                  | STM32 HAL und CMSIS-Treiber                           |
| `Lib/`                      | Eingebundene Bibliotheken als Git-Submodule           |
| `STM32F767ZITX_FLASH.ld`    | Linker-Skript für Flash-Betrieb ab `0x08008000`       |
| `STM32F767ZITX_RAM.ld`      | Linker-Skript für RAM-Betrieb (Debug)                 |
| `STM32F7_Blinky.ioc`        | STM32CubeMX-Projektdatei                              |
 
## Verwendete Bibliotheken
 
Das Projekt bindet folgende Bibliotheken über Git-Submodule ein (Ordner `Lib/`):
 
- [STM32_Basicuart](https://github.com/Diveturtle93/STM32_Basicuart)
- [STM32_Systeminfo](https://github.com/Diveturtle93/STM32_Systeminfo)
- [STM32_Resetreason](https://github.com/Diveturtle93/STM32_Resetreason)
- [STM32_Hardfault](https://github.com/Diveturtle93/STM32_Hardfault)

## Voraussetzungen
 
- STM32CubeIDE
- STM32CubeMX (optional, zum Öffnen der `.ioc`-Datei)
- [STM32 XModem-Bootloader](https://github.com/Diveturtle93/STM32_BootloaderXModem),
zuvor auf den Mikrocontroller geflasht

## Abhängigkeiten
 
- `main.h` – STM32 HAL
- [`basicuart.h`](https://github.com/Diveturtle93/STM32_Basicuart) – UART-Sende- und Empfangsfunktionen

## Lizenz
 
Dieses Projekt steht unter der [GPL-3.0 Lizenz](LICENSE).
