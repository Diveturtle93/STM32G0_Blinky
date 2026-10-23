# Changelog

Alle nennenswerten Änderungen an diesem Projekt werden hier dokumentiert.
Format nach [Keep a Changelog](https://keepachangelog.com/de/1.1.0/),
Versionierung nach [Semantic Versioning](https://semver.org/lang/de/).

## [Unveröffentlicht]

### Hinzugefügt
- STM32CubeIDE-Projekt für den STM32G071RB auf dem Nucleo-G071RB, erzeugt mit STM32CubeMX.
- Die grüne Onboard-LED LD4 (PA5) blinkt im Wechsel 1 s an und 1 s aus.
- Beim Start werden `Test App` und die Versionsnummer über USART2 (PA2/PA3, 115200 Baud, über den ST-Link) ausgegeben.
- Die Application liegt ab `0x08008000` im Flash, die Vektortabelle ist um `0x8000` verschoben, damit sie hinter dem XModem-Bootloader läuft.
- Debug-Konfiguration `STM32G0_Blinky Debug.launch` für STM32CubeIDE.
- Versions-Defines in `Core/Inc/version.h`.
- README mit Hardware, Bauen und Flashen, Schnittstellen und Abhängigkeiten.

[Unveröffentlicht]: https://github.com/Diveturtle93/STM32G0_Blinky/commits/develop
