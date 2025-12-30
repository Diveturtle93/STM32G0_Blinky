# STM32G0_Blinky

STM32G0 test with blinking LED, to ensure bootloader is working correctly.
Also uart transfer could be tested. The project based on STM32G071RB6 and
is tested on a nucleo board.

The bootloader address is set in the linker script to address 0x08008000
and use the total flash space.

The software use GPIO A5 and UART2.