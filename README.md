# myT40-PLC  (SPS) based on Teensy 4.0
Hardware for PLC based on Teensy 4.0, covering all my needs I have to be able to replace my 25 years old PS4-141-MM1 and PS4-341-MM1 from Klöcker Moeller, reusing the original case of model PS4-141-MM1.
DIN-5 connectors and extension slot are incomatible to the origin.

For lower frequencies and lower budget, standard PCB has 2 layers. 
For better return paths of capacitively coupled signals to related ground on additional layers I created a 4-layers version for Upper PCB "OP".
The final version of upper pcb (OP) will provided in 4 layers only due to a better stability.


Current developement:
---------------------
In the meantime I build up the Final version 4 as M38, but I found some more errors.
One error I found was, that some input pins on Teensy 4.0 have a lower input resistance as expencted, and that leads to the effect, that a pull-up resistor does not lift the resultig level over 2.1V, which is needed by the OR-gate to trigger the NE555 for power-on.

The combination R126/R128 to provide 3.2V for pull-up SW_MODE_2 was substituted by R126=220Ohm and R128 by zener diode 3.0V. 

Additionalle the OR-gate was changed to a level shift variant for translation 3.3V to 5V signaling.

An other effect causes an edge down on 5V power line during power-up Teensy4.0 that leeds to a RESET trigger by U48 (reset generator 5V). Therefore U48 and C1 were eliminated.

You find the pictures of prototypes V2,V3 and V4, the lather one labled as M38 with corrections is labled now as M40.

Now it is working and I will provide an actualized description for the very very final Version as V2.9 M41 in the next time.

 
---------------------
This work is licensed under the Creatjve Commons Atuributjon-NonCommercial-ShareAlike 4.0 Internatjonal
License. To view a copy of this license, visit htup://creatjvecommons.org/licenses/by-nc-sa/4.0/ or send a
letuer to Creatjve Commons, PO Box 1866, Mountain View, CA 94042, USA.
This license lets others remix, adapt, and build upon your work non-commercially, as long as they credit you
and license their new creatjons under the identjcal terms.
htup://creatjvecommons.org/licenses/by-nc-sa/4.0/

  Title    myT40-PLC Documentatjon

  Author   Clemens Niesen, Germany, 2023

  Source   These documents

  Licence  CC-BY-NC-SA

DISCLAIMER

I explicitly state that I make no representations or warranties regarding non-infringement or the absence of other defects with respect to the CC-licensed work.This means that users use the work at their own risk.

Furthermore, I point out that this project and the content of this documentation, as well as associated files, is purely a recreational project and should be understood as a case study for the reader. 

This project, these circuits are not tested or approved by official bodies. Errors can not be excluded despite all care. Whoever reproduces this project does so at his own risk and responsibility. I do not take any responsibility for damages to persons and devices, material, buildings, animals, etc..

Rebuilding is reserved exclusively for experienced people who know exactly what they are doing.  What third-party use of the control with third does and does not, lies apart from the craftsmanship of the construction also with the software, which is also beyond my responsibility. I give no guarantee and take no responsibility for the content of references, links and quotes.
