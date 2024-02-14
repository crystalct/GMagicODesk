# GMagic O'Desk

Open Hardware design of a 512Kbyte C64 multipurpose type cartridge compatible with the following types:
* Ocean 128/256/512
* Magic Desk (Magic Desk, Domark, HES Australia) up to 512Kbyte
* GMod2
* System 3
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

Cartridge Types
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
<br>Bank switching is done by writing to $DE00. The lower six (0-5) bits give the bank number (ranging from 0-64), if bit 7 is zero, the cartridge is active. If bit 7 is set ($DE00 = $80), the GAME/EXROM lines are disabled, turning on RAM at $8000-$9FFF instead of ROM. <br>GAME = 1, EXROM = 0 (8k config).
<br>Games: *Badlands, Cyberball, Vindicators, Arcade Classic Pak, Beamrider, Decathlon, Double Dragon (Melbourne House), Frogger, Galaxions/Munchman, Ghostbusters, Kung Fu Master, Leaderboard, Novablast, Park Patrol, Pastfinder, Pitfall, Pitfall 2, River Raid, Space Shuttle, Tennis, Wonderboy, Zone Ranger.*

**GMod2 (Individual Computer)**
<br>This cart uses 512KiB Flash ROM (29F040) in 64 banks, mapped in at $8000-$9fff and has a 2Kb serial EEPROM (m93C86).<br>GAME = 1, EXROM = 0 (8k config).<br>
Bank switching is done by writing to $DE00.
* bit7   (rw)  write enable (write 1), EEPROM data output (read)
* bit6   (ro)  EXROM (0=active) and EEPROM chip select (1=selected)
* bit5-0 (ro)  rom bank, bit5 EEPROM clock, bit4 EEPROM data input<br>
Specs at [http://wiki.icomp.de/wiki/GMod2](http://wiki.icomp.de/wiki/GMod2).<br>
Games: variuos modern games like *Soul Force, Aviator Acrcade II, Planet X2.1, Monstro Giganto, Boxymoxy,* etc etc.

**System 3/C64GS**
<br>ROM memory is organized in 64 banks of 8Kb ($2000), banked in at $8000-$9FFF. Bank switching is done by writing to address $DE00+X, where X is the bank number (STA $DE00,X).
<br>GAME = 1, EXROM = 0 (8k config).<br>
[schematic](./files/c64gs.png)<br>
Games: *Last Ninja Remix, Myth, C64GS cartridge (International Soccer, Flimbo's Quest, Klax and Fiendish Freddy).*

**Dinamic**
<br>ROM memory is organized in 16 banks 8Kb ($2000) banks located at $8000-$9FFF. Bank switching is done by reading from address $DE00+X, where X is the bank number (LDA $DE00,X).
<br>GAME = 1, EXROM = 0 (8k config).<br>
Games: *After the War, Aspar GP Master, Astromarine Corps, Narco Police, Satan.*

Components
----------
**All configurations**
- 29F040 (PLCC 32) [U1] or 29F400 (PSOP 44) [U2]
- 74LS273 (or 74HCT273) (SOP 20) [U4]
- 100nF (SMD 0805) x2 [C0, C4]
<br>**Magic Desk - Ocean 128/512**
- 74LS139 (or 74HCT139) (SOP 16) [U3]
- 100nF (SMD 0805) [C3]
<br>**Ocean 256**
- 74LS139 (or 74HCT139) (SOP 16) [U3]
- 100nF (SMD 0805) [C3]
- 1N4148 (SMD 0805) x2 [D3, D4]
- 10KΩ (SMD 1206) [R3]
<br>**GMod2**
- 74LS139 (or 74HCT139) (SOP 16) [U3]
- 100nF (SMD 0805) [C3]
- 93C86 (SOP 8) [U5]
- BC557 (THT) or BC857A (SOT 23)
- 1N4148 (SMD 0805) x2 [D1, D2]
- 10KΩ (SMD 1206) x2 [R1, R2]
<br>**System3 - C64GS - Dinamic**      
- 74LS02 (or 74HCT02) (SOIC 14) [U6]
- 100nF (SMD 0805) [C5]
<br/>**Optional lighting eyes (all configurations)**
- 560Ω (SMD 1206) [R4]
- SMD led (SMD 0603) x2 [DE1, DE2] (UP/DOWN reverse mounted to see the light through the hole)

Jumper configuration
--------------------
**Ocean 128**

| J11 J12 | JEXROM | JGAME | JBANK | JType0-5 | J8K |
|:---:|:---:|:---:|:---:|:---:|:---:|
|Horizontal|Short 2 and 3|Short 2 and 3|Short 1 and 2|Short 1 and 2|Closed|
|![J6](./files/j6.png)|![J2](./files/j2.png)|![J2](./files/j2.png)|![J1](./files/j1.png)|![J1](./files/j1.png)|![J4](./files/j4.png)|
<br>

**Ocean 256**

| J11 J12 | JEXROM | JGAME | JBANK | JType0-5 | J8K |
|:---:|:---:|:---:|:---:|:---:|:---:|
|Horizontal|Short 2 and 3|Short 2 and 3|Short 1 and 2|Short 1 and 2|Open|
|![J6](./files/j6.png)|![J2](./files/j2.png)|![J2](./files/j2.png)|![J1](./files/j1.png)|![J1](./files/j1.png)|![J3](./files/j3.png)|
<br>

**Ocean 512**

| J11 J12 | JEXROM | JGAME | JBANK | JType0-5 | J8K |
|:---:|:---:|:---:|:---:|:---:|:---:|
|Horizontal|Short 2 and 3|Empty|Short 1 and 2|Short 1 and 2|Closed|
|![J6](./files/j6.png)|![J2](./files/j2.png)|![J0](./files/j0.png)|![J1](./files/j1.png)|![J1](./files/j1.png)|![J4](./files/j4.png)|
<br>

**Magic Desk, Domark, HES Australia**

| J11 J12 | JEXROM | JGAME | JBANK | JType0-5 | J8K |
|:---:|:---:|:---:|:---:|:---:|:---:|
|Horizontal|Short 1 and 2|Empty|Short 1 and 2|Short 1 and 2|Closed|
|![J6](./files/j6.png)|![J1](./files/j1.png)|![J0](./files/j0.png)|![J1](./files/j1.png)|![J1](./files/j1.png)|![J4](./files/j4.png)|
<br>

**GMod2**

| J11 J12 | JEXROM | JGAME | JBANK | JType0-5 | J8K |
|:---:|:---:|:---:|:---:|:---:|:---:|
|Vertical|Short 1 and 2|Short 1 and 2|Short 1 and 2|Short 1 and 2|Closed|
|![J7](./files/j7.png)|![J1](./files/j1.png)|![J1](./files/j1.png)|![J1](./files/j1.png)|![J1](./files/j1.png)|![J4](./files/j4.png)|
<br>

**System 3, C64GS, Dinamic**

| J11 J12 | JEXROM | JGAME | JBANK | JType0-5 | J8K |
|:---:|:---:|:---:|:---:|:---:|:---:|
|Horizontal|Short 2 and 3|Empty|Short 2 and 3|Short 2 and 3|Closed|
|![J6](./files/j6.png)|![J2](./files/j2.png)|![J0](./files/j0.png)|![J2](./files/j2.png)|![J2](./files/j2.png)|![J4](./files/j4.png)|
