# Hardware hacking

## UART
Every board has UART port. This one must too, somewhere.

## Ethernet
Ender 3 V3 mainboard has an unpopulated footprint for an Ethernet jack.  

### Hardware
Brief inspection showed no vital components missing.  
Theoretically, the only requirement is an Ethernet jack with builtin transformer and LEDs.

### Software
I thought this was a lost cause, that I'd need to bootleg obscure kernel modules, but there is hope.  
`ifplugd` runs in the background, bound to `eth0`.  