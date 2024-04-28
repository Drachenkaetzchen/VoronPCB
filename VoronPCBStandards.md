Felicia's personal list for better Klipper Boards. Future vision: Voron Seal of Quality ;)

# Required

## Put the damn Klipper Settings on the silkscreen

Document MCU model, clock reference, bootloader offset and any combination of communication interfaces. Here's an example:

    Klipper Settings
    STM32F072/12 MHz
    No Bootloader
    USB PA11/PA12 or CAN PB8/PB9
    
## Provide RESET and BOOT buttons to enter DFU / BOOT mode

RESET must always be a button. BOOT can either be a button or jumper.

WIP: Button spacing. Preferably the both buttons are either placed next to each other, so that they can be operated with one finger, or
sufficiently spaced apart? Or prefer jumper altogether? but Jumper has the disadvantage that it can be forgotten, and since dfu-util
reboots the board into a freshly written firmware, a forgotten boot jumper could only be noticed after a poewr cycle, which sometimes
can take days until a user stumples upon that issue.


## Use 5V USB jumper to only power the MCU

Only the MCU and other low-power devices are allowed to run when the jumper connects 5V USB (VBUS) and the regular 5V rail. The current must not exceed
500mA.

No other potentially high-current devices on the 5V rail (VBUS) are allowed to run when the 5V jumper is present.
We don't want the Pi to (accidentally) power 5V neopixel strips, LEDs etc. 

Implementation:

- Split the 5V rail into two nets. Suggestion: +5V and +5VP.
- Connect +5VP to +5V via a power distribution switch when +5VP is present.
- Currently testing out the TPS27081ADDCR for that purpose


## Provide a LED for each voltage rail

One LED for every rail. Label them accordingly. If you have 2 5V rails like explained above, have one LED per segment.

In case something goes wrong, it sucks to waste the time of the user and helpers. 

## Don't backfeed 5V USB (VUSB) when the jumper is left on

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

# MOSFET output indications

All MOSFET driven outputs must have an LED indicator. The indicator must be placed on the driving side of the MOSFET, so that the user can verify that their
configuration is correct and that the mosfet is actually being driven.

The output side may optionally have a LED indicator, which allows the user to see when the MOSFET is requested to be on, but there's some other issue (like output protection active / fuse blown / no supply voltage present).

# MOSFET output protection

All MOSFET driven outputs must be protected against short circuits. We don't want an accidental temporary short of the heater burn people's houses down.

# Optional

## Indicate DFU mode is active

When STM32 devices are in bootloader mode, some pins get pulled high or low on startup. Use one of them to light a LED indicating that DFU is active. See STM32's AN2606 for which pins that are. 
