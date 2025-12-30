# STM32F7 Blinky

STM32F7 test with blinking LED, to ensure bootloader is working correctly.
Also uart transfer could be tested. The project based on STM32F767ZIT and
is tested on a nucleo board.

The bootloader address is set in the linker script to address 0x08008000
and use the total flash space.

The software use GPIO B14 and UART2.