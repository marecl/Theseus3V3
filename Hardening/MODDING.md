# Modding

## Hardware 

### (WIP) UART
Every board has UART port. This one must too, somewhere.

### Ethernet
Ender 3 V3 mainboard has an unpopulated footprint for an Ethernet jack.  
Brief inspection showed no vital components missing.  
Theoretically, the only requirement is an Ethernet jack with builtin transformer and LEDs.

I thought this was a lost cause, that I'd need to bootleg obscure kernel modules, but there is hope.  
`ifplugd` runs in the background, bound to `eth0`.  

## Software

Klipper is GNU-GPL licensed. I can look where I want.  
Linux kernel too. Dot dot dot.  

**Open source CrealityOS when???**

### Klipper version
Creality was kind enough to **remove the source directory** of Klipper from the production firmware. 

Luckily, they didn't remove docs. With `git diff` and some free time I managed to pull those (some got lost in the sauce):
1. `docs/CANBUS.md` 4b77f29c1b20d5fdac577c557699e0b20f78d799 (13.10.2022-31.05.2023)
2. `docs/CONTRIBUTING.md` d46c95b758b9fd67a38474d5e8cfbce6d2d12b70 (15.10.2022-15.03.2023)
3. `docs/Status_Reference.md` 2a25733cd8d32bf7c02c896da0ce7964d7474ba6 (17.12.2022-04.02.2023)
4. `docs/Bootloaders.md` be4b8b17f26ecafa264ea5379ffc3b3c20e1ce1f (10.05.2022-20.02.2023)
5. `docs/index.md` 2f9fe49cb85eed35676a2711686d6ddc0514ac5a (16.07.2022-(present))
6. `docs/_klipper3d/README` b35b6cb10b323955d716686ecf9ee31b16681393 (07.04.2022-02.04.2025)
7. `docs/_klipper3d/build-translations.sh` 9047702a00c716245c37dba72ddc0007c0b37f69 (04.06.2022-(removed))
8. `docs/Sponsors.md` f7e29b276e5c2454eaa801e78cf3f6aff29c4ba9 (25.08.2022-06.06.2023)
9. `docs/Config_Reference.md` 9b60daf62dd7c02164c53f2baa72e3e6c8af441f (22.12.2022-31.12.2023)
10. `docs/Bed_Mesh.md` 9727f00e504aaae5b7fd401e790895eeebbde784 (23.11.2022-04.01.2023)

Earliest fit: [9b60daf62dd7c02164c53f2baa72e3e6c8af441f](https://github.com/Klipper3d/klipper/commit/9b60daf62dd7c02164c53f2baa72e3e6c8af441f) (22.12.2022)
Latest fit: [1ff0a5494aba59cffb3a912c82e474aa5eebdd03](https://github.com/Klipper3d/klipper/commit/1ff0a5494aba59cffb3a912c82e474aa5eebdd03) (31.12.2022)

### Additional changes

I'll drop a full diff sometime, I think.  
There are some minor modifications like added telemetry, verbose errors etc.  
However many devices have been dropped, like support for SPI sensors, HD44780 display and some TMC drivers. Too bad because I really wanted to add some more stuff to the printer, now I have to tinker with it even more.