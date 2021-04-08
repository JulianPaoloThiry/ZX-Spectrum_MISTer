# [ZX Spectrum](https://en.wikipedia.org/wiki/ZX_Spectrum) for MiSTer Platform

The only change in this fork is the keyboard driver:

```
The ZX Spectrum has only 40 keys: 0-9, A-Z, SPACE, ENTER, CAPS SHIFT and SYMBOL
SHIFT. The ZX Spectrum+ raises this to 58, but all of the new keys function by
pressing two existing keys simultaneously. There is no physical difference
between pressing the new key and pressing the existing ones together.

This keyboard driver has been written with the general idea to have all 40
Spectrum keys match behavior exactly, but to also have the additional keys
behave in the most intuitive way possible. This can be boiled down to five
overall goals:

1. The A-Z, 0-9, and SPACE keys must behave exactly as they do on a real
   Spectrum. This is the easy one.

2. Both Shift keys must work as CAPS SHIFT, both Ctrl keys as SYMBOL SHIFT, and
   both Enter keys as ENTER. These keys must behave as if physically wired
   together. I.E., the following sequence must produce a SHIFT Q:
      Right Shift down, Left Shift down, Right Shift up, Q

Note that if 1 and 2 are met, this would be a fully functional driver, as every
Spectrum input can be accomplished. But we can do better!
      
3. Each additional key on the Spectrum+ keyboard that is wired to CAPS SHIFT
   must have an analogous key. These keys must behave as if they are physically
   wired together in the same way. In particular, pressing and releasing
   different combinations of these keys with the keys they are wired to must
   not cause any unexpected signals.
   These keys have been mapped as follows:
      BREAK        (CS+Space)  Escape
      EXTEND MODE  (CS+SS)     Tab
      EDIT         (CS+1)      Home
      CAPS LOCK    (CS+2)      Caps Lock
      TRUE VIDEO   (CS+3)      Page Up
      INV VIDEO    (CS+4)      Page Down
      Left Arrow   (CS+5)      Left Arrow or Numpad 4
      Down Arrow   (CS+6)      Down Arrow or Numpad 2
      Up Arrow     (CS+7)      Up Arrow or Numpad 8
      Right Arrow  (CS+8)      Right Arrow or Numpad 6
      GRAPH        (CS+9)      End
      DELETE       (CS+0)      BACKSPACE

4. Keys bearing symbols that can be produced on a Spectrum using the SYMBOL
   SHIFT key must produce those very symbols. This means that all of the
   following keys must act as if they are wired directly to SYMBOL SHIFT and
   the appropriate second key:
      -_  =+  ;:  '"(US) '@(UK)  ,<  .>  /?  #~(UK) Num/  Num*  Num-  Num+
   For the first eight, a Shifted keypress must also send the appropriate keys.
   Additionally, toggling Shift after pressing any of these keys must not cause
   the Spectrum to see a different key being pressed. For example, pressing
   Shift and ,< will send SS+R for the "<" symbol. Releasing Shift must not
   then cause SS+N (the "," symbol) to be sent, as this is clearly not the
   intent of the user.

Many might be tempted to stop there, but this driver goes beyond!

5. Keys bearing symbols that can be produced on a Spectrum in EXTEND MODE must
   produce those very symbols. This includes the following keys:
      [{  ]}  \|  `~(US)  #~(UK)
   The Shifted versions must work appropriately as well. Since there is no `
   symbol on the Spectrum, this key will produce the only other extended mode
   symbol, ©.

   Note that this feature is meant as a convenience only. These symbols require
   three distinct events in order to be processed correctly:
      SYMBOL SHIFT + CAPS SHIFT
      Release of CAPS SHIFT
      SYMBOL SHIFT + Key
   Because of this, it is necessary to hold the key down long enough for all
   three events to occur. This is done quite quickly, but will definitely be
   noticed by fast typists. If the keypress is too short, the Spectrum will go
   into EXTEND MODE, but will not press the appropriate symbol key afterwards.
   Note also that these keys will result in the wrong things appearing on the
   screen if pressed when EXTEND MODE is already active. This means repeatedly
   pressing these keys quickly will likely produce unexpected keywords. This is
   NOT a bug, but merely the result of the combination of the prior two points.

Having been met, the above five goals should provide the ultimate combination
of Spectrum-like behavior with additional functionality provided by all those
extra keys.

In the above descriptions, there are a few references to (US) and (UK). This is
because US and UK keyboard layouts differ as follows:
    Layout         OSD Mode
   US    UK        US    UK
   `~    `¬        ©~    
   '"    '@        '"    '@
   \|    #~        \|    #~
         \|              \|
You should change the OSD settings to match your keyboard's layout if you want
these keys to print what they display.

As a bonus, this driver also understands the Game Mode of the Recreated ZX
Spectrum keyboard. As this is a completely non-standard protocol, the OSD will
not be able to understand it at all. Qwerty Mode cannot be used because it does
not provide signals for CAPS SHIFT and SYMBOL SHIFT. Therefore, for best
results, you will need a second standard keyboard attached to the MiSTer to use
for controlling the OSD or using the Function Keys.
```

### Download precompiled binaries and system ROMs:
Go to [releases](https://github.com/JulianPaoloThiry/ZX-Spectrum_MISTer/tree/master/releases) folder.

All releases in this repository have a date one day after the equivalent release in the main repository. If you see
a release with the same date as the main release, it means the code has been merged, but not yet compiled and released.
Be patient and the "day after" release will replace that version shortly.
