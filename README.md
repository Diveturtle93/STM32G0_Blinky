# STM32G0 Blinky

Ein einfaches STM32G0-Testprojekt mit blinkender LED und UART-Ausgabe. Es dient als
Referenz-Application für den [STM32 XModem-Bootloader](https://github.com/Diveturtle93/STM32_BootloaderXModem)
und zeigt, wie ein STM32G0-Projekt für den Betrieb hinter einem Bootloader konfiguriert
wird.

## Beschreibung

Nach dem Start gibt die Application einmalig eine Begrüßung und ihre Versionsnummer über
UART aus. Danach blinkt die LED mit 1 s an und 1 s aus. Der primäre Zweck ist die
Verifikation, dass der XModem-Bootloader eine neue Firmware korrekt in den Flash schreibt
und danach zuverlässig in die Application springt.

Ausgabe nach dem Start:

```
Test App
Revision:	0.0.0.1
```

Die Versionsnummer stammt aus den Defines `VERSION_MAJOR`, `VERSION_MINOR`,
`VERSION_PATCH` und `VERSION_DEV` in `Core/Inc/version.h`.

## Hardware

- Mikrocontroller: **STM32G071RBT**
- Platine: getestet auf dem **Nucleo-G071RB**
- Takt: interner HSI-Oszillator mit 16 MHz, ohne PLL

| Signal  | Pin | Beschreibung                  |
|---------|-----|-------------------------------|
| LED     | PA5 | Onboard-LED (LD4, grün)       |
| UART TX | PA2 | USART2 Transmit (über ST-Link) |
| UART RX | PA3 | USART2 Receive (über ST-Link)  |

USART2 ist beim Nucleo-G071RB mit dem virtuellen COM-Port des ST-Link verbunden, die
Ausgabe erscheint also direkt am USB-Anschluss des Boards.

## Bauen und Flashen

### Voraussetzungen

- STM32CubeIDE
- STM32CubeMX (optional, zum Öffnen der `STM32G0_Blinky.ioc`)
- [STM32 XModem-Bootloader](https://github.com/Diveturtle93/STM32_BootloaderXModem),
  zuvor auf den Mikrocontroller geflasht

### Bauen

Das Projekt in der STM32CubeIDE importieren und mit der Build-Konfiguration `Debug` oder
`Release` bauen. Beide Konfigurationen verwenden das Linker-Skript
`STM32G071RBTX_FLASH.ld`. Nur `Debug` erzeugt zusätzlich zur `.elf` die für den
Bootloader nötige `.bin`-Datei (`Debug/STM32G0_Blinky.bin`). Für die Debug-Sitzung liegt
die Konfiguration `STM32G0_Blinky Debug.launch` bei.

### Flash-Konfiguration

Der Bootloader belegt den Flash ab `0x08000000`, die Application beginnt bei
`0x08008000`. Die Aufteilung des Flash beschreibt die
[ARCHITECTURE des Bootloaders](https://github.com/Diveturtle93/STM32_BootloaderXModem/blob/main/ARCHITECTURE.md).

Im Linker-Skript `STM32G071RBTX_FLASH.ld` ist die Startadresse entsprechend gesetzt:

```
FLASH    (rx)    : ORIGIN = 0x8008000,   LENGTH = 96K
```

Die Vektortabelle in `Core/Src/system_stm32g0xx.c` ist auf das Offset `0x00008000`
verschoben:

```c
#define USER_VECT_TAB_ADDRESS
#define VECT_TAB_OFFSET         0x00008000U
```

### Flashen

Die Firmware wird über den XModem-Bootloader übertragen. Der Ablauf steht im
[RUNBOOK des Bootloaders](https://github.com/Diveturtle93/STM32_BootloaderXModem/blob/main/RUNBOOK.md)
unter „Firmware übertragen“. Für dieses Projekt gilt:

| Platzhalter  | Wert                       |
|--------------|----------------------------|
| `‹Firmware›` | `Debug/STM32G0_Blinky.bin` |

Port, Baudrate und Reset richten sich nach dem Bootloader-Projekt, nicht nach dieser
Application. Nach erfolgreicher Übertragung erscheinen `Test App` und `Revision:` mit der
Versionsnummer, und LED LD4 blinkt.

## Schnittstellen

| Schnittstelle | Einstellung                                                               | Verwendung                         |
|---------------|---------------------------------------------------------------------------|------------------------------------|
| USART2        | 115200 Baud, 8 Datenbits, keine Parität, 1 Stoppbit, keine Flusssteuerung | Ausgabe von Begrüßung und Version  |

## Abhängigkeiten

Keine eigenen Bibliotheken, nur STM32 HAL und CMSIS aus `Drivers/`.

## Lizenz

Dieses Projekt steht unter der [GPL-3.0 Lizenz](LICENSE).
