# Z80Patcher
  
This open-hardware project aims to be able to real time patch the Z80 memory map. Sometimes when restoring old arcade game boards you face different problems regarding the data bus. Most of the times you can fix it by replacing some falty logic ICs. However, there are times that you come across a falty or fried PLD that hasn't been cracked/brute-forced yet. Often these PLDs controls the address bus or are used for encryption and protection. This solution bets on a faster access and propagation time for patching the resulting data in real time.  This way we can fix decoding or decription problems by patching a few addreses or replacing an entire rom space.  
  
## Concept
  
- The Z80Patcher board is mounted in the place of the Z80 CPU on the main board. Then, the CPU is mounted on top of the Z80Patcher board.
- One GAL22V10 monitors the Address bits of the Z80. Depending on the address being acessed, using CD4066 switches it redirects the data pins to the actual board or to the patching mechanism.  
- A GAL16V8 can be activated when needed and return the data bits according to a custom bit map or; 
- Using an EEPROM module mounted to the Z80Patcher board, it's also possible to redirect a complete and wider address range to a 27C512.  
- If needed, jumpers are available for hooking the GAL22V10 with other functional pins of the CPU.  
  
## Time Matters  
This solution heavily depends on access and propagation time, so:  
  
**Z80Patcher &#916;t &#60; Arcade Board &#916;t**  
  
Timing example:
|Bus Order       | Z80 Patcher IC | Propagation Delay       | &#916;t |
|----------------|-------------|-------------------------|---------|
| 1              |GAL22V10     |          4ns            |   4ns   |
| 2              |CD4069       |         30ns            |   34ns  |
| 2              |GAL16V8*     |         7.5ns           |  11.5ns |
| 2              |W27C512-70*  |         70ns            |  74ns   |  
| 3              |CD4066       |         35ns            |   69ns  |

*When using the eeprom module, GAL16V8 is not really needed. Likewise, if only a few patches are made, the eeprom module is not really needed.  
  
When patching the ROM address space, look for the access time of the eeprom in range. For example, if in the address range there is a M5l27C64K, the access time will be greater than 250ns and should be more than safe to use this patching method.  
  
## Z80Patcher PCB  
![PiPicoJamma 1.1 Front](https://github.com/ninomegadriver/Z80Patcher/blob/main/Z80Patcher-MainPCB.png?raw=true)  
  
## EEProm Module  
![PiPicoJamma 1.1 Front](https://github.com/ninomegadriver/Z80Patcher/blob/main/Z80Patcher-EEprom-Module.png?raw=true)  
  
  
