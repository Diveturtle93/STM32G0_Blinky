# STM32G0 Blinky
 
Ein einfaches STM32G0-Testprojekt mit blinkender LED und UART-Ausgabe. Es dient als
Referenz-Application für den [STM32 XModem-Bootloader](https://github.com/Diveturtle93/STM32_BootloaderXModem)
und zeigt, wie ein STM32G0-Projekt korrekt für den Betrieb hinter einem Bootloader
konfiguriert wird.
 
Das Projekt basiert auf dem **STM32G071RB** und wurde auf einem **Nucleo-G071RB**-Board
getestet.
 
## Beschreibung
 
Die Application blinkt eine LED im 500-ms-Takt und gibt zyklisch eine Statusmeldung
über UART aus. Der primäre Zweck ist die Verifikation, dass der XModem-Bootloader eine
neue Firmware korrekt in den Flash schreibt und danach zuverlässig in die Application
springt.
 
## Hardware
 
| Signal  | Pin | Beschreibung                  |
|---------|-----|-------------------------------|
| LED     | PA5 | Onboard-LED (LD4, grün)       |
| UART TX | PA2 | UART2 Transmit (über ST-Link) |
| UART RX | PA3 | UART2 Receive (über ST-Link)  |
 
## Flash-Konfiguration
 
Die Application ist für den Betrieb hinter dem XModem-Bootloader konfiguriert.
Die Flash-Startadresse ist im Linker-Skript `STM32G071RBTX_FLASH.ld` auf `0x08008000`
gesetzt:
 
```
FLASH (rx) : ORIGIN = 0x08008000, LENGTH = 96K
```
 
Die Vektortabelle in `system_stm32g0xx.c` ist entsprechend auf das Offset `0x00008000`
verschoben:
 
```c
#define USER_VECT_TAB_ADDRESS
#define VECT_TAB_OFFSET  0x00008000U
```
 
## Projektstruktur
 
| Ordner / Datei              | Beschreibung                                               |
|-----------------------------|------------------------------------------------------------|
| `Core/`                     | Anwendungscode (`main.c`, Interrupts, Systemkonfiguration) |
| `Drivers/`                  | STM32 HAL und CMSIS-Treiber                                |
| `STM32G071RBTX_FLASH.ld`    | Linker-Skript für Flash-Betrieb ab `0x08008000`            |
| `STM32G0_Blinky.ioc`        | STM32CubeMX-Projektdatei                                   |
 
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
