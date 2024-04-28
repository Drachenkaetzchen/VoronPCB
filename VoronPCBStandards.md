# Required

## Put the damn Klipper Settings on the silkscreen

Document MCU model, clock reference, bootloader offset and any combination of communication interfaces

## Provide RESET and BOOT buttons to enter DFU / BOOT mode

RESET must always be a button. BOOT can either be a button or jumper.

## Use 5V USB jumper to only power the MCU

Only the MCU and other low-power devices are allowed to run when the jumper connects 5V USB (VBUS) and the regular 5V rail. The current must not exceed
500mA.

No other potentially high-current devices on the 5V rail (VBUS) are allowed to run when the 5V jumper is present.
We don't want the Pi to (accidentally) power 5V neopixel strips, LEDs etc. 

# Don't backfeed 5V USB (VUSB) when the jumper is left on

 Diode to prevent backflow of the board's 5V rail to the host computer. The diode must be sized that an accidental
short between the board's higher voltage rails to the board's 5V rail doesn't impact the host computer.

# Fan voltage selection

The usual stack of a 2x3 pin header where the user needs to put on a jumper can easily lead to errors and short rails together.

The currently used pin header arrangement looks like this:

    ▣▣ 24V
    ▣▣ 12V
    ▣▣ 5V

This new arrangement is preferred, as it doesn't allow the user to accidentally shorting out voltage rails by placing a jumper in the wrong position:

    24V ▢▣
    12V ▣▣
     5V ▢▣ 

A clarification diagram should be placed near the fan headers:

      5V             12V             24V
     ▢▣             ▢▣             ▢◼
     ▣◼             ◼◼             ▣◼
     ▢◼             ▢▣             ▢▣

The arrangement can be extended to a fourth position, forming a cross-shaped selection.

# Optional

## Indicate DFU mode is active

When STM32 devices are in bootloader mode, some pins get pulled high or low on startup. Use one of them to light a LED indicating that DFU is active. See STM32's AN2606 for which pins that are. 
