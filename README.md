# GMagic O'Desk

Open Hardware design of a 512Kbyte C64 multipurpose type cartridge compatible with the following types:
* Ocean 128/256/512
* Magic Desk (Magic Desk, Domark, HES Australia) up to 512Kbyte
* GMod2
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
* 128K subtype: GAME = 0, EXROM = 0 (16k config), but only 16 banks of 8Kb ($2000), banked in at $8000-$9FFF, are accessed. [schematic](./files/ocean128.png)
  <br>Games: *Batman The Movie, Battle Command, Double Dragon, Navy Seals, Pang, Robocop 3, Space Gun,Toki.*
* 256K subtype: GAME = 0, EXROM = 0 (16k config), 32 banks of 8Kb ($2000), 16 banked in at $8000-$9FFF, and 16 banked in at $A000-$BFFF. [schematic](./files/ocean256.png)
  <br>Games: *Shadow of the Beast, Robocop 2, Chase HQ 2.*
* 512K subtype: GAME = 0, EXROM = 1 (8k config), 64 banks of 8Kb ($2000), banked in at $8000-$9FFF. [schematic](./files/ocean512.png)
  <br>Game: *Terminator 2.*

**Magic Desk, Domark, HES Australia**
<br>Bank switching is done by writing to $DE00. The lower six (0-5) bits give the bank number (ranging from 0-64), if bit 7 is zero, the cartridge is active. If bit 7 is set ($DE00 = $80), the GAME/EXROM lines are disabled, turning on RAM at $8000-$9FFF instead of ROM. <br>GAME = 0, EXROM = 1 (8k config).
<br>Games: *Badlands, Cyberball, Vindicators, Arcade Classic Pak, Beamrider, Decathlon, Double Dragon (Melbourne House), Frogger, Galaxions/Munchman, Ghostbusters, Kung Fu Master, Leaderboard, Novablast, Park Patrol, Pastfinder, Pitfall, Pitfall 2, River Raid, Space Shuttle, Tennis, Wonderboy, Zone Ranger.*

**GMod2 (Individual Computer)**
<br>This cart uses 512KiB Flash ROM (29F040) in 64 banks, mapped in at $8000-$9fff and has a 2Kb serial EEPROM (m93C86).<br>GAME = 0, EXROM = 1 (8k config).<br>
Bank switching is done by writing to $DE00.
* bit7   (rw)  write enable (write 1), EEPROM data output (read)
* bit6   (ro)  EXROM (0=active) and EEPROM chip select (1=selected)
* bit5-0 (ro)  rom bank  bit5 EEPROM clock bit4 EEPROM data input<br>
Specs at [http://wiki.icomp.de/wiki/GMod2](http://wiki.icomp.de/wiki/GMod2).<br>
Games: variuos modern games like *Soul Force, Aviator Acrcade II, Planet X2.1, Monstro Giganto, Boxymoxy,* etc etc.

**System 3/C64GS**
<br>ROM memory is organized in 64 banks of 8Kb ($2000), banked in at $8000-$9FFF. Bank switching is done by writing to address $DE00+X, where X is the bank number (STA $DE00,X).
<br>GAME = 0, EXROM = 1 (8k config).<br>
[schematic](./files/c64gs.png)

