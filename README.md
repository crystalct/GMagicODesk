# GMagic O'Desk

Open Hardware design of a 512Kbyte C64 multipurpose type cartridge compatible with the following types:
* Ocean 128/256/512
* Magic Desk (Magic Desk, Domark, HES Australia) up to 512Kbyte
* Gmod2
* System3
* C64GS
* Dinamic

Due to the similarity of the listed cartridge types, it was possible to create a single PCB which, with the appropriate configurations, will work perfectly as a basis for the creation of cartridges containing games developed for those cartridge types.<br>
Furthermore, my secondary objective was the reuse of many SMD chips at my disposal and therefore this project differs from other similar projects mainly due to the use of SMD technology, making it more complex regarding the soldering work.<br>
If you want to acquire a simpler PCB I recommend the [c64-uni-cart](https://github.com/msolajic/c64-uni-cart) by Marko Šolajić (Ocean/Magic Desk) and [GMod2](https://www.freepascal.org/~daniel/gmod2/) by Daniel Mantione (GMod2/Ocean/Magic Desk).

Bank Selection
--------------
The types of cartridge listed above can be divided into two types: "data" type bank selection and "address" type bank selection.<br>
* "Data" bank selection: Ocean, Magic Desk and GMod2. Bank switching is done by writing X to $DE00, where X is the bank number.
* "Address" bank selection: System3, CG64GS and Dinamic. Bank switching is done by accessing address $DE00+X, where X is the bank number.

To correctly map the memory inside the FLASH an 8-bit latch (74LS273) is used and the decoding of the PHI2 and I/O1 signals with a Dual 2-Line To 4-Line Decoders/Demultiplexer (74LS139) for the "data" banking and a NOR gates (74LS02) for "address" banking.

Schematics
----------
![schematics](./files/GMagicODesk.png)

Appearance
----------
![appearance](./files/PCB1.1.png)

Cartrdige Types
---------------
**Ocean**
<br>Bank switching is done by writing to $DE00. The lower six bits give the bank number (ranging from 0-63).<br>
* 128K subtype: GAME = 0, EXROM = 0 (16k config), but only 16 banks of 8KiB ($2000), banked in at $8000-$9FFF, are accessed. [schematic](./files/ocean128.png)
  <br>Games: *Batman The Movie, Battle Command, Double Dragon, Navy Seals, Pang, Robocop 3, Space Gun,Toki.*
* 256K subtype: GAME = 0, EXROM = 0 (16k config), 32 banks of 8KiB ($2000), 16 banked in at $8000-$9FFF, and 16 banked in at $A000-$BFFF. [schematic](./files/ocean256.png)
  <br>Games: *Shadow of the Beast, Robocop 2, Chase HQ 2.*
* 512K subtype: GAME = 1, EXROM = 0 (8k config) , 64 banks of 8KiB ($2000), banked in at $8000-$9FFF. [schematic](./files/ocean512.png)
  <br>Game: *Terminator 2.*

.


