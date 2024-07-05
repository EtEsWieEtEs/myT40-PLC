# myT40-PLC  (SPS) based on Teensy 4.0
Hardware for PLC based on Teensy 4.0, covering all my needs. I have to replace my 25 years old PS4-141-MM1 and PS4-341-MM1 from Klöcker Moeller, reusing the original case of the model PS4-141-MM1.

For better return paths of capacitively coupled signals to related ground on additional layers I created a 4-layers version for Upper PCB "OP". The lower PCB is designed as 2-layer board.

Attended documents describe the "Final Version V4" as "V2.9_M41", based on experience of 4 prototypes build up in the past 4 years.

Current developement:
---------------------
In the meantime I build up the Final version 4 as M38, but I found some more errors. See pictures.
One error I found was, that some input pins on Teensy 4.0 have a lower input resistance as expencted, and that leads to the effect, that a pull-up resistor does not lift the resultig level over 2.1V, which is needed by the OR-gate to trigger the NE555 for power-on.
The combination R126/R128 to provide 3.2V for pull-up SW_MODE_2 was substituted by R126=220Ohm a direct connection to 3.3V from lower board.
Additionally the OR-gate was changed to a level shift variant for translation 3.3V to 5V signaling.
An other effect causes an voltage edge down on 5V power line during power-up Teensy4.0 that leeds to a RESET trigger by U48 (reset generator 5V). Therefore U48 and C1 were eliminated.
Now it is working and I provide the actualized description for the very very final Version as V2.9 M41.

There are some things to do, e.g. external Hardware watchdog or translation of documentation into English or Spanish. Perhaps in winter.
So feel free and be inspired.

---------------------
This work is licensed under the Creatjve Commons Atuributjon-NonCommercial-ShareAlike 4.0 Internatjonal
License. To view a copy of this license, visit htup://creatjvecommons.org/licenses/by-nc-sa/4.0/ or send a
letuer to Creatjve Commons, PO Box 1866, Mountain View, CA 94042, USA.
This license lets others remix, adapt, and build upon your work non-commercially, as long as they credit you
and license their new creatjons under the identjcal terms.
htup://creatjvecommons.org/licenses/by-nc-sa/4.0/

  Title    myT40-PLC Documentatjon

  Author   Clemens Niesen, Germany, 2023 + 2024

  Source   These documents

  Licence  CC-BY-NC-SA

DISCLAIMER
----------

I explicitly state that I make no representations or warranties regarding non-infringement or the absence of other defects with respect to the CC-licensed work.This means that users use the work at their own risk.

Furthermore, I point out that this project and the content of this documentation, as well as associated files, is purely a recreational project and should be understood as a case study for the reader. 

This project, these circuits are not tested or approved by official bodies. Errors can not be excluded despite all care. Whoever reproduces this project does so at his own risk and responsibility. I do not take any responsibility for damages to persons and devices, material, buildings, animals, etc..

Rebuilding is reserved exclusively for experienced people who know exactly what they are doing.  What third-party use of the control with third does and does not, lies apart from the craftsmanship of the construction also with the software, which is also beyond my responsibility. I give no guarantee and take no responsibility for the content of references, links and quotes.
