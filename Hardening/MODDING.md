# Modding

## Hardware 

I thought this was a lost cause, that I'd need to bootleg obscure kernel modules, but there is hope.  
`ifplugd` runs in the background, bound to `eth0`.  

### LCD

ST7701, 480x800, SPI
Pin mapping:  
* CS: PA02
* POWER_EN: PC21
* RST: PB29
* SCL: PC27
* SDA: PC28
* SDO: PB16

### Touchscreen

This printer uses 2 touch modules: GT9xx/TLSC6x.  
Drivers for both are loaded and they have the same set of parameters.

Pin mapping:  
* gtp_i2c_bus_num: 4
* gtp_irq_gpio: PA03
* gtp_max_touch_number: 2
* gtp_power_on_gpio: -1
* gtp_regulator_name: -1
* gtp_reset_gpio: PA00

Parameters:  
* x_coords_flip: 0
* x_coords_max: 480
* x_coords_min: 0
* x_y_coords_exchange: 0
* y_coords_flip: 0
* y_coords_max: 800
* y_coords_min: 0

### Backlight

Info:  
* backlight_dev_name: backlight_pwm0
* default_brightness: 300
* max_brightness: 300
* power_gpio_vaild_level: 1
* pwm_active_level: 1
* pwm_freq: 50000

Pin mapping:  
* power_gpio: PC22
* pwm_gpio: PC00


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

### Devices

Slave devices for klipper.  
Creality uses GigaDevices ARM ICs, which are a drop-in replacement for STM32.  
Applied GD32F303 is an equivalent of STM32F101. Manufacturer claims nearly perfect software compatibility, hardware needs to be verified.  

All 3 have different binaries so I'm a bit skeptical whether I can get away with flashing latest Klipper.  

#### CPU

IC: Ingenic X2000E
Cores: 2@1.2GHz, 1@240MHz
Crystal: 24MHz
ISA: mips1, mips2, mips32r2

#### MCU

IC: GigaDevices GD32F303XE
Clock: 120MHz
Crystal: 8MHz
Bootloader offset: ?
Communication: USART ?

#### Nozzle MCU

IC: GigaDevices GD32F303XB
Clock: 120MHz
Crystal: 8MHz
Bootloader offset: ?
Communication: USART ?

#### Leveling MCU

IC: GigaDevices GD32F303XB
Clock: 120MHz
Crystal: 8MHz
Bootloader offset: ?
Communication: USART ?


## Unpopulated headers

Worth checking out for upgrades.

### Unplugged

Mainboard
-
| symbol | label | function | type | pins | remarks |  
| ------ | ----- | -------- | ---- | ---- | ------- |  
| `J7/J8` |  | USB | JST-PH 2.00mm 4pin | 1. 5V<br>2. D-<br>3. D+<br>4. GND | Generic USB connector |  
| `J9/J10` |  | USB | JST-PH 2.00mm 4pin | 1. 5V<br>2. D-<br>3. D+<br>4. GND | Generic USB connector |  
| `J38?` | `LED` | ? Static LED lighting | JST-XH 2.54 2pin | 1. -<br> 2. + (24V) |  | 
| `J39` | `FAN` | Fan | JST B02P-NV | 1. + (24V)<br>2. - | NCEPOWER NCEP40P80K P-MOSFET, circuitry missing protective diode |  
| `J43?` | `FAN2` | Fan 2 | JST-XH 2.54 2pin | 1. -<br> 2. + (24V) |  |  
| `J44?` | `FAN1` | Fan 1 | JST-XH 2.54 2pin | 1. -<br> 2. + (24V) |  |  
| `J53` | `ADC-HEAT` | ? Chamber temperature | JST-XH 2.54 2pin | 1. GND<br>2. ADC |  |  
| `J55` |  | USB - LiDAR | JST-XH 2.54 4pin | ? | Generic USB connector |  

Bed
-
| symbol | label | function | type | pins | remarks |  
| ------ | ----- | -------- | ---- | ---- | ------- |  

Nozzle
-
| symbol | label | function | type | pins | remarks |  
| ------ | ----- | -------- | ---- | ---- | ------- |  


### Unsoldered

Mainboard
-
| symbol | label | function | type | pins | remarks |  
| ------ | ----- | -------- | ---- | ---- | ------- |  
| `J1` |  | Serial | Header 2.54mm 3pin | 1. GND<br>2. RX<br>3. TX |  Could be 5V tolerant
| `J5` |  | USB | JST-PH 2.00mm 4pin | 1. 5V<br>2. D-<br>3. D+<br>4. GND | microUSB breakout |  
| `J26` | `TOUCH` | BLTouch | Molex PicoBlade 5pin | 1. OUT<br>2. GND<br>3. IN<br>4. 5V<br>5. GND |  |  
| `J30` | `E` | Extruder | JST-XH 2.54 4pin |  |  
| `J35` | `Y-` | Y Limit Switch | JST-PH 2.00mm 3pin | 1. V<br>2. G<br>3. S |  |  
| `J45` |  | Ethernet |  |  | With LED and transformer. To be tested. |  
| `J48` |  | ? SPI0 | JST-PH 2.00mm 6pin | 1. 3V3<br>2. MISO<br>3. MOSI<br>4. CLK<br>5. CS<br>6. GND | No filter circuit, no kernel module loaded for SPI |  
| `J49` |  | ? SPI1 | JST-PH 2.00mm 6pin | 1. 3V3<br>2. MISO<br>3. MOSI<br>4. CLK<br>5. CS1<br>6. GND | No filter circuit, no kernel module loaded for SPI |  
| `J52` | `HEAT` | ? Heated chamber | JST B02P-NV | 1. + (24V)<br>2. - | NCEPOWER NCEP40P80K P-MOSFET, circuitry missing protective diode. Probably the same as for bed: SS54 SMA/SOD128 (40V, 5A)|  
| `J56` | `OPEN1` | ? Switch | JST-PH 2.00mm 3pin  | 1. V<br>2. G<br>3. S | Missing components |  
| `J57` | `OPEN2` | ? Switch | JST-PH 2.00mm 3pin | 1. V<br>2. G<br>3. S | Missing components |  
| `J58` | `RESERVE` | ? Switch | JST-PH 2.00mm 3pin | 1. V<br>2. G<br>3. S | Missing components |  
| `J59` | `RS485` | RS485 interface | JST-XH 2.54 3pin | 1. A<br>2. B<br>3. GND | Transceiver circuitry missing |  
| `J60` | `AC_BED` | ? | JST-PH 2.00mm 4pin | 1. 5V<br>2. GND<br>3. CTR<br>4. STANDBY |  |  
| `J61?` | `LASER` | Laser | JST-XH 2.54 3pin | 1. 24V<br>2. GND<br>3. LASER |  
| `SPK` |  | Speaker | JST-XH 1.25 2pin | 1. SPKP<br>2. SPKN |  |  
| `M1` |  | Mic | D6mm, 2mm pitch | 1. MICN<br>2. MICP |  |  

Nozzle
-
| symbol | label | function | type | pins | remarks |  
| ------ | ----- | -------- | ---- | ---- | ------- |  

Bed
-
| symbol | label | function | type | pins | remarks |  
| ------ | ----- | -------- | ---- | ---- | ------- |  
